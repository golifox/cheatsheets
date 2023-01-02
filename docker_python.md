# **Docker Python cheat sheet**

```py
# install python
docker pull python


# run python in interactive mode with no persistent storage

docker run -it python

# run python in interactive mode and mount an absolute path
# to a folder named shared in the python container for persistent storage
docker run -it -v /Users/codecaine/Desktop:/shared  python  

# run python in interactive mode and mount the terminal current working directory
# to a folder named shared in the python container for persistent storage
docker run -it -v ${PWD}:/shared python

# create a volume named shared
volume to create my_volume

# get a list of volumes on system
docker volume ls

# to attach a volume for persistent storage in python
docker run -it -v my_volume:/shared  python

# in python check the folder
import os

# change system directory to shared folder
os.chdir("shared")

# list files in the current directory
os.listdir(".")

# stop all active services - containers and mounted volumes
docker system prune

# remove a volume is storage is not needed anymore
docker volume rm my_volume
```

# Docker with pip packages step my step example

Create a file named ```Dockerfile``` and add the contents below then save the file

```py
# Dockerfile, Image, Container
# Container with python version 3.10
FROM python:3.10

# Add python file and directory
ADD main.py .

# upgrade pip and install pip packages
RUN pip install --no-cache-dir --upgrade pip && \
   pip install --no-cache-dir numpy
   # Note: we had to merge the two "pip install" package lists here, otherwise
   # the last "pip install" command in the OP may break dependency resolution...

# run python program
CMD ["python", "main.py"]
```

Create a ```main.py``` in same folder as Dockerfile and add contents below then save file

```py
from numpy import random
import numpy as np


# original array
arr = np.array([1, 2, 3, 4, 5])
print('original array', arr)

# shuffled array inplace
random.shuffle(arr)
print('shuffled array', arr)

# output example
# original array [1 2 3 4 5]
# shuffled array [5 1 2 4 3]
```

Build the container in the current directory - make sure the . is after python_test for the current directory

```py
# build the container in the directory of main.py and Dockerfile
# and create a container named python_test
docker build -t python_test .
```
Run the new container

```py
docker run python_test
```
View container images and delete
```py
# list of focker images on system
docker images

# remove an image by name
docker rmi python_test
```
