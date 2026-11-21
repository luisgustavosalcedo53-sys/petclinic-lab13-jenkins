# Laboratorio 13 de Integración Continua

Proyecto basado en [jgomezz/petclinic_integration_test](https://github.com/jgomezz/petclinic_integration_test).

El proyecto requiere Java 21. Jenkins debe tener las herramientas `Java` y `Maven` configuradas. La tarea usa la rama `main` y el archivo `Jenkinsfile` de la raíz.

## Pipeline

Las etapas son Checkout, Build, Test y Package. Las pruebas usan el perfil H2 para mantener el pipeline reproducible. Jenkins publica los XML de JUnit y archiva el JAR de una ejecución correcta. El webhook de GitHub dispara una construcción al recibir un evento push firmado.

## Ejecución con H2

```sh
mvn -B -ntp clean test -Dspring.profiles.active=h2
mvn -B -ntp package -DskipTests
```

## Ejecución con MySQL

Importar los archivos `data/schema-mysql.sql` y `data/data-mysql.sql` únicamente en una base de datos de laboratorio. El archivo de esquema recrea `PETCLINIC_DB`.

Definir `DB_USERNAME` y `DB_PASSWORD` en el entorno. Opcionalmente definir `DB_URL`. El perfil mysql usa TLS obligatorio y no ejecuta scripts de inicialización automáticamente.

```sh
mvn -B -ntp clean test -Dspring.profiles.active=mysql
```

La batería de 27 pruebas fue comprobada con H2 y con MySQL 8 en EC2. Las pruebas CRUD modifican los datos de laboratorio. No ejecutar contra una base de producción.

## Evidencias

Los registros de instalación, los XML de pruebas, las capturas y el informe de ejecución se conservan fuera del repositorio. Las claves privadas, las credenciales y el secreto del webhook no se publican.
