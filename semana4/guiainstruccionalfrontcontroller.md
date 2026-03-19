# Guía Instruccional: Implementación de Front Controller en PHP con Control de Versiones (Git/GitHub)

**Asignatura:** Programación II  
**Nivel:** Universitario (UPT)  
**Tema:** Arquitectura PHP, Composer Autoload y Flujo de Trabajo Git  
**Duración Estimada:** 2 - 3 Horas Académicas  

---

## 1. Objetivos de la Clase
Al finalizar esta sesión práctica, el estudiante será capaz de:
1.  Comprender el patrón de diseño **Front Controller** como punto único de entrada.
2.  Configurar un entorno PHP profesional utilizando **Composer** para la carga automática de clases (Autoload).
3.  Gestionar el ciclo de vida del código utilizando **Git** (init, add, commit, push, pull).
4.  Publicar el código fuente en un repositorio remoto (**GitHub**) siguiendo estándares de la industria.

## 2. Prerrequisitos Técnicos
Antes de iniciar, verifique que los estudiantes tengan instalados:
*   **PHP** (Versión 8.0 o superior recomendada).
*   **Composer** (Gestor de dependencias de PHP).
*   **Git** (Control de versiones).
*   **Editor de Código** (VS Code recomendado).
*   Cuenta activa en **GitHub**.

---

## 3. Fundamentos Teóricos (Breve Introducción)

### 3.1. ¿Qué es el Front Controller?
En el desarrollo web antiguo, cada página era un archivo físico (`contacto.php`, `nosotros.php`). Esto es inseguro y difícil de mantener.
El **Front Controller** es un patrón de diseño donde **toda petición web pasa por un único archivo** (usualmente `index.php`). Este archivo decide qué lógica ejecutar.
*   **Ventaja:** Centraliza la seguridad, la configuración y la carga de clases.

### 3.2. ¿Por qué Composer Autoload?
Antes, incluíamos archivos manualmente (`require 'clases/Usuario.php'`). Con **Composer**, configuramos una regla y PHP carga las clases automáticamente cuando las necesitamos. Esto es el estándar PSR-4.

### 3.3. El Flujo Git
1.  **Local:** Trabajamos en nuestra máquina.
2.  **Staging:** Preparamos los cambios (`add`).
3.  **Commit:** Guardamos la versión (`commit`).
4.  **Remote:** Enviamos a la nube (`push`).

---

## 4. Desarrollo Práctico Paso a Paso

*Nota para el docente: Ejecute cada bloque de comandos junto con los estudiantes. No avance hasta que todos hayan completado el paso.*

### Paso 1: Inicialización del Proyecto y Git
El objetivo es crear la carpeta y vincularla a Git desde el primer segundo.

1.  Cree una carpeta para el proyecto:
    ```bash
    mkdir proyecto-upt-php
    cd proyecto-upt-php
    ```
2.  Inicialice el repositorio Git local:
    ```bash
    git init
    ```
3.  Verifique el estado (debe decir "No commits yet"):
    ```bash
    git status
    ```
4.  **Primer Commit (Inicialización):** Es buena práctica guardar el estado inicial vacío.
    ```bash
    git commit --allow-empty -m "Inicio del repositorio del proyecto"
    ```

### Paso 2: Configuración de Composer y Estructura
Vamos a preparar el terreno para el Autoload.

1.  Inicialice Composer (presione Enter para aceptar los defaults, asegúrese de que el nombre del paquete sea lógico, ej: `upt/proyecto-php`):
    ```bash
    composer init
    ```
2.  Esto crea el archivo `composer.json`.
3.  **Estructura de Carpetas:** Cree manualmente las carpetas para separar lo público de la lógica.
    ```bash
    mkdir public
    mkdir src
    ```
    *   `public/`: Aquí irá el Front Controller (`index.php`). Es la única carpeta accesible desde el navegador.
    *   `src/`: Aquí irá nuestro código PHP (Clases).
    *   `vendor/`: La creará Composer automáticamente (NO la tocamos).

### Paso 3: Configuración del Autoload (PSR-4)
Debemos decirle a Composer dónde buscar las clases.

