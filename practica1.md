# ZODB

Es una base de datos orientada a objetos para almacenar de forma transparente y persistente objetos en el lenguaje de
programación Python.

## Principales características

* Transacciones:
  - dssd
* Almacenamiento conectable de forma transparente:
    - ZODB tiene un marco de almacenamiento conectable. Esto significa que hay una variedad de implementaciones de almacenamiento para satisfacer diferentes necesidades, desde bases de datos en memoria hasta bases de datos almacenadas en archivos locales, bases de datos en servidores de bases de datos remotas y bases de datos especializadas para compresión, cifrado, etc.

* Almacenamiento en caché:
    - sddsfdc
* Control de concurrencia:
  - cdscsdc
* Escalable a traves de una red mediante ZEO:
    - s(Una implementación de almacenamiento de ZODB que permite varios procesos de clientes a la persistencia de objetos en un único servidor ZEO)

### Puntos fuertes y débiles

* Sin un idioma separado para las operaciones de la base de datos
* Muy poco impacto en su código para hacer que los objetos sean persistentes
* Ningún mapeador de base de datos que oculte parcialmente la base de datos.
* Usar un mapeo relacional de objetos no es como usar una base de datos de objetos.
* Casi no hay unión entre el código y la base de datos.
* Las relaciones entre los objetos se manejan de forma muy natural y admiten grafos de objetos complejos sin uniones.


* Pip
* ZEO
* Lento en escritura

### Principales usos
Cuando quieres concentrarte en tu aplicación sin escribir mucho código de base de datos.
Cuando tu aplicación tiene estructuras de datos y relaciones complejas.
Cuando accedes a datos a traves de metodos y atributos.
Cuando lees más datos que los que escribes.
Cuando necesitas testear la logica que utiliza tu base de datos.
### ¿ Cómo funciona ?

## Ejemplo de caso práctico:

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