# JSON Editor in a Docker container 

## Requirements

- Docker installed
- Docker basic knowledge

## Run container from DockerHub

```
docker run -dit --name json-editor -p 80:80 balluff/json-editor:latest
```

## Using Dockerfile for manual build

### Directory for dockerfile

You have to create a random directory for the dockerfile.

You can do this with `mkdir` or in the explorer.

When the directory is created you have to create a new file named `Dockerfile`. 

### Dockerfile content

You have to open the dockerfile with an editor. It is recommend to use VS Code.

The following content must be written into the dockerfile.

```dockerfile
FROM alpine:3.8
LABEL Maintainer "Marvin Gfrörer"
LABEL Version "1.0"
LABEL Vendor "Balluff"
LABEL Name "docker-json-editor"
RUN apk update && apk upgrade
RUN apk add nodejs apache2 git 
RUN mkdir apachefiles && cd  apachefiles && git clone https://github.com/Balluff/json-editor.git
RUN apk del git && rm -rf /var/cache/apk/* /tmp/*
RUN cp -r /apachefiles/json-editor/* /var/www/localhost/htdocs
RUN mkdir /run/apache2
EXPOSE 80
ENTRYPOINT ["/usr/sbin/httpd","-D","FOREGROUND"]
```

After that you have to build the dockerfile.

**Tip:** The Build process only works if you are in the directory where the dockerfile is. 

With the following command you can start the build process:

```
docker build -t apache-json-editor .
```

If you run `docker images` in your terminal, you can check if the build process was successful. You should see now one image named `apache-json-editor`.

Now you can run the container with the `apache-json-editor` image.

```
docker run -dit --name json-editor -p 80:80 apache-json-editor:latest
```

With the command `docker ps` you can check if the container was started successfully. You should see a container named `json-editor` now.

If you type in `localhost/` or `hostname/` in your browser you should see the JSON Editor site.
