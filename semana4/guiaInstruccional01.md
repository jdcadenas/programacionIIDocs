# Guía Instruccional Integrada: Arquitectura PHP, Composer y Control de Versiones (Git/GitHub)

**Asignatura:** Programación II  
**Institución:** Universidad Politécnica Territorial (UPT)  
**Tema:** Estructura MVC Básica, Front Controller, Composer y Flujo Git  
**Nivel:** Introductorio (Pre-OOP / Transición a OOP)  
**Duración:** 3 Horas Académicas  

---

## 1. Objetivo General
Establecer las bases profesionales para el desarrollo web backend. Los estudiantes aprenderán a organizar un proyecto bajo el patrón **Front Controller**, gestionar dependencias con **Composer** y versionar su código correctamente con **Git/GitHub**, preparando el terreno para futuros temas de Orientación a Objetos e Interfaces.

## 2. Prerrequisitos
*   PHP instalado (v7.4 o superior).
*   Composer instalado globalmente.
*   Git instalado y configurado.
*   Cuenta en GitHub.
*   Editor de código (VS Code).

---

## 3. Introducción Conceptual (15 Minutos)

**Para el Profesor:** *Explique que aunque aún no dominan Clases y Objetos a profundidad, la industria usa estructuras estandarizadas. Hoy aprenderán el "esqueleto" sobre el cual construiremos la lógica más adelante.*

1.  **Front Controller:** Todo el tráfico web entra por un solo puerto (`public/index.php`). Esto nos permite tener control total.
2.  **Composer:** Es el administrador de paquetes de PHP. Nos ayuda a cargar archivos automáticamente (Autoload) sin escribir muchos `require`.
3.  **Git:** Es la máquina del tiempo del código. Nos permite trabajar sin miedo a perder información.
4.  **MVC (Modelo-Vista-Controlador):** Separaremos la lógica (Controlador) de lo que ve el usuario (Vista).

---

## 4. Desarrollo Práctico Paso a Paso

### FASE 1: Inicialización del Entorno y Git

1.  **Crear carpeta del proyecto:**
    Abra la terminal y cree la carpeta raíz.
    ```bash
    mkdir sistema-upt
    cd sistema-upt
    ```

2.  **Inicializar Git (Lo primero que se hace):**
    ```bash
    git init
    ```
    *Explicación:* Esto crea la carpeta oculta `.git` donde se guardará el historial.

3.  **Primer Commit (Punto de partida):**
    ```bash
    git commit --allow-empty -m "Inicio del repositorio - Proyecto UPT"
    ```

### FASE 2: Estructura de Directorios Profesional

Una buena estructura es vital para un sistema escalable. Crearemos las carpetas manualmente.

1.  **Ejecutar en terminal:**
    ```bash
    mkdir public
    mkdir src
    mkdir src/Controllers
    mkdir src/Views
    mkdir config
    ```

2.  **Explicación de la estructura:**
    *   `public/`: Única carpeta accesible desde el navegador. Aquí vive el **Front Controller**.
    *   `src/`: Código fuente (lógica interna).
    *   `config/`: Configuraciones de base de datos o rutas (para futuras clases).

### FASE 3: Configuración de Composer y Autoload

Aunque no dominen OOP, configuraremos el autoload para que el sistema esté listo para cuando lleguen las Clases.

1.  **Inicializar Composer:**
    ```bash
    composer init
    ```
    *   Siga las instrucciones (presione Enter para aceptar valores por defecto).
    *   En "minimum stability", ponga `stable`.
    *   En "type", ponga `project`.

2.  **Configurar Autoload (PSR-4):**
    Abra el archivo `composer.json` generado y asegúrese de que la sección `autoload` quede así:
    ```json
    "autoload": {
        "psr-4": {
            "App\\": "src/"
        }
    }
    ```
    *Explicación:* Le decimos a PHP: "Si ves una clase que empieza por `App`, búscala dentro de la carpeta `src`".

3.  **Generar archivos de carga:**
    ```bash
    composer dump-autoload
    ```
    *Esto crea la carpeta `vendor` y el archivo mágico `autoload.php`.*

4.  **Configurar .gitignore (CRÍTICO):**
    Nunca subimos la carpeta `vendor` a GitHub (pesa mucho y se genera automáticamente).
    *   Cree un archivo llamado `.gitignore` en la raíz.
    *   Escriba dentro:
        ```text
        /vendor
        .DS_Store
        *.log
        ```
    *   Guarde y haga commit de la estructura:
        ```bash
        git add .
        git commit -m "Estructura de carpetas y configuración de Composer"
        ```

### FASE 4: Creación del Front Controller y Vistas

Aquí uniremos Git, Composer y PHP.

1.  **Crear una Vista Simple (`src/Views/home.php`):**
    Esto simula el HTML que ve el usuario.
    ```php
    <!DOCTYPE html>
    <html lang="es">
    <head>
        <meta charset="UTF-8">
        <title>Sistema UPT</title>
    </head>
    <body>
        <h1><?php echo $titulo; ?></h1>
        <p><?php echo $contenido; ?></p>
        <footer>Programación II - UPT</footer>
    </body>
    </html>
    ```

