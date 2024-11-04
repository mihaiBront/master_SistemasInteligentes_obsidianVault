

Inspection JSON interface
=========================
Version 1.06

## Basic concepts

### Return code
All commands returns a JSON which at least one attribute named "status". In the case the command succeeds, "status" is set to the string "ok":

```json
{"status" : "ok"}
```

:label: A successful JSON may contain other attributes.

For deferred jobs, the attribute "status" may hold the state "processing". In this case an additional attribute with the job_id shall be provided.

```json
{"status" : "processing", "job_id" : 1234}
```

In the case the command fails, "status" is set to the string "fail" and an additional attribute named "reason" is set to a plain text description in english of the error.  
For simplicity sake, no error codes are given.
```json
{"status" : "fail", "reason" : "not enough memory"}
```
### Job types
The inspection system uses the term "job" to identify an inspection task.

:books: Current job list:

- :one: **"nozzle_out"** - The nozzle out pattern is captured and processed, returns nozzle out tables.
- :two: **"uniformity"** - The uniformity pattern is captured and processed, returns linearization tables.
- :three: **"uniformity_check"** - The uniformity pattern is captured and processed, returns measurements for drawing a curve.
- :four: **"camera_hardware"** - The camera calibration pattern for hardware is captured in several images, one per camera. 
- :five: **"camera_calibrate"*** - The camera calibration pattern is captured in several images, one per camera. White zone is captured and stored in white reference. Unless protected, stitching is computed.

### Page types
A job may contain a number of pages. Each page may be of a different type. If same page type is scanned twice in the same job, second instance overwrites first. 
This works in this way to allow to repeat wrong scans.

- **"single"** - The one and only page type for this category 
- **"C"** - Cyan channel   
- **"M"** - Magenta channel   
- **"Y"** - Yellow channel   
- **"K"** - Black channel    
- **"O"** - Orange channel    
- **"V"** - Violet channel    
- **"W"** - reserved for white    
- **"cam1"** - Camera #1    
- **"cam2"** - Camera #2    
- **"cam3"** - Camera #3   
- **"cam4"** - Camera #4    
- **"cam5"** - Camera #5    
    
### Products
A product is what the inspection system returns after processing a job. It is labeled with the combination of a page type and a URL for resource location. 

Type of data will be returned as MIME type when fetching the resource pointed by the URL. The product comes as part of other JSON structures.

Example 1:
```json
{ "cam1" : "/images/img123.png" } 
```

Example 2:
```json
{ "M" : "/products/prod456.json" }
```
## High-Level commands

### Get system status
Returns a system-wide status of all inspection system

```json
{ "command": "system_status"}
```

Typical return includes many fields:
```json
{ 
"status" : "ok",       
"version" : "1.0.0.0",
"about" : "whatever to show in UI, for example EFI inspection",            
"cameras" : [                    
	{ 
		"position" : 1, 
		"serial_number" : "xxx", 
		"model_name" : "genie", 
		"vendor" : "teledyne", 
		"status" : "ok" 
	},
	{ 
		"position" : 2,
		"serial_number" : "yyy",
		"model_name" : "genie",
		"vendor" : "teledyne", "status" : "ok" 
	},
	etc...
],
"light" : { 
	status : "fail", 
	"reason" : "overheat on module #3"  
},
"current_job": {
			"data": { "single": "/products/prod32.json" },
			"finished": "2024-05-07 17:28:01",
			"job_id": 3,
			"job_type": "nozzle_out",
			"started": "2024-05-07 12:54:17",
			"status": "ok"
			}
 }
```
### Start job:
Sets a job of the given type as the current one.
```json
{ "command" : "start_job", "job_type" : "uniformity"} 
``` 

Normally, an accepted job returns a job_id and a deferred status
```json
{ "status" : "processing", "job_id" : 1234}
```

### Job Status:
Returns the status of a running job, as well as all associated products in the case the job is completed
```json
{ "command": "job_status", "job_id" : 1234 }               
```

If the job is still ongoing, the answer reflects such state. Please note job_id is echoed as an ack.
```json
{ "status" : "processing", "job_id" : 1234 }
```
  
It is possible to omit job_id. In this case the status of the active job and its ID are returned.

If the job is complete, the status JSON may contain a reference to the obtained products. In this case a single table. All products can be retrieved a by issuing a GET on the http(s) server on the given URL. 
:label: see Product definition
```json
{ "status" : "ok", "job_id" : 1234,  "data" : { "table" : "/products/product4567.txt" } } 
```

