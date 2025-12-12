# Spring MVC

## Modelo-Vista-Controlador (MVC)

El ejemplo visto en Spring Boot es tan sencillo que no necesita un patrón de diseño especial. Para aplicaciones más complejas necesitamos de un patrón que nos permita crear aplicaciones con un código bien estructurado y más fácil de modificar, así como reutilizar sus componentes en diferentes puntos de la aplicación y que puedan evolucionar de manera independiente.

Spring MVC nos proporciona un marco estructurado, flexible y eficiente para construir aplicaciones basadas en el patrón Modelo-Vista-Controlador (MVC) que cumplan todas estas funcionalidades.

El Modelo-Vista-Controlador (MVC) es un patrón de diseño que organiza una aplicación en tres **componentes principales**:

* **Modelo**: Son los datos. Es responsable de:

    * Gestionar el estado de la aplicación.

    * Interactuar con la base de datos u otros servicios para obtener y procesar datos.

    * Proveer datos a la vista.

* **Vista**: Es lo que ve el usuario. Es responsable de:

    * Renderizar información en un formato adecuado, como HTML.

    * Mostrar al usuario los resultados de las acciones ejecutadas.

* **Controlador**: Actúa como intermediario entre el modelo y la vista. Es responsable de:

    * Procesar las solicitudes del usuario (peticiones HTTP).

    * Interactuar con el modelo para obtener o modificar datos.

    * Seleccionar y devolver la vista adecuada para responder al usuario.

        ![](MVC2.png)

!!!tip "Flujo de trabajo en MVC"
    1) El usuario interactúa con la interfaz (Vista), como enviar un formulario o hacer clic en un enlace.

    2) La petición es enviada al Controlador.

    3) El Controlador procesa la petición, interactúa con el Modelo si es necesario, y selecciona la Vista que debe renderizar la respuesta.

    4) La Vista presenta la respuesta al usuario.


## MVC en Spring

Spring MVC está implementado como un servlet (el front controller)
que implementa la gestión de las peticiones del cliente web, y se
encarga de transmitirlas a un controlador adecuado. El controlador
procesa la petición y crea un modelo que contiene los datos a
devolver al usuario. Una vista se encarga de traducir el modelo a una
representación adecuada para el cliente (por ejemplo una página
HTML)


### Anotaciones comunes de Spring MVC:

A continuación se describen las anotaciones más utilizadas en cada uno de los componentes del modelo MVC en el entorno de Spring:

1) **Controlador**{.verde}
   
* **@Controller**: Define una clase como un controlador de Spring MVC. Es la principal anotación utilizada para que Spring la gestione como parte del patrón MVC.
* **@RestController**: Si el controlador está destinado a manejar solicitudes RESTful y no necesita devolver vistas, se utiliza esta anotación, que es una combinación de @Controller y @ResponseBody. Devuelve datos directamente como JSON o XML.
* **@RequestMapping**: Se usa para mapear solicitudes HTTP a métodos de un controlador. Puede configurarse para manejar diferentes tipos de solicitudes HTTP (GET, POST, etc.).
* **@GetMapping, @PostMapping, @PutMapping, @DeleteMapping**: Variantes de @RequestMapping para manejar solicitudes de tipos específicos (GET, POST, PUT, DELETE).
* **@RequestParam**: Usada para obtener parámetros de la URL (query parameters) de la solicitud HTTP.
* **@ModelAttribute**: Usada para pre-poblar un modelo con atributos antes de que se ejecute un método del controlador. Esto es útil, por ejemplo, cuando se usa en formularios.
  

2) **Modelo**{.verde}

* **@Entity**: Si estás utilizando JPA para la persistencia de datos, esta anotación define una clase como una entidad que será mapeada a una tabla de la base de datos.
* **@Table**: Usada junto con @Entity para especificar la tabla en la base de datos que corresponde a la entidad.
  
        @Entity
        @Table(name = "comarcas")
        data class Comarca(
            @Id
            @GeneratedValue(strategy = GenerationType.IDENTITY)
            val id: Int,
            val nombre: String,
            val poblacion: Int
        )

* **@Value**: Si estás utilizando Spring Expression Language (SpEL) en el modelo o en los controladores para asignar valores, puedes usar esta anotación.
  
        @Value("\${comarca.nombre}")
        var nombre: String = ""


3) **Vista**{.verde}

La vista no tiene anotaciones propias en el código fuente, sin embargo, si estás utilizando **Thymeleaf** o **JSP**,  la vista incluye elementos y sintaxis específicos que actúan como directrices para renderizar contenido dinámico.
La vista será un archivo HTML ubicado en **src/main/resources/templates**.

Las anotaciones **@RequestMapping** o **@GetMapping** en el controlador, especifican que el controlador debe devolver una vista. 

