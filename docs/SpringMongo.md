# Spring MongoDb

Spring MongoDb Es un módulo de Spring Data que facilita la integración de aplicaciones Spring con MongoDB. Además proporciona una abstracción sobre las operaciones básicas de MongoDB como CRUD, consultas personalizadas, y soporte para agregaciones.

**Requisitos**{.azul}

Tener MongoDB instalado o utilizar una instancia en la nube o en un contenedor Docker.

Spring Boot configurado con Maven (en nuestro caso) o Gradle.

**Dependencias Maven**{.azul}

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-mongodb</artifactId>
    </dependency>

**application.properties**{.azul}

    spring.data.mongodb.host=localhost
    spring.data.mongodb.port=27017
    spring.data.mongodb.database=nombre_base_de_datos

Si estás usando MongoDB con autenticación, añade:

    spring.data.mongodb.username=usuario
    spring.data.mongodb.password=contraseña
  

## Ejemplo: Menús

En este ejemplo partiremos de un archivo json con 10 menús y sus correspondientes platos. Has de añadir este documento como una colección a tu BD Mongo.

**Archivo json con los menus:**

```json
    [
    {
        "_id": "1",
        "nombre": "Menú Mediterráneo",
        "descripcion": "Un menú saludable inspirado en la dieta mediterránea.",
        "fecha": "2025-01-08",
        "platos": [
        {
            "nombre": "Ensalada Griega",
            "categoria": "Entrante",
            "ingredientes": ["Lechuga", "Tomate", "Pepino", "Aceitunas", "Queso Feta"],
            "precio": 5.5
        },
        {
            "nombre": "Moussaka",
            "categoria": "Principal",
            "ingredientes": ["Berenjena", "Carne de Cordero", "Tomate", "Bechamel", "Queso"],
            "precio": 12.0
        }
        ],
        "precioTotal": 17.5
    },
    {
        "_id": "2",
        "nombre": "Menú Asiático",
        "descripcion": "Sabores frescos y auténticos de Asia.",
        "fecha": "2025-01-09",
        "platos": [
        {
            "nombre": "Rollitos de Primavera",
            "categoria": "Entrante",
            "ingredientes": ["Zanahoria", "Col", "Brotes de Soja", "Fideos de Arroz"],
            "precio": 4.0
        },
        {
            "nombre": "Pad Thai",
            "categoria": "Principal",
            "ingredientes": ["Fideos de Arroz", "Camarones", "Tofu", "Maní"],
            "precio": 10.0
        }
        ],
        "precioTotal": 14.0
    },
    {
        "_id": "3",
        "nombre": "Menú Vegetariano",
        "descripcion": "Opciones deliciosas sin carne.",
        "fecha": "2025-01-10",
        "platos": [
        {
            "nombre": "Hummus con Pan de Pita",
            "categoria": "Entrante",
            "ingredientes": ["Garbanzos", "Tahini", "Limón", "Ajo"],
            "precio": 4.5
        },
        {
            "nombre": "Lasaña Vegetariana",
            "categoria": "Principal",
            "ingredientes": ["Pasta", "Espinacas", "Ricotta", "Tomate"],
            "precio": 9.0
        }
        ],
        "precioTotal": 13.5
    },
    {
        "_id": "4",
        "nombre": "Menú Italiano",
        "descripcion": "Especialidades clásicas de Italia.",
        "fecha": "2025-01-11",
        "platos": [
        {
            "nombre": "Bruschetta",
            "categoria": "Entrante",
            "ingredientes": ["Pan", "Tomate", "Albahaca", "Aceite de Oliva"],
            "precio": 5.0
        },
        {
            "nombre": "Pizza Margarita",
            "categoria": "Principal",
            "ingredientes": ["Masa de Pizza", "Tomate", "Mozzarella", "Albahaca"],
            "precio": 10.0
        }
        ],
        "precioTotal": 15.0
    },
    {
        "_id": "5",
        "nombre": "Menú Mexicano",
        "descripcion": "Platos picantes y llenos de sabor.",
        "fecha": "2025-01-12",
        "platos": [
        {
            "nombre": "Guacamole con Totopos",
            "categoria": "Entrante",
            "ingredientes": ["Aguacate", "Limón", "Cilantro", "Totopos"],
            "precio": 4.5
        },
        {
            "nombre": "Tacos al Pastor",
            "categoria": "Principal",
            "ingredientes": ["Tortilla", "Carne de Cerdo", "Piña", "Cebolla"],
            "precio": 9.0
        }
        ],
        "precioTotal": 13.5
    },
    {
        "_id": "6",
        "nombre": "Menú Americano",
        "descripcion": "Comida clásica de los Estados Unidos.",
        "fecha": "2025-01-13",
        "platos": [
        {
            "nombre": "Alitas de Pollo",
            "categoria": "Entrante",
            "ingredientes": ["Pollo", "Salsa BBQ", "Especias"],
            "precio": 6.0
        },
        {
            "nombre": "Hamburguesa con Queso",
            "categoria": "Principal",
            "ingredientes": ["Pan", "Carne de Res", "Queso", "Lechuga"],
            "precio": 10.0
        }
        ],
        "precioTotal": 16.0
    },
    {
        "_id": "7",
        "nombre": "Menú de Mariscos",
        "descripcion": "Frescos sabores del océano.",
        "fecha": "2025-01-14",
        "platos": [
        {
            "nombre": "Cóctel de Camarones",
            "categoria": "Entrante",
            "ingredientes": ["Camarones", "Salsa Cóctel", "Limón"],
            "precio": 7.0
        },
        {
            "nombre": "Paella de Mariscos",
            "categoria": "Principal",
            "ingredientes": ["Arroz", "Camarones", "Mejillones", "Calamares"],
            "precio": 12.0
        }
        ],
        "precioTotal": 19.0
    },
    {
        "_id": "8",
        "nombre": "Menú Francés",
        "descripcion": "Platos refinados y elegantes.",
        "fecha": "2025-01-15",
        "platos": [
        {
            "nombre": "Quiche Lorraine",
            "categoria": "Entrante",
            "ingredientes": ["Huevo", "Nata", "Tocino", "Queso Gruyère"],
            "precio": 6.5
        },
        {
            "nombre": "Ratatouille",
            "categoria": "Principal",
            "ingredientes": ["Berenjena", "Calabacín", "Tomate", "Pimiento"],
            "precio": 9.5
        }
        ],
        "precioTotal": 16.0
    },
    {
        "_id": "9",
        "nombre": "Menú Indio",
        "descripcion": "Sabores especiados y exóticos.",
        "fecha": "2025-01-16",
        "platos": [
        {
            "nombre": "Samosas",
            "categoria": "Entrante",
            "ingredientes": ["Papas", "Especias", "Masa Frita"],
            "precio": 4.0
        },
        {
            "nombre": "Pollo Tikka Masala",
            "categoria": "Principal",
            "ingredientes": ["Pollo", "Salsa de Tomate", "Especias", "Crema"],
            "precio": 11.0
        }
        ],
        "precioTotal": 15.0
    },
    {
        "_id": "10",
        "nombre": "Menú Japonés",
        "descripcion": "Delicados sabores de Japón.",
        "fecha": "2025-01-17",
        "platos": [
        {
            "nombre": "Sopa de Miso",
            "categoria": "Entrante",
            "ingredientes": ["Miso", "Tofu", "Alga Wakame"],
            "precio": 3.5
        },
        {
            "nombre": "Sushi Variado",
            "categoria": "Principal",
            "ingredientes": ["Arroz", "Pescado", "Alga Nori", "Vegetales"],
            "precio": 12.5
        }
        ],
        "precioTotal": 16.0
    }
    ]
```
**Modelo MVC**{.azul}