In this case multiple tables
```json
{ "status" : "ok", "job_id" : 1234,  "data" : [ "C" : "/products/product4568.txt", "M" : "/products/product4568.txt" ] } 
```

Products can be images too:
```json
{ "status" : "ok", "job_id" : 1234,  "data" : { "single" : "/images/img1234.png" } }
```

In the case the job failed, status returns the usual error report
```json
{ "status" : "failed", "reason" : "too much skew"}   
```

## Software workflow 
This workflow how an external program can drive the inspection system.

<u style="font-weight:600">Step 1</u>: Send a "`start_job`" JSON to put the inspection system in on-guard state.

**Request**: 
```json
{ "command" : "start_job", "job_type" : "uniformity"}  
```
 
<u style="font-weight:600">Step 2</u>: Inspection shall create a job and return its job-id

**Response**
```json
{
	"job_id": 56, 
	"job_type": "uniformity",
	"started": "2024-06-11 09:40:18",
	"status": "processing"
}
```

<u style="font-weight:600">Step 3</u>: Poll inspection by sending "job_status" commands until the status reaches the "`ok`" state.

**Request**:
```json
{ "command" : "job_status", "job_id" : 56 }
```


<u style="font-weight:600">Step 4</u>: When status reaches "ok", use GET to download any products and forward them to Fiery RIP.

**Response** 
```json
{
	"data": {
		  "M": "/products/prod1.json",
		  "single": "/products/prod6.jpg"	
	},
	"finished": "2024-06-11 09:41:26",
	"job_id": 56,
	"job_type": "uniformity",
	"started": "2024-06-11 09:40:18",
	"status": "ok"
}
```
 
 In this case, product is the uniformity table for channel magenta as a JSON. 
 There is an extra product labeled "single" which is a debug image intended for web. 
 Press software should ignore any products other that what is documented for Fiery communication.

### Error handling
On any error, the job_status command returns "failed" as state and an attribute "reason" is returning with a string describing the error.
Sending a job when any other is still active cancels the previous job. There is no timeout so a job may rest forever waiting completion.

### Clean queue of jobs:
Erases all pending jobs. Intended to provide a hard-reset with cleanup. **Use with care!**
```json
{ "command" : "clean_jobs"}          
```                 

## Lights
Light is fully controlled by the framework and external apps should not change its behavior. Some command are provided, however, for debug reasons.

### Turn light ON/OFF
Override the light mechanism to turn it on or to force it off no matter its elapsed time. Useful for camera calibration and factory related operation.
```json
{ "command" : "turn_light", "data" : "on" }             
{ "command" : "turn_light", "data" : "off" }             
```

Please note turning light ON for extended time lapses can be harmful for light modules.

### Turn light tower :
Override the light mechanism to turn it on or to force it off no matter its state. Useful for debug.
```json
{ "command" : "turn_light_tower", "light_num" : 1, "data" : "on" }        
```

## Settings

### Get all configuration:

Use this command to retrieve info on all settings. Settings may hold following types:

- boolean: **"boolean**
- integer: **"integer"**  
- real number: **"real"** 
- string:  **"string"**
- list of bools:  **"boolean_list"**
- list of integers:  **"int_list"**
- list of real numbers:  **"real_list"**
- list of strings: **"string_list"**   


Each setting belongs to a group indentified by an integer. The command can optionally filter settings that belongs to a given group. 

<u style="font-weight:600">Example 1</u>:

```json
{ "command" : "get_all_config"}
```               

<u style="font-weight:600">Example 2</u>:

```json
{ "command" : "get_all_config", "group" : 1 } 
```                        

The configuration is returned using following syntax (no group filtering is done in this sample).

```json
{
 "data": [
  {
   "data": "c:\\ProgramData\\EFI\\inspection\\database.db",
   "group": 0,
   "help": "Full path of database file in the server",
   "name": "database",
   "prompt": "Database path",
   "type": "string"
  },
  {
   "data": 90,
   "group": 0,
   "help": "Intensity of the LED light in percent",
   "name": "light_intensity",
   "prompt": "Light intensity",
   "type": "integer"
  },
  {       
   "group": 0,
   "help": "How many seconds the light remains before getting dimmed",
   "name": "light_persistence_seg",
   "prompt": "Light persistence (secs)",
   "type": "integer_list"
   "data": [
	500,
	550,
	500,
	560,
	600
   ]
   }
 ],
 "status": "ok"
}
```

