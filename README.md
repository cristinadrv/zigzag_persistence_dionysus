# Análisis de Persistencia Zigzag con Dionysus en Linux

Este proyecto implementa un análisis de datos topológico dinámicos basado en homología zigzag, utilizando el paquete **Dionysus** (disponible solo para Linux).

## Descripción del trabajo

### Entrada de datos

Se parte de un dataframe con las siguientes columnas:

- **Tiempo**: fotogramas o instantes temporales.
- **Id**: identificador único de cada individuo.
- **X, Y**: coordenadas espaciales de cada individuo.

### Construcción de símplices

- Se genera una lista con todos los posibles símplices hasta dimensión 3.
- En esta representación, los vértices corresponden a los identificadores (`Id`) de los individuos.

### Tabla lógica inicial

- Se crea un dataframe lógico (valores booleanos inicialmente en `False`).
- Cada fila representa un símplice.
- Cada columna representa un instante de tiempo.
- La tabla está ordenada por dimensión y tiempos.

### Diccionarios para traducción de índices

Se definen dos estructuras:

1. Una lista de diccionarios que, para cada instante de tiempo, asocia cada `Id` con sus coordenadas `(X, Y)`.

2. Un diccionario que relaciona los índices devueltos por la función Vietoris–Rips con las coordenadas originales.

### Aplicación de la filtración Vietoris–Rips

Se utiliza la función `d.fill_rips(lista_coordenadas, dim, r)` con:

- `lista_coordenadas`: lista de coordenadas por tiempo.
- `dim = 2`: dimensión máxima a calcular.
- `r = 0.5`: parámetro máximo de escala para la filtración.

Los índices de los símplices activos obtenidos se traducen a los identificadores `Id` mediante los diccionarios.

### Relleno y reducción de la tabla lógica

- Se marcan con `True` en la tabla los símplices presentes en cada instante.
- Se eliminan los símplices que no aparecen en ningún tiempo para optimizar la tabla.

### Inserción de tiempos intermedios

- Se comparan columnas consecutivas.
- Se crean nuevas columnas que representan la unión de ambas.
- Si esta unión no existe aún, se añade con el nombre del tiempo intermedio, por ejemplo, `t0.5`.

### Visualización con grafos

Se representan los símplices mediante grafos para ilustrar la evolución temporal de la estructura topológica.

### Creación de la lista `times`

Para cada símplice, se guarda en esta lista los tiempos en que aparece (`True`) y desaparece (`False`).

- El primer `True` indica el nacimiento del símplice.
- Los cambios entre `True` y `False` marcan eventos topológicos.

### Cálculo de la homología zigzag

Finalmente, con la lista de símplices y la lista `times`, se aplican las funciones:

- `Filtration`
- `zigzag_homology_persistence`

para obtener la homología zigzag del sistema dinámico.
