# tigervnc-standalone-server

This is a bash script for use to Install & Configure VNC Server on Ubuntu 22.04 or 20.04.

After establishing an SSH connection to your server, execute the following command to update your package index:
```
# sudo apt update
```

Next we’ll run the following command to install xfce4 and xfce4-goodies:
```
# sudo apt install xfce4 xfce4-goodies
```

Install ubuntu mate desktop
```
# sudo apt install ubuntu-mate-desktop
```

Install VNC server
```
# sudo apt install tigervnc-standalone-server
# nano ~/.vnc/xstartup
```
Add this line in xstartup file:
```
#!/bin/sh
# Start up the standard system desktop
unset SESSION_MANAGER
unset DBUS_SESSION_BUS_ADDRESS
/usr/bin/mate-session
[ -x /etc/vnc/xstartup ] && exec /etc/vnc/xstartup
[ -r $HOME/.Xresources ] && xrdb $HOME/.Xresources
x-window-manager &

```
```
# chmod +x ~/.vnc/xstartup
# vncserver
# vncpasswd
# vncserver -kill :1
# vncserver -localhost no :1
```
Setting up the VNC as a system service
```
# sudo nano /etc/systemd/system/vncserver@.service
```
Add this line :
```
[Unit]
Description=Start TigerVNC server at startup
After=syslog.target network.target
[Service]
Type=forking
User=YOUR_USERNAME
Group=YOUR_USERNAME
WorkingDirectory=/home/YOUR_USERNAME
PIDFile=/home/YOUR_USERNAME/.vnc/%H:%i.pid
ExecStartPre=-/usr/bin/vncserver -kill :%i > /dev/null 2>&1
ExecStart=/usr/bin/vncserver -depth 24 -geometry 1920x1080 -localhost :%i
ExecStop=/usr/bin/vncserver -kill :%i
[Install]
WantedBy=multi-user.target
```
Save the file (Ctrl + O, then Enter) and Exit (Ctrl + X).
Now we need to make the system aware of our new unit file. Execute the commands below:
```
# sudo systemctl daemon-reload
# sudo systemctl enable vncserver@1.service
```
With that done, we can now start, stop and restart our VNC server as a system service. To test this, let’s first stop the instance that we had launched previously with the command below:
```
# vncserver -kill :1
# sudo systemctl start vncserver@1
# sudo systemctl status vncserver@1
```
