Laboratorio #2 – Implementación del Login en Laravel
Universidad Tecnológica de Panamá
Facultad de Ingeniería de Sistemas Computacionales
Campus Víctor Levi Sasso
Curso: Desarrollo Web
Unidad: I. Modelo Vista Controlador (MVC)

1. Introducción
En este laboratorio se configuró un proyecto web utilizando Laravel y el patrón Modelo–Vista–Controlador (MVC). El objetivo fue preparar el entorno de desarrollo, conectar Laravel con una base de datos MySQL, ejecutar las migraciones y generar las funciones de registro e inicio de sesión mediante Laravel UI y Bootstrap.

Durante la práctica se trabajó con PHP, Composer, Laravel, WampServer, MySQL y herramientas de frontend. También se atendieron problemas de configuración y compatibilidad que aparecieron durante la instalación y ejecución.
2. Objetivos
- Comprender la organización de un proyecto Laravel bajo el patrón MVC.
- Configurar el entorno local y la conexión a MySQL.
- Crear las tablas necesarias mediante migraciones.
- Generar y comprobar las rutas y vistas de registro e inicio de sesión.
- Documentar las dificultades encontradas y las soluciones aplicadas.
3. Requisitos previos y tecnologías
Tecnología	Versión / uso
🪟 Sistema operativo	Windows 11
🐘 PHP	8.3.28 (CLI)
📦 Composer	2.10.3
🧱 Laravel	Framework 13.38.0
🧰 Laravel Installer	5.32.0
🖥️ WampServer	Entorno local; Apache y MySQL
🗄️ MySQL	Base de datos local laravellab
🎨 Laravel UI	4.6.3
🅱️ Bootstrap	Scaffolding de interfaz generado con Laravel UI
🟩 Node.js / npm	Instalación y compilación de recursos frontend
📝 Editor	[VS Code, Notepad]


4. Instalación y configuración del proyecto
4.1 Comprobación de herramientas
Se verificó que PHP, Composer y Laravel Installer estuvieran disponibles desde CMD/PowerShell.
php -v
composer -V
laravel -V
Captura 1 – Comprobación de PHP, Composer y Laravel Installer
<img width="513" height="170" alt="image" src="https://github.com/user-attachments/assets/7aa77892-f10e-48d9-9ea6-11b1b3c971d4" />


4.2 Creación del proyecto Laravel
Desde la carpeta de proyectos de WampServer se ejecutó el instalador:
cd C:\wamp64\www
laravel new LaravelLab
Durante el asistente se seleccionó:
- Starter pack: No.
- Frontend stack: Blade.
- Integración de Laravel Cloud: None.
El proyecto quedó ubicado en C:\wamp64\www\LaravelLab.

Captura 2 – Creación del proyecto y opciones del instalador
<img width="517" height="458" alt="Captura de pantalla 2026-10-22 173300" src="https://github.com/user-attachments/assets/6fb8673d-280c-4623-ba71-c4c429cab384" />

<img width="859" height="457" alt="Captura de pantalla 2026-10-22 173242" src="https://github.com/user-attachments/assets/874ad64a-4ecc-4857-abe1-28c50e5e7a99" />


4.3 Verificación y ejecución inicial
Se ingresó a la carpeta del proyecto y se comprobó la versión de Laravel:
cd C:\wamp64\www\LaravelLab
php artisan --version
Para iniciar el servidor local se utilizó:
php artisan serve
Se abrió la dirección local mostrada por la terminal, normalmente http://127.0.0.1:8000.

Captura 3 – Laravel ejecutándose en el navegador
<img width="959" height="436" alt="image" src="https://github.com/user-attachments/assets/13761a99-5792-4a9b-9fd2-fedbdf76f330" />


5. Base de datos y migraciones
5.1 Configuración de MySQL
Se creó la base de datos laravellab desde phpMyAdmin de WampServer. En el archivo .env del proyecto se configuró la conexión a MySQL.

Ejemplo de las variables utilizadas (la contraseña debe corresponder a la configuración local de WAMP):
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravellab
DB_USERNAME=root
DB_PASSWORD=


