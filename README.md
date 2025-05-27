# E-commerce Backend — Parcial 3er Corte

Bienvenido al backend de la tienda de electrodomésticos. Este proyecto integra **AWS Lambda**, **Terraform** y **GitHub Actions** para ofrecer una plataforma robusta y escalable de gestión de inventario, usuarios y compras.

---

## 🚀 Descripción General

Como propietario de una tienda de electrodomésticos, necesitas administrar eficientemente tu inventario y centralizar la información de productos, usuarios y carritos de compra. Esta API backend está diseñada para ser flexible y segura, facilitando tanto el registro/login de usuarios como la gestión de productos y carritos.

---

## 🛠️ Tecnologías Utilizadas

- **AWS Lambda:** Funciones serverless para la lógica de negocio.
- **Amazon DynamoDB:** Bases de datos NoSQL para usuarios, productos y carritos.
- **API Gateway:** Manejo de rutas HTTP.
- **Terraform:** Infraestructura como código para desplegar recursos AWS.
- **GitHub Actions:** CI/CD automatizado para despliegues y pruebas.

---

## 🌎 Región AWS

`us-west-1`

---

## 📦 Estructura de Componentes

### Bases de Datos (DynamoDB)
- **User**
  - PartitionKey: `uuid`
  - OrderKey: `email`
- **Products**
  - PartitionKey: `uuid`
  - OrderKey: `name`
- **Cart**
  - PartitionKey: `uuid`
  - OrderKey: `UserId`

### API Gateway

- **User**
  - `POST /user` — Registro de usuario
  - `POST /user/login` — Login de usuario
- **Product**
  - `POST /product` — Crear producto
  - `GET /product` — Obtener listado de productos
- **Cart**
  - `POST /cart` — Crear carrito de compras (compra)

### Lambdas y Permisos

| Lambda                      | Permisos DynamoDB                       |
|-----------------------------|-----------------------------------------|
| User — Registro             | dynamodb:PutItem                        |
| User — Login                | dynamodb:Scan, dynamodb:GetItem         |
| Products — Crear producto   | dynamodb:PutItem                        |
| Products — Obtener listado  | dynamodb:Scan, dynamodb:GetItem         |
| Cart — Pagar y crear carrito| dynamodb:PutItem                        |

---

## 🧑‍💻 Módulo de Usuario

### Registro de usuario (`POST /user`)

#### Body
```json
{
  "name": "string",
  "email": "string",
  "password": "string",
  "phone": "string"
}
```

#### Respuesta (201 Created)
```json
{
  "message": "Usuario registrado exitosamente",
  "data": {
    "id": "uuid-generado",
    "name": "string",
    "email": "string",
    "phone": "string",
    "createdAt": "timestamp"
  }
}
```

---

### Login de usuario (`POST /user/login`)

#### Body
```json
{
  "email": "string",
  "password": "string"
}
```

#### Respuesta (200 OK)
```json
{
  "message": "Login exitoso",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```
El token es un JWT que debe incluirse en los headers para acceder a los endpoints protegidos.

---

## 📦 Módulo de Productos

### Crear producto (`POST /product`)
**Headers:**
```json
{
  "Authorization": "Bearer token"
}
```

**Body:**
```json
{
  "name": "string",
  "brand": "string",
  "categories": ["string", "string"],
  "price": number
}
```

**Respuesta:**
```json
{
  "message": "Producto registrado exitosamente",
  "data": {
    "id": "uuid-generado",
    "name": "string",
    "brand": "string",
    "price": number,
    "userId": "string",
    "createdAt": "timestamp"
  }
}
```

---

### Obtener productos (`GET /product`)

**Respuesta:**
```json
{
  "data": [
    {
      "uuid": "string",
      "name": "string",
      "brand": "string",
      "categories": ["string", "string"],
      "price": number
    }
  ]
}
```

---

## 🛒 Carrito de Compras

### Crear/Pagar carrito (`POST /cart`)
**Headers:**
```json
{
  "Authorization": "Bearer token"
}
```

**Body:**
```json
{
  "products": [
    { "uuid": "string" },
    { "uuid": "string" }
  ],
  "total": number
}
```

**Respuesta:**
```json
{
  "products": [
    { "uuid": "string" },
    { "uuid": "string" }
  ],
  "userId": "string",
  "total": number
}
```

---

## 📝 Entregables

- Repositorio con el código backend
- Código de infraestructura (Terraform)
- Código de las funciones Lambda
- Video demostrativo de las funcionalidades

---

## 📚 Recursos útiles

- [Terraform Docs](https://developer.hashicorp.com/terraform)
- [AWS Provider Terraform Registry](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)
- [Video guía AWS Lambda + Terraform](https://www.youtube.com/watch?v=gpXmaDwfQ50)

---

¡Éxitos implementando y desplegando tu backend serverless!