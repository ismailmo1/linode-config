# webserver configuration on linux vm

## How to host multiple web apps on one server.

The goal is to have one server instance and multiple subdomains e.g. app1.ismailmo.com, app2.ismailmo.com that each host a containerised application.

This repo uses the [jwilder/nginx](https://github.com/nginx-proxy/nginx-proxy) and the [nginxproxy/acme-companion](https://github.com/nginx-proxy/acme-companion) docker images which automate the nginx config files and renewal of SSL certificates.

To use this example, clone the repo into your linux instance and then build the nginx proxy container

``` 
git clone https://github.com/ismailmo1/linode-config.git
cd linode-config
docker-compose up -d
```

This will create a bridge network that will be named "{directory_name}_default", so in this case it will be called "linode-config_default".

Then to add new subdomains and webapps you simply build the container, attach it to this network and set the VIRTUAL_HOST and LETSENCRYPT_HOST environment variables to the url you want to point to.

## Example
For app1 in this repo:

- first build the container

```
cd app1
docker build -t app1 . 
```

- then run the container with the configuration described above:

```
 docker run -d --network linode-config_default --env VIRTUAL_HOST={app1.ismailmo.com} --env LETSENCRYPT_HOST={app1.ismailmo.com} --name app1-container {app1}
```