Captura 4 – Base de datos laravellab en phpMyAdmin
<img width="958" height="482" alt="image" src="https://github.com/user-attachments/assets/cf988207-5757-4d35-a668-79c22321621c" />


5.2 Migraciones
Se limpiaron las configuraciones y se ejecutaron las migraciones para crear las tablas:
php artisan config:clear
php artisan migrate
php artisan migrate:status
Las migraciones base verificadas fueron:
- 0001_01_01_000000_create_users_table
- 0001_01_01_000001_create_cache_table
- 0001_01_01_000002_create_jobs_table
Al finalizar, las migraciones aparecieron con estado Ran.

Captura 5 – Migraciones ejecutadas correctamente
<img width="884" height="338" alt="Captura de pantalla 2026-10-22 182031" src="https://github.com/user-attachments/assets/f073c978-e553-477d-840e-8a57a17a6c35" />


5.3 Ajuste aplicado a las migraciones
Durante la práctica, MySQL mostró el error 1071 Specified key was too long al crear índices. Para el entorno local se ajustó la longitud predeterminada de cadenas en app/Providers/AppServiceProvider.php:
use Illuminate\Support\Facades\Schema;

// Dentro de boot():
Schema::defaultStringLength(191);
También se redujo la longitud de las columnas connection y queue en la migración de failed_jobs a 100 caracteres para que el índice compuesto pudiera crearse en el servidor usado durante el laboratorio.
Documenta este ajuste tal como quedó en tu repositorio y confirma que el archivo de migración guardado refleja el código final utilizado.

6. Instalación de autenticación con Laravel UI
Se instaló Laravel UI y se generó el scaffolding de Bootstrap y autenticación:
composer require laravel/ui
php artisan ui bootstrap --auth
El paquete instalado fue laravel/ui v4.6.3. El comando de scaffolding terminó con el mensaje de generación exitosa de autenticación y Bootstrap.
6.1 Dependencias frontend
En PowerShell, npm install fue bloqueado por la política de ejecución de scripts (npm.ps1). Se utilizó el ejecutable CMD de npm:
npm.cmd install
npm.cmd run dev
Esto instaló las dependencias y compiló los recursos del frontend.

Captura 6 – Instalación de Laravel UI y compilación de Bootstrap
<img width="376" height="278" alt="image" src="https://github.com/user-attachments/assets/9d0c2f1c-c22d-448d-a547-840c9cb678c1" />

<img width="479" height="135" alt="image" src="https://github.com/user-attachments/assets/202b5128-92cc-4c97-a25f-ce88a1cf0f24" />

<img width="754" height="266" alt="image" src="https://github.com/user-attachments/assets/8329d6b9-5389-4ffe-aceb-7fcd0a16a01a" />

7. Rutas, controladores y vistas (MVC)
7.1 Rutas
Se comprobó el registro de rutas con:
php artisan route:list
Entre las rutas generadas por Laravel UI se encontraron:
Método	Ruta	Función
GET	/login	Mostrar formulario de inicio de sesión
POST	/login	Procesar inicio de sesión
POST	/logout	Cerrar sesión
GET	/register	Mostrar formulario de registro
POST	/register	Registrar usuario
GET	/home	Página de inicio autenticada


Captura 7 – Rutas de autenticación
<img width="775" height="314" alt="Captura de pantalla 2026-10-22 191032" src="https://github.com/user-attachments/assets/c98306c6-fb6d-483e-aa6b-f8a0a0a4edaf" />


7.2 Organización MVC
- Rutas (routes/): definen las URL y conectan las solicitudes con los controladores.
- Controladores (app/Http/Controllers/): procesan las solicitudes y coordinan la lógica de la aplicación. En el laboratorio se generaron controladores de autenticación, como LoginController, RegisterController y HomeController.
- Vistas (resources/views/): contienen las plantillas Blade que muestran los formularios y páginas al usuario.
- Modelos (app/Models/): representan los datos y su relación con la base de datos. El modelo User se utiliza para los usuarios registrados.
8. Pruebas y resultados
8.1 Registro de usuario
Se abrió /register y se registró un usuario de prueba. El usuario se guardó en la tabla users de MySQL.