2.  **Crear un Controlador Básico (`src/Controllers/HomeController.php`):**
    *Nota para el estudiante:* Aunque es una "Clase", piénselo como una caja que guarda funciones.
    ```php
    <?php
    namespace App\Controllers;

    class HomeController {
        public function index() {
            // Datos que enviaremos a la vista
            $datos = [
                'titulo' => 'Bienvenido al Front Controller',
                'contenido' => 'Este sistema está estructurado con Composer y Git.'
            ];
            
            // Cargar la vista (Extracto de variables)
            extract($datos);
            require __DIR__ . '/../Views/home.php';
        }
    }
    ```

3.  **El Front Controller (`public/index.php`):**
    Este es el único archivo que el navegador llama.
    ```php
    <?php
    // 1. Cargar el Autoload de Composer (El require principal)
    require_once __DIR__ . '/../vendor/autoload.php';

    // 2. Importar el Controlador
    use App\Controllers\HomeController;

    // 3. Ejecutar la lógica
    $controlador = new HomeController();
    $controlador->index();
    ```

### FASE 5: Prueba Local y Commit

1.  **Probar el sistema:**
    Ejecute el servidor de PHP apuntando a la carpeta pública.
    ```bash
    php -S localhost:8000 -t public
    ```
    Abra `http://localhost:8000` en el navegador. Debería ver la vista renderizada.

2.  **Guardar cambios en Git:**
    ```bash
    git add .
    git commit -m "Implementación del Front Controller y Vista Home"
    ```

### FASE 6: Publicación en GitHub (Remote)

1.  **Crear Repositorio en GitHub:**
    *   Vaya a GitHub.com -> New Repository.
    *   Nombre: `sistema-upt-php`.
    *   **No** marcar "Initialize with README".
    *   Crear repositorio.

2.  **Vincular y Subir (Push):**
    Copie la URL HTTPS de su repositorio y ejecute en la terminal:
    ```bash
    git remote add origin https://github.com/SU_USUARIO/sistema-upt-php.git
    git branch -M main
    git push -u origin main
    ```
    *Ingrese sus credenciales (Usuario y Token) si se lo solicita.*

3.  **Verificación:**
    Recargue la página de GitHub. Debería ver todos los archivos **excepto** la carpeta `vendor`.

---

## 5. Flujo de Trabajo Futuro (Pull & Push)

Explique a los estudiantes que en las próximas clases, cuando trabajen en equipo o desde otra PC, el ciclo será:

1.  **Antes de empezar a programar:**
    ```bash
    git pull origin main
    ```
    *(Trae los cambios de la nube a tu PC)*.
2.  **Programar...** (Crear nuevas vistas, controladores).
3.  **Al terminar:**
    ```bash
    git add .
    git commit -m "Descripción de lo que hice"
    git push origin main
    ```
    *(Envía tus cambios a la nube)*.

---

## 6. Notas Pedagógicas para el Profesor

1.  **Sobre las Clases:** Si los alumnos preguntan por `class` o `namespace`, explique: *"Es una forma de organizar el código en cajas etiquetadas. Por ahora, solo copien la estructura, en la próxima unidad profundizaremos en cómo funcionan estas cajas (Orientación a Objetos)"*.
2.  **Sobre el `require`:** Haga énfasis en que en `index.php` solo hay **UN** `require` (`vendor/autoload.php`). Ese es el poder de Composer: ese solo archivo carga todo lo demás automáticamente.
3.  **Sobre la Seguridad:** Recalque que si suben este proyecto a un hosting real, solo deben exponer la carpeta `public`. Si exponen la raíz, alguien podría ver sus archivos `src` o `config`.
4.  **Evaluación:** Pida el enlace del repositorio GitHub. Verifique que:
    *   Exista la carpeta `public` con el `index.php`.
    *   Exista `.gitignore` y NO esté la carpeta `vendor` en GitHub.
    *   El historial de commits tenga mensajes claros.

---

## 7. Checklist de Entrega para el Estudiante

| Ítem | Estado |
| :--- | :--- |
| Repositorio creado en GitHub | ⬜ |
| Carpeta `vendor` ignorada en `.gitignore` | ⬜ |
| `index.php` dentro de la carpeta `public` | ⬜ |
| El sitio carga correctamente en local (`php -S`) | ⬜ |
| Al menos 3 commits en el historial | ⬜ |

---

## 8. Próximos Pasos (Roadmap del Curso)

Informe a los estudiantes que esta estructura es la base para lo que viene:
1.  **Clase 3:** Crear una **Interfaz** para estandarizar los controladores.
2.  **Clase 4:** Conectar una **Base de Datos** en la carpeta `config`.
3.  **Clase 5:** Enrutamiento dinámico (que el `index.php` decida qué controlador llamar según la URL).

*"Chicos, hoy han construido los cimientos de un framework propio. Mantengan este orden y el desarrollo de sistemas complejos será mucho más sencillo."*