**Modelo**{.verde}

Los modelos son las clases que representan las colecciones en MongoDB. Deben incluir la anotación **@Document**.

**Menu.kt**

    import org.springframework.data.annotation.Id
    import org.springframework.data.mongodb.core.mapping.Document

    @Document(collection = "menus") // Nombre de la colección en MongoDB
    data class Menu(
        @Id
        val id: String, // Identificador único del menú
        val nombre: String, // Nombre del menú
        val descripcion: String, // Descripción del menú
        val fecha: String, // Fecha asociada al menú
        val platos: List<Plato> = emptyList(), // Lista de platos
        val precioTotal: Double = 0.0 // Precio total del menú
    )

    data class Plato(
        val nombre: String,
        val categoria: String,
        val ingredientes: List<String> = emptyList(),
        val precio: Double = 0.0
    )

**Repositorios**{.verde}

proporciona una interfaz MongoRepository que simplifica las operaciones CRUD.

**MenuRepository.kt**

import org.springframework.data.mongodb.repository.MongoRepository
import org.springframework.stereotype.Repository
import org.example.primerspringmongo.model.*


    @Repository
    interface MenuRepository : MongoRepository<Menu, String> {
        fun findByNombre(nombre: String): List<Menu>
        fun findByPlatosCategoria(categoria: String): List<Menu>

    }

