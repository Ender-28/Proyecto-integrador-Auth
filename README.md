# Portal de Recibos de Sueldo

## 1. Descripción del proyecto

Sistema web interno desarrollado para la empresa **Tecnologías del Atlántico S.A.** que permite a sus empleados iniciar sesión con su cuenta de Google y consultar sus datos personales y sueldo de forma segura.

El sistema reemplaza el envío de información por correo electrónico, solucionando los problemas de organización y seguridad que este método presentaba.

## 2. Requisitos previos

- **Node.js** v18 o superior
- **MySQL** 8.0 o superior
- **Navegador web** moderno (Chrome, Firefox, Edge, Safari)
- **Cuenta de Google** para crear el proyecto OAuth 2.0
- **Git** para control de versiones

### Dependencias del proyecto

| Dependencia | Versión | Propósito |
|-------------|---------|-----------|
| express | ^4.21.2 | Framework web para Node.js |
| passport | ^0.7.0 | Middleware de autenticación |
| passport-google-oauth20 | ^2.0.0 | Estrategia de autenticación con Google |
| mysql2 | ^3.12.0 | Cliente de MySQL con soporte async/await |
| express-session | ^1.18.1 | Manejo de sesiones |
| ejs | ^3.1.10 | Motor de plantillas HTML |
| dotenv | ^16.4.7 | Carga de variables de entorno |

### APIs utilizadas

- **Google OAuth 2.0 API** – para autenticación de usuarios

## 3. Instalación

```bash
# 1. Clonar el repositorio
git clone https://github.com/Ender-28/portal-recibos-sueldo.git
cd portal-recibos-sueldo

# 2. Instalar dependencias
npm install

# 3. Configurar variables de entorno
cp .env.example .env
# Editar .env con tus datos

# 4. Importar la base de datos
mysql -u root -p < database/schema.sql

# 5. Iniciar el servidor
npm start
```

## 4. Configuración

### 4.1 Creación del proyecto en Google Cloud

1. Ir a [Google Cloud Console](https://console.cloud.google.com/)
2. Crear un nuevo proyecto o seleccionar uno existente
3. Ir a **APIs & Services > Credentials**
4. Configurar la pantalla de consentimiento de OAuth (tipo externo)

### 4.2 Obtención del Client ID

1. En **Credentials**, hacer clic en **Create Credentials > OAuth client ID**
2. Seleccionar **Web application**
3. Agregar en **Authorized JavaScript origins**: `http://localhost:3000`
4. Agregar en **Authorized redirect URIs**: `http://localhost:3000/auth/google/callback`
5. Copiar el **Client ID** y **Client Secret** generados

### 4.3 Configuración de OAuth

Completar el archivo `.env` con los datos obtenidos:

```env
GOOGLE_CLIENT_ID=tu_client_id.apps.googleusercontent.com
GOOGLE_CLIENT_SECRET=tu_client_secret
CALLBACK_URL=http://localhost:3000/auth/google/callback
```

### 4.4 Configuración de la base de datos

1. Asegurarse de tener MySQL corriendo
2. Ejecutar el script `database/schema.sql` para crear la base de datos y la tabla
3. **Importante**: Insertar registros en la tabla `empleados` con los `google_id` reales de los empleados (se obtienen tras el primer inicio de sesión exitoso, o se pueden precargar)

## 5. Ejecución

```bash
# Modo producción
npm start

# Modo desarrollo (con recarga automática)
npm run dev
```

El servidor se iniciará en `http://localhost:3000`

## 6. Estructura del proyecto

```
portal-recibos-sueldo/
├── server.js              # Punto de entrada de la aplicación
├── package.json           # Configuración del proyecto y dependencias
├── .env.example           # Ejemplo de configuración de entorno
├── .gitignore             # Archivos ignorados por Git
├── README.md              # Documentación del proyecto
├── config/
│   ├── db.js              # Configuración y conexión a MySQL
│   └── passport.js        # Estrategia de autenticación con Google
├── middleware/
│   └── auth.js            # Middleware para proteger rutas
├── routes/
│   ├── auth.js            # Rutas de autenticación (login/logout)
│   └── profile.js         # Ruta del dashboard del empleado
├── views/
│   ├── index.ejs          # Pantalla de inicio con botón de Google
│   ├── dashboard.ejs      # Panel principal con datos del empleado
│   └── error.ejs          # Página de error personalizada
├── public/
│   ├── css/
│   │   └── style.css      # Estilos de la aplicación
│   └── js/
│       └── main.js        # JavaScript del frontend
└── database/
    ├── schema.sql         # Esquema de la base de datos
    └── export.sql         # Exportación completa de la base de datos
```

## 7. Capturas de pantalla

### Pantalla de inicio
![Pantalla de inicio](screenshots/login.png)
*Vista con el botón "Iniciar sesión con Google"*

### Panel del empleado
![Dashboard](screenshots/dashboard.png)
*Datos personales y sueldo del empleado autenticado*

## 8. Integrantes

| Nombre | Rol |
|--------|-----|
| [Nombre Apellido] | Desarrollo backend |
| [Nombre Apellido] | Desarrollo frontend |
| [Nombre Apellido] | Base de datos y documentación |

## 9. Dificultades encontradas

### Problema: Configuración de OAuth con dominios locales
**Solución:** Se utilizó `http://localhost:3000` como URI autorizada. Para producción, se debe configurar el dominio real.

### Problema: Relación entre cuenta de Google y empleado
**Solución:** Se almacena el `google_id` en la tabla `empleados`. Cada empleado debe tener su `google_id` registrado en la base de datos para poder acceder.

### Problema: Manejo de sesiones
**Solución:** Se implementó `express-session` con cookies seguras (`httpOnly: true`). En producción se recomienda usar un store persistente (MySQL session store o Redis).

## 10. Uso de IA

Este proyecto fue desarrollado con asistencia de herramientas de inteligencia artificial:

| Herramienta | Tarea |
|-------------|-------|
| GitHub Copilot / Claude | Generación de estructura del proyecto, rutas Express, configuración de Passport.js |
| IA | Diseño de interfaz de usuario responsive |
| IA | Documentación y README |
| IA | Esquema de base de datos y consultas SQL |

**Nota:** Todo el código generado fue revisado, comprendido y adaptado por los integrantes del grupo.
