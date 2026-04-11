---

# documentacion de api: busqueda global de documentos

esta documentacion detalla el uso del endpoint `/documents/searchall` para la recuperacion de entidades de archivos basadas en criterios de busqueda especificos.

---

## 📌 informacion general

* **endpoint:** `/documents/searchall`
* **metodo http:** `get`
* **autenticacion:** no requerida
* **formato de request:** json
* **formato de response:** json

---

## 📌 mision de la empresa

---

## 📌 vision de la empresa

---

## 📥 estructura del request

el request consiste en un **array de objetos**, donde cada objeto define un criterio de busqueda.

### 🔹 estructura de cada objeto

```json
{
  "key": "string",
  "value": "string",
  "type": ["string"]
}
```

### 🔹 descripcion de campos

| campo | tipo          | descripcion                                              |
| ----- | ------------- | -------------------------------------------------------- |
| key   | string        | puede ser vacio o uno de los valores del enumerado index |
| value | string        | valor de busqueda                                        |
| type  | array[string] | define donde se aplicara la busqueda                     |

---

### 🔹 descripcion de campos

| campo | tipo          | descripcion                                              |
| ----- | ------------- | -------------------------------------------------------- |
| key   | string        | puede ser vacio o uno de los valores del enumerado index |
| value | string        | valor de busqueda                                        |
| type  | array[string] | define donde se aplicara la busqueda                     |

---

## 🧩 enumerados

### 🔹 valores permitidos para `key` (index)

```
["cedula", "nombre", "email", "direccion", "telefono", "contrato", 
"fecha desde", "fecha hasta", "anexo", "ruc", "identificacion", 
"apellido", "genero", "edad", "sexo", "ciudad", "pais", "ano", "jefe"]
```

### 🔹 valores permitidos para `type`

```
["filetype", "index", "filename", "descriptionfile"]
```

> puede contener uno o varios valores, o estar vacio.

---

## 🏢 infraestructura corporativa

### 📸 edificio corporativo principal

---

## 📘 ejemplos de

### 🔹 ejemplo 1

```json
[
  {
    "key": "nombre",
    "value": "diego arambulo",
    "type": ["index"]
  },
  {
    "key": "",
    "value": "cedula",
    "type": ["filetype", "index"]
  }
]
```

📌 **descripcion:**
busca archivos donde:

* tipo de archivo
  * indice

---

### 🔹 ejemplo 2

```json
[
  {
    "key": "cedula",
    "value": "0929800399",
    "type": ["filetype", "index", "filename", "descriptionfile"]
  },
  {
    "key": "fecha desde",
    "value": "2025-01-20",
    "type": [""]
  },
  {
    "key": "fecha hasta",
    "value": "2025-12-20",
    "type": [""]
  }
]
```

📌 **descripcion:**
busca archivos:

* indices
  * nombre del archivo
  * descripcion
  * tipo de archivo

---

## 📤 estructura del response

### ✅ respuesta exitosa (http 200)

```json
{
  "code": 0,
  "message": "ok",
  "documents": [
    {
      "id": "94ec0447-1ccd-47d8-a343-a36da458254c",
      "name": "pdf en blanco - copia (15)",
      "description": "archivo sin descripcion 68515",
      "extension": ".pdf",
      "file": "https://sisadazurestorage.blob.core.windows.net/default/94ec0447-1ccd-47d8-a343-a36da458254c.pdf",
      "size": 36604,
      "filetypeid": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "documentid": "4ec60004-bfde-4ef8-94cf-d71008aa1e07",
      "sequential": 68515,
      "filetypename": "otros",
      "isblocked": false
    },
    {
      "id": "8d476018-02c8-458a-9d82-c1f0fbb667e1",
      "name": "pdf en blanco - copia (16)",
      "description": "archivo sin descripcion 68516",
      "extension": ".pdf",
      "file": "https://sisadazurestorage.blob.core.windows.net/default/8d476018-02c8-458a-9d82-c1f0fbb667e1.pdf",
      "size": 36604,
      "filetypeid": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "documentid": "4ec60004-bfde-4ef8-94cf-d71008aa1e07",
      "sequential": 68516,
      "filetypename": "otros",
      "isblocked": false
    }
  ]
}
```

---

### 🔹 descripcion de campos del documento

| campo        | descripcion                         |
| ------------ | ----------------------------------- |
| id           | identificador unico del archivo     |
| name         | nombre del archivo                  |
| description  | descripcion del archivo             |
| extension    | extension del archivo               |
| file         | url publica del archivo             |
| size         | tamano en bytes                     |
| filetypeid   | identificador del tipo de archivo   |
| documentid   | identificador del documento         |
| sequential   | numero secuencial                   |
| filetypename | nombre del tipo de archivo          |
| isblocked    | indica si el archivo esta bloqueado |

---

### ⚠ sin resultados (http 404)

```json
{
  "code": 2,
  "message": "no hubieron documentos que coincidan con la busqueda",
  "documents": []
}
```

---

### ❌ error del sistema (http 500)

```json
{
  "": -1,
  "message": "error general de sistema"
}
```

---

## 🧪 ejemplo de consumo con curl

```bash
curl -x get "http://localhost:8080/documents/searchall" \
-h "content-type: application/json" \
-d '[
  {
    "key": "nombre",
    "value": "diego arambulo",
    "type": ["index"]
  },
  {
    "key": "",
    "value": "cedula",
    "type": ["filetype", "index"]
  }
]'
```

---

## 📎 notas finales

* el endpoint permite combinar multiples criterios de busqueda.
* los filtros se aplican de forma conjunta.
* se recomienda validar correctamente los valores del enumerado antes de enviar la solicitud.
* las fechas deben enviarse en formato `-mm-dd`.

---

## 🏢 historia de la empresa