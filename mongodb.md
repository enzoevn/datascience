# MongoDB

MongoDB se caracteriza por tener bases de datos, colecciones y documentos.

# Listar Bases de Datos

```bash
show dbs
```

# Crear Bases de Datos

```bash
use basededatos
```

# Listar Colecciones

```bash
show collections
```

# Crear Colecciones

```bash
db.createCollection("pasajeros")
```
Documentación https://www.mongodb.com/docs/manual/reference/method/db.createCollection/

# Insertar Documentos en MongoDb

Para insertar sólo un documentos:

```bash
db.pasajeros.insertOne(
{
    "name": "Max Schwarzmueller",
    "age": 29
  }
)
```

```bash
{
  acknowledged: true,
  insertedId: ObjectId("645c4518d0d181775cd326c1")
}
```
Notamos que por defecto mongo agregar un id a cada documento ingresado, en caso de no ser proporcionado.

Para insertar varios documentos, se debe entregar a la función un array con los documentos

```bash
db.pasajeros.insertMany(
[
  {
    "name": "Max Schwarzmueller",
    "age": 29
  },
  {
    "name": "Manu Lorenz",
    "age": 30
  },
  {
    "name": "Chris Hayton",
    "age": 35
  },
  {
    "name": "Sandeep Kumar",
    "age": 28
  },
  {
    "name": "Maria Jones",
    "age": 30
  }
]
)
```
```bash
{
  acknowledged: true,
  insertedIds: {
    '0': ObjectId("645c45dcd0d181775cd326c2"),
    '1': ObjectId("645c45dcd0d181775cd326c3"),
    '2': ObjectId("645c45dcd0d181775cd326c4"),
    '3': ObjectId("645c45dcd0d181775cd326c5"),
    '4': ObjectId("645c45dcd0d181775cd326c6")
  }
}
```

# Listar documentos de una colección

Para listar todos los documentos de una colección:

```bash
db.pasajeros.find()
```

```bash
[
{
_id: ObjectId("645c4518d0d181775cd326c1"),
name: 'Max Schwarzmueller',
age: 29
},
{
_id: ObjectId("645c45dcd0d181775cd326c2"),
name: 'Max Schwarzmueller',
age: 29
},
{
_id: ObjectId("645c45dcd0d181775cd326c3"),
name: 'Manu Lorenz',
age: 30
},
{
_id: ObjectId("645c45dcd0d181775cd326c4"),
name: 'Chris Hayton',
age: 35
},
{
_id: ObjectId("645c45dcd0d181775cd326c5"),
name: 'Sandeep Kumar',
age: 28
},
{
_id: ObjectId("645c45dcd0d181775cd326c6"),
name: 'Maria Jones',
age: 30
}
]
```


Para listar solo un documento de la colección: 

```bash
db.pasajeros.findOne()
```

```bash
{
  _id: ObjectId("645c4518d0d181775cd326c1"),
  name: 'Max Schwarzmueller',
  age: 29
}
```
Para listar un documento (muestra el primero que encuentra) con filtro:

```bash
db.pasajeros.findOne({"age": 30})
```
```bash
{
  _id: ObjectId("645c45dcd0d181775cd326c3"),
  name: 'Manu Lorenz',
  age: 30
}
```


Para listar varios documentos con filtro:

```bash
db.pasajeros.find({"age": 30})
```
```bash
[
{
_id: ObjectId("645c45dcd0d181775cd326c3"),
name: 'Manu Lorenz',
age: 30
},
{
_id: ObjectId("645c45dcd0d181775cd326c6"),
name: 'Maria Jones',
age: 30
}
]
```

Para listar varios documentos con varios filtros e indicando lo que no queremos ver:

```bash
db.pasajeros.find({"age": 30}, {_id: 0 ,"age": 0})
```

```bash
[ { name: 'Manu Lorenz' }, { name: 'Maria Jones' } ]
```
Notemos que en nuestro caso no nos muestra los campos que indicamos con "0"

Podemos también indicar que es lo que queremos ver:

```bash
db.pasajeros.find({"age": 30}, {_id: 0 ,"age": 1})
```
```bash
[ { age: 30 }, { age: 30 } ]
```
Notemos que en este caso sólo nos muestra lo que indicamos con el valor "1"

# Operadores en MongoDB
## Operadores de Comparación

```bash
$eq     # Matches values that are equal to a specified value.
$gt     # Matches values that are greater than a specified value.
$gte    # Matches values that are greater than or equal to a specified value.
$in     # Matches any of the values specified in an array.
$lt     # Matches values that are less than a specified value.
$lte    # Matches values that are less than or equal to a specified value.
$ne     # Matches all values that are not equal to a specified value.
$nin    # Matches none of the values specified in an array.
```

## Operadores Lógicos


```bash
$and     # Joins query clauses with a logical AND returns all documents that match the conditions of both clauses.
$not     # Inverts the effect of a query expression and returns documents that do not match the query expression.
$nor     # Joins query clauses with a logical NOR returns all documents that fail to match both clauses.
$or      # Joins query clauses with a logical OR returns all documents that match the conditions of either clause.
```

## Operadores de elemento

