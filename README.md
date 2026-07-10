# Captura de Recetas — El Taller de Cocina SAS

App móvil para levantar recetas, ingredientes, procesos y rendimientos con el chef, desde el celular. Viene precargada con los 164 ingredientes canónicos (con su $/gr) y las 48 recetas pendientes de captura.

## Qué hace

Cinco pestañas: **Recetas** (rendimiento, porciones, precio carta — la merma se calcula sola), **Ingredientes** (buscador sobre la lista canónica, costo al instante; si es nuevo pide proveedor y precio), **Procesos** (pasos con etapa, rol, # personas, minutos activos vs pasivos), **Rendimientos** (bruto → limpio → cocido) y **Exportar** (descarga un .xlsx con todo para cargar al sistema integrado). Acceso con PIN.

## Deploy en Railway (10 minutos)

**Paso 1 — GitHub.** Crea un repositorio nuevo (privado) en github.com → "uploading an existing file" → arrastra TODOS los archivos de esta carpeta (incluida la subcarpeta `static`) → Commit.

**Paso 2 — Railway.** En railway.app → New Project → **Deploy from GitHub repo** → autoriza y elige el repo. Railway detecta Python solo y usa el comando de arranque de `railway.json`.

**Paso 3 — Volumen (para no perder datos).** En el servicio → clic derecho → **Attach Volume** → mount path: `/data`. Sin esto, la base de datos se borra en cada redeploy.

**Paso 4 — Variables.** En el servicio → Variables → agregar:
- `DATABASE_PATH` = `/data/captura.db`
- `PIN` = el PIN que quieras (ej. 4 dígitos que sepan Jack y el chef)

**Paso 5 — Dominio.** Settings → Networking → **Generate Domain**. Esa URL es la app: ábrela en el celular y usa "Agregar a pantalla de inicio" para que quede como app.

## Uso en la sesión con el chef

1. Entrar con el PIN. 2. Pestaña Recetas → tocar la receta a capturar (quedan como "receta activa"). 3. Pesar y registrar ingredientes → el costo aparece en vivo. 4. Registrar pasos del proceso. 5. Al llenar "rinde final pesado" y marcar CAPTURADA, la receta queda lista. 6. Al final del día: Exportar → enviar el Excel a Jack.

## Correr local (opcional, para probar)

```bash
pip install -r requirements.txt
PIN=1234 uvicorn app:app --reload
# abrir http://localhost:8000
```

## Estructura

```
app.py               # API FastAPI + SQLite (crea las tablas sola)
static/index.html    # interfaz móvil (una sola página)
seed_canonicos.json  # 164 ingredientes con $/gr (del sistema integrado v4)
seed_recetas.json    # 48 recetas pendientes precargadas
railway.json         # comando de arranque para Railway
requirements.txt
```
