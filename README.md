## Consultas sobre una tabla

1. Devuelve un listado con el código de oficina y la ciudad donde hay oficinas.

   ```sql
   SELECT o.id_oficina, c.nombre_ciudad
   
   FROM oficina AS o
   
   INNER JOIN ciudad AS c ON o.id_ciudad = c.id_ciudad;
   
   +------------+---------------+
   | id_oficina | nombre_ciudad |
   +------------+---------------+
   | OFC1       | Madrid        |
   | OFC2       | Madrid        |
   | OFC3       | Barcelona     |
   +------------+---------------+
   ```

   

2. Devuelve un listado con la ciudad y el teléfono de las oficinas de España.

   ```sql
   SELECT c.nombre_ciudad, o.telefono
   
   FROM ciudad c
   
   JOIN oficina o ON c.id_ciudad = o.id_ciudad
   
   WHERE o.nombre_pais = 'España';
   
   +---------------+-----------+
   | nombre_ciudad | telefono  |
   +---------------+-----------+
   | Madrid        | 123456789 |
   | Madrid        | 987654321 |
   +---------------+-----------+
   ```

   

3. Devuelve un listado con el nombre, apellidos y email de los empleados cuyo
    jefe tiene un código de jefe igual a 7.

  ```sql
  SELECT nombre, apellido1, apellido2, email
  
  FROM empleado
  
  WHERE id_jefe = 7;
  
  +--------+-----------+-----------+--------------------------+
  | nombre | apellido1 | apellido2 | email                    |
  +--------+-----------+-----------+--------------------------+
  | Juan   | García    | López     | juan.garcia@email.com    |
  | María  | Martínez  | Sánchez   | maria.martinez@email.com |
  | Pedro  | Pérez     | González  | pedro.perez@email.com    |
  +--------+-----------+-----------+--------------------------+
  ```

  

4. Devuelve el nombre del puesto, nombre, apellidos y email del jefe de la
    empresa.

  ```sql
  SELECT p.nombre_puesto, j.nombre, j.apellido1, j.apellido2, j.email
  
  FROM jefe j
  
  JOIN puesto p ON j.id_puesto = p.id_puesto;
  
  +-------------------+---------+------------+------------+------------------------+
  | nombre_puesto     | nombre  | apellido1  | apellido2  | email                  |
  +-------------------+---------+------------+------------+------------------------+
  | Gerente           | Juan    | García     | López      | juan.garcia@email.com  |
  +-------------------+---------+------------+------------+------------------------+
  
  ```

  

5. Devuelve un listado con el nombre, apellidos y puesto de aquellos
    empleados que no sean representantes de ventas.

  ```sql
  SELECT nombre, apellido1, apellido2, puesto
  
  FROM empleado
  
  WHERE puesto != 'Representante de Ventas';
  
  +--------+-----------+------------+----------+
  | nombre | apellido1 | apellido2  | puesto   |
  +--------+-----------+------------+----------+
  | Juan   | García    | López      | Gerente  |
  | María  | Martínez  | Sánchez    | Gerente  |
  | Pedro  | Pérez     | González   | Gerente  |
  | Ana    | Díaz      | Fernández  | Empleado |
  | Carlos | Ruiz      | Jiménez    | Empleado |
  +--------+-----------+------------+----------+
  ```

  

6. Devuelve un listado con el nombre de los todos los clientes españoles.

   ```sql
   SELECT c.nombre_cliente
   
   FROM cliente AS c
   
   INNER JOIN ciudad AS ci ON c.id_ciudad = ci.id_ciudad
   
   INNER JOIN pais AS p ON ci.id_pais = p.id_pais
   
   WHERE p.nombre_pais = 'España';
   
   +----------------------+
   | nombre_cliente       |
   +----------------------+
   | Joseph               |
   | Yonia                |
   +----------------------+
   
   ```

   

7. Devuelve un listado con los distintos estados por los que puede pasar un
    pedido.

  ```sql
  SELECT DISTINCT estado_pedido
  
  FROM pedido;
  
  +---------------+
  | estado_pedido |
  +---------------+
  | Pendiente     |
  | En proceso    |
  | En espera     |
  | Completado    |
  | Cancelado     |
  +---------------+
  ```

  

8. Devuelve un listado con el código de cliente de aquellos clientes que
    realizaron algún pago en 2008. Tenga en cuenta que deberá eliminar
    aquellos códigos de cliente que aparezcan repetidos. Resuelva la consulta:
    • Utilizando la función YEAR de MySQL.
    • Utilizando la función DATE_FORMAT de MySQL.
    • Sin utilizar ninguna de las funciones anteriores.

  ```sql
  SELECT DISTINCT p.id_cliente
  
  FROM pago p
  
  WHERE p.fecha_pago >= '2008-01-01' AND p.fecha_pago <= '2008-12-31';
  
  +------------+
  | id_cliente |
  +------------+
  |          1 |
  |          2 |
  +------------+
  ```

  

9. Devuelve un listado con el código de pedido, código de cliente, fecha
    esperada y fecha de entrega de los pedidos que no han sido entregados a
    tiempo.

  ```sql
  SELECT id_pedido, id_cliente, fecha_esperada, fecha_entrega
  
  FROM pedido
  
  WHERE estado_pedido = 'Entregado tarde'
  
  +-----------+------------+----------------+---------------+
  | id_pedido | id_cliente | fecha_esperada | fecha_entrega |
  +-----------+------------+----------------+---------------+
  |         2 |          2 | NULL           | NULL          |
  +-----------+------------+----------------+---------------+
  ```

  

10. Devuelve un listado con el código de pedido, código de cliente, fecha
    esperada y fecha de entrega de los pedidos cuya fecha de entrega ha sido al
    menos dos días antes de la fecha esperada.
    • Utilizando la función ADDDATE de MySQL.
    • Utilizando la función DATEDIFF de MySQL.
    • ¿Sería posible resolver esta consulta utilizando el operador de suma + o
    resta -?

    ```sql
    SELECT id_pedido, id_cliente, fecha_esperada, fecha_entrega
    
    FROM pedido
    
    WHERE fecha_entrega IS NOT NULL AND DATEDIFF(fecha_esperada, fecha_entrega) >= 2;
    
    +-----------+------------+---------------------+---------------------+
    | id_pedido | id_cliente | fecha_esperada      | fecha_entrega       |
    +-----------+------------+---------------------+---------------------+
    | 1         | 1          | 2008-01-01          | 2008-01-15          |
    +-----------+------------+---------------------+---------------------+
    ```

    

