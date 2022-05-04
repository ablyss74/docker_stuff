# Install compiz Docker container and xfce desktop on Clear Linux for wobbly windows and other special effects.
## Tested with X11 and Nvidia driver installed.
## Not fully tested on wayland or Nouveau


### Install xfce4-desktop and Docker #Kde Plasma works too but in this example xfce4 is used.
sudo swupd bundle-add xfce4-desktop
sudo swupd bundle-add containers-basic
sudo systemctl start docker.service

### Option 1 - default
### Disable graphical target and create .xinitrc
sudo systemctl set-default multi-user.target 
[[ $HOME/.xinitrc ]] && cp $HOME/.xinitrc $HOME/.xinitrc.$$.backup
echo "exec xfce4-session" > $HOME/.xinitrc
### Now reboot, login and type: startx

### Option 2
### If you don't feel like messing w/ .xinitrc and prefer to type in the session manually, just do
> startx /usr/bin/xfce4-session

### Xfce4 desktop
### Turn on compositing in xfce settings manager
### Save and close

### Open up Terminal and copy and post these lines.
echo -e "FROM debian \\nRUN apt update \\nRUN apt upgrade -y\\nRUN apt update -y" > /tmp/Dockerfile
sudo docker build -t debiancompiz < /tmp/Dockerfile -
xhost local:${USER}
sudo docker run -it --privileged -v /dev/shm:/dev/shm:rw --net=host -e DISPLAY=${DISPLAY} debiancompiz

### Inside the Docker container Terminal
apt install compizconfig-settings-manager compiz emerald 


### Save the container at this point in the Host Terminal, not the Docker terminal
sudo docker ps
sudo docker commit <CONTAINER ID>  debiancompiz  # Replace Container ID with the first ID shown after your type sudo docker ps


### Configure Compiz - Docker Terminal
ccsm
### Turn on these plugins ( if the plugin is not listed that means its probably auto-detected and installed )
> Composite, OpenGL, Window Decorations, Wobbly Windows, Grid, Move Window, Place Window, Resize Window, Shift Switcher

### Load emerald and compiz - Docker Terminal
emerald --replace && compiz --replace &

### Emerald configuation if needed - Docker Terminal
#emerald-theme-manager &


### If things went well save the container again - Host Terminal


### Turning off compiz... - Host Terminal
### Open up terminal 
xfwm4 --replace &
### Or log out and back in with startx


### Install nvidia drivers if you want - Docker Terminal
### From inside the container...
### Dowanload Nvidia driver https://www.nvidia.com/en-us/drivers/unix/
apt install curl kmod -y
curl -O <link to driver>
bash ./NVIDIA-Linux-<your driver here>.run --accept-license --ui=none --no-kernel-module --no-questions
nvidia-smi


## Issues
### If compositing is off on the host, opengl might not work.
### If you exit the container without first running xfwm4 --replace & on the host, your windows will have no borders.
### Try to right click on the desktop and logout and restart X with startx
 
### Compiz does'n't work in Gnome or Enlightenment desktoop sessions.  Just FYI

## Extra Stuff
### This little script will watch for compiz/emerald/xfwm4 to close and load xfwm4 automatically.  
while sleep 1; do [[ ! $(ps -A | grep compiz) && ! $(ps -A | grep xfwm4) ]] && xfwm4 --replace ; done

### Delete docker contianer - If you need to start fresh or whatever reason
\# sudo docker rmi debiancompiz --force

### Enable graphical target to fallback and ignore .xinitrc
sudo systemctl set-default graphical.target
