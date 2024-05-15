# Jenkins Lab

## Configuración YML

Antes de ejecutar el Docker Compose, debemos verificar que el .yml tenga la siguiente estructura:

![image1](https://github.com/santiagoarevalo/jenkins-lab/assets/71450411/ffaabec0-a9c6-4ace-8af7-f0aaf6d37436)

Esto nos garantiza que los volúmenes están correctamente definidos.

## Ejecutar Docker Compose

Para ejecutar Docker Compose, asegúrate de que tengas Docker Compose instalado en tu sistema. Luego, sigue estos pasos:

1. Navega a la ubicación donde se encuentra tu archivo `docker-compose.yml`.

2. Ejecuta el siguiente comando en tu terminal para iniciar los contenedores definidos en el archivo `docker-compose.yml`:

```bash
docker-compose up -d
```
![image2](https://github.com/santiagoarevalo/jenkins-lab/assets/71450411/45121912-f029-4ac4-b4a4-79434e43322f)


3. Extraer passwords

```bash
docker logs id_container
```

```bash
docker exec id_container cat /var/jenkins_home/secrets/initialAdminPassword
```

![get-password-image4](https://github.com/santiagoarevalo/jenkins-lab/assets/71450411/d74f6ad9-f782-4981-b194-7f28ffc7d490)

## Iniciar Jenkins

Una vez capturada la contraseña, podemos ingresar al `localhost:8080`, ya que este es el puerto que indicamos en el manifesto yml para correr el Jenkins.
Al abrir el localhost en el navegador, obtenemos lo siguiente:

![login-jenkins](https://github.com/santiagoarevalo/jenkins-lab/assets/71450411/5aabe06b-5304-448c-8ea4-55eae35a2cba)

Aquí introducimos la contraseña que hemos capturado. Luego de ingresar con la contraseña, se nos muestra el onboarding de Jenkins para seleccionar los plugins o extensiones a instalar.

![welcome-jenkins](https://github.com/santiagoarevalo/jenkins-lab/assets/71450411/3992987b-6ae8-4920-972c-5a1306efdaf5)

Buscamos la extensión de NodeJS y la seleccionamos:

![add-nodejs-plugin](https://github.com/santiagoarevalo/jenkins-lab/assets/71450411/db412de8-a482-4cd4-9563-91b196a0867f)

Requerimos el plugin de NodeJS ya que la app del repositorio está desarrollada en Node. Una vez seleccionada, iniciamos la instalación:

![getting-started-jenkins](https://github.com/santiagoarevalo/jenkins-lab/assets/71450411/49f5d883-38f5-4e17-9fa8-4d3929c36f8f)

Finalmente nos encontramos con el panel de control de Jenkins. En este podemos administrar los proyectos de integración continua que desarrollemos.

![into-control-panel-jenkins](https://github.com/santiagoarevalo/jenkins-lab/assets/71450411/ee5d940c-1a19-4b1e-aa08-7cd79f95402d)

## Configuración Jenkins

Ahora, realizaremos algunas configuraciones para poder acceder al repositorio donde tenemos nuestra app:
1. Nos dirigimos a Administrar Jenkins
2. Seleccinamos la opción de Tools
3. Vamos hasta la parte inferior, donde se encuentran las instalaciones de NodeJS
4. Le damos un nombre a la nueva instalación y elegimos la versión específica que usa nuestra app para instalar nodejs.

![nodejs tool config](https://github.com/santiagoarevalo/jenkins-lab/assets/71450411/ce516905-8ecd-4652-8d94-946bdea7bc96)

## Creación de primera tarea

Vamos a crear nuestro primer pipeline. Para esto:
* Seleccionamos Nueva Tarea, en el panel de control
* Indicamos un nombre para nuestro pipeline
* Elegimos Crear proyecto de estilo libre

Ahora, debemos configurar la conexión con el repositorio en GitHub, el ambiente de ejecución del Jenkins y los pasos del pipeline. Para esto, realizamos las siguientes configuraciones:

### Conexión con Git

![connection git jenkins config](https://github.com/santiagoarevalo/jenkins-lab/assets/71450411/ff835b65-7dc6-417f-8bcc-6ad3668795d2)

### Configurar ambiente de ejecución

![execution environment jenkins congif](https://github.com/santiagoarevalo/jenkins-lab/assets/71450411/04fe303a-acf4-4c43-a70f-fe85df0a4f61)

### Build Steps

En este apartado, selecciamos la opción "Ejecutar línea de comandos (shell)" e ingresamos los siguientes comandos:
```
npm install
npm run build
node app.js
```

![image](https://github.com/santiagoarevalo/jenkins-lab/assets/71450411/2bb2d33d-65aa-42f4-9ae8-857e770e9347)

Luego de este, damos en crear y podemos ver visualizado nuestro primer pipeline en el panel de control:

![pipeline created in panel control](https://github.com/santiagoarevalo/jenkins-lab/assets/71450411/b8a71a65-5185-432e-a38f-24c8ae4e015c)

## Ejecución del Pipeline

Ahora, finalmente correremos el pipeline. Para esto, damos click en el botón de ejecutar. Para revisar el status de la ejecución, damos click en el apartado de "Estado del ejecutor de construcciones".
Aquí podemos tener una visualización del estado:

![pipeline visual status](https://github.com/santiagoarevalo/jenkins-lab/assets/71450411/d9ccc63a-d190-48de-a148-58ce781cb6db)

Sin embargo, como queremos abrir la app en el navegador, accedemos a Console Output:

![pipeline console output status](https://github.com/santiagoarevalo/jenkins-lab/assets/71450411/cc6b1812-5ac0-417b-8c42-7828c111969b)

Aquí podemos ver que la aplicación se ha ejecutado exitosamente. Damos click al enlace del localhost y podemos, finalmente, ver nuestra app corriendo mediante el pipeline creado.

![nodejs app running](https://github.com/santiagoarevalo/jenkins-lab/assets/71450411/1b3db835-64f0-4496-b8b3-91990f2e5174)






