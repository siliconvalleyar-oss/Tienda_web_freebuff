# 🛒 Tienda Electrónica — Servidor Web C++

[![C++17](https://img.shields.io/badge/C%2B%2B-17-blue.svg)](https://isocpp.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Servidor web en **C++17** para una tienda electrónica con carrito de compras,
autenticación de usuarios y panel de productos. Usa la librería
[cpp-httplib](https://github.com/yhirose/cpp-httplib) como servidor HTTP
embebido y [nlohmann/json](https://github.com/nlohmann/json) para manejo
de datos.

---

## ✨ Funcionalidades

- **Catálogo de productos** — Listado y detalle individual con imágenes, precios y stock.
- **Autenticación de usuarios** — Inicio de sesión con tokens de sesión.
- **Carrito de compras** — Agregar/quitar productos, cálculo automático de subtotales y total.
- **Checkout** — Formulario de compra, descuento de stock y generación de número de pedido.
- **Frontend estático** — Sitio web completo servido desde `public/` con HTML, CSS y JS.
- **API REST** — Endpoints JSON para integración con cualquier cliente.

---

## 🧰 Stack

| Tecnología         | Propósito                        |
|--------------------|----------------------------------|
| C++17              | Lenguaje principal               |
| cpp-httplib        | Servidor HTTP header-only        |
| nlohmann/json      | Parseo y generación de JSON      |
| HTML5 / CSS3 / JS  | Frontend en `public/`            |
| Makefile           | Sistema de compilación           |

---

## 📁 Estructura del proyecto

```
tienda_web_frebuuf/
├── src/                  # Código fuente C++
│   ├── main.cpp          # Punto de entrada y configuración del servidor
│   ├── database.cpp      # Carga, consulta y persistencia de productos
│   ├── auth.cpp          # Autenticación, sesiones y carrito en memoria
│   └── routes.cpp        # Handlers de todos los endpoints REST
├── include/              # Headers C++
│   ├── httplib.h         # cpp-httplib (librería externa, header-only)
│   ├── database.hpp
│   ├── auth.hpp
│   └── routes.hpp
├── public/               # Frontend estático
│   ├── index.html        # Página principal
│   ├── css/styles.css    # Estilos del sitio
│   └── js/main.js        # Lógica del frontend
├── templates/            # Plantillas HTML (login, productos, carrito, checkout)
├── data/                 # Datos en JSON
│   ├── productos.json    # Catálogo de productos (8 productos iniciales)
│   └── usuarios.json     # Usuarios de prueba
├── obj/                  # Archivos objeto (generados por compilación)
├── bin/                  # Binario compilado (server)
├── Makefile              # Sistema de compilación
├── opencode.json         # Configuración de opencode
└── .gitignore
```

---

## 📋 Prerrequisitos

- **g++** con soporte C++17
- **make**
- **nlohmann-json3-dev** (header-only)

```bash
# En Debian/Ubuntu
sudo apt install g++ make nlohmann-json3-dev
```

---

## 🔧 Instalación y compilación

```bash
# Clonar el repositorio
git clone https://github.com/siliconvalleyar-oss/Tienda_web_freebuff.git
cd Tienda_web_freebuff

# Compilar
make
```

---

## 🚀 Uso

```bash
# Compilar y ejecutar en puerto por defecto (8080)
make run

# Ejecutar en un puerto específico
make run ARGS="--port 9090"

# Compilar con flags de depuración
make debug

# Limpiar archivos compilados
make clean
```

Una vez iniciado, abrir en el navegador:

```
http://localhost:8080
```

### Usuarios de prueba

| Usuario  | Contraseña | Rol      |
|----------|-----------|----------|
| `admin`  | `admin`   | Admin    |
| `cliente`| `cliente` | Cliente  |

---

## 📡 API REST

### Productos

| Método | Ruta                    | Descripción                |
|--------|-------------------------|----------------------------|
| GET    | `/api/productos`        | Listar todos los productos |
| GET    | `/api/productos/{id}`   | Obtener detalle de un producto |

**Ejemplo — GET /api/productos**
```json
[
  {
    "id": 1,
    "nombre": "Auriculares Bluetooth Pro",
    "precio": 89.99,
    "descripcion": "Auriculares inalámbricos con cancelación de ruido activa...",
    "categoria": "Electrónica",
    "stock": 25,
    "imagen": "https://images.unsplash.com/..."
  }
]
```

### Autenticación

| Método | Ruta                | Descripción                        |
|--------|---------------------|------------------------------------|
| POST   | `/api/auth/login`   | Iniciar sesión                     |
| GET    | `/api/auth/session` | Verificar sesión actual (Bearer token o cookie) |

**Ejemplo — POST /api/auth/login**
```json
// Request
{ "usuario": "cliente", "password": "cliente" }

// Response
{
  "token": "a1b2c3d4...",
  "usuario": "cliente",
  "mensaje": "Inicio de sesión exitoso"
}
```

**Ejemplo — GET /api/auth/session**
```
Headers: Authorization: Bearer <token>
```
```json
{
  "autenticado": true,
  "usuario": "cliente",
  "nombre": "Cliente de Prueba",
  "direccion": "Calle Secundaria 456, Ciudad"
}
```

### Carrito

| Método | Ruta                    | Descripción                  |
|--------|-------------------------|------------------------------|
| POST   | `/api/carrito/agregar`  | Agregar producto al carrito  |
| GET    | `/api/carrito`          | Ver contenido del carrito    |

**Ejemplo — POST /api/carrito/agregar**
```json
// Request
{ "producto_id": 1, "cantidad": 2 }

// Response
{
  "mensaje": "Producto agregado al carrito",
  "producto": "Auriculares Bluetooth Pro",
  "cantidad": 2
}
```

**Ejemplo — GET /api/carrito**
```json
{
  "items": [
    {
      "id": 1,
      "nombre": "Auriculares Bluetooth Pro",
      "precio": 89.99,
      "cantidad": 2,
      "subtotal": 179.98,
      "imagen": "...",
      "stock": 25
    }
  ],
  "total": 179.98,
  "cantidad_items": 1
}
```

### Checkout

| Método | Ruta             | Descripción                    |
|--------|------------------|--------------------------------|
| POST   | `/api/checkout`  | Procesar la compra del carrito |

**Ejemplo — POST /api/checkout**
```json
// Request
{
  "nombre": "Juan Pérez",
  "direccion": "Av. Siempre Viva 742",
  "tarjeta": "1234567890123456"
}

// Response
{
  "exito": true,
  "mensaje": "¡Compra realizada con éxito!",
  "pedido_id": "PED-45723",
  "total": 179.98,
  "nombre": "Juan Pérez",
  "direccion": "Av. Siempre Viva 742",
  "tarjeta": "**** **** **** 3456"
}
```

---

## 🛠️ Desarrollo

### Compilación con depuración

```bash
make debug
```

Esto compila con `-g -O0 -DDEBUG` para una experiencia de depuración completa.
El binario resultante se puede ejecutar con `gdb`:

```bash
gdb ./bin/server
```

### Limpieza

```bash
make clean   # Elimina obj/*.o y bin/server
```

---

## 📄 Licencia

Distribuido bajo la licencia MIT. Consultar el archivo `LICENSE` para más información.

---

<p align="center">
  <sub>Hecho con ❤️ por FreebUfF</sub>
</p>
