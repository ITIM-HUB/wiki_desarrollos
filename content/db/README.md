[Inicio](/README.md)

# Lineamientos diseño base de datos

# 📘 Reglas de Base de Datos

> Documento de lineamientos para la estructuración, nombramiento y documentación de elementos dentro de una base de datos del proyecto.

---

## 1️⃣ Usuarios

- Según los requerimientos de cada proyecto, deben definirse los usuarios y los permisos que estos tendrán sobre los elementos de la base de datos.
- Esta definición debe realizarse desde la **conceptualización** del modelo.

---

## 2️⃣ Llaves Primarias

- **Nombre del campo:** `id`
- **Tipo de dato:** `UUID`
- **Longitud estándar:** 36 caracteres
  - Formato: `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`

---

## 3️⃣ Campos

### 🔤 Nomenclatura

- En **minúscula**
- En formato **snake_case** (`palabra1_palabra2`)
- Intentar máximo **3 palabras**
  - Ejemplos:
    - `fecha`
    - `fecha_validacion`
    - `ptomuestreo_aguasuperficial`

### 🔗 Relaciones

- `nombre1_nombre2_fk`, `nombre_fk`

### ⚙️ Enums y Dominios

- `nombre1_nombre2_enm`, `nombre_enm`
- `nombre1_nombre2_dom`, `nombre_dom`

### 🌎 Idioma

- Todo el modelo debe estar en **español**.

---

### 🗓️ Campos Tipo Fecha

- **No se permite** usar `varchar` para almacenar fechas, horas o rangos.
- Usar los tipos definidos en PostgreSQL:
  - [PostgreSQL datetime types](https://www.postgresql.org/docs/current/datatype-datetime.html)

---

### 🔠 Abreviaturas

Debe existir **un documento centralizado** con las abreviaturas permitidas.

**Ejemplos:**

| Abreviatura | Significado        |
| ----------- | ------------------ |
| `fch`       | fecha              |
| `dom`       | dominio            |
| `enm`       | enum               |
| `mt`        | metros             |
| `mt2`       | metros cuadrados   |
| `mt3`       | metros cúbicos     |
| `ha`        | hectárea           |
| `ft`        | pies               |
| `km`        | kilómetros         |
| `km2`       | kilómetro cuadrado |
| `lt`        | litros             |
| `lps`       | litros por segundo |
| `mm`        | milímetros         |
| `nro`       | número             |

**Reglas:**

- Longitud máxima: 2–3 caracteres
- Unidades y cantidades **deben ir abreviadas**

---

### 🧱 Tipo y Longitud

- Definir tipo de dato y longitud **al momento de crear** el campo.

---

### 🧩 Validaciones (Constraints)

Debe incluir las siguientes cuando apliquen:

- `FOREIGN KEY`
- `PRIMARY KEY`
- `NOT NULL`
- `UNIQUE`
- `CHECK` (mínimos y máximos)
- `EXCLUSION`

---

### 📝 Documentación

- Cada campo debe tener una **descripción clara** que indique su propósito.

---

### 📍 Geometrías

- El campo debe llamarse `geom`
- Tipo: `GEOMETRY`
- Definir tipo de geometría y EPSG

**Campos derivados por geometría**

**_Puntos_**

- `coord_x` (EPSG: 9377)
- `coord_y` (EPSG: 9377)

**_Líneas_**

- `coord_x_ini` (EPSG: 9377)
- `coord_y_ini` (EPSG: 9377)
- `coord_x_fin` (EPSG: 9377)
- `coord_y_fin` (EPSG: 9377)
- `longitud_mt`

**_Polígonos_**

- `area_ha`
- `centroide_x`, `centroide_y` (EPSG: 9377)

---

## 4️⃣ Tablas

### 📋 Aspectos Generales

- Toda tabla debe estar **documentada**
- Nombre en **minúscula**
- En formato **snake_case**
- Máximo **3 palabras**
- Nombre en **singular**
- En **español**
- Evitar abreviaciones salvo casos justificados (indicadores, coeficientes, etc.)
- Evitar **palabras reservadas**
- Cada tabla debe estar dentro de su **esquema temático**
  - No usar `public`

---

### 📚 Tipos de Tablas

#### 🔹 Normales

- Aplican las reglas generales anteriores.

#### 🔹 Maestras

- No deben tener valores repetidos (usar constraints)
- Siempre incluir el valor **“Sin información”**

#### 🔹 Dominios

- Prefijo: `dom_`  
  (Ejemplo: `dom_tipo_usuario`)
- Solo 2 columnas: `id`, `nombre`
- Debe permitir crecer en registros
- Incluir **“Sin información”**

#### 🔹 Geográficas

- Agrupar en esquema temático (`geo`, `uer`, etc.)
- No deben mezclarse con tablas alfanuméricas
- Si existen varios tipos de geometría:
  - `zona_interes_pol`
  - `zona_interes_lin`
  - `zona_interes_pto`
  - `zona_interes_multipol`
  - `zona_interes_multilin`
  - `zona_interes_multipto`

---

## 5️⃣ Relaciones (Constraints)

### 🔗 Llaves Foráneas

- En **minúscula**
- Prefijo: `fk_`
- Ejemplos:
  - `fk_barrio`
  - `fk_distrito`
  - `fk_plancha`
- Deben apuntar al campo `id` de la tabla relacionada

---

## 6️⃣ Vistas

- Prefijo: `vista_`
  - Ejemplo: `vista_pozo`, `vista_departamento`
- En **singular**
- En **snake_case**
- En **español**
- Documentadas
- Máximo **3 palabras**
- Deben contener solo los campos **necesarios**
- Las consultas deben ser **simples** y **eficientes**

---

## 7️⃣ Esquemas

- En **minúscula**
- Relacionados con la temática del proyecto
- Solo **una palabra**
- Se permiten siglas comprensibles (`uer`, `geo`)
- Documentar cada esquema

---

## 8️⃣ Enums

- Nombre corto, en **minúscula**, en **español**
- Valores cortos, máximo **10 opciones**
- Siempre incluir **“Sin información”**
- Deben estar **documentados**

---

## 9️⃣ Dominios

- Nombre corto, en **minúscula**, con prefijo `dom_`
- Ejemplo: `dom_categoria`
- Solo 2 columnas (`id`, `nombre`)
- En **español**
- Deben tener constraints de unicidad
- En formato **snake_case**
- Deben estar **documentados**

---

## 🔟 Funciones

- En **minúscula**
- Prefijo: `fn_`  
  (Ejemplo: `fn_calcular_rangos`)
- Máximo **3 palabras**
- En **snake_case**
- En **español**
- Deben estar **documentadas**
- Comentar cada bloque de código
