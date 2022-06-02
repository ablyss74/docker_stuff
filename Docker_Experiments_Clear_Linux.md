

- Some things are expected...
 1. Docker bundle is already installed
 2. You will run this with sudo or root
    If using sudo just add it to the first line of this example to invoke it.
 3. You're okay having Realtime mode set to -1 when using qjackctl  
    See the [qjackctl docker container](https://github.com/ablyss74/docker_stuff/blob/main/qjackctl%20docker%20container.md) for details on a how-to.
 4. You're aware and okay having your $HOME director mirrowed to the docker $HOME directory.
 5. You're familiar with terminal shell use and know how to execute a program with the ampersand.
 
```bash 
# Create a docker Qjackctl template
docker rmi debianq_jackctl --force && echo -e 'FROM debian \nRUN apt update \nRUN apt upgrade -y\nRUN apt install qjackctl -y\nENTRYPOINT qjackctl' > /tmp/Dockerfile && docker build -t debianq_jackctl < /tmp/Dockerfile -

# Run the program
xhost local:${USER} && docker run -it --privileged -v ${HOME}:/root -e JACK_NO_AUDIO_RESERVATION=1 --device /dev/snd -v /dev/shm:/dev/shm:rw --net=host -e DISPLAY=${DISPLAY} debianq_jackctl

# Create a startup exe with terminology
echo -e "xhost local:${USER}\nterminology -e docker run -it --privileged -v ${HOME}:/root -e JACK_NO_AUDIO_RESERVATION=1 --device /dev/snd -v /dev/shm:/dev/shm:rw --net=host -e DISPLAY=${DISPLAY} debianq_jackctl" > ./qjackctl && chmod +x ./qjackctl

# Or...

# Create a startup exe with gnome-terminal
echo -e "xhost local:${USER}\ngnome-terminal -- docker run -it --privileged -v ${HOME}:/root -e JACK_NO_AUDIO_RESERVATION=1 --device /dev/snd -v /dev/shm:/dev/shm:rw --net=host -e DISPLAY=${DISPLAY} debianq_jackctl" > ./qjackctl && chmod +x ./qjackctl


#---------------

# Create a docker jamin template
docker rmi debianq_jamin --force && echo -e 'FROM debian \nRUN apt update \nRUN apt upgrade -y\nRUN apt install jamin -y\nENTRYPOINT jamin' > /tmp/Dockerfile && docker build -t debianq_jamin < /tmp/Dockerfile -


# Run the program
xhost local:${USER} && docker run -it --privileged -v ${HOME}:/root -e JACK_NO_AUDIO_RESERVATION=1 --device /dev/snd -v /dev/shm:/dev/shm:rw --net=host -e DISPLAY=${DISPLAY} debianq_jamin

# Create a startup exe with terminology
echo -e "xhost local:${USER}\nterminology -e docker run -it --privileged -v ${HOME}:/root -e JACK_NO_AUDIO_RESERVATION=1 --device /dev/snd -v /dev/shm:/dev/shm:rw --net=host -e DISPLAY=${DISPLAY} debianq_jamin" > ./jamin && chmod +x ./jamin

# Or...

# Create a startup exe with gnome-terminal
echo -e "xhost local:${USER}\ngnome-terminal -- docker run -it --privileged -v ${HOME}:/root -e JACK_NO_AUDIO_RESERVATION=1 --device /dev/snd -v /dev/shm:/dev/shm:rw --net=host -e DISPLAY=${DISPLAY} debianq_jamin" > ./jamin && chmod +x ./jamin
```

