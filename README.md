# Laboratorio #2 – Implementación del Login en Laravel

> **Universidad Tecnológica de Panamá**  
> Facultad de Ingeniería de Sistemas Computacionales  
> Campus Víctor Levi Sasso  
> **Curso:** Desarrollo Web – Lenguaje de Programación PHP  
> **Unidad:** I. Modelo Vista Controlador (MVC)  
> **Instructor:** Ing. Irina Fong  
> **Estudiante:** [Nombre completo]  
> **Fecha de ejecución:** [Colocar fecha o fechas reales del laboratorio]

---

## 1. Introducción

En este laboratorio se configuró un proyecto web utilizando Laravel y el patrón Modelo–Vista–Controlador (MVC). El objetivo fue preparar el entorno de desarrollo, conectar Laravel con una base de datos MySQL, ejecutar las migraciones y generar las funciones de registro e inicio de sesión mediante Laravel UI y Bootstrap.

Durante la práctica se trabajó con PHP, Composer, Laravel, WampServer, MySQL y herramientas de frontend. También se atendieron problemas de configuración y compatibilidad que aparecieron durante la instalación y ejecución.

## 2. Objetivos

- Comprender la organización de un proyecto Laravel bajo el patrón MVC.
- Configurar el entorno local y la conexión a MySQL.
- Crear las tablas necesarias mediante migraciones.
- Generar y comprobar las rutas y vistas de registro e inicio de sesión.
- Documentar las dificultades encontradas y las soluciones aplicadas.

## 3. Requisitos previos y tecnologías

| Tecnología | Versión / uso |
|---|---|
| 🪟 Sistema operativo | Windows 11 |
| 🐘 PHP | 8.3.28 (CLI) |
| 📦 Composer | 2.10.3 |
| 🧱 Laravel | Framework 13.37.0 |
| 🧰 Laravel Installer | 5.32.0 |
| 🖥️ WampServer | Entorno local; Apache y MySQL |
| 🗄️ MySQL | Base de datos local `laravellab` |
| 🎨 Laravel UI | 4.6.3 |
| 🅱️ Bootstrap | Scaffolding de interfaz generado con Laravel UI |
| 🟩 Node.js / npm | Instalación y compilación de recursos frontend |
| 📝 Editor | [Indicar editor utilizado: VS Code, Notepad++, etc.] |

> **Nota:** registra aquí las versiones de WampServer, MySQL/MariaDB, Node.js y npm que realmente utilizaste. No se anotaron sus versiones durante la conversación.

## 4. Instalación y configuración del proyecto

### 4.1 Comprobación de herramientas

Se verificó que PHP, Composer y Laravel Installer estuvieran disponibles desde CMD/PowerShell.

```bat
php -v
composer -V
laravel -V
```

**Captura 1 – Comprobación de PHP, Composer y Laravel Installer**

> [!IMPORTANT]
> Inserta aquí la captura tomada durante el laboratorio donde se vean los comandos y sus versiones.

<!-- CAPTURA 1: comprobación de herramientas -->

### 4.2 Creación del proyecto Laravel

Desde la carpeta de proyectos de WampServer se ejecutó el instalador:

```bat
cd C:\wamp64\www
laravel new LaravelLab
```

Durante el asistente se seleccionó:
- Starter pack: **No**.
- Frontend stack: **Blade**.
- Integración de Laravel Cloud: **None**.

El proyecto quedó ubicado en `C:\wamp64\www\LaravelLab`.

**Captura 2 – Creación del proyecto y opciones del instalador**

> [!IMPORTANT]
> Inserta aquí una captura del proceso `laravel new LaravelLab` y de las opciones seleccionadas.

<!-- CAPTURA 2: creación del proyecto -->

### 4.3 Verificación y ejecución inicial

Se ingresó a la carpeta del proyecto y se comprobó la versión de Laravel:

```bat
cd C:\wamp64\www\LaravelLab
php artisan --version
```

Para iniciar el servidor local se utilizó:

```bat
php artisan serve
```

Se abrió la dirección local mostrada por la terminal, normalmente `http://127.0.0.1:8000`.

**Captura 3 – Laravel ejecutándose en el navegador**

> [!IMPORTANT]
> Inserta aquí una captura de la página inicial de Laravel funcionando en el navegador.

<!-- CAPTURA 3: página inicial -->

## 5. Base de datos y migraciones

### 5.1 Configuración de MySQL

Se creó la base de datos `laravellab` desde phpMyAdmin de WampServer. En el archivo `.env` del proyecto se configuró la conexión a MySQL.

Ejemplo de las variables utilizadas (la contraseña debe corresponder a la configuración local de WAMP):

