# B-N01 #
B-N01 es una maquina con ubuntu server que recrea la base de datos del sistema, cuenta con el servicio de MySQL debido a ventajas como:

·  Es codigo abierto con licencia GPL

·  Cuenta con soporte en cualquier idioma de programación (PHP,Python,Node.js...)

·  Buen rendimiento con servicios web

·  Permite restingir permisos por IP

Como este proyecto consta con máquinas con servicios web o clientes es especialmente útil poder contar con alguna de las ventajas mencionadas anteriormente.


## Jerarquía de permisos ##
Dentro de B-N01 existen usuarios en MySQL con diferentes tipos de permisos dependiendo de la funcion que tengan o queramos que tengan, como por ejemplo los siguientes:

### admin_db ###
Usuario local que administra toda la base de datos sin la necesidad de acceder como usuario root, admin_db consta con absolutamente todos los permisos que MySQL pueda otorgar, permisos como la creación de usuarios, la edición/creación de base de datos, tablas o columnas, la capacidad de eliminar estos últimos y permisos de lectura por todo el servicio MySQL.

### app_user ###
Usuario hecho específicamente para la máquina W-N01, la intención de este usuario es que la web pueda consultar la base de datos pero que la máquina de la web también pueda acceder a la base de datos si conoce el usuario y la contraseña para poder modificar información en caso de ser necesario. app_user consta con los siguientes privilegios: lectura, insertar/modificar y eliminar tablas dentro de la base de datos de barcelona_educacio siempre que se acceda desde la máquina W-N01, ya que el acceso está restringido por IP.

### lectura ###
Usuario para las máquinas clientes de la estructura (CLI-LNX-N01 y CLI-WIN-N01)
