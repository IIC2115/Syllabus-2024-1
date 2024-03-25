# Ejercicio Formativo 2 Capítulo 1

## Indice

- [Video Explicativo](#video-explicativo)
- [Análisis del archivo json](#análisis-del-archivo-json)
- [Código](#código)
  - [Cargando el archivo json](#cargando-el-archivo-json)
  - [Creando clases](#creando-clases)
  - [Análisis previo a la creación de objetos](#análisis-previo-a-la-creación-de-objetos)
  - [Cargando datos en objetos](#cargando-datos-en-objetos)
  - [Análisis del código anterior](#análisis-del-código-anterior)
  - [Consideraciones](#consideraciones)
  - [Estado actual de las variables](#estado-actual-de-las-variables)
  - [Consultas](#consultas)
    - [Encuentre los 5 géneros más populares](#encuentre-los-5-géneros-más-populares)
    - [Encuentre los 3 años con más películas estrenadas](#encuentre-los-3-años-con-más-películas-estrenadas)
    - [Encuentre a los 5 actores con la trayectoria más larga, es decir, mayor cantidad de años actuando](#encuentre-a-los-5-actores-con-la-trayectoria-más-larga-es-decir-mayor-cantidad-de-años-actuando)
    - [Encuentre el reparto de una película (2 o más actores) que se haya repetido completo en otras la mayor cantidad de veces](#encuentre-el-reparto-de-una-película-2-o-más-actores-que-se-haya-repetido-completo-en-otras-la-mayor-cantidad-de-veces)

## Video Explicativo

El video explicativo se encuentra en el siguiente [enlace](https://youtu.be/Y6ORKZIe2Uc)

## Análisis del archivo json

El archivo json es un arreglo de objetos json, donde cada objeto json representa una película. Cada objeto json tiene los siguientes atributos:

- **title**: Título de la película.
- **year**: Año de estreno de la película.
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

    - **full_name**: Nombre del actor.
    - **n_movies**: Número de películas en las que ha participado el actor.
    - **start_career**: Año en que comenzó la carrera del actor.
    - **end_career**: Año en que terminó la carrera del actor.

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

    Lo que hace este fragmento de código es crear una clase **Actor** con un constructor que recibe como parámetros el nombre completo del actor y el año en que comenzó su carrera. Además, tiene un método **update_track_record** que recibe como parámetro el año de una película, un int, y actualiza el año de inicio y fin de la carrera del actor. Lo que hará este mpétodo es actualizar el año de inicio de la carrera del actor si el año de la película es menor al año de inicio de la carrera del actor y actualizar el año de fin de la carrera del actor si el año de la película es mayor al año de fin de la carrera del actor.Se debe tener en cuenta que el año de inicio de la carrera del actor corresponderá al año de estreno de la película más antigua en la que ha participado y el año de fin de la carrera del actor corresponderá al año de estreno de la película más reciente en la que ha participado. Recordar que para ver si un actor ha partcicipado en una película se debe revisar el atributo **cast** del diccionario con los datos de la pelicula.
    
    También tienen un método **years_track_record** que retorna la cantidad de años que el actor ha estado en la industria, esto sería la diferencia entre el año en que termino su carrera y el año en que comenzó su carrera. Por último, también se tiene un método **__repr__** que sirve para representa el objeto de la clase **Actor** como un string, en este caso, se retorna el nombre completo del actor.

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
    - **year**: Año de estreno de la película.
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

    También tiene un método **__repr__** que sirve para representa el objeto de la clase **Movie** como un string, en este caso, se retorna el título, el año, el elenco y los géneros de la película.

### Análisis previo a la creación de objetos

Antes de empezar con el código que crea los objetos de las clases **Actor**, **Genre** y **Movie**, se debe tener en cuenta unos detalles importantes:

- En la información del archivo json, que ahora esta en la variable **movies**, se puede encontrar peliculas que comparten el mismo título y año, pero que tienen actores o géneros distintos, estás películas serán tratadas como películas distintas, es decir, se crearán objetos de la clase **Movie** distintos para cada una de ellas.

- Se debe tener en cuenta que varios actores participaron en más de una película, sin embargo, lo esperado es que se cree un objeto de la clase **Actor** por cada actor, es decir, que no se creen actores duplicados. Por ende los objetos de la clase **Actor** deben ser únicos.

- Se debe tener en cuenta que varios géneros pueden estar presentes en más de una película, sin embargo, lo esperado es que se cree un objeto de la clase **Genre** por cada género, es decir, que no se creen géneros duplicados. Por ende los objetos de la clase **Genre** deben ser únicos.


### Cargando datos en objetos

Las estructuras de datos seleccionada para para almacenar juntas todas las entidades del mismo tipo, fueron los **diccionarios** para los tres tipos de entidades, esto es, **actors_dict**, **genres_dict** y **movies_dict**. A continuación se muestra el fragmento de código que se encarga de cargar los datos en los objetos de las clases **Actor**, **Genre** y **Movie** y almacenarlos en los diccionarios correspondientes.

```python
actors_dict = {}
genres_dict = {}
movies_dict = {}
```

En este fragmento solamente se crean los diccionarios que contendrán los objetos de las clases **Actor**, **Genre** y **Movie**.

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
   ...
for movie in movies:
    cast = []
    genres = []
    ....
```
Tener en cuenta que **movies** es una lista de diccionarios, donde cada diccionario representa una película. En este fragmento se recorre la lista de películas. Para cada película, se inicializan las listas **cast** y **genres** que contendrán los actores y los géneros de la película actual.

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

En el segmento anterior se mencionaba que se debía tener en cuenta que varios actores participaron en más de una película, sin embargo, lo esperado es que se cree un objeto de la clase **Actor** por cada actor.Teniendo en cuenta que para crear un objeto de la clase **Actor** es necesario entregar el nombre del actor y el año de la película actual y que los actores se diferenciarán por su nombre, lo que se hará es que la primera vez que se encuentre el nombre del actor se creará un objeto de la clase **Actor** con el nombre del actor y el año de la película actual, y las siguientes veces que se encuentre el nombre del actor se buscará el objeto **Actor** ya creado en el diccionario. Para realizar esto es precisamente que se utilizan los diccionarios, en este caso **actors_dict**.

El diccionario **actors_dict** tendrá como llave el nombre del actor y como valor el objeto de la clase **Actor** correspondiente.

En este fragmento, se recorre la lista de actores de la película actual. Para cada actor, se verifica si el actor no está en el diccionario **actors_dict**, esto haciendo **if actor not in actors_dict**, que revisrá si el nombre del actor no está en las llaves del diccionario. Si no está, se crea un objeto de la clase **Actor** con el nombre del actor y el año de la película actual y se agrega al diccionario **actors_dict** usando el nombre del actor como llave. Recordar que **nombre_diccionario[llave] = valor** es solo otra forma de agregar un elemento a un diccionario. Luego se agrega el objeto **Actor** al arreglo **cast**. Si ya existe, lo que se hace es obtener el objeto **Actor** del diccionario **actors_dict**, haciendo **actors_dict[actor]**, y se hace que ejecute el método **update_track_record** con el año de la película actual, y finalmente se agrega el objeto **Actor** al arreglo **cast**.

```python
    ....
    for genre in movie["genres"]:
        if genre not in genres_dict:
            genres_dict[genre] = Genre(genre)
        genres.append(genres_dict[genre])
    ....
```

De modo similar a lo que se hizo con los actores, se debe tener en cuenta que varios géneros pueden estar presentes en más de una película, sin embargo, lo esperado es que se cree un objeto de la clase **Genre** por cada género. Teniendo en cuenta que para crear un objeto de la clase **Genre** es necesario entregar el nombre del género y que los géneros se diferenciarán por su nombre, lo que se hará es que la primera vez que se encuentre el nombre del género se creará un objeto de la clase **Genre** con el nombre del género y las siguientes veces que se encuentre el nombre del género se buscará el objeto **Genre** ya creado en el diccionario. Para realizar esto es precisamente que se utilizan los diccionarios, en este caso **genres_dict**.

El diccionario **genres_dict** tendrá como llave el nombre del género y como valor el objeto de la clase **Genre** correspondiente.

En este fragmento, se recorre la lista de géneros de la película actual. Para cada género, se verifica si el género no está en el diccionario **genres_dict**, esto haciendo **if genre not in genres_dict**, que revisrá si el nombre del género no está en las llaves del diccionario. Si no está, se crea un objeto de la clase **Genre** con el nombre del género y se agrega al diccionario **genres_dict** usando el nombre del género como llave. Luego se agrega el objeto **Genre** al arreglo **genres**. En caso de que ya exista, se obtiene el objeto **Genre** del diccionario **genres_dict**, haciendo **genres_dict[genre]**, y se agrega al arreglo **genres** solamente.

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

Para esta parte del código las listas **cast** y **genres** ya están completas, es decir, ya se crearon los objetos de las clases **Actor** y **Genre** correspondientes a los actores y géneros de la película actual. En este fragmento se verifica si la película actual ya está en el diccionario **movies_dict**. Para esto se utiliza una tupla con el título de la película, el año de la película y el elenco de la película. Esto se hace para que no se creen películas duplicadas, es decir, que no se creen objetos de la clase **Movie** para películas que ya existen en el diccionario. Si la película no está en el diccionario, se crea un objeto de la clase **Movie** con el título, el año, el elenco y los géneros de la película actual y se agrega al diccionario **movies_dict** usando una tupla con el título, el año y el elenco de la película como llave. Si la película ya está en el diccionario, se imprime un mensaje indicando que la película está duplicada y se muestra la película que ya está en el diccionario.

### Análisis del código anterior

En el fragmento de código anterior se utilizaron varias estructuras de datos para lograr el resultado esperado, a continuación se explicará a detalle el uso de estas estructuras de datos:

- Los diccionarios **actors_dict**, **genres_dict** y **movies_dict** se utilizaron para almacenar los objetos de las clases **Actor**, **Genre** y **Movie** respectivamente. Estos diccionarios se utilizaron para evitar la creación de objetos duplicados, es decir, para que no se creen objetos de la clase **Actor** o **Genre** si ya existen en el diccionario. Además, se utilizaron para poder acceder a los objetos de las clases **Actor** y **Genre** de forma eficiente, ya que se puede acceder a un objeto en tiempo constante si se conoce su llave.

- Las listas **cast** y **genres** se utilizaron para almacenar los objetos de las clases **Actor** y **Genre** de la película actual. Estas listas se utilizaron para poder crear el objeto de la clase **Movie** con el elenco y los géneros de la película actual.

- Las tuplas se utilizaron para crear una llave única para cada película en el diccionario **movies_dict**. La tupla con el título, el año y el elenco de la película se utilizó como llave para evitar la creación de películas duplicadas.

Se debe tener en cuenta que se aprovecharon los distintos beneficios de las estructuras de datos utilizadas, como la eficiencia en el acceso a los objetos de los diccionarios, la capacidad de ir añadiendo elementos de las listas y la inmutabilidad de las tuplas. Esto último también era necesario ya que los diccionarios no podían tener como llave un objeto mutable como una lista, por lo que se utilizó una tupla en su lugar.

También es importante tener en cuenta que dado que se recorrio por completo la variable **movies** y se crearon los objetos de las clases **Actor**, **Genre** y **Movie** correspondientes, simplemente se iba actualizando los valores de los atributos de los distintos objetos, por lo que al finalizar el ciclo **for** se tendrán los objetos con los valores correctos. Esto es, los objetos de la clase **Actor** tendrán el número de películas en las que han participado y los años de inicio y fin de su carrera, los objetos de la clase **Genre** tendrán el número de películas que pertenecen a ese género y los objetos de la clase **Movie** tendrán el título, el año, el elenco y los géneros de la película correspondiente.

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

Es en las funciónes **sorted** y **max** donde se utilizan las funciones lambda, ya que estas funciones permiten definir una función pequeña y sencilla en una sola línea de código. Y estás funciones necesitan una función que les indique como ordenar o encontrar el máximo de una secuencia de elementos, se debe recordar que Python no puede ordenar directamente objetos de una clase, por lo que se debe indicar como ordenar o encontrar el máximo de estos objetos. También se podría usar funciones convencionales, es decir, del tipo **def**, pero en este caso se prefirió usar funciones lambda por su simplicidad y porque no se necesitaban funciones más complejas.

### Estado actual de las variables

En este punto, se tienen los diccionarios **actors_dict**, **genres_dict** y **movies_dict** con los objetos de las clases **Actor**, **Genre** y **Movie** respectivamente. En específico la estructura de cada uno es la siguiente:

- **actors_dict**: Diccionario con los nombres de los actores como llaves y los objetos de la clase **Actor** como valores.

- **genres_dict**: Diccionario con los nombres de los géneros como llaves y los objetos de la clase **Genre** como valores.

- **movies_dict**: Diccionario con tuplas que contienen el título, el año y el elenco de las películas, que es a su vez una tupla de objetos de la clase **Actor**, como llaves y los objetos de la clase **Movie** como valores.

### Consultas

Para el desarrollo de cada consulta se creo una función que obtiene los solicitado y luego se llama a la función con los datos necesarios. Los parametros de las funciones son los diccionarios **actors_dict**, **genres_dict** y **movies_dict**, si bien los parámetros tienen el mismo nombre de estas variables, en teoría se podría cambiar el nombre de los parámetros y el código seguiría funcionando, se dejaron estos nombres para que sea más fácil de entender el código.

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

    En primer lugar, **genres_dict.values()** retorna una lista con los valores del diccionario **genres_dict**, es decir, una lista con los objetos de la clase **Genre**, me referiré a ello como la **lista de géneros**. Esta sería de la siguiente manera: **[Genre1, Genre2, Genre3, ..., GenreN]**.
    
    A continuación, lo que se buscará será ordenar esta lista de géneros en base al número de películas de cada género. Para esto, se utiliza la función **sorted** para ordenar la cual recibe como argumentos la **lista de géneros**, una función lambda que retorna el número de películas de cada género, esto accediendo al atributo **n_movies** de cada objeto de la clase **Genre**, y **reverse=True** para ordenar de forma descendente. Cuando se hace **key=lambda x: x.n_movies** esto significa que el **x** será un objeto de la clase **Genre** y se ordenará en base al atributo **n_movies** de cada objeto. Siguiendo con el código, se tiene que **sorted** retorna una lista con los objetos de la clase **Genre** ordenados de forma descendente según el número de películas de cada género y los guarda en la variable **sorted_list_genres**.

    ```python
    most_popular_genres = sorted_list_genres[:5]
    ```

    Luego, se crea una lista **most_popular_genres** que contiene los 5 primeros elementos de la lista **sorted_list_genres**. 

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

    En primer lugar, se crea un diccionario **premiers_dict** que tendrá como llave el año de estreno de la película y como valor la cantidad de películas estrenadas en ese año. Se recorre el diccionario **movies_dict** para contar la cantidad de películas estrenadas por año. Para cada película, se verifica si el año de la película no está en el diccionario **premiers_dict**. Si no está, se agrega el año al diccionario con un valor de 0. Luego, se incrementa en 1 la cantidad de películas estrenadas en ese año. Sí está, se incrementa en 1 la cantidad de películas estrenadas en ese año.

    ```python
    sorted_list_years = sorted(premiers_dict.items(), key=lambda x: x[1], reverse=True)
    ```

    Luego se busca ordenar los años en base a la cantidad de películas estrenadas en cada año. Para esto, se utiliza la función **sorted** para ordenar el diccionario **premiers_dict**. Lo que hace **premiers_dict.items()** es entregar una lista de tuplas con los elementos del diccionario, es decir, una lista de tuplas donde cada tupla contiene un año y la cantidad de películas estrenadas en ese año. Luego, se utiliza la función **sorted** de manera similar a la consulta anterior, pero en este caso se ordena en base a la cantidad de películas estrenadas en cada año. Esto significa que el **x** será una tupla con el año y la cantidad de películas estrenadas en ese año, y al hacer **key=lambda x: x[1]** se ordenará en base al segundo elemento de la tupla, es decir, la cantidad de películas estrenadas. La lista retornada por **sorted** se guarda en la variable **sorted_list_years**.

    ```python
    list_years = sorted_list_years[:3]
    ```
    Por último, se crea una lista **list_years** que contiene los 3 primeros elementos de la lista **sorted_list_years**. Estos son tuplas con el año y la cantidad de películas estrenadas en ese año, ordenados de forma descendente según la cantidad de películas estrenadas.

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

    Se debe recordar que los objetos de la clase **Actor** tienen un método **years_track_record** que retorna la cantidad de años de trayectoria del actor. También que **actors_dict.values()** retorna una lista con los valores del diccionario **actors_dict**, es decir, una lista con los objetos de la clase **Actor**. Para encontrar a los 5 actores con la trayectoria más larga, se debe ordenar la lista de actores en base a la cantidad de años de trayectoria de cada actor.
    
    Para esto, se utiliza la función **sorted**, que recibe como argumentos la lista de actores, una función lambda que retorna la cantidad de años de trayectoria de cada actor, y **reverse=True** para ordenar de forma descendente. Al hacer **key=lambda x: x.years_track_record()** se ordenará en base a la cantidad de años de trayectoria de cada actor. La lista retornada por **sorted** se guarda en la variable **sorted_list_actors**.

    ```python
    list_actors = sorted_list_actors[:5]
    ```

    Se crea una lista **list_actors** que contiene los 5 primeros elementos de la lista **sorted_list_actors**. Estos son los 5 actores con la trayectoria más larga.

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

    En primer lugar, se creará un diccionario **cast_dict** que tendrá como llave el elenco de la película y como valor la cantidad de veces que se ha repetido ese elenco completo en otras películas. Se debe recordar que el elenco es un tupla de objetos de la clase **Actor**. Se recorre el diccionario **movies_dict** para contar la cantidad de veces que se ha repetido un elenco completo en otras películas. Para cada película, se verifica si la cantidad de actores en el elenco es mayor o igual a 2. Si es así, se verifica si el elenco de la película ha sido usado como llave en el diccionario **cast_dict**. Si no ha sido usado, se agrega el elenco al diccionario con un valor de 0. Luego, se incrementa en 1 la cantidad de veces que se ha repetido ese elenco. Si el elenco tiene menos de 2 actores, se continua con la siguiente película. Entonces el estado del diccionario **cast_dict** sería algo como esto: **{(Actor1, Actor2, Actor3, ..., ActorN): cantidad_repeticiones, (Actor1, Actor2, Actor3, ..., ActorN): cantidad_repeticiones, ...}**.

    ```python
    max_value = max(cast_dict.items(), key=lambda x: x[1])
    ```

    A continuación, se busca encontrar el elenco que se ha repetido completo en otras películas la mayor cantidad de veces. Para esto, se utiliza la función **max** para encontrar el elemento máximo del diccionario **cast_dict**. Lo que hace **cast_dict.items()** es entregar una lista de tuplas con los elementos del diccionario, es decir, una lista de tuplas donde cada tupla contiene un elenco y la cantidad de veces que se ha repetido ese elenco. Luego, se utiliza la función **max** de manera similar a la consulta anterior, pero en este caso se busca el elenco que se ha repetido la mayor cantidad de veces. Esto significa que el **x** será una tupla con el elenco y la cantidad de veces que se ha repetido ese elenco, y al hacer **key=lambda x: x[1]** se buscará el elenco que se ha repetido la mayor cantidad de veces. La tupla retornada por **max** se guarda en la variable **max_value**.

    ```python
    print(f"El reparto de una película que más se ha repetido es {max_value[0]} con {max_value[1]} repeticiones")
    ```

    Finalmente, se imprime un mensaje indicando que se mostrará el elenco que se ha repetido la mayor cantidad de veces y se imprime el elenco y la cantidad de veces que se ha repetido.
