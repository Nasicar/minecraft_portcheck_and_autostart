Project to create a script that automatically starts the Minecraft server only when you want to use it.
when you try to connect to your server, this script automatically starts your server and you can connect. 
If no one is playing on the server anymore, the server will be closed automatically.

Testet with spigot server on ubuntu 24.04.1 LTS
used programms:
netcat preinstalled
grep preinstalled
screen (install with sudo apt-get install screen)

#ToDo to use it
download and unzip the data and go into folder
wget https://github.com/Nasicar/minecraft_portcheck_und_autostart/archive/refs/heads/main.zip
unzip main.zip
cd ./minecraft_portcheck_and_autostart-main

if you do not have a script to start the Minecraft server, create one
cd /"path_to_server"
touch ./minecraft_start.sh
nano ./minecraft_start.sh
copy this into this script, ONLY FOR spigot

#!/bin/sh
java -Xms4G -Xmx4G -jar ./spigot-1.21.jar
and store it

copy the script portcheck_and_autostart.sh in your minecraft_server folder, make it executable and change the config part
cp ./portcheck_and_autostart.sh /"path_to_server"/portcheck_and_autostart.sh
chmod 500 /"path_to_server"/portcheck_and_autostart.sh
nano /"path_to_server"/portcheck_and_autostart.sh

copy the script portcheck_minecraft.service in the folder /etc/systemd/system and change paths
cp ./portcheck_minecraft.service /etc/systemd/system/portcheck_minecraft.service
sudo nano /etc/systemd/system/portcheck_minecraft.service
sudo chmod 755 /etc/systemd/system/portcheck_minecraft.service
change path by WorkingDirectory and ExecStart to your server_folder

manage script with systemctl 
systemctl enable portcheck_minecraft.service      #enable script
systemctl start portcheck_minecraft.service       #start script
systemctl status portcheck_minecraft.service      #status script

Test with log in with minecraft, if the server starts
(The Server needs a bit to start)

WARNING: NOT STOP the script with order, if the server is online! if you want to stop the script, stop the server manualy 
screen -S $server_name -X stuff 'stop\n'    #$server_name is the name of the configured screen name in the script portcheck_and_autostart.sh
or log into your server and stop it with /stop in the text field, if you are allowed to do this
systemctl stop portcheck_minecraft.service