11. Devuelve un listado de todos los pedidos que fueron rechazados en 2009.

    ```sql
    SELECT id_pedido, fecha_pedido, estado_pedido, id_cliente, metodo_pago, id_pago
    
    FROM pedido
    
    WHERE estado_pedido = 'Cancelado' AND YEAR(fecha_pedido) = 2009;
    +-----------+--------------+---------------+------------+---------------------+---------+
    | id_pedido | fecha_pedido | estado_pedido | id_cliente | metodo_pago         | id_pago |
    +-----------+--------------+---------------+------------+---------------------+---------+
    |         4 | 2009-04-01   | Cancelado     |          1 | Tarjeta de crédito  |       4 |
    +-----------+--------------+---------------+------------+---------------------+---------+
    ```

    

12. Devuelve un listado de todos los pedidos que han sido entregados en el
    mes de enero de cualquier año.

    ```sql
    SELECT id_pedido, fecha_pedido, fecha_esperada, fecha_entrega,
    
    id_cliente, metodo_pago, id_pago, estado_pedido
    
    FROM pedido
    
    WHERE MONTH(fecha_entrega) = 1 AND estado_pedido = 'Entregado';
    
    +-----------+--------------+----------------+---------------+------------+--------------+--------+---------------+
    | id_pedido | fecha_pedido | fecha_esperada | fecha_entrega | id_cliente | metodo_pago  | id_pago| estado_pedido |
    +-----------+--------------+----------------+---------------+------------+--------------+--------+---------------+
    | 1         | 2008-01-01   | 2008-01-01     | 2008-01-15    | 1          | Tarjeta de crédito | 1 | Entregado    |
    +-----------+--------------+----------------+---------------+------------+--------------+--------+---------------+
    
    
    ```

    

13. Devuelve un listado con todos los pagos que se realizaron en el
    año 2008 mediante Paypal. Ordene el resultado de mayor a menor.

    ```sql
    SELECT id_pago, id_cliente, id_forma_pago, fecha_pago, total
    
    FROM pago
    
    WHERE YEAR(fecha_pago) = 2008 AND id_forma_pago = 3
    
    ORDER BY total DESC;
    
    +---------+------------+----------------+------------+---------+
    | id_pago | id_cliente | id_forma_pago  | fecha_pago | total   |
    +---------+------------+----------------+------------+---------+
    | 1       | 1          | 3              | 2008-01-01 | 1000.00 |
    | 2       | 2          | 3              | 2008-02-20 | 1500.00 |
    +---------+------------+----------------+------------+---------+
    
    ```

    

14. Devuelve un listado con todas las formas de pago que aparecen en la
    tabla pago. Tenga en cuenta que no deben aparecer formas de pago
    repetidas.

    ```sql
    SELECT DISTINCT id_forma_pago, nombre_forma_pago
    
    FROM pago
    
    JOIN forma_pago ON pago.id_forma_pago = forma_pago.id_forma_de_pago;
    
    +---------------+------------------------+
    | id_forma_pago | nombre_forma_pago      |
    +---------------+------------------------+
    |             1 | Tarjeta de crédito     |
    |             2 | Transferencia bancaria |
    |             3 | Paypal                 |
    +---------------+------------------------+
    ```

    

15. Devuelve un listado con todos los productos que pertenecen a la
    gama Ornamentales y que tienen más de 100 unidades en stock. El listado
    deberá estar ordenado por su precio de venta, mostrando en primer lugar
    los de mayor precio.

    ```sql
    SELECT id_producto, nombre, gama, cantidad_en_stock, precio_venta
    
    FROM producto
    
    WHERE gama = 'Ornamentales' AND cantidad_en_stock > 100
    
    ORDER BY precio_venta DESC;
    
    +-------------+-------------+--------------+--------------------+--------------+
    | id_producto | nombre      | gama         | cantidad_en_stock  | precio_venta |
    +-------------+-------------+--------------+--------------------+--------------+
    | PROD1       | Producto 1  | Ornamentales | 100                | 50.00        |
    +-------------+-------------+--------------+--------------------+--------------+
    
    ```

    

16. Devuelve un listado con todos los clientes que sean de la ciudad de Madrid y
    cuyo representante de ventas tenga el código de empleado 11 o 30.

```sql
SELECT c.id_cliente, c.nombre_cliente, c.nombre_contacto, c.cliente_apellido1,

c.cliente_apellido2, c.telefono, c.linea_direccion1

FROM cliente c

JOIN empleado e ON c.id_empleado_rep_ventas = e.id_empleado

JOIN ciudad ci ON c.id_ciudad = ci.id_ciudad

WHERE ci.nombre_ciudad = 'Madrid' AND (e.id_empleado = 11 OR e.id_empleado = 30);

+------------+----------------+-----------------+-------------------+-------------------+-----------+-------------------+
| id_cliente | nombre_cliente | nombre_contacto | cliente_apellido1 | cliente_apellido2 | telefono  | linea_direccion1  |
+------------+----------------+-----------------+-------------------+-------------------+-----------+-------------------+
|          1 | Empresa A      | Pedro           | González          | Fernández         | 111222333 | Calle Principal 1 |
+------------+----------------+-----------------+-------------------+-------------------+-----------+-------------------+
```



## Consultas multitabla (Composición interna)

Resuelva todas las consultas utilizando la sintaxis de SQL1 y SQL2. Las consultas con
sintaxis de SQL2 se deben resolver con INNER JOIN y NATURAL JOIN.

1. Obtén un listado con el nombre de cada cliente y el nombre y apellido de su
    representante de ventas.

  ```sql
  SELECT c.nombre_cliente, e.nombre, e.apellido1
  
  FROM cliente c
  
  NATURAL JOIN empleado e;
  
  +----------------+--------+-----------+
  | nombre_cliente | nombre | apellido1 |
  +----------------+--------+-----------+
  | Empresa C      | María  | Martínez  |
  | Empresa B      | María  | Martínez  |
  | Empresa A      | María  | Martínez  |
  | Empresa C      | Pedro  | Pérez     |
  | Empresa B      | Pedro  | Pérez     |
  | Empresa A      | Pedro  | Pérez     |
  | Empresa C      | Ana    | Díaz      |
  | Empresa B      | Ana    | Díaz      |
  | Empresa A      | Ana    | Díaz      |
  | Empresa C      | Carlos | Ruiz      |
  | Empresa B      | Carlos | Ruiz      |
  | Empresa A      | Carlos | Ruiz      |
  | Empresa C      | Juan   | García    |
  | Empresa B      | Juan   | García    |
  | Empresa A      | Juan   | García    |
  +----------------+--------+-----------+
  ```

  

