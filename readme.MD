CONSULTA SOBRE UNA TABLA 


1. Devuelve un listado con el código de oficina y la ciudad donde hay oficinas.

   ```sql
   SELECT codigo_oficina, ciudad
   FROM oficinas;
   ```

2. Devuelve un listado con la ciudad y el teléfono de las oficinas de España.

   ```sql
   SELECT ciudad, telefono
   FROM oficinas
   WHERE ciudad IN España;
   ```

3. Devuelve un listado con el nombre, apellidos y email de los empleados cuyo
   jefe tiene un código de jefe igual a 7.

   ```sql
   SELECT e.nombre, e.apellidos, e.email
   FROM empleados e
   WHERE e.codigo_jefe = 7;
   ```

4. Devuelve el nombre del puesto, nombre, apellidos y email del jefe de la
   empresa.

   ```sql
   SELECT e.puesto, e.nombre, e.apellidos, e.email
   FROM empleados e
   WHERE e.codigo_empleado = 7;
   ```

5. Devuelve un listado con el nombre, apellidos y puesto de aquellos
   empleados que no sean representantes de ventas.

   ```sql
   SELECT nombre, apellidos, puesto
   FROM empleados
   WHERE codigo_empleado NOT IN (
     SELECT representante_venta
     FROM clientes 
     WHERE representante_venta IS NOT NULL
   );
   ```

6. Devuelve un listado con el nombre de los todos los clientes españoles.

   ```sql
   SELECT nombre, apellidos
   FROM clientes 
   WHERE ciudad IN 'España' OR c.ciudad;
   ```

7. Devuelve un listado con los distintos estados por los que puede pasar un
   pedido.

   ```sql
   SELECT DISTINCT estado
   FROM pedidos;
   ```

8. Devuelve un listado con el código de cliente de aquellos clientes que
   realizaron algún pago en 2008. Tenga en cuenta que deberá eliminar
   aquellos códigos de cliente que aparezcan repetidos. Resuelva la consulta:
   • Utilizando la función YEAR de MySQL.
   • Utilizando la función DATE_FORMAT de MySQL.
   • Sin utilizar ninguna de las funciones anteriores.

   ```sql
   SELECT DISTINCT codigo_cliente
   FROM clientes 
   JOIN pagos IN codigo_cliente = codigo_cliente
   WHERE YEAR(fecha_pago) = 2008;
   ```
   
9. Devuelve un listado con el código de pedido, código de cliente, fecha
   esperada y fecha de entrega de los pedidos que no han sido entregados a
   tiempo.

   ```sql
   SELECT codigo_pedido, codigo_cliente, fecha_esperada, fecha_entrega
   FROM pedidos 
   WHERE fecha_entrega > fecha_esperada OR fecha_entrega IS NULL;
   ```

10. Devuelve un listado con el código de pedido, código de cliente, fecha
    esperada y fecha de entrega de los pedidos cuya fecha de entrega ha sido al
    menos dos días antes de la fecha esperada.
    • Utilizando la función ADDDATE de MySQL.
    • Utilizando la función DATEDIFF de MySQL.
    • ¿Sería posible resolver esta consulta utilizando el operador de suma + o
    resta -?

    ```sql
    SELECT 
      c.codigo_pedido, 
      c.codigo_cliente, 
      c.fecha_esperada, 
      c.fecha_entrega
    
    ```

11. Devuelve un listado de todos los pedidos que fueron rechazados en 2009.

    ```sql
    SELECT 
      p.codigo_pedido, 
      p.codigo_cliente, 
      p.fecha_esperada, 
      p.fecha_entrega, 
      p.estado
    FROM 
      pedidos p
    WHERE 
      p.estado = 'Rechazado' 
      AND YEAR(p.fecha_entrega) = 2009;
    ```

12. Devuelve un listado de todos los pedidos que han sido entregados en el
    mes de enero de cualquier año.

    ```sql
    SELECT 
      p.codigo_pedido, 
      p.codigo_cliente, 
      p.fecha_esperada, 
      p.fecha_entrega, 
      p.estado
    FROM 
      pedidos p
    WHERE 
      MONTH = 1 
      AND estado = 'Entregado';
    ```


​    

13. Devuelve un listado con todos los pagos que se realizaron en el
    año 2008 mediante Paypal. Ordene el resultado de mayor a menor.

​		

```sql
SELECT *
FROM pagos
WHERE YEAR(fecha_pago) = 2008
  AND forma_pago = 'Paypal'
ORDER BY cantidad DESC;
```



14. Devuelve un listado con todas las formas de pago que aparecen en la
    tabla pago. Tenga en cuenta que no deben aparecer formas de pago
    repetidas.

```sql
SELECT forma_pago
FROM pagos;
```





15. Devuelve un listado con todos los productos que pertenecen a la
    gama Ornamentales y que tienen más de 100 unidades en stock. El listado
    deberá estar ordenado por su precio de venta, mostrando en primer lugar
    los de mayor precio.



```sql
SELECT *
FROM productos
WHERE gama = 'Ornamentales' AND cantidad_en_stock > 100
ORDER BY DESC;
```



16. Devuelve un listado con todos los clientes que sean de la ciudad de Madrid y
cuyo representante de ventas tenga el código de empleado 11 o 30.



