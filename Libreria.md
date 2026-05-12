## PROMT: actúa como un administrador de base de datos: proyecto librería, que entidades se necesitan para su gestión.
## Entidades para un Sistema de Gestión de Librería
 <img width="1406" height="956" alt="image" src="https://github.com/user-attachments/assets/e7f94e08-0fc8-40ad-a788-a32a3120fd60" />

Como DBA, definiría el siguiente modelo de datos:

---

### 📦 Entidades Principales

**1. LIBRO**
Núcleo del sistema. Atributos clave: ISBN, título, año de publicación, precio, stock, descripción, idioma, número de páginas.

**2. AUTOR**
Puede tener relación muchos a muchos con Libro (un libro puede tener varios autores y un autor varios libros). Atributos: nombre, nacionalidad, biografía.

**3. EDITORIAL**
Relación uno a muchos con Libro. Atributos: nombre, país, sitio web, contacto.

**4. CATEGORÍA / GÉNERO**
Clasificación temática del libro (ficción, ciencia, historia, etc.). Relación muchos a muchos con Libro.

**5. CLIENTE**
Quien realiza compras. Atributos: nombre, email, teléfono, dirección, fecha de registro.

**6. VENTA / PEDIDO**
Encabezado de transacción. Atributos: fecha, total, estado, método de pago, cliente asociado.

**7. DETALLE\_VENTA**
Tabla intermedia que desglosa cada venta. Atributos: cantidad, precio unitario, subtotal.

**8. PROVEEDOR**
Entidad que surte el inventario. Atributos: nombre, RFC/NIF, contacto, condiciones de pago.

**9. ORDEN\_COMPRA**
Pedido realizado al proveedor para reabastecer stock. Atributos: fecha, estado, total.

**10. EMPLEADO**
Personal de la librería (cajeros, encargados). Atributos: nombre, puesto, sucursal, usuario del sistema.

---

### 🔗 Entidades de Relación (tablas intermedias)

| Entidad | Relaciona |
|---|---|
| LIBRO\_AUTOR | Libro ↔ Autor |
| LIBRO\_CATEGORÍA | Libro ↔ Categoría |
| DETALLE\_PEDIDO\_COMPRA | OrdenCompra ↔ Libro |

---

### 🏪 Entidades Opcionales (según alcance)

- **SUCURSAL** — si hay múltiples puntos de venta
- **INVENTARIO** — control por sucursal si aplica
- **RESEÑA / VALORACIÓN** — opiniones de clientes
- **DESCUENTO / PROMOCIÓN** — campañas comerciales
- **LISTA\_DESEOS** — favoritos del cliente
- **DEVOLUCIÓN** — gestión de retornos

---

### 🗺️ Relaciones clave resumidas

```
PROVEEDOR ──< ORDEN_COMPRA >── LIBRO
LIBRO >──── EDITORIAL
LIBRO >──< AUTOR
LIBRO >──< CATEGORÍA
CLIENTE ──< VENTA >── DETALLE_VENTA ──< LIBRO
EMPLEADO ──< VENTA
```

---

¿Quieres que genere el **DDL SQL** completo, el **diagrama ER**, o profundizamos en alguna entidad específica?
## PROMT: las entidades con sus atributos y tipo en forma de tabla para cada una de las entidades
<img width="661" height="750" alt="image" src="https://github.com/user-attachments/assets/6fc240d9-2181-4c77-b37d-90e99c2a8b21" />
<img width="660" height="706" alt="image" src="https://github.com/user-attachments/assets/2d09cd6f-7aae-470c-8a3f-23dd3c7b3720" />
<img width="677" height="609" alt="image" src="https://github.com/user-attachments/assets/fa755065-d56f-4fca-9ad1-d75b94970412" />
<img width="671" height="631" alt="image" src="https://github.com/user-attachments/assets/b048f32a-ce68-4bf4-8e59-cf704aaec5a8" />

PROMT: de acuerdo a tu respuesta anterior puedes generar un script en sql para descargar con el nombre de dblibreria.sql para las entidades con sus relaciones


### 📝 Archivo: `dblibreria.sql`