2. Muestra el nombre de los clientes que hayan realizado pagos junto con el
    nombre de sus representantes de ventas.

  ```sql
  SELECT c.nombre_cliente, e.nombre, e.apellido1
  
  FROM cliente c
  
  NATURAL JOIN empleado e
  
  +----------------+--------+-----------+
  | nombre_cliente | nombre | apellido1 |
  +----------------+--------+-----------+
  | Empresa C      | María  | Martínez  |
  | Empresa B      | María  | Martínez  |
  | Empresa A      | María  | Martínez  |
  | Empresa C      | Pedro  | Pérez     |
  | Empresa B      | Pedro  | Pérez     |
  | Empresa A      | Pedro  | Pérez     |
  | Empresa C      | Ana    | Díaz      |
  | Empresa B      | Ana    | Díaz      |
  | Empresa A      | Ana    | Díaz      |
  | Empresa C      | Carlos | Ruiz      |
  | Empresa B      | Carlos | Ruiz      |
  | Empresa A      | Carlos | Ruiz      |
  | Empresa C      | Juan   | García    |
  | Empresa B      | Juan   | García    |
  | Empresa A      | Juan   | García    |
  +----------------+--------+-----------+
  ```

  

3. Muestra el nombre de los clientes que no hayan realizado pagos junto con
    el nombre de sus representantes de ventas.

  ```sql
  SELECT c.nombre_cliente, e.nombre, e.apellido1
  
  FROM cliente c
  
  INNER JOIN empleado e ON c.id_empleado_rep_ventas = e.id_empleado
  
  WHERE c.id_cliente NOT IN (SELECT id_cliente FROM pago);
  
  +----------------+--------+-----------+
  | nombre_cliente | nombre | apellido1 |
  +----------------+--------+-----------+
  | Empresa D      | Juan   | García    |
  +----------------+--------+-----------+
  ```

  

4. Devuelve el nombre de los clientes que han hecho pagos y el nombre de sus
    representantes junto con la ciudad de la oficina a la que pertenece el
    representante.

  ```sql
  SELECT c.nombre_cliente, e.nombre AS nombreRepresentante, e.apellido1 AS 
  apellidoRepresentante, o.nombre_pais, ci.nombre_ciudad
  
  FROM cliente AS c
  
  INNER JOIN empleado e ON c.id_empleado_rep_ventas = e.id_empleado
  
  INNER JOIN pago AS p ON c.id_cliente = p.id_cliente
  
  INNER JOIN oficina AS o ON e.id_oficina = o.id_oficina
  
  INNER JOIN ciudad AS ci ON o.id_ciudad = ci.id_ciudad;
  
  +----------------+----------------------+------------------------+-------------+---------------+
  | nombre_cliente | nombreRepresentante | apellidoRepresentante | nombre_pais | nombre_ciudad |
  +----------------+----------------------+------------------------+-------------+---------------+
  | Empresa A      | Juan                 | García                 | España      | Madrid        |
  | Empresa B      | María                | Martínez               | España      | Madrid        |
  | Empresa A      | Juan                 | García                 | España      | Madrid        |
  | Empresa B      | María                | Martínez               | España      | Madrid        |
  +----------------+----------------------+------------------------+-------------+---------------+
  
  ```

  

5. Devuelve el nombre de los clientes que no hayan hecho pagos y el nombre
    de sus representantes junto con la ciudad de la oficina a la que pertenece el
    representante.

  ```sql
  SELECT c.nombre_cliente, e.nombre AS nombreRepresentante, e.apellido1 
  
  AS apellidoRepresentante, o.nombre_pais, ci.nombre_ciudad
  
  FROM cliente AS c
  INNER JOIN empleado e ON c.id_empleado_rep_ventas = e.id_empleado
  
  LEFT JOIN pago AS p ON c.id_cliente = p.id_cliente
  
  INNER JOIN oficina AS o ON e.id_oficina = o.id_oficina
  
  INNER JOIN ciudad AS ci ON o.id_ciudad = ci.id_ciudad
  
  WHERE p.id_pago IS NULL;
  
  +----------------+----------------------+------------------------+-------------+---------------+
  | nombre_cliente | nombre_representante | apellido_representante | nombre_pais | nombre_ciudad |
  +----------------+----------------------+------------------------+-------------+---------------+
  | Empresa D      | Juan                 | García                 | España      | Madrid        |
  +----------------+----------------------+------------------------+-------------+---------------+
  ```

  

6. Lista la dirección de las oficinas que tengan clientes en Fuenlabrada.

   ```sql
   SELECT o.linea_direccion1, o.linea_direccion2, c.nombre_ciudad, c.codigo_postal
   
   FROM oficina AS o
   
   JOIN ciudad AS c ON o.id_ciudad = c.id_ciudad
   
   JOIN cliente AS cl ON cl.id_ciudad = c.id_ciudad
   
   WHERE c.nombre_ciudad = 'Fuenlabrada';
   
   +-------------------+------------------+---------------+---------------+
   | linea_direccion1  | linea_direccion2 | nombre_ciudad | codigo_postal |
   +-------------------+------------------+---------------+---------------+
   | Paseo Marítimo 3  | Edificio A       | Fuenlabrada   | 08001         |
   +-------------------+------------------+---------------+---------------+
   ```

   

7. Devuelve el nombre de los clientes y el nombre de sus representantes junto
    con la ciudad de la oficina a la que pertenece el representante.

  ```sql
  SELECT c.nombre_cliente, e.nombre AS nombreRepresentante, e.apellido1 AS
  
  apellidoRepresentante, o.nombre_pais AS paisOficina, ci.nombre_ciudad AS ciudadOficina
  
  FROM cliente AS c
  
  INNER JOIN empleado AS e ON c.id_empleado_rep_ventas = e.id_empleado
  
  INNER JOIN oficina AS o ON e.id_oficina = o.id_oficina
  
  INNER JOIN ciudad AS ci ON o.id_ciudad = ci.id_ciudad;
  
  +----------------+----------------------+------------------------+--------------+----------------+
  | nombre_cliente | nombre_representante | apellido_representante | pais_oficina | ciudad_oficina |
  +----------------+----------------------+------------------------+--------------+----------------+
  | Empresa A      | Juan                 | García                 | España       | Madrid         |
  | Empresa B      | María                | Martínez               | España       | Madrid         |
  | Empresa D      | Juan                 | García                 | España       | Madrid         |
  +----------------+----------------------+------------------------+--------------+----------------+
  ```

  

8. Devuelve un listado con el nombre de los empleados junto con el nombre
    de sus jefes.

  ```sql
  SELECT e.nombre AS nombreEmpleado, e.apellido1 AS apellidoEmpleado, j.nombre AS 
  
  nombreJefe, j.apellido1 AS apellidoJefe
  
  FROM empleado AS e
  
  INNER JOIN jefe j ON e.id_jefe = j.id_jefe;
  
  +-----------------+-------------------+-------------+---------------+
  | nombre_empleado | apellido_empleado | nombre_jefe | apellido_jefe |
  +-----------------+-------------------+-------------+---------------+
  | Ana             | Díaz              | Juan        | García        |
  | Carlos          | Ruiz              | Juan        | García        |
  +-----------------+-------------------+-------------+---------------+
  
  ```

  

