# 25. Deployment: Heroku Cloud

[https://www.heroku.com/](https://www.heroku.com/)

Plataforma en la nube, como servicio, y nosotros nos centramos en desarrollar nuestra aplicación y de subirla a este sistema. Heroku se encarga del sistema operativo, hardware, seguridad...

Gratuito para pequeñas aplicaciones, si son aplicaciones mas grandes, es de pago

Registrarse, marcando Java como lenguaje principal así ya estará instalado (sino hay que instalarlo a través de buildpacks, una vez creada la cuenta)

## Crear una nueva app

1. Login
2. Create new app
3. Dar un nombre a la aplicación y elegir region (Europe/United States)
4. Crear aplicacion

## Addons

Se puede incluir nuevos módulos, por ejemplo, con MySQL (ClearDB MySQL), pero aunque es gratis, hay que indicar una tarjeta de crédito

## Desplegar una aplicación Java desde local

Pre requisitos:

- Hay que crear una aplicación en Heroku primero, por ejemplo, `spring-boot2-heroku`

- Instalar [Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli)

Vamos a la carpeta donde tenemos el proyecto e iniciamos la sesion:

```
heroku login
```

Crear el repositorio de heroku en el proyecto (se crea la carpeta .git):

```
git init
```

Asociar este repositorio creado con el remoto en Heroku que hemos creado (usar el mismo nombre):

```
heroku git:remote -a spring-boot2-heroku
```

Configurar el git del repo:

```
git config --global user.email "micorreo@gmail.com"
```

Realizamos el deploy, subiendo todo el código, al detectar el push se desencadena el proceso de compilación:

```
git add .
git commit -am "first commit"
git push heroku master
```

Para abrir la aplicación en el navegador:

```
heroku open
```

Ver log de la app:

```
heroku logs --tail
```

## Desplegar una aplicación Java mediante un jar usando plugins

Aquí partimos del jar generado en local y se sube, sólo el jar, así se evita subir el código fuente, sólo el código binario ejecutable

Creamos un nuevo proyecto: `spring-boot2-heroku-v2`

Agregar en el `application.properties`:

```
server.port=${PORT:8080}
```

Construimos el jar:

```
mvn clean install
```

Nos logamos en heroku:

```
heroku login
```

Creamos el repositorio git:

```
git init
```

Instalamos un plugin para el deploy

```
heroku plugins:install heroku-cli-deploy
```

Enlazamos el repo local con el remoto que hemos creado

```
heroku git:remote -a spring-boot2-heroku-v2
```

Desplegamos el jar que hemos generado, indicando la ruta donde se encuentra

```
heroku deploy ./target/spring-boot-heroku-0.0.1-SNAPSHOT.jar
```

Se muestra la URL donde está disponible la aplicación

Para abrir la aplicación en el navegador:

```
heroku open
```