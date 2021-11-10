# working-with-sleap

start by connecting to your linode
`ssh root@(your.linode.IP)`

first time you do this it will ask you if you trust this connection
output will look like this:
`The authenticity of host .... can't be established.`
`ECDSA key fingerprint is SHA256:i1qhmrkAmM/ypwvicHQFGPtVk6BxvMhgbm4Pf3yzBE0.
Are you sure you want to continue connecting (yes/no)?`

type `yes` and continue to enter your password (you won't see yourself typing..!):
`root@(your.linode.IP)'s password:`

he will spit out some stuff we don't care about and your in.  as can be indicated by where your typing line looking something like this:
`root@localhost:~#`
 
now we need to install a whole bunch of stuff...
we start with downloading and then installing anaconda:
`wget https://repo.anaconda.com/archive/Anaconda3-2021.05-Linux-x86_64.sh`
`bash Anaconda3-2021.05-Linux-x86_64.sh`

this will look something like this:

    ===================================
    End User License Agreement - Anaconda Individual Edition
    ===================================
        
    Copyright 2015-2021, Anaconda, Inc.
    ...
    ...
    ...
    Do you accept the license terms? [yes|no]
    
type `yes`  and then confirm again with enter and it will install for a while...

    Anaconda3 will now be installed into this location:
    /root/anaconda3
    
      - Press ENTER to confirm the location
      - Press CTRL-C to abort the installation
      - Or specify a different location below
    ...
    ...
    ...
it will ask if you want to init conda. type `yes` again...
this will time out - so if you don't reply in time you will need to manually do it but only after adding conda to the PATH:
`export PATH="/root/anaconda3/bin:$PATH"`
`conda init`

now we want to create our environment:
`conda create --name my_env python=3.7`

when this is done you can reboot the machine - sorry... be patient...
`reboot`

after you log back in you want to activate your environment:
`conda activate my_env`

in our env we can now install the nvidia drivers 
`apt-get -y install --no-install-recommends nvidia-driver-440`
and specify tensorflow should use the `cudatoolkit=10.1`
`conda install tensorflow-gpu cudatoolkit=10.1`

before you install sleap you can run python and see if the gpu is recognized:

	python
    >>> import tensorflow as tf
    2021-11-09 10:09:14.302439: I tensorflow/stream_executor/platform/default/dso_loader.cc:49] Successfully opened dynamic library libcudart.so.10.1
    
if this worked you can also try this:

    >>> tf.config.list_physical_devices('GPU')
response should look like this:

    2021-11-09 10:09:25.501141: I tensorflow/compiler/jit/xla_cpu_device.cc:41] Not creating XLA devices, tf_xla_enable_xla_devices not set
	2021-11-09 10:09:25.502515: I tensorflow/stream_executor/platform/default/dso_loader.cc:49] Successfully opened dynamic library libcuda.so.1
	2021-11-09 10:09:26.082063: I tensorflow/stream_executor/cuda/cuda_gpu_executor.cc:941] successful NUMA node read from SysFS had negative value (-1), but there must be at least one NUMA node, so returning NUMA node zero
	2021-11-09 10:09:26.083519: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1720] Found device 0 with properties:
	pciBusID: 0000:00:02.0 name: Quadro RTX 6000 computeCapability: 7.5
	coreClock: 1.77GHz coreCount: 72 deviceMemorySize: 23.65GiB deviceMemoryBandwidth: 625.94GiB/s
	2021-11-09 10:09:26.083685: I tensorflow/stream_executor/platform/default/dso_loader.cc:49] Successfully opened dynamic library libcudart.so.10.1
	2021-11-09 10:09:26.086476: I tensorflow/stream_executor/platform/default/dso_loader.cc:49] Successfully opened dynamic library libcublas.so.10
	2021-11-09 10:09:26.086581: I tensorflow/stream_executor/platform/default/dso_loader.cc:49] Successfully opened dynamic library libcublasLt.so.10
	2021-11-09 10:09:26.088830: I tensorflow/stream_executor/platform/default/dso_loader.cc:49] Successfully opened dynamic library libcufft.so.10
	2021-11-09 10:09:26.089294: I tensorflow/stream_executor/platform/default/dso_loader.cc:49] Successfully opened dynamic library libcurand.so.10
	2021-11-09 10:09:26.091926: I tensorflow/stream_executor/platform/default/dso_loader.cc:49] Successfully opened dynamic library libcusolver.so.10
	2021-11-09 10:09:26.093317: I tensorflow/stream_executor/platform/default/dso_loader.cc:49] Successfully opened dynamic library libcusparse.so.10
	2021-11-09 10:09:26.098889: I tensorflow/stream_executor/platform/default/dso_loader.cc:49] Successfully opened dynamic library libcudnn.so.7
	2021-11-09 10:09:26.099050: I tensorflow/stream_executor/cuda/cuda_gpu_executor.cc:941] successful NUMA node read from SysFS had negative value (-1), but there must be at least one NUMA node, so returning NUMA node zero
	2021-11-09 10:09:26.100344: I tensorflow/stream_executor/cuda/cuda_gpu_executor.cc:941] successful NUMA node read from SysFS had negative value (-1), but there must be at least one NUMA node, so returning NUMA node zero
	2021-11-09 10:09:26.101672: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1862] Adding visible gpu devices: 0
	[PhysicalDevice(name='/physical_device:GPU:0', device_type='GPU')]
 
if this worked you should be able to `pip install sleap` and use it as usual 
:D

