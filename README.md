# BaseDeDatos

Bustamante Mathias Andrés 4ºB

DDL – EJERCITACIÓN

1___________________________________________________________________________________________________________

CREATE VIEW V_PROVEEDORES_PRODUCTOS
AS
    SELECT numero, pnro
    FROM productos, proveedores
    WHERE productos.localidad != proveedores.localidad;

2___________________________________________________________________________________________________________

ALTER TABLE productos ADD IMPORTADOR VARCHAR(30);

3___________________________________________________________________________________________________________

CREATE VIEW PROVEEDORES_WILDE
AS
    SELECT *
    FROM proveedores
    WHERE proveedores.localidad = 'wilde';

4___________________________________________________________________________________________________________

DEPARTAMENTOS-EMPLEADOS

CREATE TABLE DEPARTAMENTOS(
cod-dept INT NOT NULL, 
descr VARCHAR(20) NOT NULL, 
CONSTRAINT pk_cod-dept PRIMARY KEY (cod-dept)
);

CREATE TABLE EMPLEADOS(
cod-emp INT, 
nombre VARCHAR(20) NOT NULL, 
edad INT NOT NULL, 
cod-dept INT,
CONSTRAINT pk_cod-emp PRIMARY KEY (cod-emp),
CONSTRAINT fk_cod-dept FOREIGN KEY (cod-dept) 
REFERENCES DEPARTAMENTOS (cod-dept)
);

PACIENTES-MEDICAMENTOS

CREATE TABLE PACIENTES(
cod-pac INT, 
nombre VARCHAR(20) NOT NULL, 
edad INT NOT NULL, 
CONSTRAINT pk_cod-pac PRIMARY KEY (cod-pac)
);

CREATE TABLE MEDICAMENTOS(
cod-med INT,
descr VARCHAR(20) NOT NULL, 
cod-pac INT NOT NULL,
CONSTRAINT pk_cod-med PRIMARY KEY (cod-med),
CONSTRAINT fk_ cod-pac FOREIGN KEY (cod-pac) REFERENCES PACIENTES (cod-pac)
);


DML – EJERCITACIÓN

1___________________________________________________________________________________________________________

CREATE TABLE PROVEEDORES(
numero INT NOT NULL,
nombre VARCHAR(20) NOT NULL,
domicilio VARCHAR(15) NOT NULL,
localidad VARCHAR(12),
CONSTRAINT pk_numero PRIMARY KEY (numero)
);

CREATE TABLE PRODUCTOS(
pnro INT NOT NULL,
pnombre VARCHAR(20) NOT NULL,
precio INT NOT NULL,
tamaño VARCHAR(20),
localidad VARCHAR(12) NOT NULL,
CONSTRAINT pk_pnro PRIMARY KEY (pnro)
);

CREATE TABLE PROV_PROD (
numero INT NOT NULL,
pnro INT NOT NULL,
cantidad INT NOT NULL, 
CONSTRAINT pk_prod-prov PRIMARY KEY (numero, pnro),
CONSTRAINT fk_numero FOREIGN KEY (numero) REFERENCES PROVEEDORES (numero),
CONSTRAINT fk_pnro FOREIGN KEY (pnro) REFERENCES PRODUCTOS (pnro)
);

1.1_________________________________________________________________________________________________________

SELECT *
FROM PRODUCTOS; 

1.2_________________________________________________________________________________________________________

SELECT *
FROM PROVEEDORES
WHERE localidad = 'capital';

1.3_________________________________________________________________________________________________________

SELECT *
FROM PROV_PROD
WHERE cantidad >= 200 AND cantidad <= 300; 

1.4_________________________________________________________________________________________________________

SELECT DISTINCT pnro
FROM PROV_PROD INNER JOIN PROVEEDORES 
WHERE PROV_PROD.numero = PROVEEDORES.numero AND PROVEEDORES.localidad = 'avellaneda';

1.5_________________________________________________________________________________________________________

SELECT SUM(cantidad)
FROM PROV_PROD
WHERE pnro = 001 AND numero = 103;

1.6_________________________________________________________________________________________________________

SELECT pnro, localidad
FROM PRODUCTOS
WHERE localidad LIKE '_A%';

1.7_________________________________________________________________________________________________________

SELECT DISTINCT precio
FROM PRODUCTOS INNER JOIN PROV_PROD 
WHERE PRODUCTOS.PNRO = PROV_PROD.PNRO AND PROV_PROD.numero = 102;

1.8_________________________________________________________________________________________________________

    SELECT productos.LOCALIDAD
    FROM productos
UNION
    SELECT proveedores.LOCALIDAD
    FROM proveedores; 

1.9_________________________________________________________________________________________________________

UPDATE PRODUCTOS 
SET TAMAÑO = 'chico' 
WHERE TAMAÑO = 'mediano'

1.10________________________________________________________________________________________________________

DELETE FROM PRODUCTOS 
WHERE PNRO NOT IN (SELECT DISTINCT PNRO FROM PROV-PROD);

1.11________________________________________________________________________________________________________

INSERT INTO PROVEEDORES
VALUES (107, 'rosales', '', 'wilde'); 

