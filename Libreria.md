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



