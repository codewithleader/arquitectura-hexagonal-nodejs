# Estructura de directorio para Arquitectura hexagonal

iam/
├── src/
│   ├── application/
│   │   ├── commands/        # Comandos para operaciones como crear usuarios, iniciar sesión, etc.
│   │   │   ├── CreateUserCommand.ts
│   │   │   ├── AuthenticateUserCommand.ts
│   │   │   └── ...
│   │   ├── queries/              # Consultas para leer datos, por ejemplo, detalles del usuario.
│   │   │   ├── GetUserQuery.ts
│   │   │   └── ...
│   │   └── services/             # Servicios que coordinan los comandos y consultas.
│   │       ├── UserService.ts
│   │       └── AuthService.ts
│   ├── domain/
│   │   ├── entities/             # Entidades del dominio como User.
│   │   │   ├── User.ts
│   │   │   └── ...
│   │   ├── repositories/         # Interfaces de los repositorios para abstracción de la capa de datos.
│   │   │   ├── UserRepository.ts
│   │   │   └── ...
│   │   ├── services/             # Servicios de dominio, lógica de negocio.
│   │   │   ├── PasswordHasher.ts
│   │   │   └── ...
│   │   └── value-objects/        # Objetos de valor como Email, Password, etc.
│   │       ├── Email.ts
│   │       ├── Password.ts
│   │       └── ...
│   ├── infrastructure/
│   │   ├── config/               # Configuraciones, como variables de entorno.
│   │   ├── db/                   # Implementación de la base de datos.
│   │   │   ├── UserRepositoryImpl.ts  # Implementación concreta de UserRepository.
│   │   │   └── ...
│   │   ├── web/
│   │   │   ├── controllers/      # Controladores para manejar las solicitudes HTTP.
│   │   │   │   ├── UserController.ts
│   │   │   │   └── AuthController.ts
│   │   │   ├── middleware/       # Middleware, como el manejo de autenticación.
│   │   │   └── validators/       # Validadores de datos de entrada.
│   │   └── security/             # Detalles de implementación de seguridad, como JWT.
│   │       ├── JwtProvider.ts
│   │       └── ...
│   └── presentation/             # Opcional, si se distingue de la infraestructura, para GUI o API.
│       └── rest/
│           ├── dto/              # Data Transfer Objects para la capa de presentación.
│           ├── mappers/          # Mapeadores entre entidades de dominio y DTOs.
│           └── ...
└── tests/                         # Tests, posiblemente reflejando la estructura de src.
    ├── application/
    ├── domain/
    └── infrastructure/

# Arquitectura Hexagonal por Modulos

- Ejemplo de Estructura Modular: Supongamos que tienes una aplicación con múltiples dominios de negocio, como IAM (Identity and Access Management), Productos, y Pedidos. Una estructura modular podría verse así:

src/
├── iam/                             # Módulo de IAM
│   ├── application/
│   ├── domain/
│   ├── infrastructure/
│   └── presentation/
├── products/                        # Módulo de Productos
│   ├── application/
│   ├── domain/
│   ├── infrastructure/
│   └── presentation/
├── orders/                          # Módulo de Pedidos
│   ├── application/
│   ├── domain/
│   ├── infrastructure/
│   └── presentation/
└── common/                          # Código común, utilidades, interfaces compartidas, etc.

# Domain
Centro de la Arquitectura: La capa de dominio se encuentra en el centro de la arquitectura. Es el corazón del negocio y define las reglas y lógicas que son fundamentales para la aplicación. Esta capa es independiente de cualquier otra capa, es decir, no tiene dependencias hacia afuera.
Responsabilidad: Contiene entidades, objetos de valor, eventos de dominio, y la lógica de negocio.

# Application
Orquestación: La capa de aplicación rodea al dominio y actúa como un mediador entre la lógica de negocio (dominio) y las capas externas (infraestructura, presentación). Orquesta cómo se utiliza el dominio para implementar los casos de uso de la aplicación.
Dependencia: Depende de la capa de dominio, pero no de las capas de infraestructura o presentación. Puede dependender de abstracciones que pertenecen a la infraestructura.

# Infrastructure
Soporte: La capa de infraestructura proporciona implementaciones concretas para las abstracciones definidas en las capas superiores, como bases de datos, sistemas de archivos, colas de mensajes, etc.
Dependencia: Depende de la capa de dominio y, en algunos casos, de la capa de aplicación para implementar interfaces o abstracciones definidas por ellas.
Presentation
Interfaz con el Usuario/Externo: La capa de presentación es la que interactúa directamente con los usuarios o sistemas externos. Se encarga de presentar datos al usuario y de interpretar las acciones del usuario en operaciones que la aplicación puede realizar.
Dependencia: Generalmente, depende de la capa de aplicación para procesar las acciones del usuario, invocar casos de uso, etc. No debería tener dependencias directas con la capa de dominio, manteniendo así la separación de preocupaciones.
Jerarquía y Flujo
Si tuviéramos que hablar de una jerarquía en términos de flujo de control o dependencia (donde "superior" implica estar más cerca del usuario o del exterior y "inferior" más cercano al núcleo del negocio), podríamos describirlo así:

# Presentation (Superior): Primera capa de contacto con el exterior, recibe inputs y muestra outputs.
Application: Orquesta las operaciones, traduciendo las solicitudes de la capa de presentación en acciones del dominio.
Domain (Inferior): Núcleo lógico, no sabe nada del exterior, sólo de las reglas de negocio.
Infrastructure: Soporta a todas las anteriores proporcionando las implementaciones necesarias para operar en el mundo real, pero es "exterior" respecto a la lógica de negocio.
Es importante destacar que, aunque hablamos de jerarquías y dependencias, el objetivo principal de estas separaciones es promover la inversión de dependencias donde las capas más internas definen interfaces o contratos y las capas más externas proveen implementaciones concretas, facilitando la separación de preocupaciones, la modularidad y la capacidad de prueba del software.