---
marp: true
# @auto-scaling: true
theme: default
class: invert
---


# Comandos de Linux
## Procesamiento de texto
---
# <!--fit-->
# Comandos head y tail

* Extraen las primeras (`head`) o las ultimas (`tail`) lineas de un fichero o un stream

### Ejemplo

**input:** *sample.txt*
```console
Linea 1
Linea 2
Linea 3
Linea 4
```

```console
user@server~$ tail -n 2 sample.txt # Extrae las ultimas dos
Linea 3
Linea 4
```
---
# <!--fit-->
# Comando head

### Ejemplo

**input:** *sample.txt*
```console
Linea 1
Linea 2
Linea 3
Linea 4
```

```shell
user@server~$ head -n 2 sample.txt  # Extrae las primeras dos
Linea 1
Linea 2
```
---

# Opciones head

* `-n N` Extrae las primeras lineas
* `-c N` Extrae los primeros N carácteres/bytes en lugar de lineas
* `-n -N` Extrae las primeras lineas excepto las ultimas N

---
# Ejemplos 

**input:** *sample.txt* 
```
Suscripción caducada.
¡Renueva ahora!
Evita interrupciones.
Sigue disfrutando.
Beneficios únicos.
Opciones de renovación.
Asistencia disponible.
```

```shell
user@server~$ head -c 3 sample.txt  # Extrae los primeros tres carácteres
Sus
```
```shell
user@server~$ head -n -3 sample.txt  # Extrae todo menos las últimas tres
Suscripción caducada.
¡Renueva ahora!
Evita interrupciones.
Sigue disfrutando.
```
---

# Opciones tail

* `-n N` Extrae las últimas lineas
* `-c N` Extrae los últimos N carácteres/bytes en lugar de lineas
* `-n +N` Extrae las últimas lineas excepto las N primeras



---
# Ejemplos 

**input:** *sample.txt* 
```
Suscripción caducada.
¡Renueva ahora!
Evita interrupciones.
Sigue disfrutando.
Beneficios únicos.
Opciones de renovación.
Asistencia disponible.
```

```shell
user@server~$ tail -c 3 sample.txt  # Extrae los primeros tres carácteres
le.
```
```shell
user@server~$ head -n -3 sample.txt  # Extrae todo menos las últimas tres
Sigue disfrutando.
Beneficios únicos.
Opciones de renovación.
Asistencia disponible.
```

---
# wc 
Utilizado para contar lineas, palabras y carácteres de un fichero o stream de texto

**input:** *sample.txt*
```console
Linea 1
Linea 2
Linea 3
Linea 4
```

```shell
user@server~$ wc < sample.txt 
4  8  32 
```
*sample.txt* tiene 4 lineas, 8 palabras y 32 carácteres

---

# Opciones de wc

* `-w` Cuenta las palabras
* `-c ` Cuenta los carácteres
* `-n` Cuenta las lineas

--- 
# Ejemplo útil

Contar los ficheros acabados en txt del directorio actual

```shell 
user@server~$ ls -l 
total 8
-rw-rw-r-- 1 sergi sergi 12 feb 26 21:09 file.txt
-rw-rw-r-- 1 sergi sergi  0 feb 26 21:09 hello.c
-rw-rw-r-- 1 sergi sergi 32 feb 26 21:05 sample.txt
```

```shell
user@server~$ ls -l *.txt | wc -l
2
```

---

# Cut

* Útil para extraer fragmentos de texto que siguen una secuencia

--- 

# Opciones de cut




* `-c LISTA` Corta los carácteres indicados 

*Ejemplo* 
```shell
user@server~$ echo Hola que tal | cut -c 1,2,3,4,6
Holaq
```

o bien 

```shell
user@server~$ echo Hola que tal | cut -c 3-9
la que
```

---
# Opciones de cut
* `-d DELIM` utilizado para especificar el delimitador

* `-f LIST` utilizado para especificar las columnas a extraer

---

El siguiente fichero contiene la información de los jugadores de un juego de rol. 

El formato se conoce como **csv** (*Coma separated values*) y es una manera eficiente de almacenar datos en formato de tabla mediante ficheros de texto

**input:** *players.csv*
```console
name, class, level
sergi, brujo, 33
adri, guerrero, 40
nullptr, mago, 36
```
--- 
**input:** *players.csv*
```console
name, class, level
sergi, brujo, 33
adri, guerrero, 40
nullptr, mago, 36
```

El siguiente ejemplo tiene como objetivo extraer únicamente el nombre y el nivel del jugador

```shell
user@server~$ cut -d , -f 1,3 players.csv 
name, level
sergi, 33
adri, 40
nullptr, 36
```

--- 

Si queremos quitar la cabecera, podemos pasar el resultado al comando tail, **especificando que coja todas las lineas menos a partir de la 2**

```shell
user@server~$ cut -d , -f 1,3 players.csv | tail -n +2
sergi, 33
adri, 40
nullptr, 36
```


---
# Paste

Permite juntar textos con formatos tabulares de manera horizontal

La opción `-d DELIM` permite escoger el delimitador (siendo la tabulación el delimitador por defecto)

---


**input:** *a.txt*
```console
a1
a2
a3
```

**input:** *b.txt*
```console
b1
b2
b3
```
```shell
user@server~$ paste a.txt b.txt
a1      b1
a2      b2
a3      b3
```
```shell
user@server~$ paste -d, a.txt b.txt
a1,b1
a2,b2
a3,b3
```

---

# tr

Reemplaza o elimina carácteres

**OJO**: `tr` No acepta un fichero como argumento

