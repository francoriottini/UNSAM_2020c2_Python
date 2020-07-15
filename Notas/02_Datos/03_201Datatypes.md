[Contenidos](../Contenidos.md) \| [Anterior (2 Funciones)](02_107Funciones.md) \| [Próximo (4 Contenedores)](04_202Containers.md)

# 2.3 Tipos y estructuras de datos

Esta sección introduce dos estructuras de datos elementales: las tuplas y los diccionarios.

### Tipos de datos primitivos

Python tiene pocos tipos primitivos de datos.

* Números enteros
* Números de punto flotante
* Cadenas de texto

Algo ya sabemos sobre estos tipos de datos por el capítulo anterior.

### Tipo None 

```python
email_address = None
```

`None` suele utilizarse como un comodín para reservar el lugar para un valor opcional o faltante. En los condicionales, evalúa como `False`.

```python
if email_address:
    send_email(email_address, msg)
```

### Estructuras de datos

Los programas reales tienen datos más complejos que los que podemos almacenar en los tipos primitivos. Por ejemplo, información sobre un pedido de frutas:

```code
100 cajones de Manzanas a $490.10 cada uno
```

Este es un "objeto" con tres partes:

* Nombre del símbolo ("Manzanas", una cadena)
* Número o cantidad (100, un entero)
* Precio (490.10 un flotante)

### Tuplas

Una tupla es una colección con valores agrupados juntos.

Ejemplo:

```python
s = ('Manzanas', 100, 490.1)
```

A veces, los paréntesis `()` son omitidos en la sintaxis.

```python
s = 'Manzanas', 100, 490.1
```

Salvo para las tuplas de longutid cero y uno, que son casos especiales y tienen una notación particular.

```python
t = ()            # Tupla vacía
w = ('PERA', )    # Tupla de un elemento
```

Las tuplas suelen usarse para representar registros o estructuras *simples*.
Típicamente, una tupla representa un solo *objeto* con múltiples partes. Una analogía posible es la siguiente: *Una tupla es como una fila de una base de datos*.

Los contenidos de una tupla están ordenados (como en un vector).

```python
s = ('Manzana', 100, 490.1)
nombre = s[0]                   # 'Manzana'
cantidad = s[1]                 # 100
precio= s[2]                    # 490.1
```

Eu contenido de las tuplas no puede ser modificado.

```python
>>> s[1] = 75
TypeError: object does not support item assignment
```

Podés, sin embargo, hacer una nueva tupla basada en el contenido de otra, que no es lo mismo que modificar el contenido.

```python
s = (s[0], 75, s[2])
```

### Empaquetando tuplas

Las tuplas suelen usarse para empaquetar información relacionada en una sola *entidad*.

```python
s = ('Manzanas', 100, 490.1)
```

Una tupla puede ser pasada de un lugar a otro de un programa como un solo objeto.

### Desempaquetando tuplas

Para usar una tupla en otro lado, debemos desempaquetar su contenido en diferentes variables.

```python
fruta, cajones, precio = s
print('Costo:', cajones * precio)
```

El numero de variables a la izquierda debe coincidir con la estructura de la tupla.

```python
nombre, cajones = s     # ERROR
Traceback (most recent call last):
...
ValueError: too many values to unpack
```

### Tuplas vs. Listas

Las tuplas parecieran ser listas de solo-lectura. Sin embargo, las tuplas suelen usarse para un solo item que consiste de múltiples partes mientras que las listas suelen usarse para una colección de diferentes elementos, típicamente del mismo tipo.

```python
record = ('Manzanas', 100, 490.1)                # Una tupla representando un registro dentro de un pedido de frutas

symbols = [ 'Manzanas', 'Peras', 'Mandarinas' ]  # Una lista representando tres frutas diferentes.
```

### Diccionarios

Un diccionarios es una función que manda *claves* en *valores*. A veces se loa denomina tabla de hash. Las claves sirven como índices para acceder a los valores.

```python
s = {
    'fruta': 'Manzana',
    'cajones': 100,
    'precio': 490.1
}
```