9. Devuelve un listado que muestre el nombre de cada empleados, el nombre
    de su jefe y el nombre del jefe de sus jefe.

  ```sql
  SELECT e.nombre AS nombreEmpleado, e.apellido1 AS apellido1Empleado, e.apellido2 AS apellido2Empleado, j.nombre AS
  
  nombreJefe, j.apellido1 AS apellido1Jefe, j.apellido2 AS apellido1Jefe
  
  FROM empleado AS e
  
  INNER JOIN empleado AS j ON e.id_jefe = j.id_empleado;
  
  ```

  

10. Devuelve el nombre de los clientes a los que no se les ha entregado a
    tiempo un pedido.

    ```sql
    SELECT c.nombre_cliente
    
    FROM cliente AS c
    
    INNER JOIN pedido p ON c.id_cliente = p.id_cliente
    
    WHERE p.estado_pedido = 'Entregado tarde'
    
    GROUP BY c.nombre_cliente;
    
    +----------------+
    | nombre_cliente |
    +----------------+
    | Empresa B      |
    | Empresa A      |
    +----------------+
    ```

    

11. Devuelve un listado de las diferentes gamas de producto que ha comprado
    cada cliente.

```sql
SELECT pe.id_pedido, c.nombre_cliente, c.cliente_apellido1, c.cliente_apellido2,

p.id_producto, p.nombre AS nombreProducto, p.gama AS gamaProducto, dp.cantidad,

dp.precio_unidad, dp.numero_linea

FROM pedido pe

INNER JOIN cliente c ON pe.id_cliente = c.id_cliente

INNER JOIN detalle_pedido dp ON pe.id_pedido = dp.id_pedido

INNER JOIN producto p ON dp.id_producto = p.id_producto;


+-----------+----------------+-------------------+-------------------+-------------+-----------------+---------------+----------+---------------+--------------+
| id_pedido | nombre_cliente | cliente_apellido1 | cliente_apellido2 | id_producto | nombre_producto | gama_producto | cantidad | precio_unidad | numero_linea |
+-----------+----------------+-------------------+-------------------+-------------+-----------------+---------------+----------+---------------+--------------+
|         1 | Empresa A      | González          | Fernández         | PROD1       | Producto 1      | Ornamentales  |        5 |            50 |            1 |
|         2 | Empresa B      | Sánchez           | García            | PROD2       | Producto 2      | Gama 2        |        3 |            70 |            1 |
|         3 | Empresa C      | González          | Martínez          | PROD3       | Producto 3      | Gama 1        |        2 |            60 |            1 |
|         4 | Empresa A      | González          | Fernández         | PROD1       | Producto 1      | Ornamentales  |        7 |            50 |            1 |
|         6 | Empresa B      | Sánchez           | García            | PROD3       | Producto 3      | Gama 1        |        6 |            60 |            1 |
|         7 | Empresa A      | González          | Fernández         | PROD1       | Producto 1      | Ornamentales  |       10 |            50 |            1 |
+-----------+----------------+-------------------+-------------------+-------------+-----------------+---------------+----------+---------------+--------------+
```



## Consultas multitabla (Composición externa)

Resuelva todas las consultas utilizando las cláusulas LEFT JOIN, RIGHT JOIN, NATURAL
LEFT JOIN y NATURAL RIGHT JOIN.

1. Devuelve un listado que muestre solamente los clientes que no han
    realizado ningún pago.

  ```sql
  SELECT c.id_cliente, c.nombre_cliente, c.cliente_apellido1, c.cliente_apellido2
  
  FROM cliente AS c
  
  LEFT JOIN pago AS p ON c.id_cliente = p.id_cliente
  
  WHERE p.id_cliente IS NULL;
  +------------+----------------+-------------------+-------------------+
  | id_cliente | nombre_cliente | cliente_apellido1 | cliente_apellido2 |
  +------------+----------------+-------------------+-------------------+
  |          4 | JOE            | González          | Balboa            |
  +------------+----------------+-------------------+-------------------+
  ```

  

2. Devuelve un listado que muestre solamente los clientes que no han
    realizado ningún pedido.

  ```sql
  SELECT c.id_cliente, c.nombre_cliente, c.cliente_apellido1, c.cliente_apellido2
  
  FROM cliente c
  
  LEFT JOIN pedido pe ON c.id_cliente = pe.id_cliente
  
  WHERE pe.id_pedido IS NULL;
  
  +------------+----------------+-------------------+-------------------+
  | id_cliente | nombre_cliente | cliente_apellido1 | cliente_apellido2 |
  +------------+----------------+-------------------+-------------------+
  |          4 | JOE            | González          |     Balboa        |
  +------------+----------------+-------------------+-------------------+
  ```

  

3. Devuelve un listado que muestre los clientes que no han realizado ningún
    pago y los que no han realizado ningún pedido.

  ```sql
  SELECT c.id_cliente, c.nombre_cliente, c.cliente_apellido1, c.cliente_apellido2
  
  FROM cliente AS c
  
  NATURAL LEFT JOIN pago AS p
  
  NATURAL LEFT JOIN pedido AS pe
  
  WHERE p.id_pago IS NULL AND pe.id_pedido IS NULL;
  
  +------------+----------------+-------------------+-------------------+
  | id_cliente | nombre_cliente | cliente_apellido1 | cliente_apellido2 |
  +------------+----------------+-------------------+-------------------+
  |          4 | JOE            | González          | Balboa            |
  +------------+----------------+-------------------+-------------------+
  ```

  

4. Devuelve un listado que muestre solamente los empleados que no tienen
    una oficina asociada.

  ```sql
  SELECT e.id_empleado, e.nombre, e.apellido1, e.apellido2
  
  FROM empleado e
  
  LEFT JOIN oficina o ON e.id_oficina = o.id_oficina
  
  WHERE o.id_oficina IS NULL;
  
  +-------------+----------------+-------------------+-------------------+
  | id_empleado | nombre         | apellido1         | apellido2         |
  +-------------+----------------+-------------------+-------------------+
  |           5 | Carlos         | Ruiz              | Jiménez           |
  +-------------+----------------+-------------------+-------------------+
  ```

  

5. Devuelve un listado que muestre solamente los empleados que no tienen un
    cliente asociado.

  ```sql
  SELECT e.id_empleado, e.nombre, e.apellido1, e.apellido2
  
  FROM empleado e
  
  LEFT JOIN cliente c ON e.id_empleado = c.id_empleado_rep_ventas
  
  WHERE c.id_cliente IS NULL;
  
  +-------------+--------+-----------+------------+
  | id_empleado | nombre | apellido1 | apellido2  |
  +-------------+--------+-----------+------------+
  |           3 | Pedro  | Pérez     | González   |
  |           4 | Ana    | Díaz      | Fernández  |
  |           5 | Carlos | Ruiz      | Jiménez    |
  +-------------+--------+-----------+------------+
  ```

  