```dotenv
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravellab
DB_USERNAME=root
DB_PASSWORD=
```

> No publiques contraseñas reales, claves privadas ni otros secretos del archivo `.env` en GitHub. En el repositorio debe compartirse `.env.example`, no el `.env` personal.

**Captura 4 – Base de datos `laravellab` en phpMyAdmin**

> [!IMPORTANT]
> Inserta aquí una captura de phpMyAdmin donde se vea la base de datos creada.

<!-- CAPTURA 4: base de datos -->

### 5.2 Migraciones

Se limpiaron las configuraciones y se ejecutaron las migraciones para crear las tablas:

```powershell
php artisan config:clear
php artisan migrate
php artisan migrate:status
```

Las migraciones base verificadas fueron:

- `0001_01_01_000000_create_users_table`
- `0001_01_01_000001_create_cache_table`
- `0001_01_01_000002_create_jobs_table`

Al finalizar, las migraciones aparecieron con estado `Ran`.

**Captura 5 – Migraciones ejecutadas correctamente**

> [!IMPORTANT]
> Inserta aquí una captura de `php artisan migrate` sin errores o de `php artisan migrate:status` con las migraciones en estado `Ran`.

<!-- CAPTURA 5: migraciones -->

### 5.3 Ajuste aplicado a las migraciones

Durante la práctica, MySQL mostró el error `1071 Specified key was too long` al crear índices. Para el entorno local se ajustó la longitud predeterminada de cadenas en `app/Providers/AppServiceProvider.php`:

```php
use Illuminate\Support\Facades\Schema;

// Dentro de boot():
Schema::defaultStringLength(191);
```

También se redujo la longitud de las columnas `connection` y `queue` en la migración de `failed_jobs` a 100 caracteres para que el índice compuesto pudiera crearse en el servidor usado durante el laboratorio.

> Documenta este ajuste tal como quedó en tu repositorio y confirma que el archivo de migración guardado refleja el código final utilizado.

## 6. Instalación de autenticación con Laravel UI

Se instaló Laravel UI y se generó el scaffolding de Bootstrap y autenticación:

```powershell
composer require laravel/ui
php artisan ui bootstrap --auth
```

El paquete instalado fue `laravel/ui` **v4.6.3**. El comando de scaffolding terminó con el mensaje de generación exitosa de autenticación y Bootstrap.

### 6.1 Dependencias frontend

En PowerShell, `npm install` fue bloqueado por la política de ejecución de scripts (`npm.ps1`). Se utilizó el ejecutable CMD de npm:

```powershell
npm.cmd install
npm.cmd run dev
```

Esto instaló las dependencias y compiló los recursos del frontend.

**Captura 6 – Instalación de Laravel UI y compilación de Bootstrap**

> [!IMPORTANT]
> Inserta aquí capturas de `composer require laravel/ui`, `php artisan ui bootstrap --auth` y la compilación con npm.

<!-- CAPTURA 6: Laravel UI y npm -->

## 7. Rutas, controladores y vistas (MVC)

### 7.1 Rutas

Se comprobó el registro de rutas con:

```powershell
php artisan route:list
```

Entre las rutas generadas por Laravel UI se encontraron:

| Método | Ruta | Función |
|---|---|---|
| GET | `/login` | Mostrar formulario de inicio de sesión |
| POST | `/login` | Procesar inicio de sesión |
| POST | `/logout` | Cerrar sesión |
| GET | `/register` | Mostrar formulario de registro |
| POST | `/register` | Registrar usuario |
| GET | `/home` | Página de inicio autenticada |

**Captura 7 – Rutas de autenticación**

> [!IMPORTANT]
> Inserta aquí una captura de `php artisan route:list` donde se vean las rutas `/login`, `/register`, `/logout` y `/home`.

<!-- CAPTURA 7: route:list -->

### 7.2 Organización MVC

- **Rutas (`routes/`):** definen las URL y conectan las solicitudes con los controladores.
- **Controladores (`app/Http/Controllers/`):** procesan las solicitudes y coordinan la lógica de la aplicación. En el laboratorio se generaron controladores de autenticación, como `LoginController`, `RegisterController` y `HomeController`.
- **Vistas (`resources/views/`):** contienen las plantillas Blade que muestran los formularios y páginas al usuario.
- **Modelos (`app/Models/`):** representan los datos y su relación con la base de datos. El modelo `User` se utiliza para los usuarios registrados.

## 8. Pruebas y resultados

### 8.1 Registro de usuario

Se abrió `/register` y se registró un usuario de prueba. El usuario se guardó en la tabla `users` de MySQL.

**Captura 8 – Formulario de registro**

