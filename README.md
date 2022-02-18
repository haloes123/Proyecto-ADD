
# Proyecto ADD

Creación de una API que servirá datos estadísticos sobre jugadores de la NBA.


## Creacion del proyecto

Para montar el proyecto vamos a copiar la carpeta "sf-app-aprovisioning" de add-env a una nueva carpeta y llamarla "apinba".

El servidor SQL lo mantenemos en add-env.

En el visual studio los nombres .env.webapp y docker-compose.yml en "apinba".

En ambos Dockers hacemos un:

```bash
  docker-compose up -d
```
Nos conectamos al contenedor con:

```bash
  docker-compose exec web bash
```
Y creamos el proyecto en symfony con:

```bash
  symfony new api-nba --version=4.4 --full --no-git.
```
Desde /code#:

```bash
  cd api-nba
```
Y copiamos todo el contenido en ../ excepto el docker-compose.override.yml y docker-compose.yml y hacemos:

```bash
  cd.. rm -r api-nba/
```

## Acceder a Symfony

Para acceder a Symfony:

```bash
  IP del contenedor : puerto
```
En caso de error damos permisos con:

```bash
  chmod -R 777 *
```

## Modificacion del .env de api-nba

```bash
  DB_USER=root
  DB_PASSWORD=dbrootpass
  DB_HOST=add-dbms
  DB_NAME=nba
  DATABASE_URL="mysql://${DB_USER}:${DB_PASSWORD}@${DB_HOST}:3306/${DB_NAME}?serverVersion=5.7"
```

Cargamos los datos en la base de datos:

```bash
  mysql -u root -pdbrootpass -h add-dbms < files/nba_2022-02-02.sql 
```



## Para comprobarlo

Para comprobar que funciona hacemos:

```bash
  mysql -u root -pdbrootpass -h add-dbms
  use nba
  show tables
```


## Cargar datos

Cargamos los datos de las tablas:
```bash
  python3 jugadores.py
  python3 equipos.py
  python3 estadisticas.py
  python3 partidos.py
```

##En la maquina

Cargamos en la maquina:

```bash
  composer install
```

##Generar entities

```bash
  php bin/console doctrine:mapping:convert annotation src/Entity/ --from-database
```
Una vez creadas las entities añadimos los namespaces y creamos los getters y setters borrando la "\\" de los nombres de las tablas.

Y añadimos el:

```bash
  @ORM\Entity(repositoryClass="App\Repository\EquiposRepository")
```

## API Reference

#### Conseguir todos los equipos

