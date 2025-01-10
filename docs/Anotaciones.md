# Anotaciones

Las anotaciones en Spring simplifican enormemente la configuración y el manejo de los componentes de una aplicación. Estas anotaciones eliminan la necesidad de configuraciones extensas en archivos XML y hacen que el código sea más legible y fácil de mantener.
La clave es aprender a usarlas en el contexto adecuado para lograr aplicaciones modulares, escalables y fácilmente configurables.

**Principales propósitos de las anotaciones en Spring:**

1) Inyección de dependencias

Permiten declarar y gestionar las relaciones entre los diferentes componentes de una aplicación.

Ejemplos:

* **@Autowired**: Para inyectar automáticamente dependencias.
* **@Qualifier**: Para resolver ambigüedades cuando hay múltiples beans del mismo tipo.
* **@Value**: Inyecta valores desde propiedades (application.properties) o expresiones.
---

2) Definición de componentes

Marcan clases como componentes gestionados por el contenedor de Spring, para que puedan ser inyectados y utilizados en otras partes de la aplicación.

Ejemplos:

* **@Component**: Marca una clase genérica como un componente gestionado por Spring.
* **@Service**: Especialización de @Component para servicios.
* **@Repository**: Especialización para la capa de acceso a datos. Cuando la clase interactúa directamente con la base de datos (por ejemplo, con Spring Data JPA o JDBC).
* **@Configuration**: Indica que una clase contiene definiciones de beans (equivalente a un archivo XML de configuración).
* **@Bean**: Define un bean manualmente dentro de una clase de configuración.
* **@Controller**: Marca una clase como un controlador para manejar peticiones HTTP. Cuando necesitas manejar peticiones web en una aplicación tradicional.
* **@RestController**: Combina **@Controller** y **@ResponseBody** para APIs RESTful. Cuando estás desarrollando una API REST y necesitas devolver objetos serializados directamente en la respuesta.


En Spring Boot, **las anotaciones** son fundamentales porque definen el comportamiento y la configuración de los componentes de la aplicación. A continuación, se muestran ejemplos de las anotaciones más utilizadas, organizadas por categorías:

## Anotaciones de configuración

**@SpringBootApplication**

    @SpringBootApplication
            public class Application {
                public static void main(String[] args) {
                    SpringApplication.run(Application.class, args);
                }
            }

!!!note "Marca la clase principal de la aplicación. Es una combinación de:"
    **1. @Configuration:** Habilita la configuración automática.

    **2. @EnableAutoConfiguration:** Hace que Spring escanee las dependencias incluidas en el classpath y configure automáticamente Beans relacionados.

    Por ejemplo:

    * Si tienes Spring Web en el proyecto, configurará un servidor web.
    * Si tienes una dependencia de JPA, configurará un gestor de entidades.
        
    **3. @ComponentScan:**

    Indica a Spring que debe escanear los paquetes a partir del paquete donde está definida la clase con @SpringBootApplication, buscando clases anotadas con:

        @Component
        @Service
        @Repository
        @Controller

    

**@Configuration**

Indica que una clase contiene definiciones de beans.

    @Configuration
    public class AppConfig {
        @Bean
        public MyService myService() {
            return new MyService();
        }
    }


**@PropertySource**

Carga un archivo de propiedades externo.

    @PropertySource("classpath:application.properties")
    public class AppConfig {}

## Anotaciones de componentes y Beans

**@Component**

Marca una clase como un Bean para que Spring la detecte automáticamente durante el escaneo de componentes.

  Uso típico: cualquier clase que no encaje específicamente como servicio, repositorio o controlador.

    @Component
        public class MyComponent {
            public void doSomething() {
                System.out.println("Hello from MyComponent!");
            }
        }

**@Service**

Es una especialización de @Component. Indica que la clase contiene lógica de negocio.

Uso típico: servicios de negocio.
  
    @Service
    public class MyService {
        public String process() {
            return "Processing...";
        }
    }

**@Repository**

Especialización de @Component. Indica que la clase es responsable de la interacción con la base de datos.
Habilita el manejo automático de excepciones relacionadas con la persistencia.


    @Repository
    public interface MyRepository extends JpaRepository<MyEntity, Long> {
    }

**@Bean**

Declara un bean dentro de una clase anotada con @Configuration. 

    @Configuration
        public class AppConfig {
            @Bean
            public MyService myService() {
                return new MyService();
            }
        }

## Anotaciones de controladores (MVC)   

**@Controller**

Especialización de @Component. Indica que la clase es un controlador que maneja solicitudes HTTP en aplicaciones web MVC.

Uso típico: manejar la lógica de las solicitudes y devolver vistas.
  
  

    @Controller
    public class MyController {
        @GetMapping("/home")
        public String home() {
            return "home";
        }
    }

**@RestController**

Combina @Controller y @ResponseBody. Indica que la clase es un controlador REST que devuelve directamente datos (normalmente en formato JSON o XML) en lugar de vistas.
 

    @RestController
    public class MyRestController {
        @GetMapping("/api/greet")
        public String greet() {
            return "Hello, World!";
        }
    }

**@RequestMapping**

Mapea solicitudes HTTP a métodos específicos en un controlador.

    @Controller
    public class MyController {
        @RequestMapping("/home")
        public String home() {
            return "home";
        }
    }

**@GetMapping, @PostMapping, @PutMapping, @DeleteMapping**

Son variantes especializadas de @RequestMapping para solicitudes HTTP específicas.


    @RestController
    public class MyController {
        @GetMapping("/users")
        public List<User> getUsers() {
            return userService.getAllUsers();
        }
    }

**@PathVariable**

Vincula un parámetro en la URL a un argumento en el controlador.


    @GetMapping("/users/{id}")
    public User getUser(@PathVariable Long id) {
        return userService.getUserById(id);
    }

**@RequestParam**

Vincula un parámetro de consulta (query parameter) a un argumento del controlador.


    @GetMapping("/search")
    public String search(@RequestParam String keyword) {
        return "Searching for " + keyword;
    }

**@RequestBody**

Vincula el cuerpo de la solicitud HTTP a un objeto Java.


    @PostMapping("/users")
    public User createUser(@RequestBody User user) {
        return userService.save(user);
    }

**@ResponseBody**

Indica que el resultado del método debe enviarse como la respuesta HTTP (generalmente en formato JSON o XML).


    @ResponseBody
    @GetMapping("/hello")
    public String hello() {
        return "Hello!";
    }



## Anotaciones para Inyección de Dependencias

**@Autowired**

Inyecta automáticamente un Bean en una clase.

Se puede usar en:

* Constructor
* Campo
* Método
---
  
    @Service
    public class MyService {
        @Autowired
        private MyRepository myRepository;
    }

**@Qualifier**

Se usa junto con @Autowired para especificar qué **Bean** debe inyectarse si hay múltiples candidatos.

    @Service
    public class MyService {
        @Autowired
        @Qualifier("mySpecificBean")
        private MyComponent myComponent;
    }

**@Inject**

Alternativa a @Autowired (de Java CDI), realiza la misma función.

    @Inject
    private MyComponent myComponent;

**@Value**

Inyecta valores directamente desde el archivo de configuración (application.properties) o valores literales.


    @Value("${app.name}")
    private String appName;


----

Anotaciones para Spring Data











