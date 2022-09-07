# Comando Cheatsheet

## Iniciar la pila de plataformas

    docker-compose up -d

## Detener y quitar la pila de plataformas

    docker-compose down

tenga cuidado, ¡todo el contenido de los contenedores en ejecución se eliminará!

## Mostrar todos los contenedores en ejecución

Mostrar todos los contenedores en ejecución

    docker-compose ps

## Conectar a un contenedor en ejecución

Mostrar el registro de todos los contenedores en ejecución

    docker-compose logs -f

Solo mostrar el registro de algunos servicios (`kafka-connect-1` y `kafka-connect-2` en este ejemplo)

    docker-compose logs -f kafka-connect-1 kafka-connect-2

## Conectar a un contenedor en ejecución

Ejecutar un `bash` shell mediante la introducción del identificador o nombre del contenedor (aquí `kafka-1`)

    docker exec -ti kafka-1 bash
