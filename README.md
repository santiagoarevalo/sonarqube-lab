# SonarQube Lab

## Crear Docker Compose

Para esta práctica, en la cual realizaremos un primer análisis de código con SonarQube, creamos un Docker Compose que contenga las siguientes instrucciones:

```
version: '2'

services:
  sonarqube:
    image: sonarqube
    ports:
      - "9000:9000"
    networks:
      - sonarnet
    environment:
      - SONARQUBE_JDBC_URL=jdbc:postgresql://db:5432/sonar
      - SONARQUBE_JDBC_USERNAME=sonar
      - SONARQUBE_JDBC_PASSWORD=sonar
    volumes:
      - sonarqube_conf:/opt/sonarqube/conf
      - sonarqube_data:/opt/sonarqube/data
      - sonarqube_extensions:/opt/sonarqube/extensions
      - sonarqube_bundled-plugins:/opt/sonarqube/lib/bundled-plugins

  db:
    image: postgres
    networks:
      - sonarnet
    environment:
      - POSTGRES_USER=sonar
      - POSTGRES_PASSWORD=sonar
    volumes:
      - postgresql:/var/lib/postgresql
      - postgresql_data:/var/lib/postgresql/data

networks:
  sonarnet:
    driver: bridge

volumes:
  sonarqube_conf:
  sonarqube_data:
  sonarqube_extensions:
  sonarqube_bundled-plugins:
  postgresql:
  postgresql_data:

```

## Ejecutar Docker Compose

Para ejecutar Docker Compose, asegúrate de que tengas Docker Compose instalado en tu sistema. Luego, sigue estos pasos:

1. Navega a la ubicación donde se encuentra tu archivo `docker-compose.yml`.

2. Ejecuta el siguiente comando en tu terminal para iniciar los contenedores definidos en el archivo `docker-compose.yml`:

```bash
docker-compose up -d
```
![image](https://github.com/santiagoarevalo/sonarqube-lab/assets/71450411/290a92ca-0b6c-4564-9444-86d956ff9221)

## Dentro de SonarQube
Ingresamos a la UI de SonarQube por medio de la URL `localhost:9000`, las credenciales de autenticación por defecto son Login: `admin` y Password: `admin`
Luego de esto:
- Cambiamos la contraseña por defecto
- Creamos un proyecto local, indicando un nombre, un project key y el nombre de la rama principal.
- Elegimos configuración global (por defecto)
- Elegimos el método de análisis local para el proyecto
- Generamos el token de autenticación para realizar los análisis de código del proyecto
- Seleccionamos el sistema operativo donde ejecutaremos el Sonar-Scanner para realizar la instalación de su CLI

### Instalación de Sonar-Scanner
Ahora, debemos instalar el Sonar-Scanner para realizar el análisis sobre nuestro código. Para esto hay varias formas, dependiendo el sistema operativo de la máquina en la cual esté el proyecto y SonarQube.
Para correr SonarScanner CLI mediante un archivo .zip, seguiremos los siguiente pasos:
1. Extreaemos el archivo .zip descargado en un directorio de nuestra elección.
2. Actualizamos la configuración global para que apunte a nuestro SonarQube editando ${directorio_elegido}/conf/sonar-scanner.properties:
3. Agregamos la ruta del directorio <directorio_elegido>/bin al PATH de variables de entorno.
4. Verificamos la instalación abriendo una terminal y ejectuando el comando `sonar-scanner -h`, o `sonar-scanner.bat -h` en Windows. Deberíamos obtener una salida como la siguiente:
```
usage: sonar-scanner [options]

Options:
-D,--define <arg>     Define property
-h,--help             Display help information
-v,--version          Display version information
-X,--debug            Produce execution debug output
```
5. Ahora, corremos el siguiente comando ubicados desde la raíz del proyecto para lanzar el análisis y le pasamos nuestro token de autenticación:
`sonar-scanner -Dsonar.token=myAuthenticationToken`

_Nota: Puedes consultar la documentación de SonarQube para realizar la instalación del Sonar Scanner por medio de otros métodos: [Instalación de Sonar Scanner] (https://docs.sonarsource.com/sonarqube/latest/analyzing-source-code/scanners/sonarscanner/)_ 

### Análisis del Sonar Scanner

A partir de este momento, el Sonar Scanner comienza a realizar un análisis del código fuente. Podremos ver los logs de este análisis en la terminal, y cuando haya terminado podremos acceder a dos vistas del análisis (JSON y en SonarQube)

![WhatsApp Image 2024-05-14 at 9 23 34 PM](https://github.com/santiagoarevalo/sonarqube-lab/assets/71450411/386e40f2-0fc8-48eb-8114-2f0723b0b78f)

### Resultado del análisis en JSON:
![WhatsApp Image 2024-05-14 at 9 25 23 PM](https://github.com/santiagoarevalo/sonarqube-lab/assets/71450411/4dfe8e95-1a59-4f9d-beb5-fa7b43b3aa64)

### Resultado del análisis en SonarQube:
![WhatsApp Image 2024-05-14 at 9 24 30 PM](https://github.com/santiagoarevalo/sonarqube-lab/assets/71450411/b5efadda-fa92-43f2-a0ab-689e8dd2439f)

![WhatsApp Image 2024-05-14 at 9 25 01 PM](https://github.com/santiagoarevalo/sonarqube-lab/assets/71450411/95f35342-9fda-47ef-9e56-b8bf2006bd1c)

By
[**Santiago Arévalo Valencia**](https://github.com/santiagoarevalo) 👨🏽‍💻