6. Devuelve un listado que muestre solamente los empleados que no tienen un
    cliente asociado junto con los datos de la oficina donde trabajan.

  ```sql
  SELECT e.id_empleado, e.nombre, e.apellido1, e.apellido2, o.id_oficina, o.nombre_pais,
  
  o.telefono, o.linea_direccion1, o.linea_direccion2
  
  FROM empleado AS e
  
  LEFT JOIN cliente AS c ON e.id_empleado = c.id_empleado_rep_ventas
  
  LEFT JOIN oficina AS o ON e.id_oficina = o.id_oficina
  
  WHERE c.id_cliente IS NULL;
  
  +-------------+--------+-----------+------------+------------+-------------+-----------+-------------------+------------------+
  | id_empleado | nombre | apellido1 | apellido2  | id_oficina | nombre_pais | telefono  | linea_direccion1  | linea_direccion2 |
  +-------------+--------+-----------+------------+------------+-------------+-----------+-------------------+------------------+
  |           3 | Pedro  | Pérez     | González   | OFC3       | Francia     | 456789123 | Paseo Marítimo 3  | Edificio A       |
  |           4 | Ana    | Díaz      | Fernández  | OFC1       | España      | 123456789 | Calle Principal 1 | Planta 1         |
  |           5 | Carlos | Ruiz      | Jiménez    | OFC2       | España      | 987654321 | Avenida Central 2 | NULL             |
  +-------------+--------+-----------+------------+------------+-------------+-----------+-------------------+----------------+
  ```

  

7. Devuelve un listado que muestre los empleados que no tienen una oficina
    asociada y los que no tienen un cliente asociado.

  ```sql
  SELECT e.id_empleado, e.nombre, e.apellido1, e.apellido2
  
  FROM cliente c
  
  RIGHT JOIN empleado e ON c.id_empleado_rep_ventas = e.id_empleado
  
  WHERE c.id_cliente IS NULL;
  
  +-------------+--------+-----------+------------+
  | id_empleado | nombre | apellido1 | apellido2  |
  +-------------+--------+-----------+------------+
  |           3 | Pedro  | Pérez     | González   |
  |           4 | Ana    | Díaz      | Fernández  |
  |           5 | Carlos | Ruiz      | Jiménez    |
  +-------------+--------+-----------+------------+
  ```

  

8. Devuelve un listado de los productos que nunca han aparecido en un
    pedido.

  ```sql
  SELECT p.id_producto, p.nombre AS nombreProducto, p.gama AS gamaProducto
  
  FROM producto p
  
  LEFT JOIN detalle_pedido dp ON p.id_producto = dp.id_producto
  
  +-------------+-----------------+---------------+
  | id_producto | nombre_producto | gama_producto |
  +-------------+-----------------+---------------+
  | PROD4       | Producto 4      | Gama 3        |
  +-------------+-----------------+---------------+
  ```

  

9. Devuelve un listado de los productos que nunca han aparecido en un
    pedido. El resultado debe mostrar el nombre, la descripción y la imagen del
    producto.

  ```sql
  SELECT p.nombre AS nombre_producto, p.descripcion, gp.imagen
  
  FROM producto AS p
  
  LEFT JOIN detalle_pedido AS dp ON p.id_producto = dp.id_producto
  
  LEFT JOIN gama_producto AS gp ON p.gama = gp.gama
  
  WHERE  dp.id_detalle_pedido IS NULL;
  
  +-----------------+-----------------------------+--------+
  | nombre_producto | descripcion                 | imagen |
  +-----------------+-----------------------------+--------+
  | Producto 4      | Descripción del producto    | NULL   |
  +-----------------+-----------------------------+--------+
  ```

  

10. Devuelve las oficinas donde no trabajan ninguno de los empleados que
    hayan sido los representantes de ventas de algún cliente que haya realizado
    la compra de algún producto de la gama Frutales.

    ```sql
    SELECT DISTINCT c.id_cliente, c.nombre_cliente, c.nombre_contacto, 
    
    c.cliente_apellido1, c.cliente_apellido2, c.telefono, c.linea_direccion1, 
    
    c.id_ciudad, c.id_empleado_rep_ventas
    FROM cliente c
    
    NATURAL LEFT JOIN pedido p
    
    NATURAL LEFT JOIN detalle_pedido dp
    
    NATURAL LEFT JOIN producto pr
    
    NATURAL LEFT JOIN gama_producto gp
    
    WHERE gp.gama = 'Frutales';
    
    +------------+----------------+-----------------+-------------------+-------------------+-----------+-------------------+-----------+------------------------+
    | id_cliente | nombre_cliente | nombre_contacto  | cliente_apellido1 | cliente_apellido2 | telefono  | linea_direccion1  | id_ciudad | id_empleado_rep_ventas |
    +------------+----------------+-----------------+-------------------+-------------------+-----------+-------------------+-----------+------------------------+
    |          2 | Empresa B      | María           | Sánchez           | García            | 444555666 | Avenida Central 2 |         1 |                      2 |
    +------------+----------------+-----------------+-------------------+-------------------+-----------+-------------------+-----------+------------------------+
    
    
    ```

    

11. Devuelve un listado con los clientes que han realizado algún pedido pero no
    han realizado ningún pago.

    ```sql
    SELECT c.id_cliente, c.nombre_cliente, c.nombre_contacto, c.cliente_apellido1, c.cliente_apellido2, c.telefono, c.linea_direccion1,
    
    c.id_ciudad, c.id_empleado_rep_ventas
    
    FROM cliente AS c
    
    INNER JOIN pedido AS  p ON c.id_cliente = p.id_cliente
    
    LEFT JOIN pago AS pg ON p.id_pago = pg.id_pago
    
    WHERE pg.id_pago IS NULL;
    
    +------------+----------------+-----------------+-------------------+-------------------+-----------+-------------------+-----------+------------------------+
    | id_cliente | nombre_cliente | nombre_contacto | cliente_apellido1 | cliente_apellido2 | telefono  | linea_direccion1  | id_ciudad | id_empleado_rep_ventas |
    +------------+----------------+-----------------+-------------------+-------------------+-----------+-------------------+-----------+------------------------+
    |          1 | Empresa A      | Pedro           | González          | Fernández         | 111222333 | Calle Principal 1 |         1 |                     11 |
    +------------+----------------+-----------------+-------------------+-------------------+-----------+-------------------+-----------+------------------------+
    ```

    

