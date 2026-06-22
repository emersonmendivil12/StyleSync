# StyleSync — Agentes y Skills

> Proyecto: **StyleSync** — Aplicación web/móvil con Reflex + SQLite
> Colores: tonos cálidos | Entorno: Python virtual env | Versionado: Git

---

## Skill 1: setup-proyecto

**Propósito:** Inicializar el entorno virtual, instalar dependencias y preparar la estructura del proyecto.

**Comandos:**
```bash
# Crear entorno virtual
python -m venv .venv

# Activar entorno (Windows)
.venv\Scripts\activate

# Activar entorno (Linux/macOS)
# source .venv/bin/activate

# Instalar Reflex y dependencias
pip install reflex sqlite3

# Inicializar proyecto Reflex
reflex init
```

**Estructura generada:**
```
StyleSync/
├── .venv/
├── .git/
├── .gitignore
├── assets/
├── StyleSync/
│   ├── __init__.py
│   ├── style.py       <- colores cálidos
│   ├── model.py       <- modelos SQLite
│   ├── state.py       <- estado de la app
│   └── pages/         <- rutas/páginas
├── rxconfig.py
└── requirements.txt
```

---

## Skill 2: modelos-bd

**Propósito:** Crear los modelos SQLite con SQLAlchemy (Reflex usa SQLAlchemy internamente).

**Ubicación:** `StyleSync/model.py`

**Ejemplo de modelo:**
```python
import reflex as rx
import sqlalchemy

class Producto(rx.Model, table=True):
    nombre: str
    descripcion: str
    precio: float
    imagen: str
```

**Migraciones:** Reflex usa `reflex db migrate` para sincronizar los modelos.

---

## Skill 3: colores-calidos

**Propósito:** Definir la paleta de colores cálidos y tema global.

**Ubicación:** `StyleSync/style.py`

**Paleta recomendada:**
```python
import reflex as rx

estilo_base = {
    "font_family": "Inter, sans-serif",
    "color": "#3D2B1F",          # marrón oscuro (texto)
}

colores = {
    "fondo": "#FFF5E6",           # crema claro
    "primario": "#E8751A",        # naranja quemado
    "secundario": "#F4A460",      # naranja claro
    "acento": "#D2691E",          # chocolate
    "texto": "#3D2B1F",           # marrón oscuro
    "blanco": "#FFFAF0",          # floral white
}
```

---

## Skill 4: pages-rutas

**Propósito:** Crear las páginas/rutas de la aplicación Reflex.

**Ubicación:** `StyleSync/pages/`

**Ejemplo de página:**
```python
import reflex as rx
from ..style import colores

def inicio() -> rx.Component:
    return rx.container(
        rx.heading("Bienvenido a StyleSync", color=colores["primario"]),
        padding="2em",
    )
```

**Registro en `app.py`:**
```python
app = rx.App(style=estilo_base)
app.add_page(inicio, route="/")
```

---

## Skill 5: estado-global

**Propósito:** Manejar el estado global de la aplicación con Reflex State.

**Ubicación:** `StyleSync/state.py`

```python
import reflex as rx

class EstadoApp(rx.State):
    usuario: str = ""
    loading: bool = False

    def cargar_datos(self):
        self.loading = True
        yield
        # lógica con SQLite
        self.loading = False
```

---

## Skill 6: git-gestion

**Propósito:** Comandos para gestionar el repositorio Git.

```bash
git status                    # ver estado
git add .                     # agregar cambios
git commit -m "mensaje"       # commit
git push origin main          # subir a GitHub
git pull origin main          # bajar cambios
```

**.gitignore recomendado:**
```
.venv/
__pycache__/
*.pyc
*.db
.DS_Store
```

---

## Skill 7: probar-app

**Propósito:** Comandos para ejecutar y probar la aplicación.

```bash
# Activar entorno
.venv\Scripts\activate

# Ejecutar Reflex en modo desarrollo
reflex run

# Ejecutar con dirección específica (móvil)
reflex run --loglevel debug

# Migrar base de datos (después de cambios en modelos)
reflex db migrate

# Abrir en navegador
# http://localhost:3000
```

---

## Skill 8: mobile-responsive

**Propósito:** Asegurar que la app sea responsive para móvil.

Reflex es responsive por defecto. Usar componentes como `rx.container` con `max_width` y `rx.mobile_and_tablet()` para vistas específicas.

---

## Skill 9: produccion

**Propósito:** Preparar la app para producción.

```bash
# Compilar versión estática
reflex export --frontend-only

# Los archivos estáticos se generan en style_sync_frontend/
# Para backend: reflex export --backend-only
```

---

## Resumen de comandos rápidos

| Comando | Descripción |
|---------|-------------|
| `python -m venv .venv` | Crear entorno virtual |
| `.venv\Scripts\activate` | Activar entorno (Windows) |
| `pip install reflex` | Instalar Reflex |
| `reflex init` | Inicializar proyecto |
| `reflex run` | Ejecutar en desarrollo |
| `reflex db migrate` | Migrar base de datos |
| `git add . && git commit -m "msg"` | Commit |
| `git push origin main` | Subir a GitHub |
