# Keras + Theano on Metacentrum example setup

##Installation
First setup the python environment.
```
module add python27-modules-intel
# Set pip path for --user option
export PYTHONUSERBASE=/storage/plzen1/home/<user_name>/.local
# set PATH and PYTHONPATH variables
export PATH=$PYTHONUSERBASE/bin:$PATH
export PYTHONPATH=$PYTHONUSERBASE/lib/python2.7/site-packages:$PYTHONPATH
```

Install dependencies of Keras:
```
module add hdf5-1.8.14-intel

pip install numpy --user --upgrade
pip install scipy --user --upgrade
pip install pyyaml --user --upgrade
pip install h5py --user --upgrade
```

Next install Theano:
```
pip install git+git://github.com/Theano/Theano.git --user --upgrade
```

Last step is Keras installation:
```
pip install keras --user --upgrade
```

##Theano configuration
In your $HOME directory create file .theanorc Put following settings in:
```
[global]
floatX=float32
device=gpu
compiledir_format=compiledir_%(platform)s-%(python_version)s-%(python_bitwidth)s
```
Those setting are the minimal requierements. Rest of the available options is on Theano page: http://deeplearning.net/software/theano/library/config.html

##Testing the setup
First add cuda module
```
module add cuda-6.5
```

Then start python and import theano and keras
```
user-computer:~$ python
>>> import theano
>>> from keras.models import Sequential
```

If it goes without problem it should print message:
```Using gpu device 0: <gpu name> (CNMeM is disabled)```

When everything is setup you can test MNIST CNN from Keras. Download https://github.com/fchollet/keras/blob/master/examples/mnist_cnn.py
and run it with
```
user-computer:~$python mnist_cnn.py
```

It seems that only supported clusters are **doom** and **gram**. 
* On **gram** single epoch of mnist_cnn takes about 66s. 
* On **doom** single epoch of mnist_cnn takes about 35s

## FAQ:
Q:	When importing theano it shows error - Cuda available but not used. Driver version not supported.

A:	Check you have the latest Cuda module - atm 6.5
	Check that you are on supported node. Only doom and gram clusters work with this setup.


Q:	When importing theano it shows error - libcuda_ndarray.so is not available. File not found.

A:	Check that your compiledir_format and compiledir parameters are setup correctly. The reported compiledir has to have read & write permissions. The compiledir must not have special chars in it. If you keep default compiledir_format it can has colon (:) in the path which messes up the compiler.


Q:  When importing keras python outputs lots of warnings about Runtime Api Version being different from Compile Api Version.

A:	Make sure you are using Python 2.7. And that you are **not** using theano module available at Metacentrum. Their module is configured to use Python 2.6.


Q:	PBS killed my job.

A:	Create new job with more resources. The default 400mb of memory is usually too low.