```bash
$exists   # Matches documents that have the specified field.
$type     # Selects documents if a field is of the specified type.
```
Para mas operadores visitar https://www.mongodb.com/docs/manual/reference/operator/query/#query-selectors

Para todos estos operadores tenemos que usar la siguiente sintaxis:

Para operadores $eq $gt $gte $lt $lte $ne

```bash
{ <field>: { $eq: <value> } }
```
Ejemplo:

```bash
db.pasajeros.find({ name: { $eq: "Manu Lorenz" } })
```

```bash
[
  {
    _id: ObjectId("645c45dcd0d181775cd326c3"),
    name: 'Manu Lorenz',
    age: 30
  }
]
```

```bash
db.pasajeros.find({ age: { $gte: 30 } })
```

```bash
[
  {
    _id: ObjectId("645c45dcd0d181775cd326c3"),
    name: 'Manu Lorenz',
    age: 30
  },
  {
    _id: ObjectId("645c45dcd0d181775cd326c4"),
    name: 'Chris Hayton',
    age: 35
  },
  {
    _id: ObjectId("645c45dcd0d181775cd326c6"),
    name: 'Maria Jones',
    age: 30
  }
]
```

Para operador $in y $nin

```bash
{ field: { $in: [ <value1>, <value2> ... <valueN> ] } }
```
Ejemplos:

```bash
db.pasajeros.find({ name: { $in: [ "Chris Hayton", "Maria Jones" ] } })
```

```bash
[
  {
    _id: ObjectId("645c45dcd0d181775cd326c4"),
    name: 'Chris Hayton',
    age: 35
  },
  {
    _id: ObjectId("645c45dcd0d181775cd326c6"),
    name: 'Maria Jones',
    age: 30
  }
]
```

Para operadores lógicos $and $or y $nor:

```bash
{ $and: [ { <expression1> }, { <expression2> } , ... , { <expressionN> } ] }
```

Ejemplos:

```bash
db.pasajeros.find({ $and: [ { name: { $in: [ "Chris Hayton", "Maria Jones" ] } }, { age: { $eq: 30 } } ] })
```
```bash
[
  {
    _id: ObjectId("645c45dcd0d181775cd326c6"),
    name: 'Maria Jones',
    age: 30
  }
]
```

Para operador lógico $not
```bash
{ field: { $not: { <operator-expression> } } }
```

Ejemplo:
```bash
db.pasajeros.find({ $and: [ { name: { $in: [ "Chris Hayton", "Maria Jones" ] } }, { age: { $not: { $eq: 30 }  } } ] })
```
```bash
[
  {
    _id: ObjectId("645c45dcd0d181775cd326c4"),
    name: 'Chris Hayton',
    age: 35
  }
]
```

Para $exist

```bash
{ field: { $exists: <boolean> } }
```

Ejemplos:

```bash
db.pasajeros.find({ name: { $exists: true } })
```

```bash
[
{
_id: ObjectId("645c4518d0d181775cd326c1"),
name: 'Max Schwarzmueller',
age: 29
},
{
_id: ObjectId("645c45dcd0d181775cd326c2"),
name: 'Max Schwarzmueller',
age: 29
},
{
_id: ObjectId("645c45dcd0d181775cd326c3"),
name: 'Manu Lorenz',
age: 30
},
{
_id: ObjectId("645c45dcd0d181775cd326c4"),
name: 'Chris Hayton',
age: 35
},
{
_id: ObjectId("645c45dcd0d181775cd326c5"),
name: 'Sandeep Kumar',
age: 28
},
{
_id: ObjectId("645c45dcd0d181775cd326c6"),
name: 'Maria Jones',
age: 30
}
]
```

La instrucción siguiente es equivalente a { $exists: true } :
```bash
{ $ne: null }
```

Para $type

```bash
{ field: { $type: <BSON type> } }
{ field: { $type: [ <BSON type1> , <BSON type2>, ... ] } }
```
```bash
db.pasajeros.find( { "name" : { $type : 2 } } )
```
```bash
db.pasajeros.find( { "name" : { $type : "string" } } )
```

```bash
[
{
_id: ObjectId("645c4518d0d181775cd326c1"),
name: 'Max Schwarzmueller',
age: 29
},
{
_id: ObjectId("645c45dcd0d181775cd326c2"),
name: 'Max Schwarzmueller',
age: 29
},
{
_id: ObjectId("645c45dcd0d181775cd326c3"),
name: 'Manu Lorenz',
age: 30
},
{
_id: ObjectId("645c45dcd0d181775cd326c4"),
name: 'Chris Hayton',
age: 35
},
{
_id: ObjectId("645c45dcd0d181775cd326c5"),
name: 'Sandeep Kumar',
age: 28
},
{
_id: ObjectId("645c45dcd0d181775cd326c6"),
name: 'Maria Jones',
age: 30
}
]
```
Muestra todos los documentos por que todos los nombres son string.


Los tipos pueden filtrarse por su nombre o por su código numérico, para ver los códigos numéricos de los diferentes tipos ingresar a:
https://www.mongodb.com/docs/manual/reference/operator/query/type/#-type
