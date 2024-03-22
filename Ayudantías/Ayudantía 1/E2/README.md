# Ejercicio Formativo 2 Capítulo 1

## Indice

- [Análisis del archivo json](#análisis-del-archivo-json)
- [Código](#código)
    - [Cargando el archivo json](#cargando-el-archivo-json)
    - [Creando clases](#creando-clases)
        - [Actor](#actor)
        - [Genre](#genre)
        - [Movie](#movie)
    - [Cargando datos en objetos](#cargando-datos-en-objetos)
    - [Consideraciones](#consideraciones)
        - [Funciones lambda](#funciones-lambda)
        - [Función sorted](#función-sorted)
        - [Función max](#función-max)
    - [Consultas](#consultas)
        - [Encuentre los 5 géneros más populares](#encuentre-los-5-géneros-más-populares)
        - [Encuentre los 3 años con más películas estrenadas](#encuentre-los-3-años-con-más-películas-estrenadas)
        - [Encuentre a los 5 actores con la trayectoria más larga, es decir, mayor cantidad de años actuando](#encuentre-a-los-5-actores-con-la-trayectoria-más-larga-es-decir-mayor-cantidad-de-años-actuando)
        - [Encuentre el reparto de una película (2 o más actores) que se haya repetido completo en otras la mayor cantidad de veces](#encuentre-el-reparto-de-una-película-2-o-más-actores-que-se-haya-repetido-completo-en-otras-la-mayor-cantidad-de-veces)

## Análisis del archivo json

El archivo json es un arreglo de objetos json, donde cada objeto json representa una película. Cada objeto json tiene los siguientes atributos:

- **title**: Título de la película.
- **year**: Año de lanzamiento de la película.
- **cast**: Arreglo de actores que participaron en la película. Donde cada actor es un string.
- **genres**: Arreglo de géneros de la película. Donde cada género es un string.

## Código

### Cargando el archivo json

Para cargar el archivo json, se utiliza el siguiente fragmento de código:

```python
import json

with open("movies.json", encoding="utf8") as movies_file:
    movies = json.load(movies_file)
```

Lo que hace este fragmento de código es abrir el archivo **movies.json** y cargar su contenido en la variable **movies**. El método **json.load** se encarga de interpretar el contenido del archivo como un objeto json y transformarlo en una estructura de datos de Python. En este caso, el contenido del archivo es un arreglo de objetos json, por lo que la variable **movies** será una lista de diccionarios, donde cada diccionario representa una película con los atributos **title**, **year**, **cast** y **genres**.

### Creando clases

- **Actor** : Clase que representa un actor. Tiene los siguientes atributos:

  - **name**: Nombre del actor.
  - **n_movies**: Número de películas en las que ha participado el actor.

  ```python
  class Actor:
      def __init__(self, full_name, start_career):
          self.full_name = full_name
          self.n_movies = 0
          self.start_career = start_career
          self.end_career = start_career

      def update_track_record(self, year):
          if self.start_career > year:
              self.start_career = year
          if self.end_career < year:
              self.end_career = year

      def years_track_record(self):
          return self.end_career - self.start_career

      def __repr__(self):
          return f"{self.full_name}"
  ```

  Lo que hace este fragmento de código es crear una clase **Actor** con un constructor que recibe como parámetros el nombre completo del actor y el año en que comenzó su carrera. Además, tiene un método **update_track_record** que actualiza el año de inicio y fin de la carrera del actor, un método **years_track_record** que retorna la cantidad de años que el actor ha estado en la industria y un método **\_\_repr\_\_** que retorna el nombre completo del actor.

- **Genre** : Clase que representa un género de una película. Tiene los siguientes atributos:

  - **name**: Nombre del género.
  - **n_movies**: Número de películas que pertenecen a ese género.

  ```python
  class Genre:
    def __init__(self, name):
        self.name = name
        self.n_movies = 0
    
    def __repr__(self):
        return f"{self.name}"
  ```

  Lo que hace este fragmento de código es crear una clase **Genre** con un constructor que recibe como parámetro el nombre del género. Además, inicializa el atributo **n_movies** en 0.

- **Movie** : Clase que representa una película. Tiene los siguientes atributos:

  - **title**: Título de la película.
  - **year**: Año de lanzamiento de la película.
  - **cast**: Tupla de actores que participaron en la película. Cada actor es un objeto de la clase Actor.
  - **genres**: Lista de géneros de la película. Cada género es un objeto de la clase Genre.

  ```python
  class Movie:
    def __init__(self, title, year, cast, genres):
        self.title = title
        self.year = year
        self.cast = cast
        self.genres = genres
        self.add_info()

    def add_info(self):
        for actor in self.cast:
            actor.n_movies += 1
        for genre in self.genres:
            genre.n_movies += 1
    
    def __repr__(self):
        return f"{self.title} ({self.year}) - {self.cast} - {self.genres}"
  ```

  Lo que hace este fragmento de código es crear una clase **Movie** con un constructor que recibe como parámetros el título, el año, el elenco y los géneros de la película. Además, tiene un método **add_info** que incrementa el número de películas en las que ha participado cada actor y el número de películas de cada género que se ejecuta cada vez que se crea una instancia de la clase **Movie**.

### Cargando datos en objetos

Las estructuras de datos seleccionada para para almacenar
juntas todas las entidades del mismo tipo, fueron los diccionarios para los actores, géneros y películas.

```python
actors_dict = {}
genres_dict = {}
movies_dict = {}
```

En este fragmento solamente se crean los diccionarios que contendrán los objetos de las clases **Actor**, **Genre** y **Movie** y se les asignará una llave adecuada para poder acceder a ellos.
Estos diccionarios se utilizan para evitar la creación de objetos duplicados.

```python
for movie in movies:
    cast = []
    genres = []
    for actor in movie["cast"]:
        if actor not in actors_dict:
            actors_dict[actor] = Actor(actor, movie["year"])
        else:
            actors_dict[actor].update_track_record(movie["year"])
        cast.append(actors_dict[actor])
    for genre in movie["genres"]:
        if genre not in genres_dict:
            genres_dict[genre] = Genre(genre)
        genres.append(genres_dict[genre])
    if (movie["title"], movie["year"],tuple(cast)) not in movies_dict:
        movies_dict[(movie["title"], movie["year"], tuple(cast))] = Movie(movie["title"], movie["year"], tuple(cast), genres)
    else:
        print("*"*80)
        print(f"Película duplicada: {movie['title']} - ({movie['year']}) - {tuple(cast)} - {genres}")
        movie_found = movies_dict[(movie["title"], movie["year"], tuple(cast))]
        print(f"En el diccionario ya está: {movie_found}")
        print("*"*80)
```

En esta parte del fragmento es donde se cargan los datos para los distintos objetos.

Ahora se procederá a explicar el siguiente fragmento de código a más detalle:

```python
for movie in movies:
    cast = []
    genres = []
    ....
```

En primer lugar, se recorre la lista de películas **movies**, es decir, cada **movie** es un diccionario que contiene: el título, el año, el elenco y los géneros de una película. Luego se inicializan las listas **cast** y **genres** que contendrán los actores y géneros de la película actual.

```python
    ....
    for actor in movie["cast"]:
        if actor not in actors_dict:
            actors_dict[actor] = Actor(actor, movie["year"])
        else:
            actors_dict[actor].update_track_record(movie["year"])
        cast.append(actors_dict[actor])
        ....
```

En esta parte del fragmento, se recorre la lista de actores de la película actual. Para cada actor, se verifica si ya existe en el diccionario **actors_dict**. Si no existe, se crea un objeto de la clase **Actor** con el nombre del actor y el año de la película actual y se agrega al diccionario **actors_dict** usando el nombre del actor como llave. Si ya existe, se actualiza el año de inicio o fin de la carrera del actor. Luego se agrega el objeto **Actor** al arreglo **cast**.

Se hace de esta manera para asegurarse de que cada pelicula creada tenga actores únicos, es decir, que no se creen actores duplicados.

```python
    ....
    for genre in movie["genres"]:
        if genre not in genres_dict:
            genres_dict[genre] = Genre(genre)
        genres.append(genres_dict[genre])
    ....
```

En esta parte del fragmento, se recorre la lista de géneros de la película actual. Para cada género, se verifica si ya existe en el diccionario **genres_dict**. Si no existe, se crea un objeto de la clase **Genre** con el nombre del género y se agrega al diccionario **genres_dict** usando el nombre del género como llave. Luego se agrega el objeto **Genre** al arreglo **genres**. Si ya existe, se agrega el objeto **Genre** al arreglo **genres**.

Se hace de esta manera para asegurarse de que cada pelicula creada tenga géneros únicos, es decir, que no se creen géneros duplicados.

```python
    ....
    if (movie["title"], movie["year"],tuple(cast)) not in movies_dict:
        movies_dict[(movie["title"], movie["year"], tuple(cast))] = Movie(movie["title"], movie["year"], tuple(cast), genres)
    else:
        print("*"*80)
        print(f"Película duplicada: {movie['title']} - ({movie['year']}) - {tuple(cast)} - {genres}")
        movie_found = movies_dict[(movie["title"], movie["year"], tuple(cast))]
        print(f"En el diccionario ya está: {movie_found}")
        print("*"*80)
    ....
```

En esta parte del fragmento, se verifica si la película actual ya existe en el diccionario **movies_dict**, esto ya que se detectó que habían películas con exactamente los mismos datos. Si no existe, se crea un objeto de la clase **Movie** con el título, el año, el elenco y los géneros de la película actual y se agrega al diccionario **movies_dict** usando una tupla con el título, el año y el elenco como llave. Si ya existe, se imprime un mensaje indicando que la película está duplicada y se muestra la película que ya está en el diccionario.

### Consideraciones

Para el desarrollo de las consultas se utilizaron **funciones lambda**, la función **sorted** y la función **max** , a continuación se da una breve explicación de estas y la manera de utilizarlas:

- **Funciones lambda**: Son funciones anónimas que se utilizan para definir funciones pequeñas y sencillas en una sola línea de código. Se utilizan para definir funciones que no necesitan ser reutilizadas en otras partes del código. La sintaxis de una función lambda es la siguiente:

  ```python
  lambda argumentos: expresión
  ```

  Por ejemplo, la siguiente función lambda recibe un número y retorna el cuadrado de ese número:

  ```python
  square = lambda x: x**2
  ```

  Para utilizar una función lambda, se puede asignar a una variable y luego llamar a esa variable con los argumentos necesarios.

  ```python
    print(square(5)) # Esto imprime 25
    ```
- **Función sorted**: La función **sorted** se utiliza para ordenar una secuencia de elementos. Puede recibir como argumentos una lista, tupla o cualquier otro iterable, un iterable en palabras simples es cualquier objeto que se pueda recorrer con un ciclo **for**. La función **sorted** retorna una lista con los elementos ordenados. La sintaxis de la función **sorted** es la siguiente:

    - **iterable**: Secuencia de elementos que se desea ordenar.
    - **key**: Función que se aplica a cada elemento antes de ordenarlos. Por defecto, es **None** (no se aplica ninguna función).
    - **reverse**: Booleano que indica si se ordena de forma ascendente o descendente. Por defecto, es **False** (ascendente).

- **Función max**: La función **max** se utiliza para encontrar el elemento máximo de una secuencia de elementos. Puede recibir como argumentos una lista, tupla o cualquier otro iterable. La función **max** retorna el elemento máximo de la secuencia. La sintaxis de la función **max** es la siguiente:

    - **iterable**: Secuencia de elementos de la cual se desea encontrar el máximo.
    - **key**: Función que se aplica a cada elemento antes de encontrar el máximo. Por defecto, es **None** (no se aplica ninguna función).


### Consultas

Para el desarrollo de cada consulta se creo una función que obtiene los solicitado y luego se llama a la función con los datos necesarios.

- Encuentre los 5 géneros más populares

    ```python
    def popular_genres(genres_dict):
        sorted_list_genres = sorted(genres_dict.values(), key=lambda x: x.n_movies, reverse=True)
        most_popular_genres = sorted_list_genres[:5]
        print("Los 5 géneros más populares son:")
        for genre in most_popular_genres:
            print(f"{genre.name} hay {genre.n_movies} películas de este género")
    ```

    ```python
    popular_genres(genres_dict)
    ```
        
    ```
    Los 5 géneros más populares son:
    Drama hay 8737 películas de este género
    Comedy hay 7357 películas de este género
    Western hay 3009 películas de este género
    Crime hay 1498 películas de este género
    Horror hay 1166 películas de este género
    ```

    A continuación, se explicará a detalle el código:

    ```python
    sorted_list_genres = sorted(genres_dict.values(), key=lambda x: x.n_movies, reverse=True)
    ```

    En primer lugar, **genres_dict.values()** retorna una lista con los valores del diccionario **genres_dict**, es decir, una lista con los objetos de la clase **Genre**. Luego, se utiliza la función **sorted** para ordenar la lista de géneros en base al número de películas de cada género. La función **sorted** recibe como argumentos la lista de géneros, una función lambda que retorna el número de películas de cada género y **reverse=True** para ordenar de forma descendente. De esta manera se utiliza la función lambda en conjunto con la función **sorted**, la función lambda era necesaria ya que **sorted** no puede ordenar directamente objetos de la clase **Genre** como si ordenará números por ejemplo. Cuando se hace **key=lambda x: x.n_movies** esto significa que el **x** será un objeto de la clase **Genre** y se ordenará en base al atributo **n_movies** de cada objeto.

    ```python
    most_popular_genres = sorted_list_genres[:5]
    ```

    Luego, se crea una lista **most_popular_genres** que contiene los 5 primeros elementos de la lista **sorted_list_genres**. Estos son objetos de la clase **Genre** ordenados de forma descendente según el número de películas de cada género.

    ```python 
    print("Los 5 géneros más populares son:")
    for genre in most_popular_genres:
        print(f"{genre.name} hay {genre.n_movies} películas de este género")
    ```
    Finalmente, se imprime un mensaje indicando que se mostrarán los 5 géneros más populares y se recorre la lista **most_popular_genres** para imprimir el nombre de cada género y la cantidad de películas de ese género.


- Encuentre los 3 años con más películas estrenadas.

    ```python
    def years_most_premiers(movies_dict):
    premiers_dict = {}
    for movie in movies_dict.values():
        if movie.year not in premiers_dict:
            premiers_dict[movie.year] = 0
        premiers_dict[movie.year] += 1
    sorted_list_years = sorted(premiers_dict.items(), key=lambda x: x[1], reverse=True)
    list_years = sorted_list_years[:3]
    print("Los 3 años con más películas estrenadas son:")
    for data in list_years:
        print(f"El año {data[0]} con {data[1]} películas producidas")
    ```

    ```python
    years_most_premiers(movies_dict)
    ```

    ```
    Los 3 años con más películas estrenadas son:
    El año 1919 con 632 películas producidas
    El año 1925 con 572 películas producidas
    El año 1936 con 504 películas producidas
    ```

    A continuación, se explicará a detalle el código:

    ```python
    premiers_dict = {}
    for movie in movies_dict.values():
        if movie.year not in premiers_dict:
            premiers_dict[movie.year] = 0
        premiers_dict[movie.year] += 1
    ```

    En primer lugar, se crea un diccionario **premiers_dict** que contendrá la cantidad de películas estrenadas por año. Luego, se recorre el diccionario **movies_dict** para contar la cantidad de películas estrenadas por año. Para cada película, se verifica si el año de la película no está en el diccionario **premiers_dict**. Si no está, se agrega el año al diccionario con un valor de 0. Luego, se incrementa en 1 la cantidad de películas estrenadas en ese año. Sí está, se incrementa en 1 la cantidad de películas estrenadas en ese año.

    ```python
    sorted_list_years = sorted(premiers_dict.items(), key=lambda x: x[1], reverse=True)
    ```

    Luego, se crea una lista **sorted_list_years** que contiene los elementos del diccionario **premiers_dict** ordenados de forma descendente según la cantidad de películas estrenadas en cada año. Lo que hace **premiers_dict.items()** es entregar una lista de tuplas con los elementos del diccionario, es decir, una lista de tuplas donde cada tupla contiene un año y la cantidad de películas estrenadas en ese año. Luego, se utiliza la función **sorted** de manera similar a la consulta anterior, pero en este caso se ordena en base a la cantidad de películas estrenadas en cada año. Esto significa que el **x** será una tupla con el año y la cantidad de películas estrenadas en ese año, y al hacer **key=lambda x: x[1]** se ordenará en base al segundo elemento de la tupla, es decir, la cantidad de películas estrenadas.

    ```python
    list_years = sorted_list_years[:3]
    ```
    Luego, se crea una lista **list_years** que contiene los 3 primeros elementos de la lista **sorted_list_years**. Estos son tuplas con el año y la cantidad de películas estrenadas en ese año, ordenados de forma descendente según la cantidad de películas estrenadas.

    ```python
    print("Los 3 años con más películas estrenadas son:")
    for data in list_years:
        print(f"El año {data[0]} con {data[1]} películas producidas")
    ```
    Finalmente, se imprime un mensaje indicando que se mostrarán los 3 años con más películas estrenadas y se recorre la lista **list_years** para imprimir el año y la cantidad de películas estrenadas en ese año.

- Encuentre a los 5 actores con la trayectoria más larga, es decir, mayor cantidad de a ̃nos actuando.

    ```python
    def longer_track_record(actors_dict):
        sorted_list_actors = sorted(actors_dict.values(), key=lambda x: x.years_track_record(), reverse=True)
        list_actors = sorted_list_actors[:5]
        print("Los 5 actores con trayectoria más larga son:")
        for actor in list_actors:
            print(f"{actor.full_name} con {actor.years_track_record()} años de trayectoria")
    ```

    ```python
    longer_track_record(actors_dict)
    ```

    ```
    Los 5 actores con trayectoria más larga son:
    . con 101 años de trayectoria
    and con 98 años de trayectoria
    Harrison Ford con 98 años de trayectoria
    Gloria Stuart con 80 años de trayectoria
    Lillian Gish con 75 años de trayectoria
    ```

    A continuación, se explicará a detalle el código:

    ```python
    sorted_list_actors = sorted(actors_dict.values(), key=lambda x: x.years_track_record(), reverse=True)
    ```

    En primer lugar, **actors_dict.values()** retorna una lista con los valores del diccionario **actors_dict**, es decir, una lista con los objetos de la clase **Actor**. Luego, se utiliza la función **sorted** para ordenar la lista de actores en base a la cantidad de años de trayectoria de cada actor esto al usar el método **years_track_record** de la clase **Actor**. La función **sorted** recibe como argumentos la lista de actores, una función lambda que retorna la cantidad de años de trayectoria de cada actor y **reverse=True** para ordenar de forma descendente.

    ```python
    list_actors = sorted_list_actors[:5]
    ```

    Luego, se crea una lista **list_actors** que contiene los 5 primeros elementos de la lista **sorted_list_actors**. Estos son objetos de la clase **Actor** ordenados de forma descendente según la cantidad de años de trayectoria de cada actor.

    ```python
    print("Los 5 actores con trayectoria más larga son:")
    for actor in list_actors:
        print(f"{actor.full_name} con {actor.years_track_record()} años de trayectoria")
    ```
    Finalmente, se imprime un mensaje indicando que se mostrarán los 5 actores con la trayectoria más larga y se recorre la lista **list_actors** para imprimir el nombre de cada actor y la cantidad de años de trayectoria.

- Encuentre el reparto de una película (2 o más actores) que se haya repetido completo en otras la mayor
cantidad de veces.

    ```python
    def most_repeated_casts(movies_dict):
        cast_dict = {}
        for movie in movies_dict.values():
            if len(movie.cast) >= 2:
                if movie.cast not in cast_dict:
                    cast_dict[movie.cast] = 0
                cast_dict[movie.cast] += 1
            else:
                continue
        max_value = max(cast_dict.items(), key=lambda x: x[1])
        print(f"El reparto de una película que más se ha repetido es {max_value[0]} con {max_value[1]} repeticiones")
    ```

    ```python
    most_repeated_casts(movies_dict)
    ```

    ```
    El reparto de una película que más se ha repetido es (Harold Lloyd, Bebe Daniels) con 44 repeticiones
    ```

    A continuación, se explicará a detalle el código:

    ```python
    cast_dict = {}
    for movie in movies_dict.values():
        if len(movie.cast) >= 2:
            if movie.cast not in cast_dict:
                cast_dict[movie.cast] = 0
            cast_dict[movie.cast] += 1
        else:
            continue
    ```

    En primer lugar, **movies_dict.values()** retorna una lista con los valores del diccionario **movies_dict**, es decir, una lista con los objetos de la clase **Movie**. Luego, se crea un diccionario **cast_dict** que contendrá los elencos de las películas que se han repetido y la cantidad de veces que se han repetido. Se recorre la lista de películas para contar la cantidad de veces que se ha repetido un elenco completo. Para cada película, se verifica si la cantidad de actores en el elenco es mayor o igual a 2. Si es así, se verifica si el elenco de la película no está en el diccionario **cast_dict**. Si no está, se agrega el elenco al diccionario con un valor de 0. Luego, se incrementa en 1 la cantidad de veces que se ha repetido ese elenco. Si no es mayor o igual a 2, se continua con la siguiente película. Recordar que el elenco de una película es una tupla de actores, en particular, fue para lograr esto que se guardo el elenco de cada película como una tupla ya que los diccionarios no pueden tener objetos mutables como llave, como es el caso de las listas por ejemplo. Es decir el contenido del contenido **cast_dict** es de la forma: **((actor1, actor2, actor3), 3)**.

    ```python
    max_value = max(cast_dict.items(), key=lambda x: x[1])
    ```

    Luego, se utiliza la función **max** para encontrar el elenco que se ha repetido la mayor cantidad de veces. La función **max** recibe como argumentos el diccionario **cast_dict.items()**, una lista de tuplas con los elementos del diccionario, y una función lambda que retorna la cantidad de veces que se ha repetido el elenco, esto dado que **x** será una tupla con el elenco y la cantidad de veces que se ha repetido. Al hacer **key=lambda x: x[1]** se ordenará en base al segundo elemento de la tupla, es decir, la cantidad de veces que se ha repetido el elenco.

    ```python
    print(f"El reparto de una película que más se ha repetido es {max_value[0]} con {max_value[1]} repeticiones")
    ```

    Finalmente, se imprime un mensaje indicando que se mostrará el elenco que se ha repetido la mayor cantidad de veces y se imprime el elenco y la cantidad de veces que se ha repetido.