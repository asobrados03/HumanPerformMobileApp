# 🏋️ Human Perform Mobile App

> **Aplicación móvil multiplataforma para gestión integral de centro deportivo**  
> Desarrollada durante prácticas en **Fransdata Tech Ventures** (Mayo - Agosto 2025)

![Kotlin Multiplatform](https://img.shields.io/badge/Kotlin-Multiplatform-7F52FF?style=for-the-badge&logo=kotlin)
![Jetpack Compose](https://img.shields.io/badge/Jetpack-Compose-4285F4?style=for-the-badge&logo=jetpackcompose)
![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)

> [!IMPORTANT]
> Todo las caracteristicas de la aplicación móvil aqui mencionadas han sido desarrolladas por mi en su totalidad o gran parte de ellas. El resto de caracteristicas presentes en la aplicación móvil en
> producción son autoría de mis dos compañeros del equipo de desarrollo de la aplicación móvil

## 📱 Sobre el Proyecto

**Human Perform** es una aplicación móvil multiplataforma desarrollada para un centro deportivo real en Segovia. La app permite a los usuarios gestionar sus entrenamientos, perfiles, favoritos y pagos de manera integral, mientras que el centro puede administrar sus servicios y entrenadores.

**🚀 Estado**: En producción - Utilizada por usuarios reales del centro deportivo

## ✨ Características Principales

### 🔐 **Sistema de Autenticación Completo**
- Registro de usuarios con validación integral
- Login seguro con tokens JWT + refresh tokens
- Autenticación persistente con DataStore cifrado
- Recuperación y cambio de contraseñas
- Cierre de sesión automático por expiración

### 👤 **Gestión de Perfiles Avanzada**
- Carga y edición de imágenes de perfil
- Información personal completa con direcciones
- Configuraciones personalizables
- Eliminación segura de cuentas

### 🏃 **Sistema de Entrenadores**
- Catálogo completo de entrenadores disponibles
- Sistema de favoritos con persistencia
- Información detallada y especialidades

### 💳 **Integración de Pagos**
- Google Pay integrado
- Sistema de cupones y descuentos
- Procesamiento seguro de transacciones

### 🎨 **Experiencia de Usuario Premium**
- Material Design 3 con tema oscuro
- Navegación type-safe moderna
- Feedback visual con snackbars
- Accesibilidad optimizada

## 🏗️ Arquitectura Técnica

### **Clean Architecture + Hexagonal Architecture**

```
📦 Estructura del Proyecto
├── 🎯 domain/               # Lógica de Negocio Pura
│   ├── security/            # Modulos para encriptar y guardar en el almacenamiento local
│   ├── repository/          # Interfaces de los repositorios
│   ├── storage/             # Almacenamiento local del dispositivo móvil
│   └── usecases/            # Casos de uso de la aplicación
├── 🔌 data/                # Capa de Infraestructura
│   ├── model/              # DTOs y modelos de datos
│   ├── persistence/        # Implementaciones de repositorios
│   └── network/            # Clientes HTTP (Ktor)
└── 📱 presentation/        # Capa de Presentación
    ├── app/                 # Contiene el punto de entrada de la aplicación, la configuración global de Jetpack Compose y la navegación principal
    │   ├── navigation/      # Arranque y grafo de navegación
    ├── data/                # Capa de datos (implementación)           
    ├── di/                  # Módulo de inyección manual
    ├── ui/                  # Capa de presentación (Compose)
    │   ├── components/      # Composables reutilizables
    │   ├── screens/         # Composables por pantalla
    │   ├── theme/           # Color, Typography, Shapes, Theme
    │   ├── util/            # Helpers de UI (formatters, validators)
    │   └── viewmodel/       # ViewModels + estados/fábricas actuales
    │       └── state/             
    └── worker/              # Tareas en background (WorkManager)

```

### **Multiplataforma con Kotlin Multiplatform**
- **`commonMain`**: 80% del código compartido (lógica de negocio, red, datos)
- **`androidMain`**: Implementaciones específicas Android
- **`iosMain`**: Implementaciones específicas iOS
- **`expect/actual`**: Contratos comunes con implementaciones nativas

### **Flujo de Datos Unidireccional**
```
UI Events → Use Cases → Repositories → API/Database → UI State Updates
```

## 🛠️ Stack Tecnológico

### **Frontend Móvil**
- **Kotlin Multiplatform Mobile**: Compartir código entre plataformas
- **Jetpack Compose**: UI moderna y declarativa
- **Navigation Compose**: Navegación type-safe
- **DataStore**: Persistencia cifrada local
- **Ktor Client**: Cliente HTTP multiplataforma

### **Backend API**
- **Node.js + Express.js**: Servidor RESTful
- **MariaDB**: Base de datos relacional
- **JWT**: Autenticación stateless
- **Bcrypt**: Hash seguro de contraseñas

### **Herramientas de Desarrollo**
- **Git**: Control de versiones con branching strategy
- **Metodologías Ágiles**: Sprints y entregas iterativas
- **Clean Code**: Principios SOLID y buenas prácticas

## 📊 Métricas del Proyecto

- **300 horas** de desarrollo
- **14 endpoints** REST implementados
- **2 plataformas** soportadas (Android/iOS, solo he trabajado con Android)
- **5 módulos principales** de funcionalidad
- **80% código compartido** entre plataformas

## 🔗 Endpoints API Principales

```http
POST   /auth/register          # Registro de usuarios
POST   /auth/login            # Autenticación
GET    /auth/refresh          # Renovación de tokens
GET    /users/profile         # Perfil de usuario
PUT    /users/profile         # Actualización de perfil
GET    /trainers              # Lista de entrenadores
POST   /trainers/favorite     # Marcar favoritos
```

## 🚀 Instalación y Desarrollo

```bash
# Clonar el repositorio
git clone [repository-url]

# Frontend (Kotlin Multiplatform)
cd mobile-app
./gradlew build

# Backend (Node.js)
cd api
npm install
npm run dev
```

## 🎯 Logros Técnicos Destacados

✅ **Arquitectura Escalable**: Implementación exitosa de Clean + Hexagonal Architecture  
✅ **Multiplataforma Eficiente**: 80% de código reutilizable entre Android e iOS  
✅ **Seguridad Robusta**: Sistema JWT completo con refresh tokens  
✅ **UX Premium**: Material Design 3 con tema oscuro optimizado  
✅ **Producción Real**: Aplicación utilizada por usuarios finales  

## 📝 Aprendizajes Clave

Durante este proyecto de 3 meses adquirí experiencia práctica en:

- Arquitecturas empresariales aplicadas a proyectos reales
- Desarrollo multiplataforma con Kotlin Multiplatform
- Diseño e implementación de APIs RESTful seguras
- Gestión de estado y navegación en aplicaciones móviles
- Integración de servicios de terceros (Google Pay)
- Metodologías ágiles y trabajo en equipo
- Resolución de problemas críticos en producción

---

⭐ **¿Te gustó el proyecto?** ¡Dale una estrella al repositorio!

📧 **Contacto**: alfredo.sobrados@estudiantes.uva.es  
💼 **LinkedIn**: [Tu perfil LinkedIn]
