
# Practica 1: Bases de datos colecciones y inserts

1. Conectarnos con mongosh a MongoDb
1. Crar una base de datos llamada curso 
1. Comprobar que la base de datos no existe 
1. Crear una coleccion que se llame facturas y mostrarla 
```json
db.createCollection("facturas")
```json
 show collections
 ```
 1. Insertar un  documentos  con los siguientes datos 

 | Codigo   | Valor   |
|-------------|-------------|
| Cod_Factura | 10 |
| Ciente | Frutas Ramirez |
| Total | 223 |

```json
db.facturas.insertOne(
... {
... cod_facturas: 10,
... cliente: 'Frutas Ramirez',
... total: 145
... }
... )
```

 | Codigo   | Valor   |
|-------------|-------------|
| Cod_Factura | 20 |
| Ciente | Fereteria Juan |
| Total | 140 |

```json
db.facturas.insertOne(
 {
 cod_facturas: 20,
 cliente: 'Fereteria Juan',
 total: 140
 }
 )
```

1. Crear una nueva coleccion pero usando directamente el insertOne.
    insertar un documento en la coleccion productos con los siguientes 
    datos> 
    | Codigo   | Valor   |
|-------------|-------------|
| Cod_Producto | 1 |
| Nombre | Tornillo x 1 |
| Precio | 2 |

```json
db.productos.insertOne(
 {
 cod_producto: 20,
 nombre: 'Tornillo x 2',
 precio: 2 
 }
 )
```

1. Crear un nuevo documento que tenga un array con los siguientes datos:

| Codigo   | Valor   |
|-------------|-------------|
| Cod_Producto | 2 |
| Nombre | Martillo |
| Precio | 20 |
| Unidades | 50 |
| Fabricantes | fab1, fab2, fab3, fab4 | 

```json
db.productos.insertOne(
 {
 cod_producto: 2,
 nombre: 'Martillo',
 precio: 20, 
 unidades: 50,
 fabricantes: ['fab1', 'fab2', 'fab3', 'fab4']
 }
 )
```
1. Borrar la coleccion Facturas y comprobar que se borro

```json 
db.facturas.drop()
show 

9. Insertar un documento en una coleccion denominada **fabricantes**
    para comprobar los subducumentos y la clave _id personalizada 


| Codigo   | Valor   |
|-------------|-------------|
| id | 1 |
| Nombre | fab1 |
| Localidad | ciudad: buenos aires, pais: Argentina, calle: pez 27, cod_postal:2900  |

```json
db.fabricantes.insertOne(
 {
 _id: 1,
 nombre: 'Fab1',
 localidad: {
    ciudad: 'Buenos Aires', pais: 'Argentina', calle: 'Pez 27', cod_postal:2900
 } 
 }
 )
 ```

 10. Realizar una insercion de varios documentos en la coleccion productos 


| Codigo   | Valor   |
|-------------|-------------|
| Cod_Producto | 3 |
| Nombre | Alicates |
| Precio | 10 |
| Unidades | 25 |
| Fabricantes | fab1, fab2, fab5 | 

| Codigo   | Valor   |
|-------------|-------------|
| Cod_Producto | 4 |
| Nombre | Arrandela |
| Precio | 1 |
| Unidades | 1500 |
| Fabricantes | fab1, fab3, fab6 |


db.fabricantes.insertMany(
 [
   {
     
 cod_producto: 3,
 nombre: 'Alicates',
 precio: 10, 
 unidades: 25,
 fabricantes: ['fab1', 'fab2', 'fab5']
 },
  {
 cod_producto: 4,
 nombre: 'Arrandela',
 precio: 1, 
 unidades: 1500,
 fabricantes: ['fab1', 'fab3', 'fab6']
 }
 ]
)