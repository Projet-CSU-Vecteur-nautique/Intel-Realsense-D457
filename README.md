# Intel Realsense D457

*Ce répertoire sert de documentation pour la caméra Intel Realsense D457. Il relatera des différents tests effectués.*

## Mise en place

[Démarrage rapide](https://www.intelrealsense.com/get-started-depth-camera/).

## Installation de l'OS

Suivre le [tutoriel de Nvidia](https://developer.nvidia.com/embedded/learn/get-started-jetson-nano-devkit#intro)

## Installation de ROS

### Installation classique

Suivre le [tutoriel officiel de ROS](https://wiki.ros.org/noetic/Installation/Ubuntu)

### Installation par Docker

La Jetson Nano tourne sur une architecture arm64 donc il faut télécharger une image adaptée.

#### Création du `Dockerfile`

```Dockerfile
FROM ros:noetic

RUN apt-get update
RUN apt-get install vim iputils-ping iproute2 -y
RUN apt-get install ros-noetic-rqt ros-noetic-rqt-common-plugins ros-noetic-ros-tutorials ros-noetic-rviz -y
RUN echo ". /opt/ros/$ROS_DISTRO/setup.bash" >> ~/.bashrc
```

#### Construction de l'image `ros`

```sh
# Dans le répertoire du fichier Dockerfile 
docker build -t ros .
```

#### Création du conteneur `ros_noetic`

```sh
sudo docker run -it \
    --privileged \
    --network host \
    --name=ros_noetic \
    --user=$(id -u $USER):$(id -g $USER) \
    --env="DISPLAY" \
    --env="QT_X11_NO_MITSHM=1" \
    --env="LIBGL_ALWAYS_SOFTWARE=1" \
    --volume="/etc/group:/etc/group:ro" \
    --volume="/etc/passwd:/etc/passwd:ro" \
    --volume="/etc/shadow:/etc/shadow:ro" \
    --volume="/etc/sudoers.d:/etc/sudoers.d:ro" \
    --volume="/tmp/.X11-unix:/tmp/.X11-unix:rw" \
    --volume="$HOME/.Xauthority:/root/.Xauthority:rw" \
    -u root \
    ros \
    bash
```

## Liens

- [RPlidar A2M8](https://www.slamtec.ai/home/rplidar_a2/)
- [ros](http://wiki.ros.org/rplidar)
- [rplidar_ros](https://github.com/slamtec/rplidar_ros)