```sql
-- Creación de la Base de Datos
CREATE DATABASE IF NOT EXISTS dblibreria;
USE dblibreria;

-- 1. Entidad EDITORIAL
CREATE TABLE EDITORIAL (
    id_editorial INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    pais VARCHAR(50),
    sitio_web VARCHAR(100),
    contacto VARCHAR(100)
);

-- 2. Entidad CATEGORIA
CREATE TABLE CATEGORIA (
    id_categoria INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(50) NOT NULL,
    descripcion TEXT
);

-- 3. Entidad AUTOR
CREATE TABLE AUTOR (
    id_autor INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    nacionalidad VARCHAR(50),
    biografia TEXT
);

-- 4. Entidad LIBRO
CREATE TABLE LIBRO (
    isbn VARCHAR(20) PRIMARY KEY,
    titulo VARCHAR(150) NOT NULL,
    anio_publicacion INT,
    precio DECIMAL(10, 2) NOT NULL,
    stock INT DEFAULT 0,
    descripcion TEXT,
    idioma VARCHAR(30),
    num_paginas INT,
    id_editorial INT,
    FOREIGN KEY (id_editorial) REFERENCES EDITORIAL(id_editorial)
);

-- 5. Entidad CLIENTE
CREATE TABLE CLIENTE (
    id_cliente INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    telefono VARCHAR(20),
    direccion TEXT,
    fecha_registro DATE DEFAULT (CURRENT_DATE)
);

-- 6. Entidad EMPLEADO
CREATE TABLE EMPLEADO (
    id_empleado INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    puesto VARCHAR(50),
    sucursal VARCHAR(50),
    usuario_sistema VARCHAR(50) UNIQUE
);

-- 7. Entidad VENTA (Encabezado)
CREATE TABLE VENTA (
    id_venta INT AUTO_INCREMENT PRIMARY KEY,
    fecha DATETIME DEFAULT CURRENT_TIMESTAMP,
    total DECIMAL(10, 2) NOT NULL,
    estado VARCHAR(20),
    metodo_pago VARCHAR(30),
    id_cliente INT,
    id_empleado INT,
    FOREIGN KEY (id_cliente) REFERENCES CLIENTE(id_cliente),
    FOREIGN KEY (id_empleado) REFERENCES EMPLEADO(id_empleado)
);

-- 8. Entidad DETALLE_VENTA
CREATE TABLE DETALLE_VENTA (
    id_detalle INT AUTO_INCREMENT PRIMARY KEY,
    id_venta INT,
    isbn VARCHAR(20),
    cantidad INT NOT NULL,
    precio_unitario DECIMAL(10, 2) NOT NULL,
    subtotal DECIMAL(10, 2) NOT NULL,
    FOREIGN KEY (id_venta) REFERENCES VENTA(id_venta),
    FOREIGN KEY (isbn) REFERENCES LIBRO(isbn)
);

-- 9. Entidad PROVEEDOR
CREATE TABLE PROVEEDOR (
    id_proveedor INT AUTO_INCREMENT PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    rfc_nif VARCHAR(20) UNIQUE,
    contacto VARCHAR(100),
    condiciones_pago VARCHAR(100)
);

-- 10. Entidad ORDEN_COMPRA
CREATE TABLE ORDEN_COMPRA (
    id_orden INT AUTO_INCREMENT PRIMARY KEY,
    id_proveedor INT,
    fecha DATE DEFAULT (CURRENT_DATE),
    estado VARCHAR(20),
    total DECIMAL(10, 2),
    FOREIGN KEY (id_proveedor) REFERENCES PROVEEDOR(id_proveedor)
);

-- 11. TABLAS INTERMEDIAS (Relaciones Muchos a Muchos)

-- Relación LIBRO - AUTOR
CREATE TABLE LIBRO_AUTOR (
    isbn VARCHAR(20),
    id_autor INT,
    PRIMARY KEY (isbn, id_autor),
    FOREIGN KEY (isbn) REFERENCES LIBRO(isbn),
    FOREIGN KEY (id_autor) REFERENCES AUTOR(id_autor)
);

-- Relación LIBRO - CATEGORIA
CREATE TABLE LIBRO_CATEGORIA (
    isbn VARCHAR(20),
    id_categoria INT,
    PRIMARY KEY (isbn, id_categoria),
    FOREIGN KEY (isbn) REFERENCES LIBRO(isbn),
    FOREIGN KEY (id_categoria) REFERENCES CATEGORIA(id_categoria)
);

-- Relación ORDEN_COMPRA - LIBRO (Detalle de compra)
CREATE TABLE DETALLE_PEDIDO_COMPRA (
    id_orden INT,
    isbn VARCHAR(20),
    cantidad INT NOT NULL,
    precio_compra DECIMAL(10, 2),
    PRIMARY KEY (id_orden, isbn),
    FOREIGN KEY (id_orden) REFERENCES ORDEN_COMPRA(id_orden),
    FOREIGN KEY (isbn) REFERENCES LIBRO(isbn)
);

```

---

### 💡 Notas del DBA:

1. **Integridad:** He utilizado `DECIMAL(10,2)` para los precios para evitar errores de redondeo que suelen dar los tipos `FLOAT`.
2. **Relaciones:** Las tablas intermedias (`LIBRO_AUTOR`, `LIBRO_CATEGORIA`) permiten que un libro pertenezca a varios géneros o sea escrito por varios autores, tal como se solicitó.
3. **Automatización:** Se incluyeron valores por defecto como `CURRENT_TIMESTAMP` para las ventas y `AUTO_INCREMENT` para los IDs.

¿Deseas que añada algunos datos de prueba (INSERTs) para que puedas testear las consultas?
