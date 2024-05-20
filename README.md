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
Para correr SonarScanner CLI mediante un archivo .zip, sigue los siguiente pasos:







