# 📘 Sistema de Gestión de Cursos Universitarios

Despliegue en **AWS EC2** + **API Flask (Render)** + **Base de Datos Supabase**

---

## 📁 Esquema del Proyecto

```
sistema-cursos-universidad/
├── README.md
├── esquema/
│   ├── create_tables.sql
│   ├── sample_data.sql
│   └── join_query.sql
├── backend/
│   ├── app.py
│   ├── requirements.txt
│   └── .env.example
├── frontend/
│   ├── index.html
│   ├── styles.css
│   └── script.js
└── images/
    ├── dashboard.png
    └── query_results.png
```

---

## 🎓 Descripción General

Sistema de gestión de cursos universitarios que permite administrar:

- ✅ Estudiantes
- ✅ Cursos
- ✅ Inscripciones
- ✅ Operaciones CRUD completas

**Integración entre:**
- Frontend en AWS EC2
- API Flask en Render
- Base de datos PostgreSQL en Supabase

---

## 🏗 Arquitectura General

```
Usuario → AWS EC2 (Frontend/Nginx)
              ↓
          API REST Flask (Render)
              ↓
        Supabase PostgreSQL (DBaaS)
```

---

## 🗄️ Base de Datos – Supabase (PostgreSQL)

### 📌 Diagrama ER

```
estudiantes (1) ───── (N) inscripciones (N) ───── (1) cursos
```

### 📋 Descripción de Tablas

#### 🧑 Tabla: `estudiantes`

| Campo | Tipo | Notas |
|-------|------|-------|
| `id` | SERIAL PK | Primary Key |
| `nombre` | VARCHAR(100) | NOT NULL |
| `email` | VARCHAR(100) | UNIQUE NOT NULL |
| `fecha_creacion` | TIMESTAMP | DEFAULT CURRENT_TIMESTAMP |

#### 📚 Tabla: `cursos`

| Campo | Tipo | Notas |
|-------|------|-------|
| `id` | SERIAL PK | Primary Key |
| `nombre` | VARCHAR(100) | NOT NULL |
| `descripcion` | TEXT | — |
| `creditos` | INT | NOT NULL |
| `fecha_creacion` | TIMESTAMP | DEFAULT CURRENT_TIMESTAMP |

#### 📝 Tabla: `inscripciones`

| Campo | Tipo | Notas |
|-------|------|-------|
| `id` | SERIAL PK | Primary Key |
| `estudiante_id` | INT | FK → estudiantes(id) |
| `curso_id` | INT | FK → cursos(id) |
| `fecha_inscripcion` | TIMESTAMP | DEFAULT CURRENT_TIMESTAMP |

**🔗 Relación:** Muchos a muchos (N:M) entre `estudiantes` y `cursos` mediante `inscripciones`.

---

## 🧩 Scripts SQL

- `create_tables.sql`: creación de tablas
- `sample_data.sql`: datos de prueba
- `join_query.sql`: consultas JOIN para reportes

---

## 🔑 Credenciales de Acceso (Pruebas)

### 🌐 URLs

| Servicio | URL |
|----------|-----|
| Panel Supabase | https://vykgdjbpsqdqjtfrivzh.supabase.co |
| API REST | https://vykgdjbpsqdqjtfrivzh.supabase.co/rest/v1/ |

### 🔐 Conexión PostgreSQL

```
postgresql://postgres:Password123!@db.vykgdjbpsqdqjtfrivzh.supabase.co:5432/postgres
```

### 🔑 Configuración

| Parámetro | Valor |
|-----------|-------|
| Host | `db.vykgdjbpsqdqjtfrivzh.supabase.co` |
| Port | `5432` |
| User | `postgres` |
| Password | `Password123!` |
| API Key | `sb_publishable_0_lgoaqQNFvkBumC7AQzrw_e0cPxkti` |

---

## 🔥 API REST – Flask (Render)

**📌 Proyecto para Computación en la Nube – Actividad 8**

### 🚀 Tecnologías

- Flask + Flask-CORS
- PostgreSQL (Supabase)
- Render (Backend)

### 🌐 API en producción