2.1_________________________________________________________________________________________________________

SELECT MAX (cantidad)
FROM ITEM_VENTAS;

2.2_________________________________________________________________________________________________________

SELECT SUM (cantidad)
FROM ITEM_VENTAS
WHERE codigo_producto = 'c';

2.3_________________________________________________________________________________________________________

SELECT SUM(ITEM_VENTA.cantidad) as total, codigo_producto, nombre_producto
FROM ITEM_VENTAS, PRODUCTOS
WHERE PRODUCTOS.codigo_producto = ITEM_VENTAS.codigo_producto
GROUP BY PRODUCTO.nom_prod
ORDER BY SUM(ITEM_VENTA.cantidad) DESC;

2.4_________________________________________________________________________________________________________

SELECT p.nombre_producto, SUM(ITEM_VENTA.cantidad) as CANTIDAD
FROM item_venta, productos as p
WHERE ITEM_VENTA.codigo_producto = p.codigo_producto
GROUP BY p.nombre_producto
HAVING (SUM(ITEM_VENTA.cantidad))
ORDER BY (p.nombre_producto) DESC;

2.5_________________________________________________________________________________________________________

SELECT v.cod-cliente, c.nombre_cliente, COUNT(v.numero_factura) as COMPRA
FROM CLIENTE as c, VENTAS as v
WHERE v.cod-cliente = c.cod-cliente
GROUP BY (v.cod-cliente, c.nombre-cliente) 
ORDER BY COUNT (v.numero-factura) DESC

2.6_________________________________________________________________________________________________________

SELECT iv.cod-prod, AVG(iv.cantidad) AS promedio
FROM VENTAS as v, ITEM_VENTAS as iv
WHERE iv.num-factura = v.num-factura AND v.cod-cliente = 1
GROUP BY iv.cod-prod


CONSULTAS SIMPLES


3.1_________________________________________________________________________________________________________

SELECT DISTINCT OFICINAS.descripcion
FROM OFICINAS;

3.2_________________________________________________________________________________________________________

SELECT DISTINCT PRODUCTOS.descripcion, PRODUCTOS.precio (PRODUCTOS.PRECIO * 1.21) as IVA
FROM PRODUCTOS;

3.3_________________________________________________________________________________________________________

SELECT EMPLEADOS.apellido, EMPLEADOS.nombre, EMPLEADOS.fecha_nacimiento, YEAR(GETDATE())–YEAR(FECHA_NACIMIENTO) as EDAD 
FROM EMPLEADOS;

3.4_________________________________________________________________________________________________________

SELECT *
FROM EMPLEADOS
WHERE EMPLEADOS.cod_jefe != null;

3.5_________________________________________________________________________________________________________

SELECT *
FROM EMPLEADOS
WHERE EMPLEADOS.nombre = 'maria'
ORDER BY 
EMPLEADOS.apellido ASC;

3.6_________________________________________________________________________________________________________

SELECT *
FROM CLIENTES
WHERE LIKE 'L%'
ORDER BY CLIENTES.cod_cliente;

3.7_________________________________________________________________________________________________________

SELECT *
FROM PEDIDOS
WHERE LIKE '__/03/____' 
ORDER BY PEDIDOS.fecha_pedido;

3.8_________________________________________________________________________________________________________

SELECT *
FROM OFICINAS
WHERE OFICINAS.cod_director is null;

3.9_________________________________________________________________________________________________________

SELECT TOP 4 DESC, precio_costo
FROM PRODUCTOS
ORDER BY ASC;

3.10________________________________________________________________________________________________________

SELECT TOP 3 DATOS_CONTRATO.cod_empleado
FROM DATOS_CONTRATOS
ORDER BY DATOS_CONTRATO.CUOTA DESC;


CONSULTA MULTITABLA – EJERCITACIÓN 


1___________________________________________________________________________________________________________

SELECT PRODUCTO.descripcion, FABRICANTE.razon_social, PRODUCTO.cantidad_stock
FROM PRODUCTOS, FABRICANTE INNER JOIN FABRICANTE as F 
WHERE PRODUCTO.cod_fabricante = F.cod_fabricante
ORDER BY FABRICANTE.razon_social,PRODUCTO.descripcion;

2___________________________________________________________________________________________________________

SELECT PEDIDOS.cod_pedido, PEDIDOS.fecha_pedido, EMPLEADO.apellido, CLIENTE.razon_social
FROM PEDIDOS, EMPLEADOS, CLIENTES
WHERE EMPLEADO.cod_empleado = PEDIDOS.cod_empleados AND PEDIDOS.cod_cliente = CLIENTES.cod_cliente;

3___________________________________________________________________________________________________________

SELECT EMPLEADOS.apellido, DATOS_CONTRATO.cuota as CUOTA, OFICINA.descripcion
FROM EMPLEADOS, DATOS_CONTRATO, OFICINA
WHERE EMPLEADOS.cod_empleado = DATOS_CONTRATO.cod_empleado AND OFICINA.cod_oficina = EMPLEADOS.cod_oficina
ORDER BY CUOTA DESC;

