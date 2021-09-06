# 26. Deployment: Heroku Cloud - MySQL

[https://www.heroku.com/](https://www.heroku.com/)

## Agregar tarjeta de credito a la cuenta en Heroku

Para poder usar MySQL tenemos que incluir una tarjeta de crédito en nuestra cuenta, es gratuito y no se cargará ningún movimiento, pero es necesario para poderlo usar.

Se incluye en

Account settings > Billing

## Creando app en heroku cloud

Creamos un proyecto nuevo en heroku: `spring-boot2-heroku-mysql`

Deployment method: Heroku Git (desplegaremos la aplicación desde local por consola)

Iniciamos sesion:

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
heroku git:remote -a spring-boot2-heroku-mysql
```

Agregar addon de mysql en nuestro proyecto (proveedor cleardb e ignite indicamos que es la versión gratuita):

```
heroku addons:create cleardb:ignite
```

Se crea una variable de entorno llamada `CLEARDB_DATABASE_URL` con la URL de la base de datos. Se puede ver el contenido de la variable con `heroku config | grep CLEARDB_DATABASE_url`

```
CLEARDB_DATABASE_URL: mysql://b65e5bd2a07717:c1ede8d8@us-cdbr-iron-east-04.cleardb.net/heroku_b110ea4cd06cdf7?reconnect=true
```

Esta cadena contiene los siguientes datos:

- username: `b65e5bd2a07717`
- password: `c1ede8d8`
- hostname: `us-cdbr-iron-east-04.cleardb.net`
- database: `heroku_b110ea4cd06cdf7?reconnect=true`

Pasamos estos valores al `application.properties` del proyecto:

```
spring.datasource.url=jdbc:mysql://us-cdbr-iron-east-04.cleardb.net/heroku_b110ea4cd06cdf7?reconnect=true
spring.datasource.username=b65e5bd2a07717
spring.datasource.password=c1ede8d8
```

También tenemos que incluir:

```
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.jpa.database-platform=org.hibernate.dialect.MySQL5InnoDBDialect
spring.jpa.hibernate.ddl-auto=create-drop
logging.level.org.hibernate.SQL=debug
```

Para ver lo que hemos configurado en nuestro proyecto en Heroku, revisar:

- Resources: ver aparece asociado MySQL
- Settings: en Config Vars, aparece CLEARDB_DATABASE_URL

## Deploy en Heroku

Hacemos login en heroku si no tenemos una sesión activa

Realizamos el deploy del proyecto:

```
git add .
git commit -am "migracion a MySQL"
git push heroku master
```

Abrimos la aplicación:

```
heroku open
```

Analizamos los logs:

```
heroku logs
```

Se pueden visualizar errores por la manera que tiene heroku de asignar los IDs autoincrementales, comienza en el 2,y despues va de 10 en 10, 12, 22, 32...lo que si se han usado ciertos valores concretos para asignar permisos a los roles por ejemplo, puede fallar

## Viendo la base de datos

Con los datos obtenidos en la sección anterior, se puede usar cualquier cliente MySQL para ver el contenido de la base de datos creada.

Para que podamos ver las tablas creadas, la app tiene que estar levantada, sino lo está:

- Ir a la app en heroku
- Seleccionar arriba a la derecha el botón `Open app`

**Si la app en heroku lleva mas de una hora sin recibir ninguna petición se queda en modo stand by, por optimización de recursos. Si alguien accede, se realiza de nuevo el deploy**

Si se hace algún tipo de carga inicial, incluir en los insert los IDs deseados para evitar problemas.