### Get setting:
Use this command to retrieve a given setting identified by name
```json
{ "command":"get_config", "name" : "stitching_offset" }  
```     

### Set setting:
Use this command to set a specific setting identified by name. Type must match. 
```json
{ "command":"set_config", "name" : "stitching_offset", "data" : 23456.7 }   
```

### Black capture
This command measures the black level of each pixel for FFC.
```json
{ "command" : "capture_cameras_black", "covered_cameras" : [1, 2, 3] }   
```

Data holds a list on which cameras to capture. If missing, the setting "covered_cameras" is used. 

### White capture

This command measures the white level of each pixel for FFC. A white paper must be placed below camera.
Always uses all cameras
```json
{ "command" : "capture_cameras_white" }  
```

### Stitching
#### **"compute_stitching"** 
After a stitching job has been processed, the job products are an image per camera plus the stitched image. 
Camera images comes tagged with page types "cam1" ... "cam5", stitched image comes tagged as "single"

It is possible to change the position of stitching points and recomputing the stitched image. This does not require to send another job but it is neccesary the last job used were a **"camera_calibrate"**.
```json
{
	"command" : "compute_stitching", 
	"cam1" : [[1,2], [3,4], [5,6], [7,8] ...], 
	"cam2" : [[1,2], [3,4], [5,6], [7,8] ...], 
	"cam3" : [[1,2], [3,4], [5,6], [7,8] ...], 
	"cam4" : [[1,2], [3,4], [5,6], [7,8] ...], 
	"cam5" : [[1,2], [3,4], [5,6], [7,8] ...] 
}
```

Numbers are (x, y) of 4 stitching points, per camera. 

When successful, it returns the status of the **"camera_calibrate"** job with all attached products.
#### **"get_stitching_points"**
Returns the current stitching points as stored in the configuration. 
Format is same as compute_stitching. "data" is optional

**Request**:
```json
{ "command" : "get_stitching_points", "data" : "cam2" }  
```

**Return**:
```json
{ 
	"status" : "ok",
	"disabled": [false, false, false, false, false],
	"data": [
		"cam1" : [[1,2], [3,4], [5,6], [7,8] ], 
		"cam2" : [[1,2], [3,4], [5,6], [7,8] ], 
		"cam3" : [[1,2], [3,4], [5,6], [7,8] ], 
		"cam4" : [[1,2], [3,4], [5,6], [7,8] ], 
		"cam5" : [[1,2], [3,4], [5,6], [7,8] ]
	]
}
```

**Example 1** (request):
```json
{ "command" : "get_stitching_points", "data" : "cam2" }  
```

**Example 1** (response):
```json
{ 
	"status" : "ok",
	"disabled": [false,false,false,false,false],
	"data": [
		"cam2" : [ [1,2], [3,4], [5,6], [7,8] ]
	]
}
```

**Example 2** (request)
```json
{ "command" : "get_stitching_points", "data" : "none" }
```

**Example 2** (response)
```json
{
	"data": null,
	"disabled": [false,false,false,false,false],
	"status": "ok"
}
```

#### **"detect_stitching_points_"**
To force point detection, use this command. No stitching is calculated, only stitching points are returned. 
job_id is optional and if used, refers to camera calibration job to retrieve camera image. By default is last job.

```json
{
	"command" : "detect_stitching_points",                 
	"job_id" : 14,
	"data" : "cam2"
}
```

On camera 0 an additional property **left_margin** will be returned. On cam 4 (last) **right_margin** will be returned as well.
#### Shutting down windows on inspection blade
Use following command to shutdown windows (needs permissions)
```json
{"command" : "shutdown"}
```

## Camera calibration workflow (Used by webpages)

### **FCC calibration:**

- Select which cameras are covered, set **"covered_cameras"** setting or read it into a list.
- Send a **"capture_cameras_black"** JSON command. 
- Uncover all cameras
- Send a **"capture_cameras_white"** JSON command. 


### **Calibration** 
This step measures white and stitching information for each camera.

- Start a **"camera_calibrate"** job from press software.  
- When completed, you got one image per camera as product
- you can fine tune points by sending **"compute_stitching"**

### **Save**
-  This step forces to recalculate everything when user has modified parameters. 


WEBPAGE 3rdParty

https://xxjapp.github.io/xdialog/

