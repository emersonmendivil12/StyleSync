
# StyleSync - Agents & Skills

> Aplicación web/móvil para reserva de citas en barbería.
> **Stack:** Reflex + SQLite + Python venv | **Colores:** cálidos | **Git:** sí

---

## 📦 Skill: `init-project`
**Propósito:** Inicializar el proyecto Reflex con entorno virtual, estructura de directorios y dependencias.

### Pasos:
1. Crear y activar el entorno virtual:
   ```bash
   python -m venv .venv
   source .venv/Scripts/activate   # Windows (Git Bash)
   ```
2. Instalar Reflex y dependencias:
   ```bash
   pip install reflex sqlalchemy aiosqlite email-validator python-dotenv
   pip freeze > requirements.txt
   ```
3. Inicializar proyecto Reflex:
   ```bash
   reflex init
   ```
4. Crear estructura base:
   ```
   stylesync/
   ├── assets/         # imágenes, iconos
   ├── stylesync/      # código de la app
   │   ├── models.py
   │   ├── views/
   │   ├── auth/
   │   ├── admin/
   │   ├── theme.py
   │   └── state.py
   ├── docs/
   └── requirements.txt
   ```
5. Configurar `.gitignore` (`.venv/`, `.env/`, `__pycache__/`, `*.db`)

---

## 🗄️ Skill: `database-schema`
**Propósito:** Diseñar e implementar el esquema SQLite con SQLAlchemy + Reflex.

### Modelos:

**`User`**
- id, name, email (unique), password_hash, phone, role (client/barber/admin), created_at

**`Barber`**
- id, user_id (FK), specialties, bio, avatar, is_active

**`Service`**
- id, name, description, price, duration_minutes, is_active

**`BarberService`**
- id, barber_id (FK), service_id (FK) — relación muchos a muchos

**`Appointment`**
- id, client_id (FK), barber_id (FK), service_id (FK), date, time, status (pending/confirmed/cancelled/completed), notes, created_at

**`Schedule`**
- id, barber_id (FK), day_of_week, start_time, end_time, is_available

**`Review`**
- id, client_id (FK), appointment_id (FK), rating, comment, created_at

### Archivo destino: `stylesync/models.py`
- Usar `sqlmodel` (incluido con Reflex) o `sqlalchemy` con `reflex.Model`

---

## 🔐 Skill: `auth-system`
**Propósito:** Registro, inicio de sesión y gestión de roles.

### Componentes:
- `auth/state.py` → `AuthState` con métodos: `register`, `login`, `logout`, `get_current_user`
- `auth/pages.py` → `login_page()`, `register_page()`, `forgot_password_page()`
- Proteger rutas según rol (cliente / barbero / admin)
- Almacenar sesión con `reflex.local_storage` o cookie
- Validar email y hash de contraseña con `bcrypt` o `passlib`

---

## 💈 Skill: `services-crud`
**Propósito:** CRUD de servicios de barbería.

### Componentes:
- `services/state.py` → `ServiceState`
- Vistas: listar, crear, editar, eliminar servicios
- Campos: nombre, descripción, precio, duración, activo/inactivo
- Solo accesible para admin/barberos

---

## ✂️ Skill: `barbers-management`
**Propósito:** Gestión de barberos, horarios y especialidades.

### Componentes:
- `barbers/state.py` → `BarberState`
- Vistas: perfil de barbero, asignar servicios, configurar horarios
- `ScheduleState` para gestionar disponibilidad semanal

---

## 📅 Skill: `booking-system`
**Propósito:** Flujo completo de reserva de citas.

### Flujo:
1. Cliente selecciona servicio → se filtran barberos que lo ofrecen
2. Selecciona barbero → se muestra calendario con disponibilidad
3. Elige fecha y hora → confirma reserva
4. Se crea `Appointment` con estado `pending`
5. Confirmación visible en panel del cliente y barbero

### Componentes:
- `booking/state.py` → `BookingState`
- `booking/calendar.py` → componente visual de calendario
- Vista de selección: servicio → barbero → fecha/hora → confirmar

---

## 📊 Skill: `admin-dashboard`
**Propósito:** Panel de administración con gestión completa.

### Vistas:
- Dashboard con estadísticas (citas hoy, ingresos, clientes nuevos)
- Gestión de usuarios (listar, banear, cambiar rol)
- Gestión de citas (ver todas, filtrar por estado)
- Reportes (citas por período, ingresos, barbero más solicitado)

### Componentes:
- `admin/state.py` → `AdminState`
- `admin/pages.py` → paneles protegidos por rol admin

---

## 🎨 Skill: `warm-theme`
**Propósito:** Aplicar paleta de colores cálidos a la interfaz Reflex.

### Paleta:
```python
# theme.py
import reflex as rx

WARM_PALETTE = {
    "primary": "#D4A373",    # dorado tierra
    "secondary": "#FAEDCD",  # crema
    "accent": "#CC8B3C",     # naranja quemado
    "background": "#FEFAE0", # fondo cálido
    "text": "#3E2723",       # marrón oscuro
    "card": "#FFF8E7",       # tarjetas
    "success": "#6B8E23",   # verde oliva
}

def warm_theme() -> rx.Component:
    return rx.theme(
        appearance="light",
        accent_color="amber",
        radius="medium",
        ...
    )
```

---

## 🧪 Skill: `testing`
**Propósito:** Ejecutar pruebas de la aplicación.

