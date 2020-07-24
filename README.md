# Steps to run a Jmodelica docker that supports ipython interaction on a Linux Server

## This assumes that
* Docker is installed
* Docker sudo group is created
* User is added to docker sudo group
* Jmodelica source code is available as a zip file
* Xming is installed on local computer if performing this set up on remote server
* DISPLAY environmental variable is set (for eg "localhost:10.0")

Do not clone this folder! This repo merely exists to have all the isntructions and is by no means complete.

## Steps:

### Obtain docker file
```bash
mkdir JModelica_docker
cd JModelica_docker
wget https://github.com/AvisekNaug/JModelica_docker/raw/master/Dockerfile
```
### Copy Jmodelica Source code in zip format to this directory and rename it
```bash
mv <name of your Jmodelica installation file> jmodelica.zip
```

### Build the docker
```bash
docker build --tag jmodelica:1.0 .
```

### Ensure X11 forwarding works correctly(Do this only if working on a remote server)
```bash
source x11config.sh
```

### Start the docker
If using remote server (Make sure Xming is installed on local computer and listeing on 10.0)
```bash
docker run -it -e DISPLAY=${DISPLAY} -v $XAUTH:$XAUTH -e XAUTHORITY=$XAUTH jmodelica:1.0
```
OR

If setting up container on local computer
```bash
docker run -it -e DISPLAY=${DISPLAY} jmodelica:1.0
```

### Inside the docker, try to run toy examples in ipython shell
```bash
dveloper@container_id# ipython
```

```ipython
from pyfmi.examples import fmi_bouncing_ball
fmi_bouncing_ball.run_demo()
```
This should display the images of position and velocity in a matplotlib plot.

Similarly try,
```ipython
from pyjmi.examples import cstr_casadi
cstr_casadi.run_demo()
```

```ipython
from pyjmi.examples import RLC
RLC.run_demo()
```


## To run python 3 after starting the docker do the following~~(TODO: create separate docker isntructions later)~~ done!
~~```bash
mkdir Downloads
cd Downloads/
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh
export PATH=/home/developer/miniconda3/bin:$PATH # to make conda work
conda config --set auto_activate_base false # exit and log back in for effect to take place
conda create -n myenv
conda activate myenv
conda init bash # exit and log back in for effect to take place
conda install -c conda-forge pyfmi
conda install matplotlib ipython
export PYTHONPATH=/home/developer/miniconda3/envs/myenv/lib/python3.8/site-packages:$PYTHONPATH
# Then run the examples in ipython
```

~~~```ipython
import matplotlib
matplotlib.use('tkagg')
import matplotlib.pyplot as plt # see link https://github.com/matplotlib/matplotlib/issues/8929#issuecomment-317233404
```
