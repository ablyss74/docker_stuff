Basically this is a short one liner to build and execute ubuntu:latest docker in clear linux.
Also added one for debian:latest

 - Installs the following apps by default: qjackctl kmod rakarrack jamin ardour lmms obs-studio locate 

- It assumes the following...
 1. Docker bundle is already installed
 2. You will run this with sudo or root
 3. Realtime mode is already set to -1 /// if not see the https://github.com/ablyss74/docker_stuff/blob/main/qjackctl%20docker%20container.md page for details how.
 4. You're aware and okay having your $HOME director mirrowed to the docker $HOME directory.
 5. You're familiar with terminal shell use and know how to execute a program with the ampersand.

- Useful for running hard to find apps and/or for those that don't work right on your existing system. /// However, if your /system is already broke it probably won't help :-)
```bash
# This container is mainly for jamin and other apps
echo -e 'FROM ubuntu:latest \nRUN apt update \nRUN apt upgrade -y\nRUN apt install qjackctl -y\nRUN apt install kmod -y\nRUN apt install rakarrack -y\nRUN apt install jamin -y\nRUN apt install ardour -y\nRUN apt install lmms -y\nRUN apt install obs-studio -y\nRUN apt install locate -y' > /tmp/Dockerfile && docker build -t ubuntu:latest < /tmp/Dockerfile - && modprobe snd-seq && xhost local:${USER} && docker run -it --privileged -v ${HOME}:/root -e JACK_NO_AUDIO_RESERVATION=1  --device /dev/snd -e PULSE_SERVER=unix:${XDG_RUNTIME_DIR}/pulse/native -v ${XDG_RUNTIME_DIR}/pulse/native:${XDG_RUNTIME_DIR}/pulse/native -v /dev/shm:/dev/shm:rw --net=host -e DISPLAY=${DISPLAY} ubuntu:latest
```
```bash
# This container is for projectm-jack & kmod for loading video drivers 
echo -e 'FROM debian:latest \nRUN apt update \nRUN apt upgrade -y\nRUN apt install projectm-jack -y\nRUN apt install kmod -y\nRUN apt install projectm-data -y' > /tmp/Dockerfile && docker build -t debian:latest < /tmp/Dockerfile - && xhost local:${USER} && docker run -it --privileged -v ${HOME}:/root -e JACK_NO_AUDIO_RESERVATION=1  --device /dev/snd -e PULSE_SERVER=unix:${XDG_RUNTIME_DIR}/pulse/native -v ${XDG_RUNTIME_DIR}/pulse/native:${XDG_RUNTIME_DIR}/pulse/native -v /dev/shm:/dev/shm:rw --net=host -e DISPLAY=${DISPLAY} debian:latest
```
