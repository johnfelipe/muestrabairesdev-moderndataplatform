# Solución de problemas

En este documento se documentan algunos consejos y trucos para solucionar problemas mientras se desarrolla una nueva versión del generador.

## Al correr `platys gen` Recibo un error "ya usado"

Al correr `platys gen` Recibo el siguiente error: "El nombre del contenedor "/platys" ya está en uso por contenedor":

    docker@ubuntu:/mnt/hgfs/git/gschmutz/kafka-workshop/01-environment/docker$ platys gen
    2020/11/15 07:11:09 using configuration file [config.yml] with values:  platform-name: [kafka-workshop], platform-stack: [trivadis/platys-modern-data-platform] platform-stack-version: [1.9.0-preview], structure [flat]
    {"status":"Pulling from trivadis/platys-modern-data-platform","id":"1.9.0-preview"}
    {"status":"Digest: sha256:a4f096f9d776c123999e7145d2c431baf40ba99959c26405f36bece47fb4598a"}
    {"status":"Status: Image is up to date for trivadis/platys-modern-data-platform:1.9.0-preview"}
    2020/11/15 07:11:10 Error response from daemon: Conflict. The container name "/platys" is already in use by container "699dad31ab7b2523f1687780502ab7488102d279006632859586c038853593c9". You have to remove (or rename) that container to be able to reuse that name.

Puede solucionarlo simplemente deteniendo y eliminando el `platys` contenedor, que sigue ejecutándose desde una llamada anterior a `platys`

    docker stop platys
    docker rm platys

## ¿Cómo puedo hacer funcionar el generador manualmente?

Para hacer funcionar el generador sin el `platys` CLI, realice lo siguiente `docker run` mandar:

    docker run --rm -v ${PWD}/config.yml:/tmp/config.yml -v ${PWD}:/opt/mdps-gen/destination -e DEL_EMPTY_LINES=1 trivadis/platys-modern-data-platform:1.8.0-preview

o en modo detallado

    docker run --rm -v ${PWD}/config.yml:/tmp/config.yml -v ${PWD}:/opt/mdps-gen/destination  -e DEL_EMPTY_LINES=1  -e VERBOSE=1 trivadis/platys-modern-data-platform:1.8.0-preview

## ¿Cómo puedo ejecutar el motor de plantillas jinja2 manualmente?

    docker run --rm -v /mnt/hgfs/git/gschmutz/twitter-streaming-demo/docker/documentation/templates:/templates -v /mnt/hgfs/git/gschmutz/twitter-streaming-demo/docker:/variables -e PUBLIC_IP=${PUBLIC_IP} dinutac/jinja2docker:latest /templates/services.md.j2 /variables/docker-compose.yml --format=yaml --outfile test.md
