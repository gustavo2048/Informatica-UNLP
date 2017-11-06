# Práctica 4 - MySQL

### 1. Crear usuarios para las bases de datos, usando los nombres reparacion para la versión normalizada y reparacion_dn para la desnormalizada. Habiendo creado estos usuarios, evitar el uso de root para el resto del trabajo práctico.

```
CREATE USER 'reparacion'@'localhost' IDENTIFIED BY 'pass';
GRANT ALL PRIVILEGES ON reparacion.* TO 'reparacion'@'localhost';

CREATE USER 'reparacion_dn'@'localhost' IDENTIFIED BY 'pass';
GRANT ALL PRIVILEGES ON reparacion_dn.* TO 'reparacion_dn'@'localhost';

FLUSH PRIVILEGES;
```

#### 1.1 Adicionalmente, en ambas bases:

* **cree un usuario sólo con permisos para realizar consultas de selección, es decir que no puedan realizar cambios en la base. Use los nombres 'reparacion_select’ y 'reparacion_dn_select’.** 

```
CREATE USER 'reparacion_select'@'localhost' IDENTIFIED BY 'pass';
GRANT SELECT ON reparacion.* TO 'reparacion_select'@'localhost';

CREATE USER 'reparacion_dn_select'@'localhost' IDENTIFIED BY 'pass';
GRANT SELECT ON reparacion_dn.* TO 'reparacion_dn_select'@'localhost';

FLUSH PRIVILEGES;
```

* **cree un usuario que pueda realizar consultas de selección, actualización y eliminación a nivel de filas, pero que no puedan modificar el esquema. Use los nombres 'reparacion_update’ y 'reparacion_dn_update’.**

```
CREATE USER 'reparacion_update'@'localhost' IDENTIFIED BY 'pass';
GRANT SELECT, DELETE, UPDATE ON reparacion.* TO 'reparacion_update'@'localhost';

CREATE USER 'reparacion_dn_update'@'localhost' IDENTIFIED BY 'pass';
GRANT SELECT, DELETE, UPDATE ON reparacion_dn.* TO 'reparacion_dn_update'@'localhost';

FLUSH PRIVILEGES;
```

* **cree un usuario que tenga los permisos de los anteriores, pero que además pueda modificar el esquema de la base de datos. Use los nombres 'reparacion_schema’ y 'reparacion_dn_schema’.**

```
CREATE USER 'reparacion_schema'@'localhost' IDENTIFIED BY 'pass';
GRANT SELECT, DELETE, UPDATE, CREATE, DROP, INSERT, ALTER ON reparacion.* TO 'reparacion_schema'@'localhost';

CREATE USER 'reparacion_dn_schema'@'localhost' IDENTIFIED BY 'pass';
GRANT SELECT, DELETE, UPDATE, CREATE, DROP, INSERT, ALTER ON reparacion_dn.* TO 'reparacion_dn_schema'@'localhost';

FLUSH PRIVILEGES;
```

---

### 2. Listar dni, nombre y apellido de todos los clientes ordenados por dni en forma ascendente. Realice la consulta en ambas bases. ¿Qué diferencia nota en cuanto a performance? ¿Arrojan los mismos resultados? ¿Qué puede concluir en base a las diferencias halladas?

```
# reparacion
SELECT dniCliente, nombreApellidoCliente FROM cliente ORDER BY dniCliente ASC;
# 20000 rows in set
# Time: 0.985s

# reparacion_dn
SELECT dniCliente, nombreApellidoCliente FROM reparacion ORDER BY dniCliente ASC;
# 162252 rows in set
# Time: 8.350s
```

Como se puede ver los resultados no son los mismos, la consulta hecha sobre `reparacion_dn` da mas resultados ya que se recupera a un mismo usuario las veces que este realizó una reparación. Obviamente como hay mas tuplas la performance será mas baja.

Si sobre `reparacion_db` realizamos una consulta que quite los repetidos obtenemos los siguientes resultados:

```
# reparacion_dn
SELECT dniCliente, nombreApellidoCliente FROM reparacion GROUP BY dniCliente ORDER BY dniCliente ASC;
# 162252 rows in set
# Time: 0.995s
```

Como podemos observar estos últimos resultados se asemejan mucho a los realizados sobre la tabla `reparacion`.

---

### 3. Hallar aquellos clientes que para todas sus reparaciones siempre hayan usado su tarjeta de crédito primaria (nunca la tarjeta secundaria). Realice la consulta en ambas bases.

```
# reparacion
SELECT dniCliente FROM cliente WHERE NOT EXISTS (SELECT * FROM reparacion WHERE cliente.dniCliente = reparacion.dniCliente AND tarjetaSecundaria = tarjetaReparacion);

# reparacion_dn
SELECT dniCliente FROM reparacion as r WHERE NOT EXISTS(SELECT * FROM reparacion WHERE r.dniCliente = reparacion.dniCliente and r.tarjetaSecundaria = reparacion.tarjetaReparacion)
```

---