4___________________________________________________________________________________________________________

SELECT DISTINCT CLIENTES.razon_social
FROM CLIENTES INNER JOIN PEDIDOS 
WHERE PEDIDOS.cod_cliente = CLIENTES.cod_cliente AND LIKE '__/04/____';

5___________________________________________________________________________________________________________

SELECT DISTINCT *
FROM PRODUCTOS INNER JOIN PEDIDOS, DETALLE_PRODUCTO
WHERE PRODUCTOS.cod_producto = DETALLE_PEDIDOS.cod_producto
    AND DETALLE_PRODUCTO.cod_pedido = PEDIDOS.cod_pedido
    AND MONTH(PEDIDOS.fecha_pedido) = 3;

6___________________________________________________________________________________________________________

SELECT *
FROM EMPLEADOS INNER JOIN DATOS_CONTRATO 
WHERE DATOS_CONTRATO.cod_empleado = EMPLEADO.cod_empleado 
    AND (YEAR(GETDATE()) - YEAR(DATOS_CONTRATO.fecha_contrato)) > 10
ORDER BY DATOS_CONTRATO.fecha_contrato DESC;

7___________________________________________________________________________________________________________

SELECT *
FROM CLIENTES INNER JOIN LISTAS 
WHERE CLIENTES.cod_lista = LISTAS.cod_lista AND LISTAS.descripcion = 'mayorista'
ORDER BY CLIENTES.razon_social;

8___________________________________________________________________________________________________________

SELECT DISTINCT *
FROM PRODUCTOS INNER JOIN PEDIDOS, CLIENTES, DETALLE_PEDIDO
WHERE CLIENTES.cod_cliente = PEDIDOS.cod_cliente 
    AND PEDIDO.cod_pedido = DETALLE_PEDIDO.cod_pedido 
    AND PRODUCTO.cod_producto =  DETALLE_PEDIDO.cod_producto
ORDER BY CLIENTES.razon_social, PRODUCTO.descripcion; 

9___________________________________________________________________________________________________________

SELECT PRODUCTOS.descripcion, FABRICANTE.razon_social,
    (PRODUCTO.punto_reposicion - PRODUCTO.stock) as CANTIDAD_COMPRAR
FROM PRODUCTOS, FABRICANTE
WHERE PRODUCTOS.cod_fabricante = FABRICANTE.cod_fabricante 
    AND PRODUCTO.cantidad_stock < PRODUCTO.punto_reposicion
ORDER BY FABRICANTE.razon_social, PRODUCTOS.descripcion;

10__________________________________________________________________________________________________________

SELECT *
FROM EMPLEADOS INNER JOIN DATOS_CONTRATO 
WHERE EMPLEADOS.cod_empleado = DATOS_CONTRATO.cod_empleado 
    AND (DATOS_CONTRATO.cuota < 50000 
    OR DATOS_CONTRATO.cuota > 100000);


CATÁLOGO DEL SISTEMA – EJERCITACIÓN 


1
SELECT TBNAME
FROM SYSCOLUMNS
WHERE NAME = 'LOCALIDAD';

2
SELECT COLCOUNT
FROM SYSTABLES
WHERE NAME = 'PRODUCTOS';

3
SELECT Creator, COUNT(*)
FROM SYSTABLES
GROUP BY Creator;

4
SELECT DISTINCT TBNAME
FROM SYSINDEXES;


CATÁLOGO DEL SISTEMA – EJERCITACIÓN 


1___________________________________________________________________________________________________________

PACIENTES 
CLAVE PRIMARIA = COD_PACIENTE

MEDICAMENTOS
 CLAVE PRIMARIA = COD_MED

GASTOS
CLAVE PRIMARIA = COD_PACIENTE, COD_MED 
CLAVE AJENAS=COD_PACIENTE, COD_MED

2___________________________________________________________________________________________________________

CURSOS  
CLAVE PRIMARIA = NUMCURSO

OFRECIMIENTOS
CLAVE PRIMARIA = NUMOFR, NUMCURSO
CLAVE AJENA = NUMCURSO

PROFESORES
CLAVE PRIMARIA = NUMOFR, NUMCURSO, NUMEMP 
CLAVE AJENA = NUMCURSO, NUMEMP, NUMOFR

ESTUDIANTES
CLAVES PRIMARIAS =  NUMCURSO, NUMOFR, NUMEMP
CLAVES AJENAS = NUMCURSO, NUMOFR, NUMEMP 

EMPLEADOS
CLAVE PRIMARIA = NUMEMP

3___________________________________________________________________________________________________________

PRIMARY KEY(cod_emp);

CONSTRAINT ck_cod_emp
CHECK (cod_empleado >= 100 AND cod_empleado <= 1000);

CONSTRAINT uk_tipo_doc_num_doc
UNIQUE (tipo_doc, num_doc);

CONSTRAINT ck_categoria
CHECK categoria IN ('Senior', 'Semi Senior', 'Junior');

CONSTRAINT fk_cod_ofic
FOREIGN KEY (cod_ofic) 
REFERENCES OFICINAS (cod_ofic);