```http
  /equipos

```
Captura:
![App Screenshot](https://raw.githubusercontent.com/haloes123/Proyecto-ADD/main/ADDDDDDDDDDD/Endpoint1.png)

#### Conseguir jugadores por equipo

```http
  /equipos/{nombre}
```

| Parameter  | Type     | Description                              |
| :--------- | :------- | :--------------------------------------- |
| `nombre`   | `string` | **Required**. Listará para un equipo dado su nombre, toda la información que tiene. |

Captura:
![App Screenshot](https://raw.githubusercontent.com/haloes123/Proyecto-ADD/main/ADDDDDDDDDDD/Endpoint2.png)

#### Conseguir jugadores por equipo

```http
  /equipo/jugadores
```
Captura:
![App Screenshot](https://raw.githubusercontent.com/haloes123/Proyecto-ADD/main/ADDDDDDDDDDD/Endpoint3.png)

#### Conseguir todos los jugadores de un equipo dado el nombre

```http
  /equipo/jugadores/{nombre}
```

| Parameter  | Type     | Description                              |
| :--------- | :------- | :--------------------------------------- |
| `nombre`   | `string` | **Required**. Listará para cada equipo dado su nombre, todos sus jugadores. |

Captura:
![App Screenshot](https://raw.githubusercontent.com/haloes123/Proyecto-ADD/main/ADDDDDDDDDDD/Endpoint4.png)

#### Conseguir jugadores

```http
  /jugadores
```

Captura:
![App Screenshot](https://raw.githubusercontent.com/haloes123/Proyecto-ADD/main/ADDDDDDDDDDD/Endpoint5.png)

#### Conseguir jugador dado su nombre

```http
  /jugadores/{nombre}
```

| Parameter  | Type     | Description                              |
| :--------- | :------- | :--------------------------------------- |
| `nombre`   | `string` | **Required**. Listará para un jugador dado su nombre, toda la información que tiene. |


Captura:
![App Screenshot](https://raw.githubusercontent.com/haloes123/Proyecto-ADD/main/ADDDDDDDDDDD/Endpoint6.png)

#### Conseguir fisico de un jugador dado su nombre

```http
  /jugador/fisico/{nombre}
```

| Parameter  | Type     | Description                              |
| :--------- | :------- | :--------------------------------------- |
| `nombre`   | `string` | **Required**. Listará para un jugador dado su nombre, altura en cm y peso en kg y su posición. |

Captura:
![App Screenshot](https://raw.githubusercontent.com/haloes123/Proyecto-ADD/main/ADDDDDDDDDDD/Endpoint7.png)

#### Conseguir estadisticas de jugador dado su nombre

```http
  estadisticas/jugador/{nombre}
```
| Parameter  | Type     | Description                              |
| :--------- | :------- | :--------------------------------------- |
| `nombre`   | `string` | **Required**. Listará para un jugador dado todas sus estadisticas. |

Captura:
![App Screenshot](https://raw.githubusercontent.com/haloes123/Proyecto-ADD/main/ADDDDDDDDDDD/Endpoint8.png)

#### Conseguir media estadisticas dado nombre de jugador

```http
  estadisticas/jugador/{nombre}/avg
```

| Parameter  | Type     | Description                              |
| :--------- | :------- | :--------------------------------------- |
| `nombre`   | `string` | **Required**. Listará para un jugador dado su nombre, la media de todas sus estadísticas |

Captura:
![App Screenshot](https://raw.githubusercontent.com/haloes123/Proyecto-ADD/main/ADDDDDDDDDDD/Endpoint9.png)


#### Conseguir los resultado de equipo local

```http
  partidos/resultados/local/{nombre}
```

| Parameter  | Type     | Description                              |
| :--------- | :------- | :--------------------------------------- |
| `nombre`   | `string` | **Required**. Listará para un equipo dado su nombre, todos los resultados en los que ha jugado como local |

Captura:
![App Screenshot](https://raw.githubusercontent.com/haloes123/Proyecto-ADD/main/ADDDDDDDDDDD/Endpoint10.png)

#### Conseguir los resultados de equipo visitante

```http
  partidos/resultados/visitante/{nombre}
```

| Parameter  | Type     | Description                              |
| :--------- | :------- | :--------------------------------------- |
| `nombre`   | `string` | **Required**. Listará para un equipo dado su nombre, todos los resultados en los que ha jugado como visitante |

Captura:
![App Screenshot](https://raw.githubusercontent.com/haloes123/Proyecto-ADD/main/ADDDDDDDDDDD/Endpoint11.png)

#### Conseguir la media del equipo local

```http
  partidos/resultados/media/local/{nombre}
```

| Parameter  | Type     | Description                              |
| :--------- | :------- | :--------------------------------------- |
| `nombre`   | `string` | **Required**. Listará para un equipo dado su nombre, la media de puntos recibidos como local. |

Captura:
![App Screenshot](https://raw.githubusercontent.com/haloes123/Proyecto-ADD/main/ADDDDDDDDDDD/Endpoint12.png)

#### Conseguir la media del equipo visitante

```http
  partidos/resultados/media/visitante/{nombre}
```

| Parameter  | Type     | Description                              |
| :--------- | :------- | :--------------------------------------- |
| `nombre`   | `string` | **Required**. Listará para un equipo dado su nombre, la media de puntos recibidos como visitante. |

Captura:
![App Screenshot](https://raw.githubusercontent.com/haloes123/Proyecto-ADD/main/ADDDDDDDDDDD/Endpoint13.png)
## Authors

- Harold López Espinoza

