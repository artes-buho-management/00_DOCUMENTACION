# 📦 ENTREGA TÉCNICA — ARTES BÚHO MANAGEMENT

> **Para:** El equipo técnico que continúa el trabajo (David / sucesor).
> **De:** RUBEN COTON (desarrollador original).
> **Fecha:** 21 de mayo de 2026.
> **Carpeta:** `C:\Users\elrub\Desktop\ARTES-BUHO-MANAGEMENT\`
> **GitHub org:** `artes-buho-management` (repos públicos, link compartible).

---

## ⏰ PLAZO DE 15 DÍAS

Esta carpeta y los repos públicos en GitHub **estarán disponibles HASTA el 5 de junio de 2026**. Después se **eliminarán definitivamente**.

➜ **Descargad TODO lo que necesitéis antes de esa fecha.**

---

## 🚨 LO PRIMERO QUE TIENES QUE SABER

### 1. NO hay credenciales API en estos archivos

Se han eliminado **todos** los archivos `.env`, `secrets/`, claves privadas (`*.key`, `*.pem`), service-accounts JSON y `client_secret*.json`.

**Cada proyecto incluye un archivo `.env.example`** que indica QUÉ claves necesita, pero SIN valores.

ARTES BÚHO debe **generar sus propias credenciales** antes de arrancar cualquier proyecto. Ver sección **"Credenciales a generar"** más abajo.

### 2. Son 41 proyectos en 9 categorías

Ver `01_INFORME_TECNICO_DENSO.md` para el detalle de cada uno.

### 3. NO hay node_modules / venv / dist

Para hacer la entrega más ligera, no se incluyen dependencias instaladas. En cada proyecto:

- Node.js: `npm install`
- Python: `pip install -r requirements.txt` (o `pip install -r pyproject.toml`)
- Apps Script: `clasp pull` desde el script ID del README

### 4. Hay 3 apps en producción ahora mismo en VPS de RUBEN COTON

| App | URL |
|---|---|
| `APP_ARTES-BUHO` | https://app.artesbuhomanagement.com (orientativo) |
| `APP_ARTES-BUHO_BELLA-BESTIA` | https://bella-bestia.artesbuhomanagement.com |
| `ARTES-BUHO_RAMON` | Bot Telegram activo |

⚠️ **Antes del fin de plazo, ARTES BÚHO debe migrar estas 3 apps a su propio VPS** o se caerán cuando Rubén las apague.

---

## 📁 ESTRUCTURA DE LA ENTREGA

```
ARTES-BUHO-MANAGEMENT/
├── 00_DOCUMENTACION/
│   ├── 00_LEEME_PRIMERO.md           ← Este archivo
│   ├── 01_INFORME_TECNICO_DENSO.md   ← Explicación de TODOS los proyectos
│   ├── 02_IMPORTAR-A-CLAUDE.md       ← Cómo continuar con Claude Code
│   ├── 03_CREDENCIALES_A_GENERAR.md  ← Lista de claves que tenéis que crear
│   ├── 04_DEPLOYS_ACTIVOS.md         ← Migración de los 3 deploys en VPS
│   └── WHATSAPP-DAVID.txt            ← Mensaje WhatsApp para David
└── repos/
    ├── 01_apps_principales/          11 apps web/GAS principales
    ├── 02_hubs_infraestructura/      Hub APIs Google + Emailing + HTML + Firma
    ├── 03_crms_y_hojas/              CRMs operativos (9)
    ├── 04_bots_y_asistentes/         Bot RAMON (asistente IA)
    ├── 05_automatizaciones_gas/      Automatizaciones Apps Script
    ├── 06_drive_y_backups/           Drive + copias seguridad
    ├── 07_contabilidad_factusol_holded/  Contabilidad
    ├── 08_mapa_musical/              Curso M.A.P.A. Musical
    └── 09_otros/                     PROMOTORA
```

---

## ▶️ CÓMO USAR ESTA ENTREGA — PASO A PASO

1. **Leer ahora mismo:** este archivo + `01_INFORME_TECNICO_DENSO.md`
2. **Importar a Claude Code:** seguir `02_IMPORTAR-A-CLAUDE.md`
3. **Generar credenciales:** seguir `03_CREDENCIALES_A_GENERAR.md`
4. **Migrar deploys activos:** seguir `04_DEPLOYS_ACTIVOS.md`
5. **Empezar por el HUB:** `repos/02_hubs_infraestructura/ARTES-BUHO_API-GOOGLE/`. Es el corazón — todas las demás apps lo invocan.
6. **Después el bot RAMON:** `repos/04_bots_y_asistentes/ARTES-BUHO_RAMON/`. Es el asistente principal de la agencia.

---

## 🔗 ENLACES GITHUB (repos públicos, abrir con cualquier link)

La organización completa: **https://github.com/artes-buho-management**

⚠️ Los repos son **públicos** y se pueden clonar sin autenticación. Si ARTES BÚHO los va a desarrollar internamente, **cambiadlos a privados después de clonar**.

---

## 📞 CONTACTO

Para dudas técnicas durante el periodo de transición:

- **RUBEN COTON:** manager@rubencoton.com / WhatsApp +34 613 00 93 36

---

> Cualquier información que falte, indicarlo cuanto antes. Este documento es la **fuente oficial de verdad** de la entrega.
