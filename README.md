# 🛍️ Proyecto Backend - E-commerce API

Una API REST completa construida con **Node.js y Express** para un sistema de comercio electrónico robusto. Proporciona funcionalidades de autenticación, gestión de productos, carrito de compras, órdenes y más, con diferentes roles de usuario (buyer, business, admin).

---

## 📋 Tabla de Contenidos

- Descripción del Proyecto
- Características Principales
- Requisitos Previos
- Instalación y Configuración
- Ejecución del Proyecto
- Endpoints Principales
- Estructura del Proyecto
- Seguridad y Buenas Prácticas
- Testing
- Contribución
- Licencia

---

## 📝 Descripción del Proyecto

Este proyecto es una API backend para un sistema de e-commerce que permite:

- **Gestión de usuarios** con tres roles distintos: compradores (buyers), vendedores (business) y administradores (admin)
- **Autenticación y autorización** segura con JWT y validación de roles
- **Catálogo de productos** con creación, actualización y eliminación
- **Carrito de compras** con gestión de productos
- **Sistema de órdenes** con estados (pending, confirmed, cancelled)
- **Notificaciones por email** para confirmaciones de órdenes
- **Validación de datos** robusta con Joi
- **Documentación interactiva** con Swagger/OpenAPI

---

## ✨ Características Principales

### Autenticación y Autorización
- Registro e inicio de sesión con validación de credenciales
- JWT (JSON Web Tokens) para autenticación segura
- Control de acceso basado en roles (RBAC)
- Recuperación de contraseña con email
- Cookies HTTP-only para mayor seguridad

### Gestión de Usuarios
- Tres tipos de usuarios con permisos diferenciados
- Actualización de perfil
- Listar usuarios por rol

### Productos
- CRUD completo de productos
- Asociación de productos con negocios
- Control de stock
- Validación de datos de entrada

### Carrito de Compras
- Crear y gestionar carritos por usuario
- Agregar/eliminar productos
- Cálculo automático de totales

### Órdenes
- Crear órdenes desde el carrito
- Gestión de estados de órdenes
- Historial de órdenes por comprador y vendedor
- Transacciones ACID en creación de órdenes
- Actualización automática de stock

### Comunicaciones
- Envío de emails de confirmación
- Envío de SMS a través de Twilio (opcional)

### Documentación
- Swagger/OpenAPI integrado en `/api-docs`
- Documentación completa de endpoints

---

## 🛠️ Tecnologías Utilizadas

| Categoría | Tecnología |
|-----------|-----------|
| **Runtime** | Node.js |
| **Framework** | Express.js v4.21.2 |
| **Base de Datos** | MongoDB con Mongoose v8.9.2 |
| **Autenticación** | Passport.js, JWT |
| **Validación** | Joi |
| **Hashing** | bcrypt |
| **Email** | Nodemailer |
| **SMS** | Twilio |
| **Documentación** | Swagger/OpenAPI |
| **Logging** | Winston |
| **Rate Limiting** | express-rate-limit |
| **Testing** | Mocha, Chai, Supertest |

---

## 📦 Requisitos Previos

Antes de ejecutar el proyecto, asegúrate de tener instalado:

- **Node.js** >= 16.x
- **npm** >= 8.x o **yarn**
- **MongoDB** >= 5.0 (local o cloud, ej: MongoDB Atlas)
- **Git**
- Una cuenta de **Gmail** para envío de emails (con contraseña de aplicación)
- *(Opcional)* Cuenta de **Twilio** para envío de SMS

---

## 🚀 Instalación y Configuración

### 1. Clonar el Repositorio

```bash
git clone https://github.com/tu-usuario/jhonedwar-ed-shop.git
cd jhonedwar-ed-shop
```

### 2. Instalar Dependencias

```bash
npm install
```

### 3. Configurar Variables de Entorno

Crea un archivo `.env` en la raíz del proyecto basándote en `.env.example`:

```bash
cp .env.example .env
```

Edita el archivo `.env` y configura las siguientes variables:

