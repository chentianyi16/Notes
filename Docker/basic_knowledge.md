This file is used to record some ideas and insights while making base images. 
# Basic knowledge of docker images
## Layers
Docker images can be simply treated as Docker building blocks and consist of layers and each layer is an image it self without a human-assigned tag. So basically, a docker image looks like the following graph:
![docker image](../Images/J2cge.jpg)
Following are some basic concepts of the layers:
- Each layer is an image itself, just one without a human-assigned tag. They have auto-generated IDs though.
- Each layer stores the changes compared to the image it’s based on.
- An image can consist of a single layer (that’s often the case when the squash command was used).
- Each instruction in a Dockerfile results in a layer. (Except for multi-stage builds, where usually only the layers in the final image are pushed, or when an image is squashed to a single layer).
- Layers are used to avoid transferring redundant information and skip build steps which have not changed (according to the Docker cache).
## Some insight about docker image
[This blog](https://vsupalov.com/whats-a-docker-image/) explained in detail about what exactly a docker image is. In short, a docker image is basically a diff which contains information on what changed on the image it's based on. And each image has a parent (well, apart from those based on scratch) it refers to.
## Dockerfile
A dockerfile consists of a series of instructions, and each RUN or ENV or COPY will result in a new image layer/intermediate image/image being created. As introduced before, each layer is an image itself, therefore, a container can be runned on any single layer with its ID. \
Also, the layers can be squashed. This operation can reduce a chain of images to a single image. It takes all changes and sums them up into a single image.
## Some Tips to reduce the size of docker images and speed up docker image build
In the above sections, some concepts were introduced. According to those concepts, there are some little tricks to reduce build time and image size.
- Avoid docker commit. By using 
```shell
docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]
```
a container can be packaged into another docker image. Using this operation will make the docker image seriously inflated (an 8g docker became 102g after several commits). Therefore, avoid rebuilds as much as possible will keep iterations short and avoid busy waiting.
- 