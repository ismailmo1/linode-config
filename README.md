# webserver-config

## Playground repo to figure out how to host multiple web apps on one server.

The goal is to have one server instance and multiple subdomains e.g. app1.ismailmo.com, app2.ismailmo.com that each host a containerised application.

This repo uses the jwilder/nginx docker image which automates the nginx config files, see the [original repo](https://github.com/nginx-proxy/nginx-proxy) for details.

To use this, clone the repo into your linux instance and then build the nginx proxy container
``` 
docker-compose up -d
```

This will create a bridge network that will be named "{directory_name}_default", so in this case it will be called "linode-config_default".

Then to add new subdomains and webapps you simply build the container, attach it to this network and set the VIRTUAL_HOST environment variable to the url you want to point to as per [the documentation](https://hub.docker.com/r/jwilder/nginx-proxy/) e.g.

```
docker run -d --network linode-config_default -e VIRTUAL_HOST=app3.ismailmo.com
```
