# ubank_challenge
This is the repository made to show my work in the Ubank challenge

# Diccionario de variables


## Descripción de Tablas

### Tabla de proyectos

Cada project id es una meta de ahorro creada en Enero de 2020, si quiero entender de qué tratan las metas puedo ir a ver el nombre de la categoría del proyecto en en la tabla catalog.csv (https://s3.us-west-2.amazonaws.com/secure.notion-static.com/badf69e9-2522-4feb-9da8-8a78ca2b45c9/catalog.csv?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20200930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20200930T003904Z&X-Amz-Expires=86400&X-Amz-Signature=9710e23fd541c354a7d32ca879ca51fac201d8b7939260ba3ff5b07e5072176e&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22catalog.csv%22) en todas aquellas marcadas como type project_categories

La tabla projects tiene las siguientes variables:

project_id: el id del proyecto
name: el nombre del proyecto
goal_date: la fecha de creación de la meta
user_id: el id de usuario
project_category_id: el id de la categoría del proyecto
total: es el monto meta de ahorro en pesos mexicanos

### Tabla de reglas

Para cada proyecto, hay una o muchas reglas asociadas que están en la tabla de rules. Esta tabla de reglas se compone de las siguientes variables:

rule_id: id de regla (id único de la tabla) 
project_id: id de proyecto (que se cruza con la tabla de proyecto)
rule_type_id: id del tipo de regla (que es el que permite cruzar con la tabla catalog en aquellos tipos rule_types para identificar el nombre de la regla: ahorro manual, placer culpable, etc)
amount: monto asociado a la regla, el cual puede ser un monto fijo o % de de dscto o un redondeo, etc
frecuency: la frecuencia que espera tener de aplicación de la regla para cumplir la meta, la cual es 0 cuando son permanentes como el redondeo o el descuento del salario
categories: 

### Tabla de transacciones

Para cada usuario además, hay una o muchas transacciones registradas que están en la tabla transactions, las cuales o

user_id: el id de usuario
description: la descripción
transaction_date: la fecha de transacción
amount: el monto


## Ejemplo con un caso para que se entiendan las asociaciones

"project_id","name","goal_date","user_id","project_category_id","total"
"7593859139ff4b00b09c6a5c2d7d4602",
Hogar,"2020-06-02 00:00:00.0","700b1ad8bcb948d2b948c20d8e4160cd",edd09901ae364ddb80347e40005d2244,5000.00


En este ejemplo, para el proyecto 7593859139ff4b00b09c6a5c2d7d4602, puedo encontrar asociados estas dos reglas de ahorro asociada a este id de regla 7593859139ff4b00b09c6a5c2d7d4602 

"rule_id","project_id","rule_type_id","amount","frecuency","categories"
"2f206c2f263f4332aa8985dbe25fc0b5","7593859139ff4b00b09c6a5c2d7d4602",c175e7bf6cf64677903bac9389a80cd9,10.00,0,""
"7cffbe8d84534f489637b0ab8694f234","7593859139ff4b00b09c6a5c2d7d4602",b30b058a53634cbcb1f589af13e6689f,228.00,7,""

Si cruzo ahora los rule_type_id con el id de la tabla catalago filtrando solo aquellos type_rule, puedo encontrar el nombre de la regla, que en este caso son:

Primera regla: Redondear
Segunda regla: Monto fijo 

la primera tiene un monto variable que se consigue rodeando al múltiplo de 10 más cercano  y la segunda un monto fijo de 228 pesos mexicanos que se descontaran hasta 7 veces al mes.


Luego puedo cruzar el user_id de la tabla proyecto con el user_id de la tabla de usauario y puedo ver las transacciones asociadas al usuario que puso esa meta durante Enero 2020, que son estas:

"user_id","description","transaction_date","amount"
"700b1ad8bcb948d2b948c20d8e4160cd",SORIANA477 VILL,"2019-11-30 00:00:00.0",-184.05
"700b1ad8bcb948d2b948c20d8e4160cd",COPIZZA,"2019-12-11 00:00:00.0",-161.00
