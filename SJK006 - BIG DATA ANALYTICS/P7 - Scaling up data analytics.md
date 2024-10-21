# 7.1 Recap of architectures

**LAMBDA ARCHITECTURE**
![[Pasted image 20241014182926.png]]
*Interested in batch processing*

**KAPPA ARCHITECTURE**

![[Pasted image 20241014183035.png]]
**DATA VALUE CHAIN**
![[Pasted image 20241014183104.png]]

# 7.2 Optimizing Data Storage and Transfer

The first requisite is being able to fit the whole dataframe in memory (RAM). If we don't succeed in that, we are fucked, need more ram.

Another way is partitioning data or working on disc, but those are much slower.

![[Pasted image 20241014183140.png]]

## Row vs Columnar layouts
Data is stored as a sequence of bytes, not as a bi-dimensional structure. 
Data of interest must be stored closed to each other for efficient retrieval (blocks/pages): 
- In a row-oriented layout, records are arranged sequentially 
- In a columnar layout, columns are arranged sequentially (e.g. Pandas ) 
- In a hybrid layout, rows are split in blocks and each block is arranged with columnar layout

## Pillars of optimization:
![[Pasted image 20241014184133.png]]

**PARQUET**

Organization:
- One datasset (usually) = Multiple files
- Row groups (128MB)
- With column chunks
- Pages (Default to 1MB)
	- Metadata(min/max/count)
	- Rep/Def levels
	- Encoded values
- Encoding:
	- Plain: bytes[data]
	- RLE: Run length encodings + bit packaging + dictionary compression
![[Pasted image 20241014184329.png]]
![[Pasted image 20241014184402.png]]

**ALTERNATIVES and PLUGINS to PANDAS**

![[Pasted image 20241014184510.png]]

- *POLA-RS*
	- Lazy frames, loads a structure, sort of template to memory and instead of performing computations on the *DF*, it constructs a workflow that is not executed until `lf.collect()` is called.

- *VAEX.io*
	- Also implements lazy frames, but in a smaller measure -> Virtual Columns (not stored, but stored the formulas if they are derved of other columns)
	- Oriented to visual representation of temporal-spatial data
	
- *DUCK-DB*: Encapsulates pandas inside an SQL interface.