🔗 **https://api-estudiantes-cursos.onrender.com**

### 📚 Endpoints

#### 👨‍🎓 Estudiantes

| Método | Endpoint | Descripción |
|--------|----------|-------------|
| `GET` | `/estudiantes` | Obtener todos |
| `GET` | `/estudiantes/{id}` | Obtener por ID |
| `POST` | `/estudiantes` | Crear |
| `PUT` | `/estudiantes/{id}` | Actualizar |
| `DELETE` | `/estudiantes/{id}` | Eliminar |

### 🧪 Pruebas CRUD (cURL)

#### Crear estudiante

```bash
curl -X POST "https://api-estudiantes-cursos.onrender.com/estudiantes" \
-H "Content-Type: application/json" \
-d '{"nombre":"Estudiante Prueba","email":"prueba@instructor.com"}'
```

#### Obtener todos

```bash
curl "https://api-estudiantes-cursos.onrender.com/estudiantes"
```

#### Actualizar

```bash
curl -X PUT "https://api-estudiantes-cursos.onrender.com/estudiantes/1" \
-H "Content-Type: application/json" \
-d '{"nombre":"Actualizado","email":"a@a.com"}'
```

#### Eliminar

```bash
curl -X DELETE "https://api-estudiantes-cursos.onrender.com/estudiantes/1"
```

---

## 🌐 Frontend en AWS EC2

**URL del sistema (Frontend)**

🔗 **http://3.131.93.247**

### 🟧 AWS EC2 – Configuración del Servidor

#### Especificaciones

| Parámetro | Valor |
|-----------|-------|
| Instancia | t2.micro |
| SO | Ubuntu Server 22.04 |
| Servidor Web | Nginx |
| Región | us-east-1 |

### 🛠 Configuración Nginx

```nginx
server {
    listen 80;
    server_name 3.131.93.247;
    root /var/www/sistema-cursos-universidad;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~* \.(css|js)$ {
        expires 1y;
        add_header Cache-Control "public, immutable";
    }
}
```

### 🧩 Comandos de despliegue EC2

#### Instalar servidor

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install nginx -y
sudo systemctl enable nginx
sudo ufw allow 'Nginx Full'
```

#### Subir frontend

```bash
scp -r ./frontend/* ubuntu@EC2_IP:/var/www/sistema-cursos-universidad
sudo systemctl restart nginx
```

### 🔐 Seguridad AWS (Security Groups)

```yaml
- HTTP:
    port: 80
    source: 0.0.0.0/0

- SSH:
    port: 22
    source: <IP administrador>
```

---

## 🛠 Instrucciones para Desarrolladores (Modo Local)

### 1️⃣ Clonar el repositorio

```bash
git clone https://github.com/tuusuario/sistema-cursos-universidad.git
cd sistema-cursos-universidad
```

### 2️⃣ Frontend Local

Abrir `/frontend/index.html` o usar Live Server.

### 3️⃣ API Flask Local

```bash
cd backend
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

Crear `.env`:

```env
SUPABASE_URL=
SUPABASE_KEY=
```

Ejecutar:

```bash
python app.py
```

API local → **http://127.0.0.1:5000**

---

## 📊 Estado del Proyecto

- ✅ CRUD completo
- ✅ API funcionando en Render
- ✅ Frontend desplegado AWS
- ✅ Base de datos Supabase conectada
- ✅ Documentación completa
- ✅ Pruebas OK

---

## 🔗 Enlaces Importantes

| Recurso | URL |
|---------|-----|
| GitHub | https://github.com/ozt07/Cloud08 |
| API Producción | https://api-estudiantes-cursos.onrender.com |
| Frontend AWS | http://3.131.93.247 |
| Supabase | https://vykgdjbpsqdqjtfrivzh.supabase.co |
| Flask Docs | https://flask.palletsprojects.com/ |
| Supabase Docs | https://supabase.com/docs |

---

## 👨‍💻 Autor

**Proyecto de Computación en la Nube - Actividad 8**

---

## 📄 Licencia

Este proyecto es de código abierto para fines educativos.