> [!IMPORTANT]
> Inserta aquí una captura del formulario de registro. Evita mostrar contraseñas o datos personales reales.

<!-- CAPTURA 8: registro -->

### 8.2 Inicio de sesión

Se abrió `/login` y se comprobó el acceso con las credenciales del usuario de prueba creado durante el registro.

**Captura 9 – Formulario de login**

> [!IMPORTANT]
> Inserta aquí una captura del formulario de login y, si es posible, otra del acceso exitoso a `/home`.

<!-- CAPTURA 9: login -->

### 8.3 Resultado visible del laboratorio

> [!IMPORTANT]
> **Captura 10 – Resultado final:** coloca aquí una imagen que muestre el resultado visible del laboratorio, tal como solicita la guía. Puede ser la página `/home` luego de iniciar sesión correctamente.

<!-- CAPTURA 10: resultado final del laboratorio -->

## 9. Dificultades encontradas y soluciones

### Dificultad 1: PowerShell bloqueó `npm install`

**Problema:** PowerShell no permitió ejecutar `npm.ps1` debido a la política de ejecución de scripts.

**Solución:** se ejecutaron los comandos mediante el archivo CMD de npm:

```powershell
npm.cmd install
npm.cmd run dev
```

### Dificultad 2: Laravel intentaba conectarse a una base de datos incorrecta

**Problema:** Laravel solicitaba la base de datos `laravel`, aunque se había creado `laravellab`.

**Solución:** se revisó el archivo `.env`, se quitaron los comentarios de las variables `DB_` y se configuró `DB_DATABASE=laravellab`. Luego se ejecutó `php artisan config:clear`.

### Dificultad 3: Índices demasiado largos en MySQL

**Problema:** MySQL devolvió el error `1071 Specified key was too long` al crear índices de las tablas `users` y `failed_jobs`.

**Solución:** se ajustó la longitud predeterminada de cadenas en `AppServiceProvider` a 191 y se redujeron las columnas `connection` y `queue` de la migración de `failed_jobs` a 100 caracteres. Después se recrearon las tablas del laboratorio y las migraciones finalizaron correctamente.

### Dificultad 4: Laravel UI utilizaba middleware incompatible con Laravel 13

**Problema:** los controladores generados por Laravel UI llamaban a `$this->middleware()` en el constructor y aparecía el error `Call to undefined method ...::middleware()`.

**Solución aplicada durante el laboratorio:** se adaptaron `LoginController`, `RegisterController` y `HomeController` para declarar middleware mediante `HasMiddleware` y `Middleware`, en lugar del método antiguo del constructor.

> Incluye en el repositorio los controladores finales que realmente quedaron funcionando. Revisa que los cambios descritos coincidan con tu código actual antes de entregar.

## 10. Respaldo de la base de datos

La guía solicita incluir un respaldo de la base de datos en el repositorio. Desde phpMyAdmin:

1. Selecciona la base de datos `laravellab`.
2. Abre la pestaña **Exportar**.
3. Selecciona el formato SQL.
4. Descarga el respaldo y guárdalo en una carpeta del repositorio, por ejemplo `database/backup/laravellab.sql`.

**Captura 11 – Exportación de la base de datos**

> [!IMPORTANT]
> Inserta aquí una captura del proceso de exportación o del archivo SQL generado.

<!-- CAPTURA 11: backup -->

> Antes de subir el respaldo, revisa que no contenga datos personales o contraseñas reales. Si el laboratorio solo contiene usuarios ficticios, indícalo.

## 11. Referencias

Completa y verifica las referencias que realmente consultaste durante el laboratorio. La tarea exige **al menos tres fuentes**. Estas referencias oficiales son puntos de partida; agrega la fecha de consulta si tu formato académico lo requiere.

1. Laravel. (s. f.). *Laravel documentation*. https://laravel.com/docs
2. Laravel. (s. f.). *Laravel UI*. GitHub. https://github.com/laravel/ui
3. Composer. (s. f.). *Composer documentation*. https://getcomposer.org/doc/
4. WampServer. (s. f.). *WampServer*. https://www.wampserver.com/

## 12. Fecha de ejecución

- **Fecha(s):** [Escribir las fechas reales en que se realizó la práctica]
- **Fecha de entrega:** [Colocar la fecha indicada por la docente]

## 13. Autor

Este laboratorio ha sido desarrollado por el estudiante de la Universidad Tecnológica de Panamá:

- **Nombre:** [Nombre completo]
- **Correo:** [Correo académico]
- **Curso:** Desarrollo Web – Lenguaje de Programación PHP
- **Instructor del laboratorio:** Ing. Irina Fong
