# tigervnc-standalone-server

This is a bash script for use ubuntu server by tigervnc.

Update Server 
```
# sudo apt update
```

Install ubuntu mate desktop
```
# sudo apt install ubuntu-mate-desktop
```

Install tigervnc standalone server
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

