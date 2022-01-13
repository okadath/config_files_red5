descargar a tu pc y subirlo por ftp, para esto debes tambien tener la maquina con ssh, floating ip y firewall, sigue la documentacion

sftp -i test_red5 root@165.227.251.131 
put red5pro-server-us-6d0df64b-b317-4467-85cc-c3f06f1fb22d.zip /home/
quit


y loguearse con ssh 

ssh -i test_red5 root@165.227.251.131 


apt-get update
apt-get install -y default-jre unzip libva1 libva-drm1 libva-x11-1 libvdpau1 jsvc ntp

se necesita apache jscv para definir un servicio (como el socket de gunicorn)



el server de red5 viene con un servicio por default, pero hay que definir el nuestro


instalar cerbot con snap, no como lo indica la documentacion, esta desactualizada

certbot certonly --standalone --email vintaw.01@gmail.com --agree-tos -d red5.events.house



IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at:
   /etc/letsencrypt/live/red5.events.house/fullchain.pem
   Your key file has been saved at:
   /etc/letsencrypt/live/red5.events.house/privkey.pem
   Your certificate will expire on 2021-06-21. To obtain a new or
   tweaked version of this certificate in the future, simply run
   certbot again. To non-interactively renew *all* of your
   certificates, run "certbot renew"
 - If you like Certbot, please consider supporting our work by:

   Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
   Donating to EFF:                    https://eff.org/donate-le






sudo openssl pkcs12 -export \
  -in /etc/letsencrypt/live/red5.events.house/fullchain.pem \
  -inkey /etc/letsencrypt/live/red5.events.house/privkey.pem \
  -out /etc/letsencrypt/live/red5.events.house/fullchain_and_key.p12 \
  -name tomcat

password Qwerty35$



sudo keytool -importkeystore \
  -deststorepass Qwerty35$ \
  -destkeypass Qwerty35$ \
  -destkeystore /etc/letsencrypt/live/red5.events.house/keystore.jks \
  -srckeystore /etc/letsencrypt/live/red5.events.house/fullchain_and_key.p12 \
  -srcstoretype PKCS12 \
  -srcstorepass Qwerty35$ \
  -alias tomcat


sudo keytool -export -alias tomcat \
  -file /etc/letsencrypt/live/red5.events.house/tomcat.cer \
  -keystore /etc/letsencrypt/live/red5.events.house/keystore.jks \
  -storepass Qwerty35$ -noprompt


sudo keytool -import -trustcacerts -alias tomcat \
  -file /etc/letsencrypt/live/red5.events.house/tomcat.cer \
  -keystore /etc/letsencrypt/live/red5.events.house/truststore.jks \
  -storepass Qwerty35$  -noprompt

verificar los pasos anteriores

sudo ls /etc/letsencrypt/live/red5.events.house/


rtmps.keystorepass=Qwerty35$
rtmps.keystorefile=/etc/letsencrypt/live/red5.events.house/keystore.jks
rtmps.truststorepass=Qwerty35$
rtmps.truststorefile=/etc/letsencrypt/live/red5.events.house/truststore.jks



borrar los streams
rm -rf /usr/local/webapps/live/streams/