### Operaciones usuales

Para obtener el valor almacenado en un diccionario usamos las claves.

```python
>>> print(s['fruta'], s['cajones'])
Manzanas 100
>>> s['precio']
490.10
>>>
```

Para agregar o modificar valores, simplemente asignamos usando la clave.

```python
>>> s['cajones'] = 75
>>> s['fecha'] = '6/8/2020'
>>>
```

para borrar un valor, usamos el comando `del`.

```python
>>> del s['fecha']
>>>
```

### ¿Por qué diccionarios?

Lo diccionarios son útiles cuando hay *muchos* valores diferentes y esos valores pueden ser modificados o manipulados. Los diccionarios muchas veces cumplen una tarea fudamental: hacen que el código sea más legible.

```python
s['precio']
# vs
s[2]
```

## Ejercicios

En los últimos ejercicios, escribiste un programa que leía el archivo
`Data/camion.csv`. Usando el módulo `csv`, es fácil leer el archivo fila por fila.

```python
>>> import csv
>>> f = open('Data/camion.csv')
>>> filas = csv.reader(f)
>>> next(filas)
['nombre', 'cajones', 'precio']
>>> fila = next(filas)
>>> fila
['LIMA', '100', '32.20']
>>>
```

A veces, además de leerlo, queremos hacer otras cosas con el archivo *.csv*, como por ejemplo usarlo para cálculos. Lamentablemente una fila de datos en crudo es suficiente para trabajar. 

```python
>>> fila = ['LIMA', '100', '32.20']
>>> cost = fila[1] * fila[2]
Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
TypeError: can't multiply sequence by non-int of type 'str'
>>>
```

Para poder hacer más cosas con estos datos, vamos a querer interpretar la fila de datos de alguna manera particular, pasándola a otro tipo de objeto que nos resulte más útil para trabajar. Por ejemplo, tuplas o diccionarios.

### Ejercicio 2.9: Tuplas

En el intérprete interactivo, creá la siguiente tupla que representa la fila de antes, pero con las columnas numéricas pasadas a formatos adecuados:

```python
>>> t = (fila[0], int(fila[1]), float(fila[2]))
>>> t
('LIMA', 100, 32.2)
>>>
```

A partir de esta tupla, ahora podés calcular el costo total multiplicando cajones por precio:

```python
>>> cost = t[1] * t[2]
>>> cost
3220.0000000000005
>>>
```

¿Qué pasó? ¿Qué hace ese 5 al final?

Este error no es un problema de Python, sino de la forma en la que la máquina representa los números de punto flotante. Así como en base 10 no podemos escribir un tercio de manera exacta, en base 2 escribir un quinto requiere infinitos dígitos. Al usar una representación finita (una cantidad acotada de dígitos) la máquina redondea los números. La aritmética de punto flotante no es exacta. 

Esto pasa en todos los lenguajes de programación que usan punto flotante, pero en muchos casos estos pequeños errores quedan ocultos al imprimir. Por ejemplo:

```python
>>> print(f'{cost:0.2f}')
3220.00
>>>
```

Las tuplas son de sólo lectura. Verificalo tratando de cambiar el número de cajones a 75.

```python
>>> t[1] = 75
Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
TypeError: 'tuple' object does not support item assignment
>>>
```

Aunque no podés cambiar al tupla, sí podés reemplazar la tupla por una nueva.

```python
>>> t = (t[0], 75, t[2])
>>> t
('LIMA', 75, 32.2)
>>>
```

Siempre que reasignes una variable como recién lo hiciste con `t`, el valor anterior de la variable se pierde. Aunque la asignación de arriba pueda parecer como que estás modificando la tupla, en realidad estás creando una nueva tupla y tirando la vieja.

Las tuplas muchas veces se usan para empaquetar y despempaquetar valores dentro de variables. Probá esto:

```python
>>> nombre, cajones, precio = t
>>> nombre
'LIMA'
>>> cajones
75
>>> precio
32.2
>>>
```

Tomá las variables de arriba y empaquetalas en una tupla.