12. Devuelve un listado con los datos de los empleados que no tienen clientes
    asociados y el nombre de su jefe asociado.

```sql
SELECT e.id_empleado, e.nombre, e.apellido1, e.apellido2, e.id_jefe, j.nombre AS nombreJefe, j.apellido1 AS apellido1Jefe, j.apellido2 AS
apellido2Jefe

FROM empleado e

LEFT JOIN cliente c ON e.id_empleado = c.id_empleado_rep_ventas

INNER JOIN jefe j ON e.id_jefe = j.id_jefe

WHERE c.id_cliente IS NULL;

+-------------+--------+-----------+------------+---------+------------+---------------+---------------+
| id_empleado | nombre | apellido1 | apellido2  | id_jefe | nombreJefe | apellido1Jefe | apellido2Jefe |
+-------------+--------+-----------+------------+---------+------------+---------------+---------------+
|           4 | Ana    | Díaz      | Fernández  |       1 | Juan       | García        | López         |
+-------------+--------+-----------+------------+---------+------------+---------------+---------------+
```

## Consultas resumen

1. ¿Cuántos empleados hay en la compañía?

   ```sql
   SELECT COUNT(*) AS total_empleados
   FROM empleado;
   
   +-----------------+
   | total_empleados |
   +-----------------+
   |               5 |
   +-----------------+
   ```

   

2. ¿Cuántos clientes tiene cada país?

   ```sql
   SELECT p.nombre_pais, COUNT(c.id_cliente) AS totalClientes
   
   FROM cliente AS c
   
   INNER JOIN ciudad AS ci ON c.id_ciudad = ci.id_ciudad
   
   INNER JOIN pais AS p ON ci.id_pais = p.id_pais
   
   GROUP BY p.nombre_pais;
   
   +-------------+---------------+
   | nombre_pais | totalClientes |
   +-------------+---------------+
   | España      |             2 |
   | Francia     |             1 |
   | Italia      |             1 |
   +-------------+---------------+
   ```

   

3. ¿Cuál fue el pago medio en 2009?

   ```sql
   SELECT AVG(total) AS pagoPromedio2009
   
   FROM pago
   
   WHERE YEAR(fecha_pago) = 2009;
   +--------------------+
   | Pago_Promedio_2009 |
   +--------------------+
   |        2000.000000 |
   +--------------------+
   ```

   

4. ¿Cuántos pedidos hay en cada estado? Ordena el resultado de forma
    descendente por el número de pedidos.

  ```sql
  SELECT estado_pedido, COUNT(*) AS cantidad_pedidos
  
  FROM pedido
  
  GROUP BY estado_pedido
  
  ORDER BY cantidad_pedidos DESC;
  
  +-----------------+------------------+
  | estado_pedido   | cantidad_pedidos |
  +-----------------+------------------+
  | Entregado tarde |                2 |
  | Cancelado       |                2 |
  | Entregado       |                1 |
  | En espera       |                1 |
  | En proceso      |                1 |
  +-----------------+------------------+
  ```

  

5. Calcula el precio de venta del producto más caro y más barato en una
    misma consulta.

  ```sql
  SELECT MAX(precio_venta) AS precioCaro, MIN(precio_venta) AS precioBarato
  
  FROM producto;
  +-----------------+-------------------+
  | precioCaro      | precioBarato      |
  +-----------------+-------------------+
  |           80.00 |             50.00 |
  +-----------------+-------------------+
  ```

  

6. Calcula el número de clientes que tiene la empresa.

   ```sql
   SELECT COUNT(id_cliente) AS numeroClientes
   
   FROM cliente;
   
   +-----------------+
   | numeroClientes  |
   +-----------------+
   |               4 |
   +-----------------+
   
   ```

   

7. ¿Cuántos clientes existen con domicilio en la ciudad de Madrid?

   ```sql
   SELECT COUNT(c.id_cliente) AS clientesMadrid
   
   FROM cliente AS c
   
   INNER JOIN ciudad AS ci ON c.id_ciudad = ci.id_ciudad
   
   WHERE ci.nombre_ciudad = 'Madrid';
   
   
   +--------------------+
   | clientesMadrid     |
   +--------------------+
   |                  2 |
   +--------------------+
   ```

   

8. ¿Calcula cuántos clientes tiene cada una de las ciudades que empiezan
    por M?

  ```sql
  SELECT ci.nombre_ciudad, COUNT(c.id_cliente) AS clientesCiudad
  
  FROM cliente AS c
  
  INNER JOIN ciudad AS ci ON c.id_ciudad = ci.id_ciudad
  
  WHERE ci.nombre_ciudad LIKE 'M%'
  
  GROUP BY ci.nombre_ciudad;
  
  +---------------+----------------+
  | nombre_ciudad | clientesCiudad |
  +---------------+----------------+
  | Madrid        |              2 |
  +---------------+----------------+
  ```

  

9. Devuelve el nombre de los representantes de ventas y el número de clientes
    al que atiende cada uno.

  ```sql
  SELECT e.nombre AS nombreRepresentante, e.apellido1 AS apellidoRepresentante, COUNT(c.id_cliente) AS clientes
  
  FROM empleado e
  
  LEFT JOIN cliente c ON e.id_empleado = c.id_empleado_rep_ventas
  
  WHERE e.puesto = 'Gerente'
  
  GROUP BY e.nombre, e.apellido1;
  
  +---------------------+-----------------------+----------+
  | nombreRepresentante | apellidoRepresentante | clientes |
  +---------------------+-----------------------+----------+
  | María               | Martínez              |        1 |
  | Pedro               | Pérez                 |        0 |
  | Juan                | García                |        2 |
  +---------------------+-----------------------+----------+
  
  ```

  

10. Calcula el número de clientes que no tiene asignado representante de
    ventas.

    ```sql
    SELECT COUNT(c.id_cliente) AS clientesSinRepresentante
    
    FROM cliente c
    
    LEFT JOIN empleado e ON c.id_empleado_rep_ventas = e.id_empleado
    
    WHERE c.id_empleado_rep_ventas IS NULL;
    
    +--------------------------+
    | clientesSinRepresentante |
    +--------------------------+
    |                        0 |
    +--------------------------+
    
    ```

    

