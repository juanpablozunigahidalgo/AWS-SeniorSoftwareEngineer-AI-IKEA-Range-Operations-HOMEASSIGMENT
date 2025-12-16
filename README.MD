# IKEA Range Operations - Senior Software Engineer Assessment 🇸🇪

API RESTful desarrollada para gestionar el inventario y operaciones de stock de productos, simulando el entorno de operaciones de IKEA.

## 🚀 Despliegue en Vivo (Live Demo)

La aplicación ha sido desplegada exitosamente utilizando **Render**. Puedes acceder a la API y a la documentación en los siguientes enlaces:

- **🌐 Base URL:** [https://aws-seniorsoftwareengineer-ai-ikea-range.onrender.com](https://aws-seniorsoftwareengineer-ai-ikea-range.onrender.com)
- **📄 Documentación Swagger:** [https://aws-seniorsoftwareengineer-ai-ikea-range.onrender.com/api-docs](https://aws-seniorsoftwareengineer-ai-ikea-range.onrender.com/api-docs)

> **Nota:** Al estar en una instancia gratuita de Render, el servidor puede tardar unos 50 segundos en "despertar" en la primera petición.

---

## 🛠 Tech Stack

- **Lenguaje:** TypeScript / Node.js
- **Framework:** Express / NestJS (ajustar según corresponda)
- **Base de Datos:** PostgreSQL / In-Memory (ajustar según tu config)
- **Documentación:** Swagger (OpenAPI 3.0)
- **Infraestructura/Despliegue:** Render

## 📍 Endpoints Principales

La API expone las siguientes rutas para la gestión de productos:

### Products
- `GET /products` - Obtener todo el inventario.
- `POST /products` - Crear un nuevo producto (requiere SKU, nombre y stock inicial).

### Stock Operations
- `POST /products/{id}/increment` - Aumentar el stock de un producto.
- `POST /products/{id}/decrement` - Disminuir el stock (incluye validación de stock insuficiente).

## 💻 Ejecutar Localmente

Si deseas correr el proyecto en tu máquina local:

1. Clonar el repositorio.
2. Instalar dependencias:
   ```bash
   npm install