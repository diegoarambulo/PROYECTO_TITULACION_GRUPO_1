# Documentación de API: Búsqueda Global de Documentos

Esta documentación detalla el uso del endpoint `/documents/searchAll` para la recuperación de entidades de archivos basadas en criterios de búsqueda específicos.

## 1. Información General

| Especificación | Detalle |
| :--- | :--- |
| **Endpoint** | `/documents/searchAll` |
| **Método HTTP** | `POST` |
| **Autenticación** | No requerida (Acceso Público) |
| **Formato de Datos** | `application/json` |

---

## 2. Parámetros del Request (Body)

Todos los campos listados a continuación son **obligatorios**.

| Campo | Tipo | Descripción | Ejemplo |
| :--- | :--- | :--- | :--- |
| `fromDate` | `String (ISO 8601)` | Fecha de inicio de rango de búsqueda. | `"2026-01-01"` |
| `toDate` | `String (ISO 8601)` | Fecha fin de rango de búsqueda. | `"2026-04-30"` |
| `name` | `String` | Nombre descriptivo de la búsqueda o entidad. | `"Auditoria_Q1"` |
| `creatorName` | `String` | Nombre del autor/creador del archivo. | `"Admin Usuario"` |
| `fileName` | `String` | Nombre específico del archivo. | `"reporte.pdf"` |
| `keyWords` | `Array[String]` | Lista de palabras clave para el filtro. | `["anexo", "legal"]` |

---

## 3. Estructura de la Entidad: `document`

* **fileName** (`String`): Nombre del archivo almacenado.
* **creatorName** (`String`): Nombre del autor del documento.
* **state** (`String`): Estado del archivo. Valores permitidos: **`A`** (Activo) o **`I`** (Inactivo).
* **creationDate** (`String/DateTime`): Fecha exacta de creación.
* **publicUrl** (`String`): Enlace directo para visualización o descarga.
* **keyWords** (`Array[String]`): Etiquetas de clasificación.

---

## 4. Respuestas del Servidor

### 4.1 Búsqueda Exitosa (Múltiples Resultados)
Se devuelve cuando existen uno o más documentos que coinciden con los filtros.

* **Código HTTP:** `200 OK`
* **Cuerpo de respuesta:**

```json
{
  "code": 0,
  "message": "OK",
  "documents": [
    {
      "fileName": "cedula_identidad.pdf",
      "creatorName": "Soporte Técnico",
      "state": "A",
      "creationDate": "2026-02-10T14:20:00Z",
      "publicUrl": "https://storage.cdn.com/files/id_9823.pdf",
      "keyWords": ["cedula", "identidad"]
    },
    {
      "fileName": "pasaporte_escaneado.png",
      "creatorName": "Soporte Técnico",
      "state": "I",
      "creationDate": "2026-02-12T09:00:00Z",
      "publicUrl": "https://storage.cdn.com/files/pass_1122.png",
      "keyWords": ["pasaporte", "viaje"]
    },
    {
      "fileName": "anexo_contrato.docx",
      "creatorName": "Legal Dept",
      "state": "A",
      "creationDate": "2026-03-01T11:45:30Z",
      "publicUrl": "https://storage.cdn.com/files/anx_554.docx",
      "keyWords": ["anexo", "contrato", "legal"]
    }
  ]
}
```

### 4.2 Búsqueda sin Coincidencias
Consulta válida, pero la base de datos no arrojó resultados para esos parámetros.

* **Código HTTP:** `200 OK`
* **Cuerpo de respuesta:**

```json
{
  "code": 2,
  "message": "no hubieron documentos que coincidan con la busqueda",
  "documents": []
}
```

### 4.3 Error del Sistema
Fallo interno durante el procesamiento de la solicitud.

* **Código HTTP:** `500 Internal Server Error`
* **Cuerpo de respuesta:**

```json
{
  "code": -1,
  "message": "Error general de sistema"
}
```

---

## 5. Ejemplo de Consumo (cURL)

```bash
curl -X POST https://api.tuservicio.com/documents/searchAll \
-H "Content-Type: application/json" \
-d '{
    "fromDate": "2026-01-01",
    "toDate": "2026-12-31",
    "name": "Busqueda_Filtro_A",
    "creatorName": "Soporte Técnico",
    "fileName": "cedula",
    "keyWords": ["identidad"]
}'
```