11. Calcula la fecha del primer y último pago realizado por cada uno de los
    clientes. El listado deberá mostrar el nombre y los apellidos de cada cliente.

    ```sql
    SELECT c.nombre_cliente, c.nombre_contacto, c.cliente_apellido1, c.cliente_apellido2, MIN(pa.fecha_pago) AS primera_fecha_pago, MAX(pa.fecha_pago) AS ultima_fecha_pago
    FROM cliente AS c
    LEFT JOIN pago AS pa ON c.id_cliente = pa.id_cliente
    GROUP BY c.id_cliente;
    
    +----------------+-----------------+-------------------+-------------------+--------------------+-------------------+
    | nombre_cliente | nombre_contacto | cliente_apellido1 | cliente_apellido2 | primera_fecha_pago | ultima_fecha_pago |
    +----------------+-----------------+-------------------+-------------------+--------------------+-------------------+
    | Empresa A      | Pedro           | González          | Fernández         | 2007-04-05         | 2008-01-01        |
    | Empresa B      | María           | Sánchez           | García            | 2008-02-20         | 2010-05-12        |
    | Empresa C      | Juan            | González          | Martínez          | 2009-03-10         | 2009-03-10        |
    | Empresa D      | Pedro           | González          | Balboa            | NULL               | NULL              |
    +----------------+-----------------+-------------------+-------------------+--------------------+-------------------+
    ```

    

12. Calcula el número de productos diferentes que hay en cada uno de los
    pedidos.

    ```sql
    SELECT id_pedido, COUNT(DISTINCT id_producto) AS productos
    
    FROM detalle_pedido
    
    GROUP BY id_pedido;
    
    +-----------+----------------------+
    | id_pedido | productos            |
    +-----------+----------------------+
    |         1 |                    1 |
    |         2 |                    1 |
    |         3 |                    1 |
    |         4 |                    1 |
    |         5 |                    1 |
    |         6 |                    1 |
    |         7 |                    1 |
    +-----------+----------------------+
    ```

    

13. Calcula la suma de la cantidad total de todos los productos que aparecen en
    cada uno de los pedidos.

    ```sql
    SELECT id_pedido, SUM(cantidad) AS cantidad_total
    
    FROM detalle_pedido
    
    GROUP BY id_pedido;
    
    +-----------+----------------+
    | id_pedido | cantidad_total |
    +-----------+----------------+
    |         1 |              5 |
    |         2 |              3 |
    |         3 |              2 |
    |         4 |              7 |
    |         5 |              4 |
    |         6 |              6 |
    |         7 |             10 |
    +-----------+----------------+
    ```

    

14. Devuelve un listado de los 20 productos más vendidos y el número total de
    unidades que se han vendido de cada uno. El listado deberá estar ordenado
    por el número total de unidades vendidas.

    ```sql
    SELECT dp.id_producto, p.nombre AS nombreProducto, SUM(dp.cantidad) AS totalUnidadesVendidas
    
    FROM detalle_pedido AS dp
    
    JOIN producto AS p ON dp.id_producto = p.id_producto
    
    GROUP BY dp.id_producto
    
    ORDER BY totalUnidadesVendidas DESC
    
    LIMIT 20;
    
    +-------------+----------------+-----------------------+
    | id_producto | nombreProducto | totalUnidadesVendidas |
    +-------------+----------------+-----------------------+
    | PROD1       | Producto 1     |                    63 |
    | PROD2       | Producto 2     |                    39 |
    | PROD3       | Producto 3     |                    38 |
    +-------------+----------------+-----------------------+
    ```

    

15. La facturación que ha tenido la empresa en toda la historia, indicando la
    base imponible, el IVA y el total facturado. La base imponible se calcula
    sumando el coste del producto por el número de unidades vendidas de la
    tabla detalle_pedido. El IVA es el 21 % de la base imponible, y el total la
    suma de los dos campos anteriores.

    ```sql
    SELECT SUM(dp.cantidad * p.precio_venta) AS base_imponible, SUM(dp.cantidad * p.precio_venta) * 0.21 AS iva, SUM(dp.cantidad * p.precio_venta) * 1.21 AS total_facturado
        
    FROM detalle_pedido AS dp
    
    JOIN producto AS p ON dp.id_producto = p.id_producto;
    
    +----------------+-----------+-----------------+
    | base_imponible | iva       | total_facturado |
    +----------------+-----------+-----------------+
    |        8160.00 | 1713.6000 |       9873.6000 |
    +----------------+-----------+-----------------+
    ```

    

16. La misma información que en la pregunta anterior, pero agrupada por
    código de producto.

    ```sql
    SELECT dp.id_producto,SUM(dp.cantidad * p.precio_venta) AS base_imponible, SUM(dp.cantidad * p.precio_venta) * 0.21 AS iva, SUM(dp.cantidad * p.precio_venta) * 1.21 AS total_facturado
        
    FROM detalle_pedido AS dp
    
    JOIN producto AS p ON dp.id_producto = p.id_producto
    
    GROUP BY dp.id_producto;
    
    +-------------+----------------+----------+-----------------+
    | id_producto | base_imponible | iva      | total_facturado |
    +-------------+----------------+----------+-----------------+
    | PROD1       |        3150.00 | 661.5000 |       3811.5000 |
    | PROD2       |        2730.00 | 573.3000 |       3303.3000 |
    | PROD3       |        2280.00 | 478.8000 |       2758.8000 |
    +-------------+----------------+----------+-----------------+
    ```

    

17. La misma información que en la pregunta anterior, pero agrupada por
    código de producto filtrada por los códigos que empiecen por OR.

    ```sql
    SELECT dp.id_producto,SUM(dp.cantidad * p.precio_venta) AS base_imponible, SUM(dp.cantidad * p.precio_venta) * 0.21 AS iva, SUM(dp.cantidad * p.precio_venta) * 1.21 AS total_facturado
        
    FROM detalle_pedido AS dp
    
    JOIN producto AS p ON dp.id_producto = p.id_producto
    
    WHERE p.gama LIKE 'OR%'
        
    GROUP BY dp.id_producto;
    
    +-------------+----------------+----------+-----------------+
    | id_producto | base_imponible | iva      | total_facturado |
    +-------------+----------------+----------+-----------------+
    | PROD1       |        3150.00 | 661.5000 |       3811.5000 |
    +-------------+----------------+----------+-----------------+
    ```

    

18. Lista las ventas totales de los productos que hayan facturado más de 3000
    euros. Se mostrará el nombre, unidades vendidas, total facturado y total
    facturado con impuestos (21% IVA).

    ```sql
    SELECT p.nombre AS nombreProducto, SUM(dp.cantidad) AS unidadesVendidas, 
    SUM(dp.cantidad * p.precio_venta) AS totalFacturado, 
    SUM(dp.cantidad * p.precio_venta) * 1.21 AS total_con_iva
    
    FROM detalle_pedido dp
    
    JOIN producto p ON dp.id_producto = p.id_producto
    
    GROUP BY p.nombre
    
    HAVING total_facturado > 3000;
    
    +-----------------+-------------------+-----------------+---------------+
    | nombreProducto  | unidadesVendidas  | totaFacturado   | total_con_iva |
    +-----------------+-------------------+-----------------+---------------+
    | Producto 1      |                63 |         3150.00 |     3811.5000 |
    +-----------------+-------------------+-----------------+---------------+
    ```

    