**Controlador**{.verde}

**MenuController.kt**

    import org.example.primerspringmongo.model.Menu
    import org.example.primerspringmongo.repository.MenuRepository
    import org.springframework.stereotype.Controller
    import org.springframework.ui.Model
    import org.springframework.web.bind.annotation.*

    @Controller
    @RequestMapping("/menus")
    class MenuController(private val menuRepository: MenuRepository) {

        // Listar todos los menús
        @GetMapping
        fun listarMenus(model: Model): String {
            val menus = menuRepository.findAll()
            model.addAttribute("menus", menus)
            return "menus/listar"
        }

        @GetMapping("/buscar")
        fun buscarPorNombre(@RequestParam nombre: String, model: Model): String {
            val menus = menuRepository.findByNombre(nombre) // Consulta en el repositorio
            model.addAttribute("menus", menus) // Pasar los menús al modelo
            model.addAttribute("nombre", nombre) // Pasar el nombre buscado
            return "menus/listar" // Nombre de la plantilla Thymeleaf
        }

        // Guardar un nuevo menú
        @PostMapping("/guardar")
        fun guardarMenu(@ModelAttribute menu: Menu): String {
            menuRepository.save(menu)
            return "redirect:/menus"
        }

        // Ver detalles de un menú
        @GetMapping("/{id}")
        fun verDetalles(@PathVariable id: String, model: Model): String {
            val menu = menuRepository.findById(id)
            if (menu.isPresent) {
                model.addAttribute("menu", menu.get())
                return "menus/detalles"
            }
            return "redirect:/menus"
        }

        // Mostrar formulario para editar un menú
        @GetMapping("/editar/{id}")
        fun mostrarFormularioEditar(@PathVariable id: String, model: Model): String {
            val menu = menuRepository.findById(id)
            if (menu.isPresent) {
                model.addAttribute("menu", menu.get())
                return "menus/editar"
            }
            return "redirect:/menus"
        }

        // Actualizar un menú existente
        @PostMapping("/actualizar/{id}")
        fun actualizarMenu(@PathVariable id: String, @ModelAttribute menu: Menu): String {
            if (menuRepository.existsById(id)) {
                menuRepository.save(menu)
            }
            return "redirect:/menus"
        }

        // Eliminar un menú
        @GetMapping("/eliminar/{id}")
        fun eliminarMenu(@PathVariable id: String): String {
            menuRepository.deleteById(id)
            return "redirect:/menus"
        }
    }    

**PlatoController.kt**

    import org.example.primerspringmongo.repository.MenuRepository
    import org.springframework.stereotype.Controller
    import org.springframework.ui.Model
    import org.springframework.web.bind.annotation.*

    @Controller
    @RequestMapping("/platos")
    class PlatoController(private val menuRepository: MenuRepository) {

        // Listar todos los platos de un menú
        @GetMapping("/{menuId}")
        fun listarPlatos(@PathVariable menuId: String, model: Model): String {
            val menu = menuRepository.findById(menuId)
            if (menu.isPresent) {
                model.addAttribute("menu", menu.get())
                model.addAttribute("platos", menu.get().platos)
                return "platos/listar"
            }
            return "redirect:/menus"
        }

        @GetMapping("/categoria")
        fun buscarPorCategoria(@RequestParam categoria: String, model: Model): String {
            val menus = menuRepository.findByPlatosCategoria(categoria) // Consulta en el repositorio
            model.addAttribute("menus", menus) // Lista de menús encontrados
            model.addAttribute("categoriaBuscada", categoria) // Categoría buscada
            return "platos/buscarPorCategoria" // Plantilla Thymeleaf
        }

        @GetMapping("/{menuId}/ingredientes/{platoNombre}")
        fun verIngredientes(
            @PathVariable menuId: String,
            @PathVariable platoNombre: String,
            model: Model
        ): String {
            val menu = menuRepository.findById(menuId)
            if (menu.isPresent) {
                val plato = menu.get().platos.find { it.nombre == platoNombre }
                if (plato != null) {
                    println("Plato encontrado: $plato")
                    println("Ingredientes: ${plato.ingredientes}")
                    model.addAttribute("menu", menu.get())
                    model.addAttribute("plato", plato)
                    model.addAttribute("ingredientes", plato.ingredientes ?: emptyList<String>())
                    return "platos/ingredientes"
                }
            }
            model.addAttribute("error", "El plato o el menú no existen.")
            return "error"
        }

    }

 **Vista**{.verde}   