```python
>>> t = (nombre, 2*cajones, precio)
>>> t
('LIMA', 150, 32.2)
>>>
```

### Ejercicio 2.10: Diccionarios como estructuras de datos

Una alternativa a la tupla es un diccionario.

```python
>>> d = {
        'nombre' : fila[0],
        'cajones' : int(fila[1]),
        'precio'  : float(fila[2])
    }
>>> d
{'nombre': 'LIMA', 'cajones': 100, 'precio': 32.2 }
>>>
```

Calculá el costo total de este lote:

```python
>>> cost = d['cajones'] * d['precio']
>>> cost
3220.0000000000005
>>>
```

Compará este ejemplo con el mismo cálculo hecho con tuplas más arriba. Cambiá el número de cajones a 75.

```python
>>> d['cajones'] = 75
>>> d
{'nombre': 'LIMA', 'cajones': 75, 'precio': 32.2 }
>>>
```

A diferencia de las tuplas, los diccionarios se pueden modificar libremente. Agregá algunos atributos:

```python
>>> d['fecha'] = (6, 11, 2007)
>>> d['cuenta'] = 12345
>>> d
{'nombre': 'LIMA', 'cajones': 75, 'precio':32.2, 'fecha': (6, 11, 2007), 'cuenta': 12345}
>>>
```

### Ejercicio 2.11: Más operaciones con diccionarios

Si pasás un diccionario a una lista, obtenés sus claves.

```python
>>> list(d)
['nombre', 'cajones', 'precio', 'fecha', 'cuenta']
>>>
```

Análogamente, si usás el comando `for` para iterar sobre el diccionario, obtenes las claves:

```python
>>> for k in d:
        print('k =', k)

k = nombre
k = cajones
k = precio
k = fecha
k = cuenta
>>>
```

Probá esta variante:
Try this variant that performs a lookup at the same time:

```python
>>> for k in d:
        print(k, '=', d[k])

nombre = LIMA
cajones = 75
precio = 32.2
fecha = (6, 11, 2007)
cuenta = 12345
>>>
```

También podés obtener todas las claves del diccionario usando el método `keys()`:

```python
>>> claves = d.keys()
>>> claves
dict_keys(['nombre', 'cajones', 'precio', 'fecha', 'cuenta'])
>>>
```

Es poco usual utilizar `keys()`, porque devuelve un objeto especial de tipo `dict_keys`.

Esta asignación va a implicar que `claves` guardará las claves del diccionario `d` aunque éste cambie. Por ejemplo, probá esto:

##################### This is an overlay on the original dictionary that always gives you
the current keys—even if the dictionary changes. For example, try
this:

```python
>>> del d['cuenta']
>>> claves
dict_keys(['nombre', 'cajones', 'precio', 'fecha'])
>>>
```

Fijate que `cuenta` desapareció de `claves` aunque no volviste a llamar al comando `d.keys()`.

Una manera más elegante de trabajar con claves y valores a la vez es usar el método `items()`. Esto te devuelve tuplas de la forma `(clave,valor)`.

```python
>>> items = d.items()
>>> items
dict_items([('nombre', 'LIMA'), ('cajones', 75), ('precio', 32.2), ('fecha', (6, 11, 2007))])
>>> for k, v in d.items():
        print(k, '=', v)

nombre = LIMA
cajones = 75
precio = 32.2
fecha = (6, 11, 2007)
>>>
```

Si tenés tuplas como `items` podés crear un diccionario usando la función `dict()`. Probá esto:

```python
>>> items
dict_items([('nombre', 'LIMA'), ('cajones', 75), ('precio', 32.2), ('fecha', (6, 11, 2007))])
>>> d = dict(items)
>>> d
{'nombre': 'LIMA', 'cajones': 75, 'precio':32.2, 'fecha': (6, 11, 2007)}
>>>
```


[Contenidos](../Contenidos.md) \| [Anterior (2 Funciones)](02_107Funciones.md) \| [Próximo (4 Contenedores)](04_202Containers.md)
