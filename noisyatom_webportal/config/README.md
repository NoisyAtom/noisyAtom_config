# Configuration Docuements
This contains descriptions of configuration documents, what they do and how to use them for the project.



## Gunicorn Documents
The web portal uses NGINX as a HTTP/WEB proxy. It provides separation and SSL certification to the portal. It protects
against attacks like denial of service. From there it routes onto Gunicorn which provides a process engine to allow
multiple django instances to be spun up for multiple users.

For Ubuntu 14 there is only one configuration file called **gunicorn.conf**. It should be deployed with Ubuntu 14 onlu
and will not work with Ubuntu 18 and above.

For Ubuntu 18.04 the web portal needs to deploy **gunicorn.socket** and gunicorn.service. Since Linux now uses **systemd**
we need to deploy Gunicorn as a service which is launched by systemd when there are IP packets arriving on the defined
Linux socket. Systemd will run a socket daemon for Gunicorn. Then it will automatically launch the **gunicorn.service**
file, which launches instances of the Django web platform.



## NAECOMMERCE
The naecommerce documents are configuration files used to deploy into NGINX /etc/nginx/sites-available directory.
This is used to configure what NGINX will proxy too. What sections are static content (e.g. images). How NGINX will
proxy to Gunicorn. What PEM files are used for SSL certification.

There is an naecommerce-template document which simply shows how a template could be build for other projects.

Both documents are used to launch the old NoisyAtom.tech site which was based on Ubuntu 14.04. Those files will not
work on anything above Ubuntu 18.



## NOISYATOM
The noisyatom documents are configuration files used to deploy into NGINX /etc/nginx/sites-available directory.
This is used to configure what NGINX will proxy too. What sections are static content (e.g. images). How NGINX will
proxy to Gunicorn. What PEM files are used for SSL certification.

This document is used to deploy onto NGINX running on Ubuntu 18.04. It has been tested on Ubuntu 20+ at it works.
