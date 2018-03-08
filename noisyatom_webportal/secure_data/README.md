# Secure Data Folder

This folder contains ansible scripts that will backup and pull secure date from the server. Specifically this  will be data associated 
with HTTPS. The web site uses let's encrypt to certify DNS.

## Snapshot of LetsEncrypt Info Page

Congratulations! You have successfully enabled https://noisyatom.tech and
https://www.noisyatom.tech

You should test your configuration at:
https://www.ssllabs.com/ssltest/analyze.html?d=noisyatom.tech
https://www.ssllabs.com/ssltest/analyze.html?d=www.noisyatom.tech
-------------------------------------------------------------------------------

IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at:
   /etc/letsencrypt/live/noisyatom.tech/fullchain.pem
   Your key file has been saved at:
   /etc/letsencrypt/live/noisyatom.tech/privkey.pem
   Your cert will expire on 2018-06-06. To obtain a new or tweaked
   version of this certificate in the future, simply run certbot again
   with the "certonly" option. To non-interactively renew *all* of
   your certificates, run "certbot renew"
 - Your account credentials have been saved in your Certbot
   configuration directory at /etc/letsencrypt. You should make a
   secure backup of this folder now. This configuration directory will
   also contain certificates and private keys obtained by Certbot so
   making regular backups of this folder is ideal.
 - If you like Certbot, please consider supporting our work by:

   Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
   Donating to EFF:                    https://eff.org/donate-le