1.  Abra `composer.json` en su editor.
2.  Busque la sección `"autoload"` y configúrela así:
    ```json
    "autoload": {
        "psr-4": {
            "App\\": "src/"
        }
    }
    ```
    *Explicación:* Esto significa que cualquier clase que empiece con `App\` se buscará dentro de la carpeta `src/`.
3.  Genere los archivos de autoload:
    ```bash
    composer dump-autoload
    ```
4.  **Git Ignore (Crítico):** Nunca subimos la carpeta `vendor` a Git.
    *   Cree un archivo llamado `.gitignore` en la raíz.
    *   Agregue este contenido:
        ```text
        /vendor
        .DS_Store
        *.log
        ```
5.  **Commit de Estructura:** Guarde estos cambios importantes.
    ```bash
    git add .
    git commit -m "Configuración de Composer y estructura de carpetas"
    ```

### Paso 4: Creación de la Lógica (Clase en Src)
Vamos a crear una clase simple para probar que el autoload funciona.

1.  Dentro de `src/`, cree un archivo llamado `Saludo.php`.
2.  **Código (`src/Saludo.php`):**
    ```php
    <?php
    namespace App;

    class Saludo {
        public function obtenerMensaje() {
            return "Bienvenido a Programación II - UPT. Front Controller Activo.";
        }
    }
    ```
    *Nota:* El nombre de la clase debe coincidir con el archivo. El `namespace` debe coincidir con lo definido en `composer.json` (`App`).

### Paso 5: Implementación del Front Controller
Este es el punto de entrada único.

1.  Dentro de `public/`, cree un archivo llamado `index.php`.
2.  **Código (`public/index.php`):**
    ```php
    <?php
    // 1. Cargar el autoloader de Composer
    require_once __DIR__ . '/../vendor/autoload.php';

    // 2. Importar la clase (Gracias al namespace)
    use App\Saludo;

    // 3. Lógica del Controlador
    $saludo = new Saludo();
    
    // 4. Salida HTML
    ?>
    <!DOCTYPE html>
    <html lang="es">
    <head>
        <meta charset="UTF-8">
        <title>Front Controller UPT</title>
    </head>
    <body>
        <h1><?php echo $saludo->obtenerMensaje(); ?></h1>
        <p>Este sitio está versionado con Git.</p>
    </body>
    </html>
    ```

### Paso 6: Prueba Local
Antes de subir a GitHub, verificamos que funcione.

1.  Desde la terminal, en la raíz del proyecto, ejecute el servidor embebido de PHP apuntando a la carpeta pública:
    ```bash
    php -S localhost:8000 -t public
    ```
2.  Abra el navegador en `http://localhost:8000`.
3.  Debería ver el mensaje de bienvenida.
4.  Detenga el servidor (Ctrl + C).
5.  **Commit del Código:**
    ```bash
    git add .
    git commit -m "Implementación del Front Controller y clase Saludo"
    ```

### Paso 7: Publicación en GitHub (Remote)
Ahora llevaremos el código a la nube.

1.  Vaya a **GitHub.com** -> **New Repository**.
2.  Nombre: `proyecto-php-upt`.
3.  **NO** lo inicialice con README (ya tenemos historial local).
4.  Cree el repositorio.
5.  Copie la URL HTTPS del repositorio (ej: `https://github.com/usuario/proyecto-php-upt.git`).
6.  En su terminal, vincule el remoto:
    ```bash
    git remote add origin https://github.com/SU_USUARIO/proyecto-php-upt.git
    ```
7.  Verifique la rama (debe ser `main`):
    ```bash
    git branch -M main
    ```
8.  Envíe el código (**Push**):
    ```bash
    git push -u origin main
    ```
    *(Si solicita credenciales, use su usuario y el Personal Access Token).*

### Paso 8: Flujo de Actualización (Pull & Push)
Simulemos un cambio para practicar el ciclo completo.

1.  Modifique `src/Saludo.php` (cambie el texto del mensaje).
2.  Guarde el archivo.
3.  **Antes de enviar**, traiga cambios (buena práctica, aunque aquí trabaja solo):
    ```bash
    git pull origin main
    ```
4.  Prepare y confirme:
    ```bash
    git add .
    git commit -m "Actualización del mensaje de bienvenida"
    ```
5.  Envíe los cambios:
    ```bash
    git push origin main
    ```
6.  Verifique en GitHub que el archivo se actualizó.

---

## 5. Nota Importante sobre Hosting PHP
**Atención Estudiantes:**
GitHub Pages (la función de publicar sitios web de GitHub) **solo soporta sitios estáticos** (HTML, CSS, JS). **No ejecuta PHP**.

Para este curso, el "Publicar en Git" significa que el **código fuente** está seguro y versionado en la nube. Para ejecutar este proyecto en internet en un entorno real, necesitarían un hosting que soporte PHP (como 000webhost, InfinityFree, o un VPS) y conectarían ese servidor a este repositorio Git.

**Para efectos de esta clase:** El repositorio en GitHub es su entrega final. El profesor clonará su repo y lo ejecutará en local para evaluar.

---

## 6. Checklist de Evaluación

El estudiante deberá entregar el enlace a su repositorio GitHub. Se evaluará:

| Criterio | Descripción | Puntos |
| :--- | :--- | :--- |
| **Estructura** | Carpetas `public/` y `src/` correctamente separadas. | 20% |
| **Composer** | Archivo `composer.json` configurado con PSR-4 y `vendor/` en `.gitignore`. | 30% |
| **Front Controller** | `index.php` en `public/` carga el autoload e instancia la clase correctamente. | 30% |
| **Git Flow** | El historial de commits muestra al menos 3 mensajes coherentes (Init, Estructura, Código). | 20% |

---

## 7. Solución de Problemas Comunes (Troubleshooting)

1.  **Error: `Class 'App\Saludo' not found`**
    *   *Causa:* Olvidaron ejecutar `composer dump-autoload` después de editar el `composer.json` o el namespace no coincide con la carpeta.
    *   *Solución:* Verificar `namespace App;` en el archivo PHP y `"App\\": "src/"` en el JSON. Ejecutar `composer dump-autoload`.

2.  **Error: `vendor/` subido a GitHub**
    *   *Causa:* No se creó el `.gitignore` antes del primer push.
    *   *Solución:* Crear `.gitignore`, agregar `/vendor`, ejecutar `git rm -r --cached vendor`, hacer commit y push.

3.  **Error: `permission denied (publickey)`**
    *   *Causa:* Problemas de autenticación con GitHub.
    *   *Solución:* Usar la URL HTTPS en lugar de SSH para esta clase, o configurar las claves SSH correctamente.

---

## 8. Cierre de la Clase
