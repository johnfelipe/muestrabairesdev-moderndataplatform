# ¿Qué hacer cuando se ha cambiado la dirección IP pública?

Esta breve instrucción muestra qué hacer cuando tiene una pila de Platys en ejecución y se ha cambiado la dirección IP pública del host de Docker (esto puede suceder si su host de Docker se ejecuta en una máquina virtual en la nube).

Algunos servicios de platys ya no se ejecutarán correctamente, si la dirección IP cambia, por lo tanto, la pila debe detenerse y eliminarse y luego iniciarse nuevamente. Por lo tanto, usted debe **Intente evitar un cambio de la dirección IP**, es decir, en la nube asignando una dirección IP de pila!

Puede comprobar cuál era su IP al ejecutar la pila mostrando el valor de la variable PUBLIC_IP

    echo $PUBLIC_IP

En los bloques de código a continuación asumimos que el `DATAPLATFORM_HOME` se establece la variable y apunta a la `docker` donde se encuentran los metadatos de la pila de redacción de Docker (la carpeta que contiene el `docker-compose.yml` archivo).

Navegue en la pila y detenga y elimine todos los servicios. ¡Tenga en cuenta que se eliminarán todos los datos dentro de su contenedor en ejecución!

    cd $DATAPLATFORM_HOME

    docker-compose down

Cambiar las variables de entorno para que contengan la nueva dirección IP

    export PUBLIC_IP=$(curl ipinfo.io/ip)
    export DOCKER_HOST_IP=$(ip addr show ${NETWORK_NAME} | grep "inet\b" | awk '{print $2}' | cut -d/ -f1)

Compruebe que el valor de la variable es la dirección IP correcta

    echo $PUBLIC_IP

Actualizar el `.bash_profile` para volver a seleccionar la nueva dirección IP (de modo que después de un nuevo inicio de sesión sea correcta).

Abra el archivo con un editor (cambie la variable de nombre de usuario a su usuario actual)

    export USERNAME=ubuntu
    nano /home/$USERNAME/.bash_profile

Guarde el archivo con **Ctrl-O** y salir con **Ctrl-X**.

Reiniciar la pila

    cd $DATAPLATFORM_HOME

    docker-compose up -d

Comprobar los archivos de registro

    docker-compse logs -f