**input:** *sample.txt*
```console
Linea 1
Linea 2
Linea 3
Linea 4
```

```shell
user@server~$ tr -d '\n' < sample.txt # \n representa un salto de linea
Linea 1Linea 2Linea 3Linea 4
```

---
## Ejemplos

Cambiar todas las 'a' por '@'

```shell
user@server~$ tr a @ < sample.txt
Line@ 1
Line@ 2
Line@ 3
Line@ 4
```

Cambiar las mínusculas por mayúsculas

```shell
user@server~$ tr [a-z] [A-Z] < sample.txt 
LINEA 1
LINEA 2
LINEA 3
LINEA 4
```

--- 

# Sort

Ordena según algún criterio

Por defecto ordena alfabéticamente a partir del primer carácter

Podemos utilizar opciones similares a las de `cut` para ordenar ficheros tipo **csv**


--- 

## Ordenar los personajes por nombre

**input:** *players.csv*
```console
name, class, level
sergi, brujo, 33
adri, guerrero, 40
nullptr, mago, 36
```

```shell
user@server~$ tail -n +2 players.csv | sort  # usamos tail para eliminar la cabecera csv
adri, guerrero, 40
nullptr, mago, 36
sergi, brujo, 33
```

--- 

## Ordenar los personajes por nivel (de mayor a menor)

Dado que ahora queremos ordenar segun un número:
* Debemos especificar que el input es un formato delimitado por coma `-t,`
* Que ordene según la columna 3 `-k3`
* Que el criterio de ordenación es numérico y no alfabético

**input:** *players.csv*
```console
name, class, level
sergi, brujo, 33
adri, guerrero, 40
nullptr, mago, 36
```

```shell
user@server~$ sort -t, -k3 -n  < players.csv
player, class, level
sergi, brujo, 33
nullptr, mago, 36
adri, guerrero, 40
```

--- 

# uniq

Filtra las lineas duplicadas

**input:** *colors.txt*
```console
rojo
lila
verde
azul
rojo
rojo
verde
```

```shell
user@server~$ uniq colors.txt
rojo
lila
verde
azul
rojo
verde
```
---


# Join

Permite unir ficheros tabulares de manera similar a como lo haríamos en SQL para unir tablas

---

Supongamos que tenemos tres ficheros:
* **player.csv** contiene el id de cada jugador y su nombre
* **objetos.csv** contiene el id de cada objeto y su nombre
* **inventario.csv** relaciona el id de un jugador y el id de los objetos que posee en el inventario


--- 
**objetos.csv**
```console
1, Espada de hierro
2, Poción de maná
3, Poción de vida
4, Bastón arcano
5, Daga de ritual
6, Piedra de alma
```

**player.csv**
```console
1, sergi
2, adri
3, nullptr
```

**inventario.csv**
```console
1, 6
1, 5
1, 3
2, 1
2, 3
3, 4
```

---
Supongamos que los tres ficheros fueran tablas SQL

Si quisieramos unir las tablas jugador e inventario con el fin de ver los objetos que posee cada jugador hariamos algo como:

```sql
SELECT * FROM player
INNER JOIN inventario on inventario.playerID = player.playerID 
```


--- 

Para hacerlo con el comando `join` hariamos algo como:

```shell
user@server~$ join  -t, -1 1 -2 1 -o 1.2 2.2 player.csv inventario.csv
sergi,6
sergi,5
sergi,3
adri,1
adri,3
nullptr,4
```

* El argumento `-1 2` hace referencia a que estamos usando el 2º campo del primer fichero

* El argumento `-2 1` hace referencia a que estamos usando el 1er campo del 2º fichero 

Ambos parámetros son el equivalente al `ON` en la consulta SQL  

--- 
Con este comando hemos obtenido una lista que expone cada jugador junto al objeto que posee. 
```shell
user@server~$ join  -t, -1 1 -2 1 -o 1.2 2.2 player.csv inventario.csv
sergi,6
sergi,5
sergi,3
adri,1
adri,3
nullptr,4
```

Para conseguir mostrar el nombre del jugador junto al nombre (y no el id) de cada objeto que posee debemos **unir este resultado con el fichero objetos.csv**

--- 

**OJO** El comando `join` exige que el los campos de unión estén ordenados. 

Dado que queremos unir segun el id del objeto ordenamos el resultado según la segunda columna

```shell
user@server~$  join  -t, -1 1 -2 1 -o 1.2 2.2 player.csv inventario.csv | sort -t, -nk2
adri,1
adri,3
sergi,3
nullptr,4
sergi,5
sergi,6
```

---

Añadiendo otro join con objetos.csv conseguimos el nombre de cada jugador junto a los items que tiene en su inventario

```shell
user@server~$  join  -t, -1 1 -2 1 -o 1.2 2.2 player.csv inventario.csv | sort -t, -nk2 | join -t, -1 2 -2 1 -o 1.1 2.2 - objetos.csv
adri,Espada de hierro
adri,Poción de vida
sergi,Poción de vida
nullptr,Bastón arcano
sergi,Daga de ritual
sergi,Piedra de alma
```

**IMPORTANTE** Dado que join necesita dos ficheros como input, utilizamos el simbolo -  para hacer referencia al resultado de la tubería

--- 

Podríamos filtrar los items de un usuario concreto simplemente añadiendo un grep

```shell
user@server~$   join  -t, -1 1 -2 1 -o 1.2 2.2 player.csv inventario.csv | sort -t, -nk2 | join -t, -1 2 -2 1 -o 1.1 2.2 - objetos.csv | grep sergi
sergi,Poción de vida
sergi,Daga de ritual
sergi,Piedra de alma
```
