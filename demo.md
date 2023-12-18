# Demo

-----------------------------------------

Para la demo voy acceder con el usuario arantxa-pi y voy a crear un usuario nuevo que voy a añadir al proyecto.
Me salgo y accedo con el usuario nuevo.

## Comprobaciones previas

- Mostrar Harbor
- Mostrar web desplegada
- Mostrar en Gitlab:
    - Proyecto-integrado (app)
    - Pipeline editor. Fichero .gitlab-ci.yml
    - Variable creada para acceso ssh (en proyecto-integrado > Settings > CI/CD > Variables)
    - Registro de pipelines

## Runner

- Registrar nuevo runner: habría que registrar el runner con el ejecutor shell de la siguiente forma (también habría que añadir el usuario gitlab-runner al grupo docker, pero eso ya lo he hice, así que me lo ahorro):

```bash
      sudo gitlab-runner register -n \
        --url "http://172.22.200.23" \
        --registration-token 8xZnRpgRGL22g2SMMRMW \
        --executor shell \
        --description "Runner shell"
```

- Comprobar fichero /etc/gitlab-runner/config.toml

## Usuario nuevo

- Crear usuario demo-clase. 
- Darle permisos de developer sobre el proyecto proyecto-integrado.
- Salir y acceder con nuevo usuario

- Clonar repo

## Cambios en el repositorio

- Hacer cambio en app (poner en pag. inicio “Hola, estamos haciendo la demo en clase”)
- Hacer cambio en pipeline (subir imagen con el nombre prueba-$proyecto-integrado
- Hacer commit y push

## Comprobaciones posteriores

- Comprobar que se ha lanzado el pipeline y termina sin errores
- Comprobar jobs lanzados
- Comprobar runner utilizado
- Comprobar que se ha subido la imagen a Harbor
- Comprobar la aplicación desplegada con los cambios
