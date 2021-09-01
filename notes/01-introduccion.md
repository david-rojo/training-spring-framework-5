# 01. Introducción

Características de Spring:

- Inyección de dependencia
- Modelo Vista Controlador (MVC)
- Modelo Programación Reactiva (WebFlux)
- Acceso a Datos y Persistencia (JPA)
- Programación orientada a aspectos (AOP)
- Cero configuración
- Mínimo JDK 8 requerido
- Programación funcional con Reactor

Componentes:

- Spring Boot
- Spring IoC
- Spring MVC
- Spring WebFlux
- Vistas Thymeleaf
- Spring Security
- JWT
- Spring Data JPA e Hibernate
- Spring Data Mongo
- Exportar a PDF y Excel
- Subida de archivos
- API REST
- Angular 7, jQuery y Bootstrap 4

## Herramientas necesarias

- JDK 11
- maven
- [Spring Tools Suite IDE](https://spring.io/tools)
- docker

## Configuración de Java en Linux

Para configurar PATH y JAVA_HOME en Linux, se puede usar `/etc/profile` o `/etc/environment` o `~/.bashrc` (para el usuario)

Abrir archivo `~/.bashrc` con cualquier editor y agregar:

```
export JAVA_HOME=/usr/java/jdk-13.0.1
export PATH=$JAVA_HOME/bin:$PATH
```

Guardamos y cerramos
Luego en el terminal, ejecutar:

```
source ~/.bashrc
```

Y se comprueba en el terminal:

```
echo $PATH
echo $JAVA_HOME
```