```env
# Servidor
PORT=3000
NODE_ENV=development

# Base de Datos
MONGO=mongodb://localhost:27017/jhonedwar-ecommerce
# Alternativa: MongoDB Atlas
# MONGO=mongodb+srv://usuario:contraseña@cluster.mongodb.net/jhonedwar-ecommerce

# Autenticación
SECRET_JWT=tu_clave_secreta_super_segura_aqui

# Frontend
FRONTEND_URL=http://localhost:3000

# Email (Gmail)
EMAIL_NODEMAILER=tu_email@gmail.com
PASSWORD_NODEMAILER=tu_contraseña_aplicacion_gmail

# Twilio (Opcional)
TWILIO_ACCOUNT_SID=tu_account_sid
TWILIO_AUTH_TOKEN=tu_auth_token
TWILIO_PHONE_NUMBER=+1234567890
```

**⚠️ Notas Importantes:**
- Genera una contraseña de aplicación en [Google Account Security](https://myaccount.google.com/apppasswords)
- Mantén `.env` fuera del control de versiones (ya está en `.gitignore`)
- Usa valores seguros para `SECRET_JWT` (mínimo 32 caracteres)

### 4. Crear Carpeta de Logs

```bash
mkdir -p logs
```

---

## 🏃 Ejecución del Proyecto

### Modo Desarrollo

Con recarga automática gracias a Nodemon:

```bash
npm run dev
```

El servidor estará disponible en: `http://localhost:3000`
Documentación Swagger: `http://localhost:3000/api-docs`

### Modo Producción

```bash
npm start
```

---

## 🔌 Endpoints Principales

### 🔐 Autenticación (`/api/users`)

#### Registro
```
POST /api/users/register
Content-Type: application/json

{
  "firstName": "Juan",
  "lastName": "Pérez",
  "email": "juan@example.com",
  "password": "SecurePass123",
  "role": "buyer"
}

Respuesta (201):
{
  "status": "success",
  "message": "User registered",
  "payload": {
    "_id": "60f7b3b3b3b3b3b3b3b3b3b3",
    "email": "juan@example.com",
    "role": "buyer"
  }
}
```

#### Login
```
POST /api/users/login
Content-Type: application/json

{
  "email": "juan@example.com",
  "password": "SecurePass123",
  "role": "buyer"
}

Respuesta (200):
{
  "status": "success",
  "message": "Ok login",
  "payload": {
    "_id": "60f7b3b3b3b3b3b3b3b3b3b3",
    "email": "juan@example.com",
    "role": "buyer"
  }
}
```

#### Logout
```
GET /api/users/logout

Respuesta (200):
{
  "message": "sesión cerrada"
}
```

#### Recuperar Contraseña
```
POST /api/users/passwordReset
Content-Type: application/json

{
  "email": "juan@example.com"
}

Respuesta (200):
{
  "status": "success",
  "message": "Password reset email sent",
  "payload": {...}
}
```

---

### 📦 Productos (`/api/products`)

#### Obtener Todos los Productos
```
GET /api/products
Authorization: Bearer <token>

Respuesta (200):
{
  "status": "success",
  "message": "Request processed successfully.",
  "payload": [
    {
      "_id": "60f7b3b3b3b3b3b3b3b3b3b3",
      "title": "Laptop Dell XPS",
      "price": 1299.99,
      "stock": 25,
      "business": "60f7b3b3b3b3b3b3b3b3b3b3"
    }
  ]
}
```

#### Crear Producto (Solo Business)
```
POST /api/products
Authorization: Bearer <token>
Content-Type: application/json

{
  "title": "iPhone 15",
  "price": 999.99,
  "stock": 50,
  "business": "60f7b3b3b3b3b3b3b3b3b3b3"
}

Respuesta (201):
{
  "status": "success",
  "message": "Product created successfully",
  "payload": {...}
}
```

#### Actualizar Producto
```
PATCH /api/products/:id
Authorization: Bearer <token>
Content-Type: application/json

{
  "price": 899.99,
  "stock": 30
}
```

#### Eliminar Producto
```
DELETE /api/products/:id
Authorization: Bearer <token>
```

---

### 🛒 Carrito (`/api/cart`)

#### Obtener Carrito del Comprador
```
GET /api/cart/carts
Authorization: Bearer <token>
```

#### Crear Carrito
```
POST /api/cart
Authorization: Bearer <token>
Content-Type: application/json

{
  "products": []
}
```

#### Agregar Producto al Carrito
```
POST /api/cart/products/:idCart
Authorization: Bearer <token>
Content-Type: application/json

{
  "productId": "60f7b3b3b3b3b3b3b3b3b3b3",
  "quantity": 2
}
```

#### Eliminar Carrito
```
DELETE /api/cart/:idCart
Authorization: Bearer <token>
```

---

### 📋 Órdenes (`/api/order`)

#### Crear Orden
```
POST /api/order
Authorization: Bearer <token>
Content-Type: application/json

{
  "idBuyer": "60f7b3b3b3b3b3b3b3b3b3b3",
  "idBusiness": "60f7b3b3b3b3b3b3b3b3b3b3",
  "products": [
    {
      "_id": "60f7b3b3b3b3b3b3b3b3b3b3",
      "quantity": 2
    }
  ]
}

Respuesta (200):
{
  "status": "success",
  "message": "Order create successfully",
  "payload": {
    "_id": "60f7b3b3b3b3b3b3b3b3b3b3",
    "buyer": "60f7b3b3b3b3b3b3b3b3b3b3",
    "business": "60f7b3b3b3b3b3b3b3b3b3b3",
    "status": "pending",
    "totalPrice": 199.98
  }
}
```

#### Obtener Órdenes del Comprador
```
GET /api/order/buyer/:idBuyer
Authorization: Bearer <token>
```

#### Obtener Órdenes del Negocio
```
GET /api/order/business/:idBusiness
Authorization: Bearer <token>
```

#### Actualizar Estado de Orden (Solo Business)
```
POST /api/order/:id
Authorization: Bearer <token>
Content-Type: application/json

{
  "resolve": "confirmed"
}
```

---

### 👥 Negocios (`/api/business`)

#### Obtener Todos los Negocios (Solo Buyers)
```
GET /api/business
Authorization: Bearer <token>
```

#### Obtener Negocio por ID
```
GET /api/business/:id
Authorization: Bearer <token>
```

#### Agregar Producto al Negocio
```
POST /api/business/:id/product
Authorization: Bearer <token>
Content-Type: application/json

{
  "title": "Producto Nuevo",
  "price": 49.99,
  "stock": 100
}
```

---

### 👤 Compradores (`/api/buyer`)

#### Obtener Todos los Compradores (Solo Business)
```
GET /api/buyer
Authorization: Bearer <token>
```

#### Obtener Comprador por ID
```
GET /api/buyer/:id
Authorization: Bearer <token>
```

#### Actualizar Comprador
```
PATCH /api/buyer/:id
Authorization: Bearer <token>
Content-Type: application/json

{
  "firstName": "Nuevo Nombre",
  "phone": "+1234567890"
}
```

---

## 📁 Estructura del Proyecto

```
jhonedwar-ed-shop/
│
├── src/
│   ├── config/              # Configuraciones
│   │   ├── config.js        # Variables de entorno
│   │   ├── logger.js        # Winston logger
│   │   ├── passport.config.js
│   │   ├── rateLimiter.js
│   │   └── dirname.js
│   │
│   ├── controllers/         # Lógica de solicitudes
│   │   ├── auth.controller.js
│   │   ├── product.controller.js
│   │   ├── cart.controller.js
│   │   ├── orders.controller.js
│   │   ├── buyer.controller.js
│   │   └── business.controller.js
│   │
│   ├── services/            # Lógica de negocio
│   │   ├── auth.service.js
│   │   ├── product.service.js
│   │   ├── cart.service.js
│   │   ├── orders.service.js
│   │   ├── buyer.service.js
│   │   ├── business.service.js
│   │   └── passwordReset.service.js
│   │
│   ├── daos/                # Data Access Objects (MongoDB)
│   │   ├── admin.dao.js
│   │   ├── buyer.dao.js
│   │   ├── business.dao.js
│   │   ├── cart.dao.js
│   │   ├── orders.dao.js
│   │   ├── product.dao.js
│   │   └── passwordReset.dao.js
│   │
│   ├── models/              # Esquemas de Mongoose
│   │   ├── admin.model.js
│   │   ├── buyer.model.js
│   │   ├── business.model.js
│   │   ├── cart.model.js
│   │   ├── orders.model.js
│   │   ├── product.model.js
│   │   └── passwordReset.model.js
│   │
│   ├── routes/              # Rutas de la API
│   │   ├── auth.routes.js
│   │   ├── product.routes.js
│   │   ├── cart.routes.js
│   │   ├── order.routes.js
│   │   ├── buyer.routes.js
│   │   └── business.routes.js
│   │
│   ├── middlewares/         # Middleware personalizado
│   │   ├── authorization.js # Validación de roles
│   │   ├── errorHandler.js  # Manejo de errores
│   │   └── validate.js      # Validación de datos
│   │
│   ├── validations/         # Esquemas de validación (Joi)
│   │   ├── auth.validation.js
│   │   ├── product.validation.js
│   │   ├── cart.validation.js
│   │   └── order.validation.js
│   │
│   ├── dtos/                # Data Transfer Objects
│   │   ├── buyer.dto.js
│   │   └── business.dto.js
│   │
│   ├── docs/                # Documentación Swagger
│   │   └── swagger/
│   │       ├── swagger.js
│   │       ├── paths/       # Definición de endpoints
│   │       └── schemas/     # Definición de modelos
│   │
│   ├── utils/               # Funciones utilitarias
│   │   ├── AppError.js      # Clase de error personalizada
│   │   ├── asyncHandler.js
│   │   ├── generateToken.js
│   │   ├── hashingUtils.js
│   │   ├── passportCall.js
│   │   ├── sendConfirmationEmail.js
│   │   ├── sendPasswordResetEmail.js
│   │   ├── twilioSms.js
│   │   └── generateCustomResponses.js
│   │
│   └── app.js               # Configuración principal de Express
│
├── test/                    # Tests automatizados
│   └── routes/
│       ├── product.router.test.js
│       ├── cart.router.test.js
│       ├── order.router.test.js
│       ├── buyer.router.test.js
│       └── business.router.test.js
│
├── logs/                    # Logs de la aplicación (generado)
├── public/                  # Archivos estáticos
├── .env.example            # Variables de entorno (ejemplo)
├── .gitignore              # Archivos a ignorar en Git
├── package.json            # Dependencias del proyecto
└── README.md               # Este archivo
```

### Explicación de Capas

- **Controllers**: Reciben las solicitudes HTTP y orquestan la respuesta
- **Services**: Contienen la lógica de negocio y validaciones
- **DAOs**: Interactúan directamente con la base de datos
- **Models**: Definen la estructura de datos en MongoDB
- **Routes**: Mapean URLs a controladores
- **Middlewares**: Procesan solicitudes (autenticación, validación, errores)
- **Utils**: Funciones reutilizables

---

## 🔒 Seguridad y Buenas Prácticas

### Autenticación y Autorización

✅ **JWT (JSON Web Tokens)**: Todos los endpoints protegidos usan JWT para autenticación
✅ **Roles**: Control de acceso basado en roles (RBAC) - buyer, business, admin
✅ **Cookies HTTP-only**: Se almacenan tokens en cookies con flag `httpOnly` para prevenir XSS
✅ **Validación de Contraseña**: Uso de bcrypt para hash seguro con 10 rounds

```javascript
// Ejemplo de protección de ruta
router.get('/:id', passportCall('jwt'), authorization(['buyer']), getBusinessById)
```

### Validación de Datos

✅ **Joi**: Validación exhaustiva de entrada con esquemas predefinidos
✅ **Sanitización**: Limpieza automática de espacios en blanco
✅ **Tipos Estrictos**: MongoDB ObjectID validation

```javascript
// Ejemplo de esquema de validación
export const createProductSchema = Joi.object({
  title: Joi.string().min(2).max(100).required().trim(),
  price: Joi.number().positive().precision(2).required(),
  stock: Joi.number().integer().min(0).required()
});
```

### Manejo de Errores

✅ **AppError Personalizado**: Clase de error con statusCode
✅ **Error Handler Middleware**: Centraliza el manejo de excepciones
✅ **Logging**: Winston registra errores con stack trace completo

### Rate Limiting

✅ **express-rate-limit**: Máximo 100 requests por IP cada 15 minutos

```javascript
const generalLimiter = rateLimit({
  windowMs: 15 * 60 * 1000,
  max: 100,
  message: 'Demasiadas peticiones desde esta IP'
});
```

### Logging

✅ **Winston**: Logger multi-nivel (error, warn, info, http, debug)
✅ **Archivos Separados**: Logs de error en `error.log` y todos en `combined.log`
✅ **Rotación de Logs**: Máximo 5 archivos de 5MB cada uno

### Transacciones ACID

✅ **MongoDB Transactions**: Las órdenes usan transacciones para garantizar consistencia

```javascript
// Ejemplo de transacción
const session = await mongoose.startSession();
session.startTransaction();
try {
  // Operaciones múltiples
  await session.commitTransaction();
} catch (error) {
  await session.abortTransaction();
}
```

### Protección CORS

✅ **CORS Configurado**: Solo acepta solicitudes desde `FRONTEND_URL`

### Variables de Entorno

✅ **dotenv**: Configuración segura sin exponerlas en el código
✅ `.env` en `.gitignore`: Nunca se commitea información sensible

### Validación de Roles Dinámicos

```javascript
export const authorization = (roles) => {
  return async (req, res, next) => {
    if (!req.user) return res.status(401).json({ message: "No autorizado" });
    if (!roles.includes(req.user.role)) {
      return res.status(403).json({ message: "Sin permisos" });
    }
    next();
  };
};
```

---

## 🧪 Testing

El proyecto incluye tests automatizados con Mocha, Chai y Supertest.

### Ejecutar Tests

```bash
npm test
```

### Archivos de Test

- `test/routes/product.router.test.js` - Tests de endpoints de productos
- `test/routes/cart.router.test.js` - Tests de carrito
- `test/routes/order.router.test.js` - Tests de órdenes
- `test/routes/buyer.router.test.js` - Tests de compradores
- `test/routes/business.router.test.js` - Tests de negocios

### Ejemplo de Test

```javascript
describe('Test router /api/products', function(){
  it('should return an array of products', async function(){
    const {_body, statusCode} = await requester.get('/api/products')
    expect(statusCode).to.be.equal(200)
    expect(_body).to.be.an('array')
  })
})
```

---

## 🤝 Contribución

Las contribuciones son bienvenidas. Para contribuir:

1. Fork el repositorio
2. Crea una rama para tu feature (`git checkout -b feature/AmazingFeature`)
3. Commit tus cambios (`git commit -m 'Add some AmazingFeature'`)
4. Push a la rama (`git push origin feature/AmazingFeature`)
5. Abre un Pull Request

### Estándares de Código

- Usa nombres descriptivos para variables y funciones
- Mantén funciones pequeñas y enfocadas
- Añade logs significativos con Winston
- Incluye validación Joi para nuevos endpoints
- Escribe tests para nuevas funcionalidades

---

## 📄 Licencia

Este proyecto está bajo la licencia **ISC**. Consulta el archivo `LICENSE` para más detalles.

---

## 👨‍💻 Autores y Créditos

**Desarrollado por:** Equipo de Desarrollo

**Contacto:** dev@example.com

**Agradecimientos:**
- Express.js
- MongoDB
- Passport.js
- Swagger/OpenAPI
- Winston Logger

---

## 📞 Soporte

Para reportar bugs o solicitar features, abre un issue en el repositorio.
