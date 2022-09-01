## <center>Proyecto de ciclo de Grado Superior de Desarrollo de Aplicaciones Multiplataforma<center>

&nbsp;

# <center>APP Gestor de Formación en Centros de Trabajo</center>

&nbsp;

&nbsp;

### <center>David Molina Gámez</center>

### <center>2º DAM</center>

&nbsp;

&nbsp;

## 0. Índice

- ### [1. Resumen del Proyecto](#resumen)
- ### [2. Introducción](#introduccion)
- ### [3. Requisitos](#requisitos)
  - ### [3.1 Funcionales](#r_funcionales)
  - ### [3.2 No Funcionales](#r_no_funcionales)
  - ### [3.3 Prototipo del Boceto](#boceto)
- ### [5. Tecnologías](#tecnologias)
- ### [6. Aclaraciones en el código](#code)
- ### [7. Resultados](#resultados)
- ### [8. Conclusiones](#conclusiones)
- ### [9. Anexos](#anexos)

&nbsp;

## <a id="resumen">1. Resumen del proyecto</a>

En este proyecto se propone una aplicación móvil para docentes encargados de la Formación en Centros de Trabajo de los alumnos de Formación Profesional.

Se espera obtener una aplicación simple e intuitiva para que todos los profesores puedan gestionar de forma cómoda, sencilla y rápida las prácticas de FCT de los alumnos del centro educativo.

&nbsp;

## <a id="introduccion">2. Introducción</a>

Este proyecto busca informatizar e implementar una solución más rápida y accesible para aquellos docentes que ya utilizan un sistema manual de gestión de visitas en sus centros.

La idea final de este proyecto es poder implementarlo en centros educativos que necesiten controlar las visitas a las empresas y la Formación en Centros de Trabajo de sus alumnos.

&nbsp;

&nbsp;

&nbsp;

## <a id="requisitos">3. Requisitos</a>

Pequeña descripción de los requisitos del proyecto.

&nbsp;

## &nbsp;&nbsp;&nbsp;&nbsp;<a id="r_funcionales">3.1 Requisitos Funcionales</a>

|                                                               Requisito                                                                |       Estado        |
| :------------------------------------------------------------------------------------------------------------------------------------: | :-----------------: |
|                                                            CRUD de empresas                                                            | :white_check_mark:  |
|                                                            CRUD de acuerdos                                                            | :white_check_mark:  |
|                                                            CRUD de visitas                                                             | :white_check_mark:  |
|                                             Administrador puede autorizar o no las visitas                                             | **No implementado** |
| Conexión con la API de Google Places para encontrar las empresas con mayor facilidad, calcular dietas en función de la distancia, etc. | **No implementado** |
|                               Los profesores podrán activar o desactivar las notificaciones desde la app                               | **No implementado** |

&nbsp;

## &nbsp;&nbsp;&nbsp;&nbsp;<a id="r_no_funcionales">3.2 Requisitos No Funcionales</a>

|                                                          Requisito                                                          |       Estado        |
| :-------------------------------------------------------------------------------------------------------------------------: | :-----------------: |
|                   No habrá registros en la app móvil. Será el administrador quien añada nuevos usuarios.                    | :white_check_mark:  |
|                        Los profesores no podrán crear alumnos, también lo harán los administradores.                        | :white_check_mark:  |
| Notificaciones para que los administradores sepan cuando se ha creado una visita y poder aceptarlas de forma casi inmediata | :white_check_mark:  |
|                Importar datos de alumnos y profesores desde un CSV para hacer el volcado a la base de datos.                | **No implementado** |
|                   Acceso con google (correo corporativo) o con biometría (si el dispositivo lo permite).                    | **No implementado** |
|            Si la fecha de la visita es anterior a la del día actual, se elimina de la lista de próximas visitas             | **No implementado** |

&nbsp;

&nbsp;

&nbsp;

## &nbsp;&nbsp;&nbsp;&nbsp;<a id="boceto">3.3 Prototipo del Boceto</a>

![](assets/Proyecto.png)

&nbsp;

&nbsp;

## <a id="tecnologias">5. Tecnologías</a>

<p align="center">
<a><img src="https://skillicons.dev/icons?i=nodejs,express,mysql,ts,graphql,flutter,dart,md,figma"/></a>
</p>

