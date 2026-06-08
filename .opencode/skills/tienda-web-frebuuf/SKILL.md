---
name: tienda-web-frebuuf
description: Use when working on the "tienda web frebuuf" project, a C++ web server for an electronics store. Handles building, running, debugging, and understanding the project structure.
---

# Tienda Web Frebuuf

Proyecto: Servidor web en C++ para tienda electrónica.

## Stack

- **Lenguaje**: C++17
- **Servidor HTTP**: cpp-httplib (header-only, en `include/httplib.h`)
- **JSON**: nlohmann-json3-dev (header-only, `/usr/include/nlohmann/`)
- **Frontend**: HTML/CSS/JS estático en `public/`

## Dependencias del sistema

```bash
sudo apt install nlohmann-json3-dev g++ make
```

## Comandos

| Comando       | Descripción                        |
|---------------|------------------------------------|
| `make`        | Compila el proyecto                |
| `make run`    | Compila y ejecuta el servidor      |
| `make run ARGS="--port 9090"` | Ejecutar en puerto específico |
| `make debug`  | Compila con flags de depuración    |
| `make clean`  | Elimina objetos y binario          |

## Estructura del proyecto

```
├── src/           # Código fuente (.cpp)
│   ├── main.cpp       # Punto de entrada
│   ├── database.cpp   # Carga/consulta de productos
│   ├── auth.cpp       # Autenticación de usuarios
│   └── routes.cpp     # Rutas de la API REST
├── include/       # Headers (.hpp)
│   ├── httplib.h      # cpp-httplib (librería externa)
│   ├── database.hpp
│   ├── auth.hpp
│   └── routes.hpp
├── public/        # Archivos estáticos (frontend)
│   ├── index.html
│   ├── css/
│   ├── js/
│   └── images/
├── templates/     # Plantillas HTML para el servidor
├── data/          # Datos JSON (productos, usuarios)
├── obj/           # Objetos compilados (gitignored)
├── bin/           # Binario server (gitignored)
├── Makefile
└── .gitignore
```

## Uso

- Servidor en `http://localhost:8080` por defecto
- Puerto configurable con `--port`
- Usuarios de prueba: `admin/admin` y `cliente/cliente`

## Endpoints API

- `GET /api/productos` — Lista todos los productos
- `POST /api/login` — Inicia sesión (body: `{"usuario": "...", "password": "..."}`)
- `GET /api/carrito` — Ver carrito (requiere sesión)
- `POST /api/carrito/agregar` — Agregar al carrito
- `POST /api/carrito/eliminar` — Quitar del carrito
- `POST /api/checkout` — Finalizar compra
