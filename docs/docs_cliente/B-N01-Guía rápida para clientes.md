# Guía rápida para clientes: base de datos barcelona\_educacio

Servidor de base de datos B-N01 · Proyecto P0.0-ASIXc2C-G01

## 1. Qué es este servicio

B-N01 ofrece una base de datos MySQL con el **listado de equipamientos educativos de Barcelona** (datos abiertos del Ajuntament de Barcelona, Open Data BCN). Esta guía explica cómo conectarte y consultar los datos.

## 2. Qué necesitas

- **Una cuenta** (usuario y contraseña) que te facilita el administrador.
- **Que tu equipo esté autorizado.** Cada cuenta solo funciona desde una IP concreta. Si te conectas desde otra, recibirás `Access denied`. Si cambias de equipo o de red, avísalo al administrador.
- **Conexión de red** con el servidor, sin bloqueos de firewall.
- **Un cliente MySQL**: la terminal, una herramienta gráfica o el driver de tu aplicación.

## 3. Datos de conexión

| Parámetro | Valor |
| --- | --- |
| Servidor | 192.168.1.2 |
| Puerto | 3306 |
| Base de datos | barcelona\_educacio |
| Usuario | El que te asigne el administrador |
| Contraseña | La que te facilite el administrador |
| Codificación | utf8mb4 |

No uses `localhost`: la base de datos está en otra máquina.

## 4. Cómo conectarte

### 4.1 Desde la terminal

Primero comprueba que llegas al servidor:

```bash
nc -zv -w 3 192.168.1.2 3306
```

Si responde `succeeded`, conéctate. Escribe `-p` sin contraseña y el programa te la pedirá:

```bash
mysql -u TU_USUARIO -p -h 192.168.1.2 barcelona_educacio
```

Si te falta el cliente en Ubuntu o Debian: `sudo apt install mysql-client`. Para salir, escribe `exit`.

### 4.2 Desde una herramienta gráfica

En DBeaver, MySQL Workbench o similares, crea una conexión MySQL con los datos de la sección 3.

## 5. Qué datos hay

La tabla se llama **`equipamientos`** y tiene unas 2.700 filas, una por centro o equipamiento educativo. Las columnas más útiles son:

| Columna | Contenido |
| --- | --- |
| name | Nombre del centro |
| addresses\_road\_name | Calle |
| addresses\_start\_street\_number | Número |
| addresses\_zip\_code | Código postal |
| addresses\_district\_name | Distrito |
| addresses\_neighborhood\_name | Barrio |
| secondary\_filters\_name | Tipo de enseñanza |
| geo\_epgs\_4326\_lat y geo\_epgs\_4326\_lon | Latitud y longitud |
| timetable | Horario |

La tabla tiene 40 columnas en total. **Todas son de tipo texto**, por lo que para operar con números hay que convertirlos, por ejemplo con `CAST(geo_epgs_4326_lat AS DECIMAL(10,8))`. Algunos campos pueden venir vacíos.

## 6. Consultas de ejemplo

Primeras filas:

```sql
SELECT name, addresses_road_name, addresses_district_name
FROM equipamientos
LIMIT 10;
```

Centros de un distrito:

```sql
SELECT name, addresses_road_name
FROM equipamientos
WHERE addresses_district_name = 'Eixample'
LIMIT 20;
```

Buscar por nombre:

```sql
SELECT name, addresses_zip_code
FROM equipamientos
WHERE name LIKE '%escola%'
LIMIT 20;
```

Centros por distrito:

```sql
SELECT addresses_district_name, COUNT(*) AS total
FROM equipamientos
GROUP BY addresses_district_name
ORDER BY total DESC;
```

Usa siempre `LIMIT` en las consultas exploratorias. En una aplicación, utiliza **consultas preparadas** y nunca pegues texto del usuario directamente en el SQL.

## 7. Qué puedes hacer

Tus permisos dependen del tipo de cuenta:

| Tipo de cuenta | Permisos |
| --- | --- |
| Solo lectura | Consultar datos (`SELECT`) |
| Aplicación | Consultar, insertar, modificar y borrar datos |

Ninguna cuenta de cliente puede crear o borrar tablas ni gestionar usuarios. Si recibes `Access denied` al intentarlo, es el comportamiento esperado.&#32;

## 8. Normas de uso

- No compartas tu contraseña. Cada servicio y cada persona deben tener su propia cuenta.
- No escribas la contraseña en la línea de comandos ni en código visible o público.
- No modifiques ni borres datos, salvo que tu cuenta esté pensada para ello.
- Evita consultas pesadas sin filtro: limita los resultados.

## 9. Problemas frecuentes

| Mensaje o síntoma | Qué significa | Qué hacer |
| --- | --- | --- |
| La conexión se queda esperando y termina en timeout | Un firewall bloquea el acceso desde tu equipo | Contacta con el administrador |
| Connection refused | El servicio no está disponible | Avisa al administrador |
| Access denied for user | Usuario o contraseña incorrectos, o tu IP no está autorizada | Revisa los datos y confirma tu IP con el administrador |
| Unknown database | Nombre de la base mal escrito | Usa `barcelona_educacio` |
| Table doesn't exist | Nombre de tabla incorrecto | Usa `equipamientos` |
| Tildes o caracteres raros | Codificación distinta | Configura la conexión en `utf8mb4` |
