# StyleSync ✂️

> Aplicación web/móvil para reserva de citas en barbería.
> **Stack:** Reflex + SQLite + Python | **Colores:** cálidos | **Responsive:** web + móvil

---

## 📋 Descripción

StyleSync es un sistema completo de reservas para barberías que permite a los clientes agendar citas de forma fácil y automatizada, mientras que los barberos y administradores gestionan horarios, servicios y estadísticas desde un panel centralizado.

### Funcionalidades

| Clientes | Barberos / Admin |
|----------|-----------------|
| Registro y autenticación | Gestión de horarios y días libres |
| Visualización de servicios con precios y duración | Visualización de agenda (confirmadas, pendientes, canceladas) |
| Selección de barbero por especialidad | CRUD de servicios y precios |
| Reserva de citas (fecha, hora, servicio, barbero) | Notificaciones de nuevas reservas/cancelaciones |
| Cancelación y reprogramación | Reportes: citas, ingresos, clientes frecuentes |
| Historial de citas | Dashboard con estadísticas |
| Calificaciones y reseñas | Gestión de usuarios (roles, bans) |

---

## 🚀 Tecnologías

| Componente | Tecnología |
|------------|-----------|
| **Frontend** | Reflex (Python) — responsive web/móvil |
| **Backend** | Reflex (Python) — full-stack framework |
| **Base de datos** | SQLite vía SQLModel / SQLAlchemy |
| **Autenticación** | Reflex local_storage + bcrypt/passlib |
| **Entorno** | Python virtual env (`.venv`) |
| **Versionado** | Git |

---

## 🎨 Paleta de colores cálidos

| Color | Hex | Uso |
|-------|-----|-----|
| Dorado tierra | `#D4A373` | Primario |
| Crema | `#FAEDCD` | Secundario |
| Naranja quemado | `#CC8B3C` | Acento |
| Fondo cálido | `#FEFAE0` | Background |
| Marrón oscuro | `#3E2723` | Texto |
| Tarjetas | `#FFF8E7` | Cards |
| Verde oliva | `#6B8E23` | Success |

---

## 📁 Estructura del proyecto

```
StyleSync/
├── .venv/                    # Entorno virtual
├── assets/                   # Imágenes, iconos, CSS
├── stylesync/                # Código principal
│   ├── __init__.py
│   ├── models.py             # Modelos SQLite (SQLModel)
│   ├── theme.py              # Tema cálido
│   ├── state.py              # Estado global
│   ├── pages.py              # Páginas públicas (home, about)
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
│   └── admin/
│       ├── __init__.py
│       ├── state.py          # AdminState
│       └── pages.py          # Dashboard
├── docs/
├── AGENTS.md
├── requirements.txt
├── .gitignore
└── rxconfig.py               # Configuración de Reflex
```

---

## 🗄️ Modelos de base de datos

- **User** — id, name, email, password_hash, phone, role (client/barber/admin), created_at
- **Barber** — id, user_id (FK), specialties, bio, avatar, is_active
- **Service** — id, name, description, price, duration_minutes, is_active
- **BarberService** — id, barber_id (FK), service_id (FK)
- **Appointment** — id, client_id (FK), barber_id (FK), service_id (FK), date, time, status, notes, created_at
- **Schedule** — id, barber_id (FK), day_of_week, start_time, end_time, is_available
- **Review** — id, client_id (FK), appointment_id (FK), rating, comment, created_at

---

## 🔧 Instalación y ejecución

```bash
# 1. Clonar el repositorio
git clone <url>
cd StyleSync

# 2. Crear y activar entorno virtual
python -m venv .venv
source .venv/Scripts/activate          # Windows (Git Bash)
# .venv\Scripts\activate               # Windows (CMD/PowerShell)
# source .venv/bin/activate            # Linux / macOS

# 3. Instalar dependencias
pip install reflex sqlalchemy aiosqlite email-validator python-dotenv
pip freeze > requirements.txt

# 4. Inicializar proyecto Reflex
reflex init

# 5. Ejecutar en modo desarrollo
reflex run
```

Acceder a:
- **Frontend:** http://localhost:3000
- **Backend API:** http://localhost:8000

---

## 🧪 Comandos útiles

| Comando | Descripción |
|---------|-------------|
| `reflex run` | Ejecutar en modo desarrollo |
| `reflex run --loglevel debug` | Ejecutar con logs detallados |
| `reflex db migrate` | Sincronizar modelos con SQLite |
| `reflex export --frontend-only` | Compilar frontend estático |
| `pytest tests/ -v` | Ejecutar pruebas unitarias |
| `sqlite3 stylesync.db ".tables"` | Ver tablas en la BD |
| `sqlite3 stylesync.db "SELECT * FROM user;"` | Consultar datos |
| `rm -f stylesync.db && reflex run` | Reiniciar BD desde cero |

---

## 🐙 Git — Convención de commits

| Tipo | Propósito |
|------|-----------|
| `feat` | Nueva funcionalidad |
| `fix` | Corrección de bug |
| `refactor` | Refactorización de código |
| `style` | Cambios de estilo/UI |
| `docs` | Documentación |
| `chore` | Mantenimiento |
| `db` | Cambios en esquema de BD |

---

## 📄 Licencia

MIT