<!--
En el siguiente ejemplo, en la carpeta **src/main/resources/templates/comarca/**, puedes tener un archivo **listar.html**, que corresponde a la vista que se renderizará en el navegador. 

    @GetMapping("/comarcas")
    fun listarComarcas(model: Model): String {
        model.addAttribute("comarcas", comarcaService.obtenerComarcas())
        return "comarca/listar"  // Devuelve el nombre de la plantilla Thymeleaf
    }

-->

**Vista con Thymeleaf**{.azul}

Si usas **Thymeleaf** para la vista, las anotaciones en los archivos de plantilla son prefijos para atributos de HTML. Estos prefijos permiten el manejo dinámico de datos en la vista.

<u>Ejemplo</u>

    <h1>Lista de Comarcas</h1>
    <table>
        <tr th:each="comarca : ${comarcas}">
            <td th:text="${comarca.nombre}"></td>
            <td th:text="${comarca.poblacion}"></td>
        </tr>
    </table>


A continuación, se detallan los elementos comunes de las vistas con **Thymeleaf**:

* **th:text**: Rellena el contenido de un elemento HTML con el valor dinámico.
  
        <p th:text="${mensaje}">Mensaje por defecto</p>

* **th:each**: Itera sobre una colección.

        <ul>
            <li th:each="item : ${items}" th:text="${item}"></li>
        </ul>

>>>Esto genera una lista basada en los elementos de la colección items.

* **th:if** y **th:unless**: Renderiza un contenido condicionalmente.
 
        <p th:if="${condicion}">Esto se muestra si la condición es verdadera</p>
        <p th:unless="${condicion}">Esto se muestra si la condición es falsa</p>

* **th:href** y **th:src**: Construye enlaces dinámicos para atributos como href o src.

        <a th:href="@{/ruta/{id}(id=${itemId})}">Ver detalle</a>
        <img th:src="@{/imagenes/logo.png}" alt="Logo">

* **th:action**: Define la URL para un formulario.

        <form th:action="@{/procesar}" method="post">
            <input type="text" name="nombre">
            <button type="submit">Enviar</button>
        </form>

* **th:value** y **th:field**: Usado para rellenar valores dinámicos en campos de formulario.

        <input type="text" th:field="*{nombre}" />

<!--        

**Vista sin motores de plantilla**{.azul}

Si tu aplicación no utiliza motores de plantillas y solo devuelve datos en formatos como JSON o XML, entonces las vistas suelen ser gestionadas directamente por el controlador. En este caso:

**@RestController** en el controlador garantiza que el contenido se devuelva en el formato adecuado, sin necesidad de vistas explícitas.
El contenido es generado dinámicamente a través de bibliotecas como Jackson (para JSON).

<u>Ejemplo</u> de un controlador que devuelve JSON:

    @RestController
    class PersonaRestController {
        @GetMapping("/api/personas")
        fun listarPersonas(): List<Persona> {
            return listOf(
                Persona(1, "Juan"),
                Persona(2, "Ana")
            )
        }
    }

-->

### Primera Aplicación Spring MVC

Al igual que se describe en el apartado de Spring Boot, podemos crear los proyectos Spring MVC de dos maneras:

* Mediante una herramienta web online (<https://start.spring.io/>) denominada **Spring Initializr**, donde por medio de unos parámetros de configuración, genera automáticamente un proyecto Maven o Gradle, según elijamos, en un archivo comprimido Zip, con la estructura de la aplicación para ser importada directamente desde un IDE. 

* Mediante un IDE, como Eclipse, IntelliJ...etc, teniendo instalados los plugins necesarios. 
  
En nuestro caso crearemos un proyecto **Maven** de Spring Boot en **IntelliJ**. La estructura general de un proyecto Spring MVC en IntelliJ sería esta:

![](mvc_estructura.png) 

Los pasos a seguir serían:

  * Configurar el proyecto
  * Añadir las dependencias necesarias.
  * Estructurar el proyecto en los componentes MVC.
  * Configurar el fichero de propiedades.


      
!!!tip "Enunciado de la aplicación"
    En este ejemplo vamos a crear una aplicación sencilla que muestre una lista de nombres de personas, y también permita añadir un nombre de persona nuevo. Todo mediantes **Spring MVC** y **Thymeleaf**.


**Configurar el proyecto**{.azul}
   
1) En **IntelliJ** creamos el proyecto y lo configuramos desde **File-->New-->Project**:
   
* Elige **Spring Boot**.
* Configura las siguientes opciones:
  
    * Language: **Kotlin**
    * Build System: **Maven**

* Especifica un nombre para el proyecto: **PrimerSpringMVCsencillo**
* Última versión de **JDK**
* Última versión de **Java**


2) Posteriormente seleccionamos las **dependencias** necesarias:
  
 * **Spring Web** (para el desarrollo de aplicaciones web)
 * **Thymeleaf** (motor de plantillas para la vista)
 * **Spring Boot DevTools** (opcional, para facilitar el desarrollo)

Después de aceptar, y si todo ha ido correctamente, ya tendremos nuestro proyecto creado y preparado para añadir los elementos de programación.

|   ![ref](mvc_sencillo1.png)  |   ![ref](mvc_sencillo2.png)    |
|---|---|

Al iniciar nuestra aplicación, lo primero que observamos es que se crea
una clase **PrimerSpringMVCsencilloApplication**  que sirve como contenedor para la configuración de la aplicación. No necesita implementar métodos adicionales, ya que Spring Boot se encarga de todo gracias a la anotación **@SpringBootApplication**.

![ref](mvc_sencillo0.png)


**Estructura del proyecto**{.azul}

La estructura del proyecto podría ser esta:

![](mvc_estructurasimple.png) 

* En la carperta **src/main/kotlin/org/example/primerspringmvcsencillo** crearemos los paquetes:
    * controller
    * model
    * service

* En la carperta **src/main/resources/template** crearemos la carpeta **persona** donde añadiremos las vistas que muestren el resultado de la aplicación.

![](mvc_sencillo_estructura.png) 

**Implementación de la aplicación**{.azul}

Ahora ya podemos añadir la programación necesaria para nuestra aplicación siguiendo la estructura MVC creada. Dentro de cada paquete crearemos los siguientes archivos:

* **Model/Persona.kt**:

El modelo Persona es muy simple, solo tendrá un nombre.

    package com.ejemplo.model

    data class Persona(
        val id: Int,
        val nombre: String
    )


* **Service/PersonaService.kt**: 

El servicio se encargará de la lógica de negocio. En este caso, solo vamos a mantener una lista de personas en memoria.

    package com.ejemplo.service

    import org.example.primerspringmvcsencllo.model.Persona
    import org.springframework.stereotype.Service

    @Service
    class PersonaService {

        private val personas = mutableListOf(
            Persona(1, "Juan"),
            Persona(2, "Ana"),
            Persona(3, "Luis")
        )

        fun obtenerPersonas(): List<Persona> = personas

        fun agregarPersona(persona: Persona) {
            personas.add(persona)
        }
    }


* **Controller/PersonaController.kt**

 El controlador maneja las solicitudes de las vistas y realiza la interacción con el servicio.

    package com.ejemplo.controller

    import org.example.primerspringmvcsencllo.model.Persona
    import org.example.primerspringmvcsencllo.service.PersonaService
    import org.springframework.stereotype.Controller
    import org.springframework.ui.Model
    import org.springframework.web.bind.annotation.GetMapping
    import org.springframework.web.bind.annotation.PostMapping
    import org.springframework.web.bind.annotation.RequestParam

        @Controller
        class PersonaController(private val personaService: PersonaService) {

            @GetMapping("/personas")
            fun listarPersonas(model: Model): String {
                model.addAttribute("personas", personaService.obtenerPersonas())
                return "persona/listar"
            }

            @GetMapping("/personas/agregar")
            fun mostrarFormularioAgregar(): String {
                return "persona/agregar"
            }

            @PostMapping("/personas/agregar")
            fun agregarPersona(@RequestParam nombre: String): String {
                val persona = Persona(0, nombre)  // ID auto-generado
                personaService.agregarPersona(persona)
                return "redirect:/personas"
            }
        }

|![ref](mvc_sencillo4.png)|![ref](mvc_sencillo3.png)
|---|---|



* **Plantillas Thymeleaf**

Para la vista utilizaremos dos plantillas Thymeleaf, una para listar los nombres de la personas y otra para agregar una persona nueva. En la carpeta **src/resources/templates/persona**, crearemos los siguienes archivos html:

a) Plantilla **listar.html**
  
    <!DOCTYPE html>
    <html xmlns:th="http://www.thymeleaf.org">
    <head>
        <title>Lista de Personas</title>
    </head>
    <body>
    <h1>Lista de Personas</h1>
    <ul>
        <li th:each="persona : ${personas}" th:text="${persona.nombre}"></li>
    </ul>
    <a href="/personas/agregar">Agregar Persona</a>
    </body>
    </html>

b) Plantilla **agregar.html**

Esta plantilla proporciona un formulario para agregar una nueva persona.

    <!DOCTYPE html>
    <html xmlns:th="http://www.thymeleaf.org">
    <head>
        <title>Agregar Persona</title>
    </head>
    <body>
        <h1>Agregar Persona</h1>
        <form action="/personas/agregar" method="post">
            <label for="nombre">Nombre:</label>
            <input type="text" id="nombre" name="nombre" required/>
            <button type="submit">Agregar</button>
        </form>
    </body>
    </html>

![ref](mvc_sencillo5.png)     

**Configurar el Archivo application.properties**{.azul}
   
En el directorio src/main/resources, configura el archivo **application.properties** con las propiedades básicas:

    spring.thymeleaf.prefix=classpath:/templates/
    spring.thymeleaf.suffix=.html
    server.port=8080

!!!warning ""
    Recuerda que puedes cambiar el puerto si lo tienes ocupado. Puedes probar con el puerto 8888.

**Ejecutar la aplicación**{.azul}

La aplicación estará disponible en http://localhost:8080. 

Aquí podrás:

* Ver la lista de personas al acceder a **/personas**.
* Agregar una nueva persona a través del formulario en **/personas/agregar**.


| ![](mvc_sencillo6.png)|![](mvc_sencillo7.png)|![](mvc_sencillo8.png)|
|---|---|---|    

!!!note "Nota"
    Gracias a **Spring DevTools**, cualquier cambio que realices en el código (por ejemplo, en las plantillas o en los controladores) se reflejará automáticamente en la aplicación sin tener que reiniciarla manualmente.

