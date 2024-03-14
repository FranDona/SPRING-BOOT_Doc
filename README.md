# SPRING-BOOT_Doc

[//]: # (version: 1.0)
[//]: # (author: Fran Dona Villar)
[//]: # (date: 2024-03-14)

- [SPRING-BOOT\_Doc](#spring-boot_doc)
  - [¿Que es Spring Boot?](#que-es-spring-boot)
  - [Instalaicon Spring Boot](#instalaicon-spring-boot)
    - [Poner en Español STS](#poner-en-español-sts)
  - [Crear un nuevo Proyecto](#crear-un-nuevo-proyecto)

## ¿Que es Spring Boot?
Spring Boot es un framework de desarrollo de aplicaciones Java que simplifica la configuración y el desarrollo mediante la automatización de tareas repetitivas. Proporciona configuraciones predeterminadas inteligentes y herramientas integradas para crear aplicaciones rápidamente con un mínimo esfuerzo.

## Instalaicon Spring Boot
1. Empezaremos dirigiéndonos a la [página de spring](https://spring.io/tools)
   
2. Seleccionaremos la versión que necesitemos en función de nuestro SO (En nuestro caso `Linux x86_64`)
   
3. Nos dirigimos a Descargas y creamos una carpeta llamada sts 
   
4. Movemos el archivo descargado a la carpeta sts
   
5. Descomprimimos el archivo con tar **-xvf nombre_archivo**
   
6. Movemos el archivo descomprimido sts-4.22.0.RELEASE hacia /usr/sahre/sts
    ```bash
    sudo mv sts-4.22.0.RELEASE /usr/sahre/sts
    ```
    La carpeta se creará automáticamente

7. Nos colocamos en la ruta mencionada anteriormente
   
8. Arrancamos la aplicación sts con:
   ```bash
   ./SpringToolSuite4
   ```

9.  Nos pedirá donde queremos guardar el área de trabajo, en mi caso usare **/home/nombre/Documentos/sts**

Con esto ya tendremos funcionando STS pero vamos a crear un acceso directo para poder abrirlo más cómodamente.

```bash
cd /usr/share/applications
sudo nano sts.desktop
```

Agregamos el siguiente contenido para crear el acceso directo:

```console
[Desktop Entry]
Version=1.0
Type=Application
Name=STS
GenericName=STS
Comment=STS
Exec=/usr/share/sts/SpringToolSuite4
TryExec=/usr/share/sts/SpringToolSuite4
Icon=/usr/share/sts/icon.xpm
Terminal=false

Para actualizar los enlaces usamos:
#sudo update-desktop-database /usr/share/applications
```

### Poner en Español STS
Para ponerlo en español hacemos lo siguiente:
1. Para ponerlo en español: Help > Install new software
2. En Name ponemos Babel y en Location: https://download.eclipse.org/technology/babel/update-site/latest/
3. Tras unos segundos (¡tarda un poquito!), le damos a Babel Language Packs in Spanish, marcando la casilla y pulsamos [Next]
4. De nuevo tras varios segundos, le damos a [next], aceptamos la licencia y [Finish]. Cuando termine la instalación (miramos la esquina inferior derecha), reiniciamos Eclipse.

## Crear un nuevo Proyecto
Crear el proyecto desde la aplicación:

1. Entramos dentro de STS y vamos a: Archivo>Nuevo>Spring Starter Project

2. Rellenamos los datos que nos piden:
   - Le ponemos un nombre al proyecto si queremos
   - Maven
   - 17
   - Jar
   - Java

3. Pulsamos siguiente y añadimos las dependencias
   - Spring Boot DevTools
   - Thymeleaf
   - Spring Web

4. Pulsamos Finalizar para terminar.
---
Crear el proyecto desde [start.spring.io](https://start.spring.io):

1. Entramos al enlace [https://start.spring.io](https://start.spring.io)<br>
   
    Seleccionamos la sigueintes opciones
   - Project: Maven
   - Spring Boot: 3.2.3
   - Project Metadata: Colocamos los nombres que queramos usar
   - Dependencies: Spring Boot DevTools, Thymeleaf, Spring Web

2. Generamos el archivo pulsando CTRL + ENTER
   
3. Guardamos el archivo en nuestra area de trabajo de STS para tenerlo más controlado

4. Entramos dentro de STS y vamos a: Archivo>