19. Muestre la suma total de todos los pagos que se realizaron para cada uno
    de los años que aparecen en la tabla pagos.

    ```sql
    SELECT YEAR(fecha_pago) AS año, SUM(total) AS totalPagos
    
    FROM pago
    
    GROUP BY YEAR(fecha_pago)
    
    ORDER BY año;
    
    +------+------------+
    | año  | totalPagos |
    +------+------------+
    | 2007 |    1200.00 |
    | 2008 |    2500.00 |
    | 2009 |    2000.00 |
    | 2010 |    1800.00 |
    +------+------------+
    ```

    ## Consultas variadas

    1. Devuelve el listado de clientes indicando el nombre del cliente y cuántos
        pedidos ha realizado. Tenga en cuenta que pueden existir clientes que no
        han realizado ningún pedido.

      ```sql
      SELECT c.nombre_cliente AS nombre_cliente, COUNT(p.id_pedido) AS cantidad_pedidos
      
      FROM cliente c
      
      LEFT JOIN pedido p ON c.id_cliente = p.id_cliente
      
      GROUP BY c.id_cliente, c.nombre_cliente
      
      ORDER BY cantidad_pedidos DESC;
      
      +----------------+------------------+
      | nombre_cliente | cantidad_pedidos |
      +----------------+------------------+
      | Yonia          |                3 |
      | Joseph         |                2 |
      | York           |                1 |
      | Miles          |                0 |
      +----------------+------------------+
      ```

      

    2. Devuelve un listado con los nombres de los clientes y el total pagado por
        cada uno de ellos. Tenga en cuenta que pueden existir clientes que no han
        realizado ningún pago.

      ```sql
      SELECT c.nombre_cliente AS nombreCliente, COALESCE(SUM(pa.total), 0) AS totalPagado
      
      FROM cliente c
      
      LEFT JOIN pago pa ON c.id_cliente = pa.id_cliente
      
      GROUP BY  c.id_cliente, c.nombre_cliente
      
      ORDER BY total_pagado DESC;
      
      +----------------+--------------+
      | nombreCliente | totalPagado |
      +----------------+--------------+
      | Yonia        |      3300.00  |
      | Joseph       |      2200.00  |
      | York         |      2000.00  |
      | Miles        |         0.00  |
      +----------------+--------------+
      ```

      

    3. Devuelve el nombre de los clientes que hayan hecho pedidos en 2008
        ordenados alfabéticamente de menor a mayor.

      ```sql
      +----------------+--------------+
      | nombre_cliente | total_pagado |
      +----------------+--------------+
      | Yonia          |      3300.00 |
      | Joseph         |      2200.00 |
      | York           |      2000.00 |
      | Miles          |         0.00 |
      +----------------+--------------+
      ```

      

    4. Devuelve el nombre del cliente, el nombre y primer apellido de su
        representante de ventas y el número de teléfono de la oficina del representante de ventas, de aquellos clientes que no hayan realizado ningún pago.

      ```sql
      SELECT c.nombre_cliente, e.nombre, e.apellido1, e.apellido2, of.telefono
      
      FROM cliente AS c
      
      INNER JOIN empleado AS e ON c.id_empleado_rep_ventas = e.id_empleado
      
      INNER JOIN oficina AS of ON e.id_oficina = of.id_oficina
      
      WHERE c.id_cliente NOT IN (SELECT DISTINCT id_cliente FROM pago);
      
      +---------------+---------+-----------+-----------+--------------+
      | nombre_cliente| nombre  | apellido1 | apellido2 | telefono     |
      +---------------+---------+-----------+-----------+--------------+
      | Joseph        | Juan    | García    | López     | 123456789    |
      | Miles         | Pedro   | Pérez     | González  | 123456789    |
      +---------------+---------+-----------+-----------+--------------+
      
      ```
    
      
    
    5. Devuelve el listado de clientes donde aparezca el nombre del cliente, el
        nombre y primer apellido de su representante de ventas y la ciudad donde
        está su oficina.
    
      ```sql
      SELECT c.nombre_cliente AS nombreCliente, CONCAT(e.nombre, e.apellido1) AS nombre_repVentas, ci.nombre_ciudad AS ciudadOficina
      
      FROM cliente AS c
      
      LEFT JOIN empleado AS e ON c.id_empleado_rep_ventas = e.id_empleado
      
      LEFT JOIN oficina AS ofc ON e.id_oficina = ofc.id_oficina
      
      LEFT JOIN ciudad AS ci ON ofcid_ciudad = ci.id_ciudad;
      
      +---------------+------------------+---------------+
      | nombreCliente | nombre_repVentas | ciudadOficina |
      +---------------+------------------+---------------+
      | Joseph        | JuanGarcía       | Madrid        |
      | Yonia         | MaríaMartínez    | Madrid        |
      | York          | NULL             | NULL          |
      | Miles         | JuanGarcía       | Madrid        |
      +---------------+------------------+---------------+
      
      ```

      
    
    6. Devuelve el nombre, apellidos, puesto y teléfono de la oficina de aquellos
        empleados que no sean representante de ventas de ningún cliente.
    
      ```sql
      SELECT e.nombre, e.apellido1, e.apellido2, e.puesto, of.telefono
      
      FROM empleado AS e
      
      INNER JOIN oficina AS of ON e.id_oficina = of.id_oficina
      
      WHERE e.id_empleado NOT IN (SELECT DISTINCT id_empleado_rep_ventas FROM cliente);
      
      +--------+-----------+-----------+-----------+-------------+
      | nombre | apellido1 | apellido2 | puesto    | telefono    |
      +--------+-----------+-----------+-----------+-------------+
      | Ana    | Díaz      | Fernández | Empleado  | 123456789   |
      | Carlos | Ruiz      | Jiménez   | Empleado  | 987654321   |
      +--------+-----------+-----------+-----------+-------------+
      
      ```
    
      
    
    7. Devuelve un listado indicando todas las ciudades donde hay oficinas y el
        número de empleados que tiene.
    
      ```sql
      SELECT c.nombre_ciudad AS ciudad, COUNT(e.id_empleado) AS numero_empleados
      
      FROM ciudad AS c
      
      INNER JOIN oficina AS o ON c.id_ciudad = o.id_ciudad
      
      INNER JOIN empleado AS e ON o.id_oficina = e.id_oficina
      
      GROUP BY c.nombre_ciudad
      
      ORDER BY numero_empleados DESC;
      
      +-------------+------------------+
      | ciudad      | numero_empleados |
      +-------------+------------------+
      | Madrid      |                4 |
      | Fuenlabrada |                1 |
      +-------------+------------------+
      ```
    
      