- ### BackEnd

  El backend está realizado usando Express, NodeJS y MySQL, el código está escrito en Typescript.

  La API consiste en una API REST con dos endpoints: el del login y el de las consultas a la BBDD, hecho con GraphQL.

    <br/>

- ### FrontEnd

  El frontend está realizado usando Flutter.

    <br/>
  La documentación del proyecto está generada en Markdown. Los diseños y bocetos de la aplicación están hechos con Figma.

&nbsp;

&nbsp;

&nbsp;

&nbsp;

## <a id="code">6. Aclaración de código</a>

Explicación del código más complejo de la aplicación.

- ### FRONT

  - ### Animación del HomeScreen

    Para la animación del botón de añadir usé en flutter un componente con estados ([`StatefulWidget`](https://api.flutter.dev/flutter/widgets/StatefulWidget-class.html)). Dentro del estado del componente tenemos definidos el controlador de la animación ([`AnimationController`](https://api.flutter.dev/flutter/animation/AnimationController-class.html)) y la propia animación ([`Animation`](https://api.flutter.dev/flutter/animation/Animation-class.html))

    ```dart
    class _AddComponentState extends State<AddComponent> with SingleTickerProviderStateMixin {
        late AnimationController _controller;
        late Animation _animation;

        ...
    }
    ```

    En cuanto se renderiza este componente (`initState`), inicializamos el controlador, la animación y le añadimos un `eventListener` al controlador.

    ```dart
    @override
    void initState() {
        super.initState();
        _controller = AnimationController(
            vsync: this,
            duration: const Duration(miliseconds: 350),
            reverseDuration: const Duration(miliseconds: 275)
        );

        _animation = CurvedAnimation(
            parent: _controller,
            curve: Curves.easeOut,
            reverseCurve: Curves.easeIn
        );

        _controller.addListener(() {
            setState(() {});
        });
    }
    ```

    <br>

    También tenemos un método `dispose()` que se encarga de eliminar la animación del árbol de componentes (`Widget Tree`) cuando el objeto sea destruido (cuando navegamos a otra pantalla)

    ```dart
    @override
    void dispose() {
        super.dispose();
    }
    ```

    Finalmente definimos la función `build()` que se encargará de crear el componente. El botón principal tiene definida la función `onPressed()`:

    ```dart
    onPressed: () {
        setState(() {
            if (toggle) {
                toggle = !toggle;
                _controller.forward();
                Future.delayed(const Duration(miliseconds: 10), () {
                    alignment1 = const Alignment(-0.4, -2.8);
                });
                Future.delayed(const Duration(miliseconds: 10), () {
                    alignment1 = const Alignment(0, -5);
                });
                Future.delayed(const Duration(miliseconds: 10), () {
                    alignment1 = const Alignment(-0.4, -2.8);
                });
            } else{
                toggle = !toggle;
                _controller.reverse();
                alignment1 = const Alignment(0, 0);
                alignment2 = const Alignment(0, 0);
                alignment3 = const Alignment(0, 0);
            }
        });
    }
    ```

    &nbsp;

  - ### GraphQL Queries
    Para ver un ejemplo, usaremos el ReadAll de Alumnos. Para hacer una query a graphql desde flutter primero definimos la query dentro de un string:

  ```dart
  final String readAllStudents = """
    query {
        getAllStudents {
            id
            firstName
            lastName
        }
    }
  """;
  ```

  > En esta consulta, sólo estamos cogiendo el nombre y los apellidos del alumno y el id (que usaremos como parámetro para el getById en los DetallesAlumno).

  Una vez definida la consulta, en el build del componente retornamos la Query:

  ```dart
  @override
  Widget build(BuildContext context) {
    return Query(
        // En las options, pasamos el graphQLDocument de la query (el que hemos creado antes)
        options: QueryOptions(document: gql(readAllStudents)),

        // La función callback `refetch()` nos servirá para recargar la página
        // La función `fetchMore()` nos servirá para la paginación de los datos
        builder: (QueryResult result, {VoidCallback? refetch, FetchMore? fetchMore}) {

            // Si el resultado contiene errores, llamamos al componente Error con el mensaje
            // que queramos (en este caso, directamente el que nos devuelve el back)
            if (result.hasException) {
                return Error(
                    error: result.exception.toString(),
                );
            }

            // Si los datos están cargando, mostramos un icono de progreso circular en el
            // centro de la pantalla.
            if (result.isLoading) {
                return const Center(
                    child: CircularProgressIndicator(),
                );
            }

            List? students = result.data!['getAllStudents'];

            // Si la petición es válida pero la lista está vacía, retorna un mensaje de no hay
            // alumnos todavía.
            if (students == null) {
                return const Error(error: 'No hay alumnos todavía');
            }


            Future<void> _refresh() async {
                refetch();
                return Future.delayed(const Duration(miliseconds: 500));
            }

            // Este RefreshIndicator nos servirá para poder recargar haciendo scroll down
            return RefreshIndicator(
                onRefresh: _refresh,
                child: ListView.builder(
                    itemCount: students.length,
                    itemBuilder: (context, index) {
                        final student = students[index];

                        return StudentCard(
                            firstName: student['firstName'],
                            firstName: student['lastName'],
                            firstName: student['id']
                        );
                    }
                ),
            );
        }
    );
  }
  ```

<br />

- ### BACK

  - ### Bot para las notificaciones

    Quería implementar notificaciones en mi APP, pero como no pude llegar a hacerlas desde Flutter, decidí hacer un Bot de Telegram para los administradores de la aplicación. Este bot se encarga de informar a los administradores de que cualquier otro profesor ha creado una visita nueva.

    Esto es debido a que los administradores (director, vicedirector, etc.) tienen que aceptar o declinar una visita que haya creado algún profesor.

    ```ts
    const bot = require("node-telegram-bot-api");
    const token = "....";

    export const _bot = new bot(token, { polling: true });
    export let BOT_ID;

    _bot.onText(/^\/start/, (msg) => {
      BOT_ID = msg.chat.id;
      _bot.sendMessage(
        BOT_ID,
        "Bienvenido al chat de notificaciones de la app gestorFCT"
      );
    });
    ```

    Cuando creamos una nueva visita, mandamos la notificación:

    ```ts
    _bot.sendMessage(
      BOT_ID,
      `El profesor ${user.firstName} ${user.lastName} ha creado una nueva visita en la fecha ${date}`
    );
    ```

&nbsp;

## <a id="resultados">7. Resultados </a>

Puedes descargar el APK de la aplicación generada [aquí](https://gitlab.iesvirgendelcarmen.com/Dmolina23/appgestorfct/-/blob/master/build/gestorFCT.apk).

Para las notificaciones, puedes añadir el bot en telegram desde este link: [t.me/gestorfct_bot](https://t.me/gestorfct_bot)

&nbsp;

## <a id="conclusiones">8. Conclusiones</a>

Hacer esta aplicación para la gestión de la formación en centros de trabajo de los alumnos de Formación Profesional ha supuesto todo un reto ya que desde el primer momento quería que fuera 100% funcional y poder implementarla en el centro en un futuro. Debido a la falta de tiempo, no he podido implementar todas las funcionalidades que me hubiese gustado; aun así, creo que he podido llegar a completar casi todo lo planteado y con un buen resultado.

Como he dicho, ha supuesto un reto por el tiempo, pero también por las tecnologías usadas en el desarrollo. He usado tecnologías totalmente nuevas a las que hemos aprendido en clase, como Flutter o GraphQL.

Aunque estas tecnologías eran nuevas para mí, considero que este proyecto me ha servido para conocerlas más en profundidad y aprender a desarrollar con estas tecnologías que están actualmente en auge.

&nbsp;

## <a id="anexos">9. Anexos</a>

- ### Repositorio del Frontend

  En este repositorio está todo el código del front, pero en [este repositorio](https://gitlab.iesvirgendelcarmen.com/Dmolina23/gestorfct) puedes encontrar todos los commits del frontend desde el inicio del proyecto

- ### Repositorio del Backend

  En este repositorio está todo el código del front, pero en [este repositorio](https://gitlab.iesvirgendelcarmen.com/Dmolina23/gestorfct-back) puedes encontrar todos los commits del backend desde el inicio del proyecto.

- ### Presentación del Proyecto
  Puedes ver la presentación del proyecto [aquí](https://www.canva.com/design/DAFECrw9hUs/COXv8y-L0ze5Jvb9SOFrkA/edit?utm_content=DAFECrw9hUs&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton), o descargarla en PDF desde [aquí]()
