# 🏋️ Human Perform Mobile App

> **Aplicación móvil multiplataforma para gestión integral de centro deportivo**  
> Desarrollada durante prácticas en **Fransdata Tech Ventures** (Mayo - Agosto 2025)

![Kotlin Multiplatform](https://img.shields.io/badge/Kotlin-Multiplatform-7F52FF?style=for-the-badge&logo=kotlin)
![Jetpack Compose](https://img.shields.io/badge/Jetpack-Compose-4285F4?style=for-the-badge&logo=jetpackcompose)
![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)

> [!IMPORTANT]
> Todo las caracteristicas de la aplicación móvil aqui mencionadas han sido desarrolladas por mi en su totalidad o gran parte de ellas. El resto de caracteristicas presentes en la aplicación móvil en
> producción son autoría de mis dos compañeros del equipo de desarrollo de la aplicación móvil.

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

### 🎨 **Experiencia de Usuario Premium**
- Material Design 3 con tema oscuro
- Navegación type-safe moderna
- Feedback visual con snackbars
- Accesibilidad optimizada

## 🏗️ Arquitectura Técnica

### 📋 Visión General del Sistema

```mermaid
graph TB
    subgraph "📱 Mobile App (Kotlin Multiplatform)"
        direction TB
        UI[🎨 UI Layer<br/>Jetpack Compose]
        VM[🧠 ViewModels<br/>State Management]
        UC[⚙️ Use Cases<br/>Business Logic]
        REPO[🔌 Repositories<br/>Data Abstraction]
        
        UI --> VM
        VM --> UC
        UC --> REPO
    end
    
    subgraph "🌐 Backend Services"
        direction TB
        API[🚀 Express.js API<br/>Node.js]
        AUTH[🔐 JWT Auth<br/>Middleware]
        DB[(🗄️ MariaDB<br/>Database)]
        
        API --> AUTH
        API --> DB
    end
    
    subgraph "☁️ External Services"
        GPAY[💳 Google Pay API]
        STORAGE[📁 File Storage]
    end
    
    REPO -.->|HTTP/REST| API
    API -.->|Integration| GPAY
    API -.->|File Upload| STORAGE
    
    classDef mobileLayer fill:#e1f5fe,stroke:#01579b,stroke-width:2px
    classDef backendLayer fill:#f3e5f5,stroke:#4a148c,stroke-width:2px
    classDef externalLayer fill:#e8f5e8,stroke:#1b5e20,stroke-width:2px
    
    class UI,VM,UC,REPO mobileLayer
    class API,AUTH,DB backendLayer
    class GPAY,STORAGE externalLayer
```

### 🎯 Clean Architecture + Hexagonal Architecture

La aplicación implementa **Clean Architecture** con principios de **Arquitectura Hexagonal**, garantizando:

- ✅ **Separación de responsabilidades** clara entre capas
- ✅ **Independencia de frameworks** y tecnologías externas  
- ✅ **Testabilidad** mediante inversión de dependencias
- ✅ **Mantenibilidad** y facilidad de evolución

```
📦 Estructura del Proyecto de la App Móvil
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

```mermaid
graph TD
    subgraph "🎯 Domain Layer (Core Business Logic)"
        ENT[📋 Entities<br/>User, Trainer, Payment]
        UC2[⚙️ Use Cases<br/>LoginUseCase, GetTrainersUseCase]
        REPO2[🔌 Repository Interfaces<br/>UserRepository, TrainerRepository]
        
        UC2 --> ENT
        UC2 --> REPO2
    end
    
    subgraph "🔌 Data Layer (Infrastructure)"
        REPO_IMPL[💾 Repository Implementations<br/>UserRepositoryImpl]
        DTO[📦 DTOs & Models<br/>UserDto, TrainerDto]
        CLIENT[🌐 HTTP Clients<br/>Ktor Client]
        CACHE[💽 Local Storage<br/>DataStore]
        
        REPO_IMPL --> DTO
        REPO_IMPL --> CLIENT
        REPO_IMPL --> CACHE
    end
    
    subgraph "📱 Presentation Layer"
        VM2[🧠 ViewModels<br/>LoginViewModel, ProfileViewModel]
        UI2[🎨 UI Components<br/>Jetpack Compose]
        
        VM2 --> UI2
    end
    
    VM2 -.-> UC2
    REPO_IMPL -.->|implements| REPO2
    
    classDef domainLayer fill:#fff3e0,stroke:#e65100,stroke-width:3px
    classDef dataLayer fill:#e8f5e8,stroke:#2e7d32,stroke-width:2px
    classDef presentationLayer fill:#e3f2fd,stroke:#1565c0,stroke-width:2px
    
    class ENT,UC2,REPO2 domainLayer
    class REPO_IMPL,DTO,CLIENT,CACHE dataLayer
    class VM2,UI2 presentationLayer
```

### 🤝 Kotlin Multiplatform - Código Compartido

**80% del código compartido** entre Android e iOS usando la estrategia `expect/actual`:

```mermaid
graph LR
    subgraph "📦 Kotlin Multiplatform Project"
        direction TB
        
        subgraph "🤝 commonMain (Shared Code - 80%)"
            COMMON_DOMAIN[🎯 Domain Logic<br/>Entities, Use Cases]
            COMMON_DATA[💾 Data Layer<br/>Repositories, DTOs]
            COMMON_NET[🌐 Network Layer<br/>Ktor Client]
            COMMON_UI[🎨 UI Logic<br/>ViewModels, States]
        end
        
        subgraph "🤖 androidMain (Android Specific)"
            ANDROID_PLAT[📱 Android Platform<br/>DataStore, File Access]
            ANDROID_UI[🎨 Android UI<br/>Activities, Themes]
        end
        
        subgraph "🍎 iosMain (iOS Specific)"
            IOS_PLAT[📱 iOS Platform<br/>Keychain, File Access]
            IOS_UI[🎨 iOS UI<br/>View Controllers]
        end
        
        COMMON_DOMAIN --> ANDROID_PLAT
        COMMON_DOMAIN --> IOS_PLAT
        COMMON_UI --> ANDROID_UI
        COMMON_UI --> IOS_UI
    end
    
    classDef commonCode fill:#f3e5f5,stroke:#7b1fa2,stroke-width:3px
    classDef androidCode fill:#e8f5e8,stroke:#388e3c,stroke-width:2px
    classDef iosCode fill:#e3f2fd,stroke:#1976d2,stroke-width:2px
    
    class COMMON_DOMAIN,COMMON_DATA,COMMON_NET,COMMON_UI commonCode
    class ANDROID_PLAT,ANDROID_UI androidCode
    class IOS_PLAT,IOS_UI iosCode
```

### 🔐 Flujo de Autenticación JWT

Sistema completo con **access tokens** y **refresh tokens** para máxima seguridad:

```mermaid
sequenceDiagram
    participant U as 📱 User
    participant APP as 🎨 Mobile App
    participant API as 🚀 API Server
    participant DB as 🗄️ Database
    
    U->>APP: Enter credentials
    APP->>API: POST /auth/login
    API->>DB: Validate user
    DB-->>API: User data
    API->>API: Generate JWT + Refresh Token
    API-->>APP: {accessToken, refreshToken}
    APP->>APP: Store tokens in DataStore
    
    Note over APP,API: Subsequent API calls
    APP->>API: GET /users/profile<br/>Authorization: Bearer {accessToken}
    
    alt Token expired
        API-->>APP: 401 Unauthorized
        APP->>API: POST /auth/refresh<br/>{refreshToken}
        API-->>APP: New {accessToken}
        APP->>API: Retry original request
    end
    
    API-->>APP: Protected resource data
```

### 📁 Estructura de Proyecto

```mermaid
graph TD
    ROOT[📦 FitCenter Project]
    
    subgraph "📱 Mobile Application"
        MOBILE[🎯 mobile-app/]
        
        subgraph "Kotlin Multiplatform Modules"
            SHARED[🤝 shared/<br/>commonMain, androidMain, iosMain]
            ANDROID[🤖 androidApp/<br/>Android specific UI]
            IOS[🍎 iosApp/<br/>iOS specific UI]
        end
        
        subgraph "Shared Module Structure"
            DOMAIN_MOD[🎯 domain/<br/>entities, usecases, repositories]
            DATA_MOD[💾 data/<br/>models, clients, implementations]
            COMMON_UI_MOD[🎨 commonUI/<br/>viewmodels, navigation]
        end
    end
    
    subgraph "🌐 Backend API"
        API_ROOT[⚡ api/]
        
        subgraph "API Structure"
            ROUTES[🛣️ routes/<br/>auth, users, trainers]
            MIDDLEWARE[🔒 middleware/<br/>auth, validation]
            MODELS[📋 models/<br/>database schemas]
            CONFIG[⚙️ config/<br/>database, jwt]
        end
    end
    
    ROOT --> MOBILE
    ROOT --> API_ROOT
    MOBILE --> SHARED
    MOBILE --> ANDROID
    MOBILE --> IOS
    SHARED --> DOMAIN_MOD
    SHARED --> DATA_MOD  
    SHARED --> COMMON_UI_MOD
    API_ROOT --> ROUTES
    API_ROOT --> MIDDLEWARE
    API_ROOT --> MODELS
    API_ROOT --> CONFIG
    
    classDef rootStyle fill:#fff3e0,stroke:#f57c00,stroke-width:3px
    classDef mobileStyle fill:#e8f5e8,stroke:#388e3c,stroke-width:2px
    classDef backendStyle fill:#e3f2fd,stroke:#1976d2,stroke-width:2px
    classDef moduleStyle fill:#fce4ec,stroke:#c2185b,stroke-width:1px
    
    class ROOT rootStyle
    class MOBILE,SHARED,ANDROID,IOS,DOMAIN_MOD,DATA_MOD,COMMON_UI_MOD mobileStyle
    class API_ROOT,ROUTES,MIDDLEWARE,MODELS,CONFIG backendStyle
```

### 🔄 Flujo de Datos Unidireccional

```
👆 UI Events → 🧠 ViewModels → ⚙️ Use Cases → 🔌 Repositories → 🌐 API/Database
                    ↓                            ↓                    ↓
📱 UI State Updates ←────────────────────────────┴────────────────────┘
```


## 💡 Decisiones Técnicas Clave

### **¿Por qué Clean Architecture?**
- **Testabilidad**: Cada capa se puede testear independientemente
- **Flexibilidad**: Fácil cambiar de Ktor a Retrofit, o MariaDB a PostgreSQL
- **Escalabilidad**: Agregar nuevas features sin afectar código existente

### **¿Por qué Kotlin Multiplatform?**
- **Eficiencia**: 80% del código reutilizable = menos bugs, menos mantenimiento
- **Consistencia**: Misma lógica de negocio en ambas plataformas
- **Performance**: Compilación nativa, no híbrido como React Native

### **¿Por qué JWT + Refresh Tokens?**
- **Seguridad**: Access tokens de corta duración (15min) + refresh tokens seguros
- **UX**: Login automático sin interrumpir la experiencia del usuario
- **Escalabilidad**: Stateless, fácil de escalar horizontalmente

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
- **Clean Code**: Principios SOLID y buenas prácticas
- **IDEs**: WebStorm y Android Studio

## 📊 Métricas del Proyecto

- **+350 horas** de desarrollo
- **14 endpoints** REST implementados
- **2 plataformas** soportadas (Android/iOS, solo he trabajado con Android)
- **5 módulos principales** de funcionalidad
- **80% código compartido** entre plataformas

## 🔗 Endpoints API desarrollados

> \[!NOTE]
> Todos los endpoints protegidos requieren **token JWT** válido vía `verifyToken`.

---

### **Autenticación**

| Método | Ruta                    | Descripción                                                                                                |
| ------ | ----------------------- | ---------------------------------------------------------------------------------------------------------- |
| `POST` | `/auth/register`        | Registro de usuario nuevo con imagen de perfil (`profile_pic`). Procesa y comprime imagen si es necesario. |
| `POST` | `/auth/login`           | Autenticación de usuario y entrega de tokens.                                                              |
| `POST` | `/auth/refresh`         | Renovación de token de acceso.                                                                             |
| `PUT`  | `/auth/change-password` | Cambia la contraseña del usuario autenticado.                                                              |
| `PUT`  | `/auth/reset-password`  | Restablece la contraseña (flujo de recuperación).                                                          |

---

### **Usuarios**

| Método   | Ruta                   | Descripción                                                              |
| -------- | ---------------------- | ------------------------------------------------------------------------ |
| `PUT`    | `/user`                | Actualiza el perfil del usuario (con soporte para nueva foto de perfil). |
| `DELETE` | `/user`                | Elimina la cuenta del usuario autenticado.                               |
| `DELETE` | `/user/photo`          | Elimina la foto de perfil del usuario.                                   |
| `POST`   | `/user/:userId/coupon` | Asigna un cupón a un usuario específico.                                 |
| `GET`    | `/user/:userId/coupon` | Obtiene el cupón asignado a un usuario.                                  |
| `POST`   | `/user/document`       | Sube un documento asociado al usuario (campo `file`).                    |

---

### **Entrenadores**

| Método | Ruta                    | Descripción                                  |
| ------ | ----------------------- | -------------------------------------------- |
| `GET`  | `/list_coaches`         | Lista de entrenadores disponibles.           |
| `POST` | `/user/preferred-coach` | Asigna entrenador preferido.                 |
| `GET`  | `/user/preferred-coach` | Obtiene el entrenador preferido del usuario. |

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
- Trabajo en equipo
- Resolución de problemas críticos en producción

---

⭐ **¿Te gustó el proyecto?** ¡Dale una estrella al repositorio!
