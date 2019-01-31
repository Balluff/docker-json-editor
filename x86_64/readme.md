# JSON Editor als Container (Dockerfile)

## Voraussetzungen

- Docker installiert
- Docker Grundkenntnisse

## Verzeichnis erstellen

Für das Dockerfile muss ein beliebiges Verzeichnis erstellt werden. 

Dies kann entweder mit dem `mkdir` Befehl oder über den Dateimanager gemacht werden.

Wenn das Verzeichnis erstellt ist, muss eine Datei mit dem Name `Dockerfile` erstellt werden.

Dies kann entweder mit `touch Dockerfile` oder über den Dateimanager gemacht werden.

## Dockerfile erstellen

Das Dockerfile muss mit einem Editor geöffnet werden. Es empfiehlt sich VS Code.

Folgender Inhalt muss in das Dockerfile geschrieben werden:

```
FROM alpine:3.8
LABEL Maintainer "Marvin Gfroerer"
RUN apk update && apk upgrade
RUN apk add nodejs apache2 git 
RUN mkdir apachefiles && cd  apachefiles && git clone https://github.com/Balluff/json-editor.git
RUN apk del git && rm -rf /var/cache/apk/* /tmp/*
RUN cp -r /apachefiles/json-editor/* /var/www/localhost/htdocs
RUN mkdir /run/apache2
EXPOSE 80
ENTRYPOINT ["/usr/sbin/httpd","-D","FOREGROUND"]
```

Anschließend muss das Image gebaut werden.

**Hinweis:** Der Buildprozess funktioniert nur, wenn man sich im Terminal im Verzeichnis des Dockerfile befindet! `cd [Dockerfileverzeichnis]`

Mit folgendem Befehl wird der Buildprozess gestartet:

```
docker build -t apache-json-editor .
```

Mit dem Befehl `docker images` kann überprüft werden ob der Buildvorgang erfolgreich abgeschlossen wurde. Es muss ein Image mit dem Name `apache-json-editor` erscheinen.

```
docker run -dit --name json-editor -p 80:80 apache-json-editor:latest
```

Mit dem Befehl `docker ps` kann überprüft werden ob der Container erfolgreich gestartet wurde. Es muss ein Container mit dem Name `apache-json-editor` erscheinen.

Der JSON Editor kann nun mit `localhost/` oder `hostname/` im Webbrowser aufgerufen werden.

Weitere Informationen zu httpd (apache) im [Docker Hub](https://hub.docker.com//_/httpd/)

Weitere Informationen zu Docker und der Verwendung des `docker run` Befehls in der [Docker Dokumentation](http://swhkb/docker.dokumentation).