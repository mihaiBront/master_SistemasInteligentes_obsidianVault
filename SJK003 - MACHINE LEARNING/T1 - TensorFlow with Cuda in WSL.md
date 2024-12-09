## Step 1: Update and Upgrade Packages

Once you have your Linux distribution set up, it is essential to update your package list and upgrade all packages to ensure you have the latest software.

```bash
sudo apt update
sudo apt upgrade -y
```

## Step 2: Install NVIDIA Drivers

To enable GPU support, ensure you have the latest NVIDIA drivers installed on your Windows system. Download and install the drivers from the [NVIDIA website](https://www.nvidia.com/Download/index.aspx "NVIDIA website") .

**[Check this guide by ubuntu, for example](https://ubuntu.com/server/docs/nvidia-drivers-installation)**

## Step 3: Install CUDA Toolkit

CUDA is a parallel computing platform and application programming interface (API) model created by NVIDIA. Installing the CUDA Toolkit enables GPU acceleration for TensorFlow.

[NVIDIA CUDA Toolkit](https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&Distribution=Ubuntu&target_version=24.04&target_type=deb_local)
### 3.1. Add the repository pin
```bash
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2404/x86_64/cuda-ubuntu2404.pinsudo

sudo mv cuda-ubuntu2404.pin /etc/apt/preferences.d/cuda-repository-pin-600
```
### 3.2. Add the repository
```bash
wget https://developer.download.nvidia.com/compute/cuda/12.6.3/local_installers/cuda-repo-ubuntu2404-12-6-local_12.6.3-560.35.05-1_amd64.deb

sudo dpkg -i cuda-repo-ubuntu2404-12-6-local_12.6.3-560.35.05-1_amd64.deb

sudo cp /var/cuda-repo-ubuntu2404-12-6-local/cuda-*-keyring.gpg /usr/share/keyrings/

```
### 3.3. Install Cuda toolkit

``` bash
sudo apt-get update
sudo apt-get -y install cuda-toolkit-12-6
```
### 3.4. Drivers
```bash
sudo apt-get install -y nvidia-open
sudo apt-get install -y cuda-drivers
```

### 3.5 Check installation

```bash
nvidia-smi
```

You should see this:
![[Pasted image 20241206102652.png]]

## Step 4: Install cuDNN

cuDNN is a GPU-accelerated library for deep neural networks. It is required for TensorFlow to leverage the GPU for model training and inference.

Visit the [NVIDIA cuDNN page](https://developer.nvidia.com/cudnn "NVIDIA cuDNN page") and download the cuDNN tar file for your CUDA version.

### 4.1 Add repo to `apt`
```bash
wget https://developer.download.nvidia.com/compute/cudnn/9.6.0/local_installers/cudnn-local-repo-ubuntu2404-9.6.0_1.0-1_amd64.deb
sudo dpkg -i cudnn-local-repo-ubuntu2404-9.6.0_1.0-1_amd64.deb
sudo cp /var/cudnn-local-repo-ubuntu2404-9.6.0/cudnn-*-keyring.gpg /usr/share/keyrings/
```

### 4.2 Install cuDNN

```bash
sudo apt-get update
sudo apt-get -y install cudnn
```


## Step 5: Check Tensorflow Finds the GPU

```python
print("TensorFlow Version:", tf.__version__)

# List available devices
print("Available devices:")
for device in tf.config.list_physical_devices():
    print(device)

# Check for GPU
gpus = tf.config.list_physical_devices('GPU')
if gpus:
    print("GPU is available: ", gpus)
else:
    print("No GPU found")

from tensorflow.python.client import device_lib
print(device_lib.list_local_devices())
```