templates/menus/

**listar.html**

    <!DOCTYPE html>
    <html xmlns:th="http://www.thymeleaf.org">
    <head>
    <title>Lista de Menús</title>
    </head>
    <body>
    <h1>Lista de Menús</h1>
    <a href="/menus/crear">Crear Nuevo Menú</a>
    <table border="1">
    <thead>
    <tr>
        <th>Nombre</th>
        <th>Descripción</th>
        <th>Acciones</th>
    </tr>
    </thead>
    <tbody>
    <tr th:each="menu : ${menus}">
        <td th:text="${menu.nombre}"></td>
        <td th:text="${menu.descripcion}"></td>
        <td>
        <a th:href="@{'/menus/' + ${menu.id}}">Detalles</a>
        <a th:href="@{'/menus/editar/' + ${menu.id}}">Editar</a>
        <a th:href="@{'/menus/eliminar/' + ${menu.id}}" onclick="return confirm('¿Seguro que quieres eliminar este menú?')">Eliminar</a>
        </td>
    </tr>
    </tbody>
    </table>
    </body>
    </html>

**crear.html**

        <!DOCTYPE html>
        <html xmlns:th="http://www.thymeleaf.org">
        <head>
            <title>Crear/Editar Menú</title>
        </head>
        <body>
        <h1 th:text="${#strings.equals(menu.id, '') ? 'Crear Nuevo Menú' : 'Editar Menú'}"></h1>
        <form th:action="@{${#strings.equals(menu.id, '') ? '/menus/guardar' : '/menus/actualizar/' + menu.id}}" th:object="${menu}" method="post">
            <label for="nombre">Nombre:</label>
            <input type="text" id="nombre" name="nombre" th:value="${menu.nombre}" required><br>

            <label for="descripcion">Descripción:</label>
            <textarea id="descripcion" name="descripcion" th:text="${menu.descripcion}" required></textarea><br>

            <label for="fecha">Fecha:</label>
            <input type="date" id="fecha" name="fecha" th:value="${menu.fecha}" required><br>

            <label for="precioTotal">Precio Total:</label>
            <input type="number" id="precioTotal" name="precioTotal" th:value="${menu.precioTotal}" step="0.01" required><br>

            <button type="submit">Guardar</button>
            <a href="/menus">Cancelar</a>
        </form>
        </body>
        </html>

**detalles.html**

    <!DOCTYPE html>
    <html xmlns:th="http://www.thymeleaf.org">
    <head>
        <title>Detalles del Menú</title>
    </head>
    <body>
    <h1>Detalles del Menú</h1>
    <p><strong>Nombre:</strong> <span th:text="${menu.nombre}"></span></p>
    <p><strong>Descripción:</strong> <span th:text="${menu.descripcion}"></span></p>
    <p><strong>Fecha:</strong> <span th:text="${menu.fecha}"></span></p>
    <p><strong>Precio Total:</strong> <span th:text="${menu.precioTotal}"></span></p>

    <h2>Platos</h2>
    <ul>
        <li th:each="plato : ${menu.platos}">
            <strong th:text="${plato.nombre}"></strong> - <span th:text="${plato.categoria}"></span>
            (<span th:text="${plato.precio}"></span> €)
        </li>
    </ul>
    <a href="/menus">Volver</a>
    </body>
    </html>