Captura 8 – Formulario de registro
<img width="957" height="350" alt="Captura de pantalla 2026-10-22 194616" src="https://github.com/user-attachments/assets/88f0cdcf-49e0-4f60-b749-2caf14b4b52d" />


8.2 Inicio de sesión
Se abrió /login y se comprobó el acceso con las credenciales del usuario de prueba creado durante el registro.

Captura 9 – Formulario de login
<img width="959" height="329" alt="Captura de pantalla 2026-10-22 192240" src="https://github.com/user-attachments/assets/ec4d16d1-fedd-4818-9196-aa9be732ee7e" />

8.3 Resultado visible del laboratorio
<img width="959" height="180" alt="Captura de pantalla 2026-10-22 194633" src="https://github.com/user-attachments/assets/1af96d55-6d9b-4fe3-9d26-6d3d83d2ff25" />


9. Dificultades encontradas y soluciones
Dificultad 1: PowerShell bloqueó npm install
Problema: PowerShell no permitió ejecutar npm.ps1 debido a la política de ejecución de scripts.

Solución: se ejecutaron los comandos mediante el archivo CMD de npm:
npm.cmd install
npm.cmd run dev
Dificultad 2: Laravel intentaba conectarse a una base de datos incorrecta
Problema: Laravel solicitaba la base de datos laravel, aunque se había creado laravellab.

Solución: se revisó el archivo .env, se quitaron los comentarios de las variables DB_ y se configuró DB_DATABASE=laravellab. Luego se ejecutó php artisan config:clear.
Dificultad 3: Índices demasiado largos en MySQL
Problema: MySQL devolvió el error 1071 Specified key was too long al crear índices de las tablas users y failed_jobs.

Solución: se ajustó la longitud predeterminada de cadenas en AppServiceProvider a 191 y se redujeron las columnas connection y queue de la migración de failed_jobs a 100 caracteres. Después se recrearon las tablas del laboratorio y las migraciones finalizaron correctamente.
Dificultad 4: Laravel UI utilizaba middleware incompatible con Laravel 13
Problema: los controladores generados por Laravel UI llamaban a $this->middleware() en el constructor y aparecía el error Call to undefined method ...::middleware().

Solución aplicada durante el laboratorio: se adaptaron LoginController, RegisterController y HomeController para declarar middleware mediante HasMiddleware y Middleware, en lugar del método antiguo del constructor.
Incluye en el repositorio los controladores finales que realmente quedaron funcionando. Revisa que los cambios descritos coincidan con tu código actual antes de entregar.

10. Respaldo de la base de datos
La guía solicita incluir un respaldo de la base de datos en el repositorio. Desde phpMyAdmin:
1. Selecciona la base de datos laravellab.
2. Abre la pestaña Exportar.
3. Selecciona el formato SQL.
4. Descarga el respaldo y guárdalo en una carpeta del repositorio, por ejemplo database/backup/laravellab.sql.
Captura 11 – Exportación de la base de datos
<img width="959" height="374" alt="image" src="https://github.com/user-attachments/assets/d2190677-86af-437e-962c-ba37587a35a5" />


11. Referencias
Completa y verifica las referencias que realmente consultaste durante el laboratorio. La tarea exige al menos tres fuentes. Estas referencias oficiales son puntos de partida; agrega la fecha de consulta si tu formato académico lo requiere.
1. Laravel. (s. f.). Laravel documentation. https://laravel.com/docs
2. Laravel. (s. f.). Laravel UI. GitHub. https://github.com/laravel/ui
3. Composer. (s. f.). Composer documentation. https://getcomposer.org/doc/
4. WampServer. (s. f.). WampServer. https://www.wampserver.com/
12. Fecha de entrega
- Fecha 23/10/2026
13. Autor
Este laboratorio ha sido desarrollado por el estudiante de la Universidad Tecnológica de Panamá:
- Nombre: Marcos Navarro
- Correo: marcos.navarro@utp.ac.pa
- Curso: Desarrollo Web
- Instructor del laboratorio: Ing. Irina Fong
