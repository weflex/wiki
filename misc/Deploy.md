Tips On Deploy A Project
====


Setup a server from scratch (with examples on CentOS)
----

install nginx

    yum install -y epel-release
    yum install -y nginx
    
install docker

    yum install -y docker

install daemon script for docker

    curl -X GET \
         -L "https://raw.githubusercontent.com/weflex/weflex/master/bin/scripts/dockerd.service?token=ABZMY4j8_K0AScjwtPnPPTYyfCR3-P8gks5XGJg9wA%3D%3D" \
         -o /etc/systemd/system/dockerd.service
         
start dockerd

    service dockerd start
    
install spawn script

    curl -X GET \
         -L "https://raw.githubusercontent.com/weflex/weflex/master/bin/scripts/spawn?token=ABZMYxNMAJIF4XUtrHT6vsAeowd8tK5vks5XGJj1wA%3D%3D" \
         -o /usr/local/bin/spawn

instal git

    yum install -y git

find out which package provides the missing files

    yum provides <your-file-name>


Deploy with docker
----

login onto daocloud registry

    docker login daocloud.io
    
setup environment variables (see spawn script for more options and details)

    vi ~/.bashrc
    
start container

    spawn <your-project> <build-id>


Setup Nginx
----

    vi /etc/nginx/nginx.conf

set nginx up to work as a proxy

    ... // other nginx configurations
    
    http {
      ... // other setups and servers
      
      server {
        listen       80; // or other port user can access
        location     / {
          proxy_pass http://127.0.0.1:<service-port>;
        }
      }
    }

or you can use it as a static file server

    ... // other nginx configurations
    
    http {
      ... // other setups and servers
      
      server {
        listen       80; // or other port user can access
        location     / {
          root <your-static-file-root-directory>
        }
      }
    }

then restart the server

    nginx -s reload