**editar.html**

    <!DOCTYPE html>
    <html xmlns:th="http://www.thymeleaf.org">
    <head>
    <title>Crear/Editar Menú</title>
    </head>
    <body>
    <h1 th:text="${#strings.equals(menu.id, '') ? 'Crear Nuevo Menú' : 'Editar Menú'}"></h1>
    <form th:action="@{${#strings.equals(menu.id, '') ? '/menus/guardar' : '/menus/actualizar/' + menu.id}}" th:object="${menu}" method="post">
    <label for="nombre">Nombre:</label>
    <input type="text" id="nombre" name="nombre" th:value="${menu.nombre}" required><br>

    <label for="descripcion">Descripción:</label>
    <textarea id="descripcion" name="descripcion" th:text="${menu.descripcion}" required></textarea><br>

    <label for="fecha">Fecha:</label>
    <input type="date" id="fecha" name="fecha" th:value="${menu.fecha}" required><br>

    <label for="precioTotal">Precio Total:</label>
    <input type="number" id="precioTotal" name="precioTotal" th:value="${menu.precioTotal}" step="0.01" required><br>

    <button type="submit">Guardar</button>
    <a href="/menus">Cancelar</a>
    </form>
    </body>
    </html>

**templates/platos/**

**listar.kt**

    <!DOCTYPE html>
    <html xmlns:th="http://www.thymeleaf.org">
    <head>
        <title>Platos del Menú</title>
    </head>
    <body>
    <h1 th:text="'Platos del Menú: ' + ${menu.nombre}"></h1>
    <a th:href="@{/menus}">Volver a Menús</a>
    <table border="1">
        <thead>
        <tr>
            <th>Nombre</th>
            <th>Categoría</th>
            <th>Precio</th>
            <th>Ingredientes</th>
        </tr>
        </thead>
        <tbody>
        <tr th:each="plato : ${platos}">
            <td th:text="${plato.nombre}"></td>
            <td th:text="${plato.categoria}"></td>
            <td th:text="${plato.precio} + ' €'"></td>
            <td>
                <a th:href="@{'/platos/' + ${menu.id} + '/ingredientes/' + ${plato.nombre}}">Ver Ingredientes</a>
            </td>
        </tr>
        </tbody>
    </table>
    </body>
    </html>

**ingredientes.html**

    <!DOCTYPE html>
    <html xmlns:th="http://www.thymeleaf.org">
    <head>
        <title>Ingredientes</title>
    </head>
    <body>
    <h1 th:text="'Ingredientes del plato: ' + ${plato?.nombre}"></h1>

    <div th:if="${error}">
        <p th:text="${error}" style="color: red;"></p>
    </div>

    <ul th:if="${ingredientes != null && !ingredientes.isEmpty()}">
        <li th:each="ingrediente : ${ingredientes}" th:text="${ingrediente}"></li>
    </ul>

    <p th:if="${ingredientes == null || ingredientes.isEmpty()}" style="color: gray;">
        No hay ingredientes disponibles para este plato.
    </p>

    <a th:href="@{'/platos/' + ${menu.id}}">Volver a los platos</a>
    </body>
    </html>

**buscarPorCategoria.html**

    <!DOCTYPE html>
    <html xmlns:th="http://www.thymeleaf.org">
    <head>
    <title>Buscar Menús por Categoría</title>
    </head>
    <body>
    <h1>Resultados para categoría: <span th:text="${categoriaBuscada}"></span></h1>

    <table border="1" th:if="${menus}">
    <thead>
    <tr>
        <th>ID</th>
        <th>Nombre</th>
        <th>Descripción</th>
        <th>Platos</th>
    </tr>
    </thead>
    <tbody>
    <tr th:each="menu : ${menus}">
        <td th:text="${menu.id}"></td>
        <td th:text="${menu.nombre}"></td>
        <td th:text="${menu.descripcion}"></td>
        <td>
        <ul>
            <li th:each="plato : ${menu.platos}" th:if="${plato.categoria == categoriaBuscada}" th:text="${plato.nombre}"></li>
        </ul>
        </td>
    </tr>
    </tbody>
    </table>

    <p th:if="${menus == null || menus.isEmpty()}" style="color: gray;">
    No se encontraron menús con platos de la categoría <span th:text="${categoriaBuscada}"></span>.
    </p>

    <a href="/menus">Volver a la lista de menús</a>
    </body>
    </html>
