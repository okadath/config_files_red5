descargar a tu pc y subirlo por ftp, para esto debes tambien tener la maquina con ssh, floating ip y firewall, sigue la documentacion

sftp -i test_red5 root@165.227.251.131 
put red5pro-server-us-6d0df64b-b317-4467-85cc-c3f06f1fb22d.zip /home/
quit


y loguearse con ssh 

ssh -i test_red5 root@165.227.251.131 


apt-get update
apt-get install -y default-jre unzip libva1 libva-drm1 libva-x11-1 libvdpau1 jsvc ntp

se necesita apache jscv para definir un servicio (como el socket de gunicorn)