### Comandos:
```bash
# Activar entorno virtual
source .venv/Scripts/activate

# Ejecutar la app Reflex (desarrollo)
reflex run

# Ejecutar con recarga en caliente
reflex run --loglevel debug

# Construir versión estática (producción)
reflex export --frontend-only

# Pruebas unitarias (si se crean)
pytest tests/ -v
```

---

## 🐙 Skill: `git-management`
**Propósito:** Gestionar el proyecto con Git.

### Comandos base:
```bash
git init
git add .
git commit -m "chore: initial project setup with Reflex + SQLite"
git branch -M main
git remote add origin <url>
git push -u origin main
```

### Convención de commits:
| Tipo       | Propósito                          |
|------------|------------------------------------|
| `feat`     | Nueva funcionalidad               |
| `fix`      | Corrección de bug                 |
| `refactor` | Refactorización de código         |
| `style`    | Cambios de estilo/UI              |
| `docs`     | Documentación                     |
| `chore`    | Tareas de mantenimiento           |
| `db`       | Cambios en esquema de base de datos |

---

## 🚀 Skill: `run-app`
**Propósito:** Comandos para probar y ejecutar la aplicación.

### Desarrollo:
```bash
# 1. Activar entorno virtual
source .venv/Scripts/activate

# 2. Ejecutar Reflex en modo desarrollo
reflex run

# 3. Abrir en navegador
# http://localhost:3000 (frontend)
# http://localhost:8000 (backend API)
```

### Verificar base de datos:
```bash
# Ver tablas creadas
sqlite3 stylesync.db ".tables"
# Ver contenido de una tabla
sqlite3 stylesync.db "SELECT * FROM user;"
```

### Limpiar y reiniciar:
```bash
# Eliminar base de datos y reiniciar
rm -f stylesync.db
reflex run
```

---

## 📐 Skill: `project-structure`
**Propósito:** Crear la estructura completa del proyecto Reflex.

```
StyleSync/
├── .venv/                    # Entorno virtual (ignorado por git)
├── assets/                   # Archivos estáticos (imgs, iconos, CSS)
├── stylesync/                # Código principal de la app
│   ├── __init__.py
│   ├── models.py             # Modelos SQLite (SQLModel)
│   ├── theme.py              # Tema cálido
│   ├── state.py              # Estado global
│   ├── auth/
│   │   ├── __init__.py
│   │   ├── state.py          # AuthState
│   │   └── pages.py          # Login / Register
│   ├── services/
│   │   ├── __init__.py
│   │   ├── state.py          # ServiceState
│   │   └── pages.py
│   ├── barbers/
│   │   ├── __init__.py
│   │   ├── state.py          # BarberState
│   │   └── pages.py
│   ├── booking/
│   │   ├── __init__.py
│   │   ├── state.py          # BookingState
│   │   └── pages.py          # Calendario + formulario
│   ├── admin/
│   │   ├── __init__.py
│   │   ├── state.py          # AdminState
│   │   └── pages.py          # Dashboard
│   └── pages.py              # Páginas públicas (home, about)
├── docs/
├── AGENTS.md
├── requirements.txt
├── .gitignore
└── rxconfig.py               # Configuración de Reflex
```

---

## 📱 Skill: `mobile-responsive`
**Propósito:** Asegurar que la app sea responsive para web y móvil.

### Consideraciones:
- Reflex es responsive por defecto gracias a su sistema de componentes
- Usar `rx.container` con `max_width` para controlar el ancho
- Para vistas específicas en móvil, usar `rx.mobile_and_tablet()` y `rx.desktop_only()`
- Priorizar diseño mobile-first con breakpoints en 640px, 768px, 1024px
- Los componentes `rx.grid` y `rx.flex` se adaptan automáticamente

### Ejemplo:
```python
import reflex as rx

def card_responsive() -> rx.Component:
    return rx.flex(
        rx.desktop_only(rx.text("Vista escritorio")),
        rx.mobile_and_tablet(rx.text("Vista móvil/tablet")),
    )
```

---

## 🚀 Skill: `produccion`
**Propósito:** Compilar y preparar la aplicación para producción.

### Comandos:
```bash
# Compilar frontend estático
reflex export --frontend-only

# Compilar backend
reflex export --backend-only

# Compilar ambos
reflex export

# Los archivos generados están en:
# - .web/ (frontend compilado)
# - backend_app/ (backend compilado)
```

### Frontend estático:
```bash
# Los archivos estáticos se exportan en:
# stylesync_frontend.zip  (después de reflex export --frontend-only)
# Descomprimir y servir con Nginx, Vercel, etc.
```

### Backend en producción:
```bash
# El backend se exporta como aplicación Python
# Servir con gunicorn / uvicorn:
# uvicorn stylesync.app:app --host 0.0.0.0 --port 8000
```

---

## 🧠 Cómo usar estos skills con opencode

Cada skill se puede invocar con el agente de opencode. Ejemplo:

```
@opencode Usa el skill init-project para inicializar el proyecto
@opencode Usa el skill database-schema para crear los modelos
@opencode Usa el skill auth-system para implementar autenticación
@opencode Usa el skill booking-system para crear el flujo de reservas
@opencode Usa el skill mobile-responsive para hacer la app responsive
@opencode Usa el skill produccion para compilar a producción
```

Para consultar la estructura de un skill específico, buscar en `AGENTS.md` el bloque correspondiente.
