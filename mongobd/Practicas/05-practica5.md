# Agregaciones

1. Para hacer esta práctica vamos a cargar unos datos ficticios de empresas.
2. Tienes un fichero denominado “productos.json”
3. Debes poner el resultado de las consultas en cada apartado

- Cuenta los productos de tipo “medio”, usando un método básico
```json
db.productos.find(
    {
        tipo:"medio"
    }
).count()
```
![alt text](/img/practica5/image-7.png)


- Indicar con un distinct, las empresas (fabricantes) que hay en la colección

```json
db.productos.find(
    {
        fabricantes
    }
).disting()
```

![alt text](/img/practica5/image.png)

- Usando aggregate, visualizar los productos que tengan más de 80 unidades
```json
db.productos.aggregate([
    { $match: { unidades: { $gt: 80 } } }
])
```

![alt text](/img/practica5/image-1.png)


- Con $project visualizar solo el nombre, unidades y precio de los productos que tengan menos de 10 unidades
```json
db.getCollection('productos').aggregate(
  [
    { $match: { unidades: { $lt: 10 } } },
    {
      $project: {
        nombre: 1,
        unidades: 1,
        precio: 1
      }
    }
  ],
  { maxTimeMS: 60000, allowDiskUse: true }
);
```
![alt text](/img/practica5/image-2.png)

- Con $project ponemos el fabricante pero le cambiamos el nombre por “empresa”. Usamos el mismo comando anterior
```json
db.getCollection('productos').aggregate(
  [
    { $match: { unidades: { $lt: 10 } } },
    {
      $project: {
        nombre: 1,
        unidades: 1,
        precio: 1,
        empresa: '$fabricante'
      }
    }
  ],
  { maxTimeMS: 60000, allowDiskUse: true }
);
```
![alt text](/img/practica5/image-3.png)

- Añadir a la consulta anterior un campo calculado que se llame total y que multiplique precio por unidades.
```json
db.getCollection('productos').aggregate(
  [
    { $match: { unidades: { $lt: 10 } } },
    {
      $project: {
        nombre: 1,
        unidades: 1,
        precio: 1,
        empresa: '$fabricante',
        total: {
          $multiply: ['$precio', '$unidades']
        }
      }
    }
  ],
  { maxTimeMS: 60000, allowDiskUse: true }
);
```

![alt text](/img/practica5/image-4.png)

- Hacer que el nombre salga en mayúsculas con el operador $toUpper
```json
db.getCollection('productos').aggregate(
  [
    { $match: { unidades: { $lt: 10 } } },
    {
      $project: {
        nombre: { $toUpper: '$nombre' },
        unidades: 1,
        precio: 1,
        empresa: '$fabricante',
        total: {
          $multiply: ['$precio', '$unidades']
        }
      }
    }
  ],
  { maxTimeMS: 60000, allowDiskUse: true }
);
```
![alt text](/img/practica5/image-5.png)

- Añadir un campo calculado que ponga el nombre del producto y el tipo concatenado con el operador $concat. Le llamamos al campo “completo”
```json
db.getCollection('productos').aggregate(
  [
    { $match: { unidades: { $lt: 10 } } },
    {
      $project: {
        nombre: { $toUpper: '$nombre' },
        unidades: 1,
        precio: 1,
        empresa: '$fabricante',
        total: {
          $multiply: ['$precio', '$unidades']
        },
        completo: {
          $concat: ['$nombre', ' ', '$tipo']
        }
      }
    }
  ],
  { maxTimeMS: 60000, allowDiskUse: true }
);
```
![alt text](/img/practica5/image-8.png)

- Ordena el resultado por el campo “total”
```json
db.getCollection('productos').aggregate(
  [
    { $match: { unidades: { $lt: 10 } } },
    {
      $project: {
        nombre: { $toUpper: '$nombre' },
        unidades: 1,
        precio: 1,
        empresa: '$fabricante',
        total: {
          $multiply: ['$precio', '$unidades']
        },
        completo: {
          $concat: ['$nombre', ' ', '$tipo']
        }
      }
    },
    { $sort: { total: 1 } }
  ],
  { maxTimeMS: 60000, allowDiskUse: true }
);
```

![alt text](/img/practica5/image-6.png)

- Haciendo una nueva consulta, averiguar el numero de productos por tipo de producto

```json
db.getCollection('productos').aggregate(
  [
    {
      $group: {
        _id: '$tipo',
        numeroDeProductos: { $sum: 1 }
      }
    }
  ],
  { maxTimeMS: 60000, allowDiskUse: true }
);
```

![alt text](/img/practica5/image-9.png)

- Añadir el valor mayor y el menor
```json
db.getCollection('productos').aggregate(
  [
    {
      $group: {
        _id: '$tipo',
        numeroDeProductos: { $sum: 1 },
        valorMaximo: { $max: '$precio' },
        valorMinimo: { $min: '$precio' }
      }
    }
  ],
  { maxTimeMS: 60000, allowDiskUse: true }
);
```
![alt text](/img/practica5/image-10.png)

- Añade el total de unidades por cada tipo
```json
db.getCollection('productos').aggregate(
  [
    {
      $group: {
        _id: '$tipo',
        numeroDeProductos: { $sum: 1 },
        valorMaximo: { $max: '$precio' },
        valorMinimo: { $min: '$precio' },
        totalUnidades: { $sum: '$unidades' }
      }
    }
  ],
  { maxTimeMS: 60000, allowDiskUse: true }
);
```

![alt text](/img/practica5/image-11.png)

- Con el operador $set y el operador “$substr” visualiza todos los datos del producto "Small Metal Tuna" y los primeros 5 caracteres del nombre.
```json
db.getCollection('productos').aggregate(
  [
    { $match: { nombre: 'Small Metal Tuna' } },
    {
      $set: {
        primeros5: { $substr: ['$nombre', 0, 5] }
      }
    }
  ],
  { maxTimeMS: 60000, allowDiskUse: true }
);
```

- Creamos una salida que tenga el nombre del articulo y el total (precio por unidades) y lo guardamos en una colección denominada productos2
```json
db.productos.aggregate([
    { 
        $project: { 
            nombre: 1, 
            total: { $multiply: ["$precio", "$unidades"] } 
        }
    },
    { 
        $out: "productos2" 
    }
])
```


- Comprobamos que se ha creado
```json
 show collections
```

![alt text](/img/practica5/image-12.png)

- Hacemos un find para comprobar el resultado
```json
db.productos2.find({})
```

![alt text](/img/practica5/image-13.png)

- Usando $cond y $project vamos a visualizar el nombre del producto, el precio y un campo llamado valoración que ponga “barato” si el precio es menor de 250 y caro si es mayor o igual
```json
db.getCollection('productos').aggregate(
  [
    {
      $project: {
        nombre: 1,
        precio: 1,
        valoración: {
          $cond: {
            if: { $lt: ['$precio', 250] },
            then: 'barato',
            else: 'caro'
          }
        }
      }
    }
  ],
  { maxTimeMS: 60000, allowDiskUse: true }
);
```

![alt text](/img/practica5/image-14.png)