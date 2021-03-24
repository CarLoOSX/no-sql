# ZODB

Es una base de datos orientada a objetos para almacenar de forma transparente y persistente objetos en el lenguaje de
programación Python.

## Principales características

* Transacciones:
  - Atómicas, se modifican los objetos por partes. Bueno para la gestión de errores.
  - Pruemeven el aislamiento necesarío para no generar conflictos sobre la base de datos.
  - Muchas de las BD No-SQL no tienen transacciones y por tanto no tienen muchas nociones de consistencia. La mayoria de la gente las utiliza por su alto escalmiento pero por tanto se tiene que tener en cuenta la consistencia de los datos.
  

* Almacenamiento conectable de forma transparente:
    - ZODB tiene un framework de almacenamiento conectable o pluggable. Esto significa que hay una variedad de implementaciones de almacenamiento para satisfacer diferentes necesidades, desde bases de datos en memoria hasta bases de datos almacenadas en archivos locales, bases de datos en servidores de bases de datos remotas y bases de datos especializadas para compresión, cifrado, etc.
    - Hay infinidad de plugins para ZODB, algunos de ellos son, al tener una arquitectura plugable, la definicion de estas interfaces la podemos encontrar en:
      http://www.zodb.org/en/latest/reference/storages.html
      
    - De esta manera podríamos decir que independientemente de la implementación todos se utilizan de la misma manera.
  

* Almacenamiento en caché:
    - Cada conexión de base de datos tiene una caché que es una réplica de base de datos parcial y consistente. Al acceder a los objetos de la base de datos, se accede a los datos que ya están en la memoria caché sin ninguna interacción con la base de datos. Cuando se modifican los datos, las invalidaciones se envían a los clientes, lo que provoca la invalidación de los objetos almacenados en caché. La próxima vez que se acceda a los objetos invalidados, se cargarán desde la base de datos.
    - Las aplicaciones no tienen que invalidar las entradas de la caché. La base de datos invalida las entradas de la caché automáticamente.


* Escalable a traves de una red mediante ZEO:
    - Una implementación de almacenamiento de ZODB que permite varios procesos de clientes a la persistencia de objetos en un único servidor ZEO.

## Puntos fuertes y débiles

#### PROS
* Sin un idioma separado para las operaciones de la base de datos
* Muy poco impacto en el código para hacer que los objetos sean persistentes
* Ningún mapeador de base de datos que oculte parcialmente la base de datos.
* Usar un mapeo relacional de objetos no es como usar una base de datos de objetos.
* Casi no hay unión entre el código y la base de datos.
* Las relaciones entre los objetos se manejan de forma muy natural y admiten grafos de objetos complejos sin uniones.
* Muy Plugable
* Caché transparente para el usuario.

#### CONTRAS
* Pip (Único para python)
* ZEO (Necesitas de servicios propietario para el escalamiento)
* Lento en escritura (No recomendado para aplicaciones con mas escritura que lectura)


### Principales usos

* Aplicaciones hechas en python.


## Cuando lo utilizarías ?
* Cuando quieres concentrarte en tu aplicación sin escribir mucho código de base de datos.
* Cuando tu aplicación tiene estructuras de datos y relaciones complejas.
* Cuando accedes a datos a traves de metodos y atributos.
* Cuando lees más datos que los que escribes.
* Cuando necesitas testear la logica que utiliza tu base de datos.
* Cuando necesitas que la lectura de datos sea muy rápida, por su caché.


### ¿ Cómo funciona ?



## Ejemplo de caso práctico:


Uso de librería
```
# account.py

import persistent

class Account(persistent.Persistent):

    def __init__(self):
        self.balance = 0.0

    def deposit(self, amount):
        self.balance += amount

    def cash(self, amount):
        assert amount < self.balance
        self.balance -= amount

```
Notificar un cambio a BD (self._p_changed = True)
```
import persistent

class Book(persistent.Persistent):

   def __init__(self, title):
       self.title = title
       self.authors = []

   def add_author(self, author):
       self.authors.append(author)
       self._p_changed = True
```

Guardando estructuras
```
import persistent
import persistent.list

class Book(persistent.Persistent):

   def __init__(self, title):
       self.title = title
       self.authors = persistent.list.PersistentList()

   def add_author(self, author):
       self.authors.append(author)
```

Mas documentación en:

http://www.zodb.org/en/latest/guide/writing-persistent-objects.html