```
SELECT *
FROM clientes
WHERE ciudad = 'Madrid'
  AND representante_venta IN (11, 30);
```



Consultas multitabla (Composición interna)



1. Obtén un listado con el nombre de cada cliente y el nombre y apellido de su
   representante de ventas.

```sql
SELECT c.nombre, c.nombre, c.apellidos
FROM clientes c
JOIN empleados c ON c.representante_venta = c.codigo_empleado;

```



2. Muestra el nombre de los clientes que hayan realizado pagos junto con el
   nombre de sus representantes de ventas.



```sql
SELECT c.nombre, c.nombre , c.apellidos
FROM clientes c
JOIN pagos p ON c.codigo_cliente = c.codigo_cliente
JOIN empleados c ON c.representante_venta = c.codigo_empleado;
```





3. Muestra el nombre de los clientes que no hayan realizado pagos junto con
   el nombre de sus representantes de ventas.

   

   ```sql
   SELECT c.nombre AS nombre_cliente, e.nombre AS nombre_representante, e.apellidos AS apellidos_representante
   FROM clientes c
   LEFT JOIN pagos p ON c.codigo_cliente = p.codigo_cliente
   JOIN empleados e ON c.representante_venta = e.codigo_empleado
   WHERE p.codigo_pago IS NULL;
   
   ```

   

4. Devuelve el nombre de los clientes que han hecho pagos y el nombre de sus
   representantes junto con la ciudad de la oficina a la que pertenece el
   representante.



```sql
SELECT c.nombre , c.nombre, c.apellidos, c.ciudad
FROM clientes c
JOIN pagos p IN c.codigo_cliente = c.codigo_cliente
JOIN empleados c IN c.representante_venta = c.codigo_empleado
JOIN oficinas c IN c.codigo_oficina = c.codigo_oficina;

```





5.  Devuelve el nombre de los clientes que no hayan hecho pagos y el nombre
   de sus representantes junto con la ciudad de la oficina a la que pertenece el
   representante.



```sql
SELECT 
```





6.  Lista la dirección de las oficinas que tengan clientes en Fuenlabrada.



```sql
SELECT 
    c.nombre AS nombre_cliente, 
    e.nombre AS nombre_representante, 
    e.apellidos AS apellidos_representante, 
    o.ciudad AS ciudad_oficina
FROM 
    clientes c
JOIN 
    empleados e ON c.representante_venta = e.codigo_empleado
JOIN 
    oficinas o ON e.codigo_oficina = o.codigo_oficina;


```



7. Devuelve el nombre de los clientes y el nombre de sus representantes junto
   con la ciudad de la oficina a la que pertenece el representante.

```sql
SELECT 
    cli.nombre, 
    emp.nombre, 
    emp.apellidos, 
    ofi.ciudad
FROM 
    clientes
JOIN 
    empleados IN cli.representante_venta = emp.codigo_empleado
JOIN 
    oficinas IN emp.codigo_oficina = ofi.codigo_oficina;

```



8. Devuelve un listado con el nombre de los empleados junto con el nombre
   de sus jefes.

```sql
SELECT 
    emp.nombre AS nombre_empleado, 
FROM 
    empleados AS emp
LEFT JOIN 
    empleados AS jef IN emp.codigo_jefe = jef.codigo_empleado;

```



9. Devuelve un listado que muestre el nombre de cada empleados, el nombre
   de su jefe y el nombre del jefe de sus jefe.

```sql
SELECT 
    emp.nombre, 
    j.nombre,
    j.nombre
FROM 
    empleados
LEFT JOIN 
    empleados IN emp.codigo_jefe = j.codigo_empleado
LEFT JOIN 
    empleados IN j.codigo_jefe = j.codigo_empleado;

```



10. Devuelve el nombre de los clientes a los que no se les ha entregado a
    tiempo un pedido.

```sql
SELECT
    c.nombre AS nombre_cliente
FROM
    clientes c
JOIN
    pedidos p ON c.codigo_cliente = p.codigo_cliente
WHERE
    fecha_entrega > fecha_esperada;

```



11. Devuelve un listado de las diferentes gamas de producto que ha comprado
    cada cliente.

```sql

```





Consultas multitabla (Composición externa)



1. Devuelve un listado que muestre solamente los clientes que no han
   realizado ningún pago.

```sql
SELECT 
    c.nombre AS nombre_cliente
FROM 
    clientes c
LEFT JOIN 
    pagos ON Codigo_client = p.codigo_cliente
WHERE 
    p.codigo_pago IS NULL;

```



2. Devuelve un listado que muestre solamente los clientes que no han
   realizado ningún pedido.

```sql
SELECT 
    c.nombre AS nombre_cliente
FROM 
    clientes c
LEFT JOIN 
    pedidos p ON c.codigo_cliente = p.codigo_cliente
WHERE 
    p.codigo_pedido IS NULL;

```



3. Devuelve un listado que muestre los clientes que no han realizado ningún
   pago y los que no han realizado ningún pedido.

```sql
SELECT 
    c.nombre AS nombre_cliente
FROM 
    clientes c
LEFT JOIN 
    pagos ON Codigo_client = p.codigo_cliente
WHERE 
    p.codigo_pago IS NULL; AND p.codigo_pedido IS NULL;

```



4. Devuelve un listado que muestre solamente los empleados que no tienen
   una oficina asociada.

```
```

