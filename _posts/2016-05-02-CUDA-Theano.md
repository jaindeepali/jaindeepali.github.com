---
layout: post-no-feature
title: "Running Theano on GPU with CUDA Toolkit"
comments: false
category: articles
tags: [Machine Learning, Theano, GPU]
description: ""
---

Setting up CUDA toolkit and Nvidia drivers on my **HP Pavilion 15 Notebook** kept messing up with my display manager. After numerous X-Server breakdowns, here is how I got Theano to run on GPU safely. I setup **CUDA-7.5** on **Ubuntu 14.04**.

* ### Check system compatibility

	* First of all, verify that your GPU is CUDA-compatible. Run 
		
		```
		lspci | grep NVIDIA
		```

		and note the GPU model. Make sure your GPU is listed [here](https://developer.nvidia.com/cuda-gpus).

	* Check for existing Nvidia Drivers by running     	
		
		```
		nvidia-smi
		``` 
		
		If you already have Nvidia Drivers, then this should show you the Driver Version. Note it. 

* ### Install dependencies

	```
	sudo apt-get install build-essential gcc g++
	```

* ### Install Nvidia drivers and CUDA toolkit

	* Download CUDA **.run** package from [Nvidia website](https://developer.nvidia.com/cuda-downloads). This package contains 3 components:
		* Nvidia GPU Driver
		* CUDA Toolkit
		* CUDA Samples

		It is a good idea to extract these 3 components as individual installers via

		```
		sudo sh cuda_*.run -extract=~/cuda_installers
		```

	* Check the Nvidia driver version from its filename. For eg. in my case it was version **352** ( Filename: `NVIDIA-Linux-x86_64-352.39.run` ). You can skip installation of the driver if you already have this (or higher) version installed.

	* #### Nvidia driver installation
		* Remove any programs or configuration starting with `nvidia-*`.

			```
			sudo apt-get --purge remove nvidia-*
			```

		* Hit `Ctrl`+`Alt`+`F1` to login to physical terminal.

		* Stop X-Server:
			
			```
			sudo lightdm stop
			```

		* Install Nvidia Driver

			```
			cd cuda_installers
			```

			```
			sudo sh NVIDIA-Linux-x86_64-352.39.run
			``` 

			Accept the EULA and follow prompts. If the installers complaints for missing dependencies, you may need to install some extra packages.

			```
			sudo apt-get install dkms fakeroot linux-headers-generic
			``` 

	* #### CUDA toolkit installation

		```
		sudo sh cuda-linux64-rel-7.5.18-19867135.run
		```

		Install the samples for testing:

		```
		sudo sh cuda-samples-linux-7.5.18-19867135.run 
		```

* ### Test CUDA installation by running a sample

	```
	cd ~/NVIDIA_CUDA-7.5_Samples/1_Utilities/deviceQuery
	```

	```
	./deviceQuery
	```

	If all went fine, you should see result **PASS**. Now, start X-Server and log out of physical terminal

	```
	sudo lightdm start
	```

* ### Set up the required environment variables

	Append following lines in *~/.bashrc* or *~/.zshrc*

	```
	PATH=/usr/local/CUDA-7.5/bin:$PATH
	```

	```
	LD_LIBRARY_PATH=/usr/local/CUDA-7.5/lib64:$LD_LIBRARY_PATH
	```

* ### Install Python SciPy Stack

	```
	sudo apt-get install python-numpy python-scipy python-matplotlib ipython ipython-notebook python-pandas python-sympy python-nose
	```

* ### Install the released version of Theano from PyPI

	```
	sudo pip install Theano
	```

* ### Configure Theano to run on GPU

	Save a file *~/.theanorc* with following text.

{% highlight shell linenos=table %}
[global]
floatX = float32
device = gpu

[nvcc]
fastmath = True

[cuda]
root=/usr/local/cuda-7.5/
{% endhighlight %}

* ### Test to see if GPU is being used

	Copy following program in a file and run it.

{% highlight python linenos=table %}
from theano import function, config, shared, sandbox
import theano.tensor as T
import numpy
import time

vlen = 10 * 30 * 768  # 10 x #cores x # threads per core
iters = 1000

rng = numpy.random.RandomState(22)
x = shared(numpy.asarray(rng.rand(vlen), config.floatX))
f = function([], T.exp(x))
print(f.maker.fgraph.toposort())
t0 = time.time()
for i in range(iters):
r = f()
t1 = time.time()
print("Looping %d times took %f seconds" % (iters, t1 - t0))
print("Result is %s" % (r,))
if numpy.any([isinstance(x.op, T.Elemwise) 
for x in f.maker.fgraph.toposort()]):
print('Used the cpu')
else:
print('Used the gpu')
{% endhighlight %}

Hopefully this will show **'Used the gpu'**. 

Now you are all set to play with your GPU. Enjoy!