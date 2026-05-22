# 📚 INFORME TÉCNICO DENSO — ARTES BÚHO MANAGEMENT

> Documento principal de la entrega. Explica **todos y cada uno** de los 41 proyectos.
> Orden: **de lo general a lo específico**.

---

## ÍNDICE

1. [Visión general de ARTES BÚHO Management](#1-visión-general)
2. [Arquitectura global del ecosistema](#2-arquitectura-global)
3. [Las 9 categorías de proyectos](#3-categorías)
4. [Hub central: ARTES-BUHO_API-GOOGLE](#4-hub-central)
5. [Cliente especial: BELLA BESTIA (línea contable + dashboard)](#5-bella-bestia)
6. [Asistente IA principal: RAMON BOT](#6-ramon-bot)
7. [Detalle proyecto por proyecto (41)](#7-detalle-por-proyecto)
8. [Cómo arrancar cualquier proyecto](#8-cómo-arrancar)
9. [Despliegues activos en VPS](#9-despliegues-activos)
10. [Cuenta operativa y dominios](#10-cuenta-y-dominios)

---

## 1. Visión general

**ARTES BÚHO MANAGEMENT** es una **agencia de booking y management musical** que representa artistas, vende artistas a distritos de Madrid, festivales y bodas, gestiona ayudas/subvenciones culturales y ofrece servicios de contabilidad de decisión a sus clientes.

Cliente operativa principal: **`booking@artesbuhomanagement.com`** (Google Workspace).

El ecosistema de software se construyó sobre **Google Workspace (Sheets, Drive, Gmail) + Apps Script + IA (Gemini/Claude) + Coolify VPS** para automatizar:

- 🎯 Booking de artistas
- 📊 Contabilidad (Factusol + Holded + Google Sheets)
- 💌 Emailing masivo
- 📞 Atención WhatsApp con IA
- 🗂️ CRMs de contactos, festivales, ayudas, bandas
- 🤖 Asistente IA autónomo (RAMON)

---

## 2. Arquitectura global

```
                   ┌─────────────────────────────────┐
                   │   Google Workspace              │
                   │   booking@artesbuhomanagement   │
                   └────────────┬────────────────────┘
                                │
                  ┌─────────────┴─────────────┐
                  │                           │
          ┌───────▼────────┐         ┌────────▼─────────┐
          │  HUB OAUTH     │         │  Apps Script     │
          │  API-GOOGLE    │         │  ~20 proyectos   │
          └───────┬────────┘         └──────────────────┘
                  │
       ┌──────────┼──────────┬────────────┬───────────────┐
       │          │          │            │               │
  ┌────▼────┐ ┌───▼────┐ ┌───▼─────┐  ┌───▼─────┐  ┌──────▼───────┐
  │EMAILING │ │ HTML   │ │ FIRMA   │  │ BELLA   │  │   RAMON BOT   │
  │ Coolify │ │ Builder│ │ DIGITAL │  │ BESTIA  │  │ IA Autónomo   │
  └─────────┘ └────────┘ └─────────┘  └─────────┘  │  Telegram     │
                                                   │  Gemini+Claude│
                                                   └───────────────┘

         CONTABILIDAD: Factusol (local DB) → Sheets → Streamlit/Looker
                       Holded (ERP) → Looker / Panel BI

         WHATSAPP: Meta Business API → Gemini → Respuesta automática

         CRMs: Festivales, Distritos Madrid, Ayudas, Bandas, Cirradio...
```

**Hubs centrales (sin ellos no funciona casi nada):**
1. `ARTES-BUHO_API-GOOGLE` — OAuth y APIs Google para todas las apps Node.js
2. `ARTES-BUHO_RAMON` — Asistente IA, gestiona Gmail/Calendar/Drive
3. `ARTES-BUHO_EMAILING` — Plataforma envío masivo

---

## 3. Categorías

| # | Carpeta | Repos | Propósito |
|---|---|---|---|
| 1 | `01_apps_principales/` | 11 | Apps web + Google Apps Script (Gestión core) |
| 2 | `02_hubs_infraestructura/` | 4 | Hubs reutilizables (API Google, Emailing, HTML, Firma) |
| 3 | `03_crms_y_hojas/` | 9 | CRMs en Google Sheets + apps script |
| 4 | `04_bots_y_asistentes/` | 1 | Bot IA autónomo (RAMON) |
| 5 | `05_automatizaciones_gas/` | 5 | Automatizaciones Apps Script (WhatsApp, Gmail Router) |
| 6 | `06_drive_y_backups/` | 2 | Backups y gestión Google Drive |
| 7 | `07_contabilidad_factusol_holded/` | 7 | Contabilidad (Factusol + Holded + Sheets) |
| 8 | `08_mapa_musical/` | 1 | Curso M.A.P.A. Musical (Román) |
| 9 | `09_otros/` | 1 | Promotora (Excel + VBA) |

---

## 4. Hub Central

### `ARTES-BUHO_API-GOOGLE`

📍 `repos/02_hubs_infraestructura/ARTES-BUHO_API-GOOGLE/`

**Es la pieza más importante del ecosistema.** Cualquier app Node.js que necesite hablar con Google (Sheets, Drive, Gmail, Places, Maps, YouTube, Calendar, Docs) lo hace **a través de este hub**.

- **Stack:** Node.js
- **Auth:** OAuth 2.0 + Service Account (depende del proyecto)
- **APIs encapsuladas:**
  - Google Sheets v4
  - Google Drive v3
  - Gmail
  - Calendar v3
  - Docs v1
  - Places (búsqueda de leads)
  - Maps
  - YouTube Data v3
- **Configuración:** `.env` con `GOOGLE_OAUTH_CLIENT_ID`, `GOOGLE_OAUTH_CLIENT_SECRET`, `GOOGLE_PLACES_API_KEY`, paths a service account
- **Setup:** `npm install && npm run oauth-setup` (autoriza una sola vez)

⚠️ **Sin generar nuevas credenciales OAuth/Service Account en GCP, todo el ecosistema Node.js queda inoperativo.**

---

## 5. BELLA BESTIA

Cliente principal de ARTES BÚHO. Toda la línea contable + el dashboard BI están en estos 2 repos:

### 5.1 `APP_ARTES-BUHO_BELLA-BESTIA`

📍 `repos/01_apps_principales/APP_ARTES-BUHO_BELLA-BESTIA/`

**App web operativa de contabilidad para Bella Bestia.** Es la app que el cliente usa día a día.

- **Stack:** React 19 + Vite (frontend) + Node.js (backend)
- **DB:** Google Sheets (service account)
- **Deploy:** **EN PRODUCCIÓN** → `bella-bestia.artesbuhomanagement.com` (Coolify VPS)
- **Tema:** Colores corporativos ARTES BÚHO (rojo/amarillo/blanco)
- **Funciones:** Gastos, ingresos, facturación, cuadre cuentas
- **Migración:** ARTES BÚHO necesita su propio VPS + dominio + service account Google

### 5.2 `APP_ARTES-BUHO_BELLA-BESTIA-FORECAST`

📍 `repos/01_apps_principales/APP_ARTES-BUHO_BELLA-BESTIA-FORECAST/`

**Dashboard BI sobre datos de Bella Bestia.** Para forecasts y análisis.

- **Stack:** Python + Streamlit
- **Fuente:** Google Sheets de Bella Bestia
- **Deploy:** Local / Streamlit Cloud
- **Uso:** Análisis predictivo de ingresos/gastos

---

## 6. RAMON BOT

### `ARTES-BUHO_RAMON`

📍 `repos/04_bots_y_asistentes/ARTES-BUHO_RAMON/`

**Asistente ejecutivo autónomo para ARTES BÚHO.** Gemelo del bot Rocío (que es de RUBEN COTON personal — NO incluido en esta entrega).

- **Stack:** Python (FastAPI + APScheduler)
- **Deploy:** **EN PRODUCCIÓN** → Coolify VPS (bot Telegram activo)
- **Funcionalidades:**
  - Lee Gmail (cuenta `booking@`)
  - Clasifica emails con IA
  - Crea borradores de respuesta
  - Gestiona Calendar (citas)
  - Lee/escribe Drive y Sheets
  - Consulta Holded (facturas/clientes)
  - Avisa al usuario por **Telegram bot**
- **Cascada IA:**
  1. **Gemini** (primera elección)
  2. **Anthropic Claude** (escalado)
  3. **Ollama local** (fallback)
- **DB:** PostgreSQL
- **Regla irrenunciable:** **Ramón NUNCA envía correos.** Solo lee, clasifica, archiva, crea borradores. Cero envíos automáticos.

⚠️ **Es uno de los proyectos más complejos y críticos.** Requiere migrar bot Telegram + DB + VPS.

---

## 7. Detalle por proyecto

### CATEGORÍA 1 — APPS PRINCIPALES (11)

---

#### 1.1 `APP_ARTES-BUHO`

- **Qué hace:** Panel central de acceso privado a todas las apps de la empresa.
- **Quién lo usa:** El equipo interno de ARTES BÚHO.
- **Stack:** Node.js Express + cookies de dominio `.artesbuhomanagement.com`
- **Deploy:** Coolify VPS (producción)
- **Cómo arrancar:** `npm install && npm start`
- **Variables necesarias:** Ver `.env.example`

#### 1.2 `APP_ARTES-BUHO_BELLA-BESTIA`

Ver [sección 5.1](#5-bella-bestia).

#### 1.3 `APP_ARTES-BUHO_BELLA-BESTIA-FORECAST`

Ver [sección 5.2](#5-bella-bestia).

#### 1.4 `APP_ARTES-BUHO_BUSCA-CONTACTOS`

- **Qué hace:** Buscador de leads usando **Google Places API**. Identifica negocios potencialmente interesantes para vender artistas.
- **Stack:** Google Apps Script (hoja Sheets) + microservicio Python Flask/Selenium para enriquecer datos
- **Deploy:** Apps Script + Docker local
- **Variables clave:** `GOOGLE_PLACES_API_KEY`, alias aceptados: `GOOGLE_MAPS_API_KEY`, `PLACES_API_KEY`
- **Regla activa:** **NO usar scraping** de Google Search. **SÍ usar Google Places API**.

#### 1.5 `APP_ARTES-BUHO_CENTRAL-CONTACTOS`

- **Qué hace:** Hub central de contactos (ex `APP_ARTES-BUHO_CENTRAL-CONTACTOS+`).
- **Stack:** Apps Script
- **Estado:** Documentación + prompts (sin código operativo todavía)

#### 1.6 `APP_ARTES-BUHO_CONTABILIDAD`

- **Qué hace:** App web simple para controlar **gastos, facturas y asientos manuales**. MVP usando Google Sheets como DB.
- **Stack:** Google Apps Script
- **URL despliegue:** `contabilidad.artesbuhomanagement.com`
- **Diferencia con CONTABILIDAD-DECISION:** Esto es operativo (gastos del día a día). El otro es estratégico (líneas de negocio).

#### 1.7 `APP_ARTES-BUHO_CONTABILIDAD-DECISION`

- **Qué hace:** **Contabilidad de decisión** por líneas de negocio. No sustituye el software contable oficial, sirve para ver qué línea (Sala Bella Bestia, Escuela, Management, etc.) gana o pierde.
- **Stack:** Apps Script + Node.js para audit scripts
- **Líneas de negocio identificadas:** Sala Bella Bestia, Escuela, Management, Festivales, Distritos, Cirradio, etc.

#### 1.8 `APP_ARTES-BUHO_CRM-AYUDAS`

- **Qué hace:** CRM para gestionar **ayudas y subvenciones culturales**. Sigue convocatorias.
- **Stack:** Google Apps Script + Sheets
- **Integraciones:** Gmail (avisos)

#### 1.9 `APP_ARTES-BUHO_CRM-AYUDAS-GAS`

- **Qué hace:** Variante anterior del CRM ayudas (versión Apps Script standalone, antes de hacerla "app").
- **Stack:** Apps Script
- **Diferencia con CRM-AYUDAS:** Solo el código GAS; CRM-AYUDAS es la versión con frontend.

#### 1.10 `APP_ARTES-BUHO_CRM-CENTRAL`

- **Qué hace:** Buscador/actualizador central **multi-CRM** de contactos. Une 7 CRMs en un solo punto.
- **Stack:** Apps Script
- **CRMs unidos:** Festivales, Distritos, Ayudas, Mundo Discográfico, Bandas, Cirradio, Marketing

#### 1.11 `APP_ARTES-BUHO_DISTRITOS-MADRID`

- **Qué hace:** **CRM de distritos de Madrid** para colocar artistas en fiestas de distrito. Gestiona pliegos y adjudicaciones.
- **Stack:** Node.js (`googleapis`) + OAuth
- **Alcance:** Solo empresas adjudicatarias de pliegos. Objetivo: **vender artistas**.

---

### CATEGORÍA 2 — HUBS INFRAESTRUCTURA (4)

---

#### 2.1 `ARTES-BUHO_API-GOOGLE`

Ver [sección 4](#4-hub-central). **PIEZA CRÍTICA.**

#### 2.2 `ARTES-BUHO_EMAILING`

- **Qué hace:** **Plataforma de emailing masivo** con motor propio.
- **Stack:** Node Express + watchdog (auto-restart en fallo)
- **Deploy:** **EN PRODUCCIÓN** → Coolify VPS
- **Cuenta envío:** `booking@artesbuhomanagement.com`
- **Funciones:** Crear campañas, segmentar audiencia, enviar, reportes
- **Cap diario:** Configurable (típicamente 1500-2000/día por Google Workspace)

#### 2.3 `ARTES-BUHO_FIRMA-DIGITAL`

- **Qué hace:** Firma digital **PAdES** de PDFs usando **DNIe** (Documento Nacional Electrónico) y lector de tarjetas.
- **Stack:** Node Express + Autofirma (Cliente @firma del Gobierno de España)
- **IA:** Ollama local con Qwen 2.5 para asistir el flujo
- **Uso:** Firmar contratos de booking, acuerdos artistas

#### 2.4 `ARTES-BUHO_HTML`

- **Qué hace:** **Email Builder** HTML. Permite crear plantillas de email visualmente para usar luego en `ARTES-BUHO_EMAILING`.
- **Stack:** Node Express
- **Funciones:** Drag & drop, previsualización, export HTML compatible Gmail/Outlook

---

### CATEGORÍA 3 — CRMS Y HOJAS (9)

---

#### 3.1 `ARTES-BUHO_BUSQUEDA-WEB-INDUSTRIA-MUSICAL`

- **Qué hace:** Rastreo web de contactos de la **industria musical**. Identifica festivales, salas, agencias hermanas.
- **Stack:** Apps Script

#### 3.2 `ARTES-BUHO_CRM-AYUDAS-LEGACY`

- **Qué hace:** Versión legacy del CRM ayudas (anterior a APP_ARTES-BUHO_CRM-AYUDAS).
- **Stack:** Apps Script + Gemini
- **Estado:** Mantener por trazabilidad, no usar activamente.

#### 3.3 `ARTES-BUHO_CRM-MARKETING-CIRRADIO`

- **Qué hace:** CRM de **marketing y promoción** del programa Cirradio.
- **Stack:** Apps Script + Gemini (IA para enriquecimiento de contactos)
- **Nota:** "Cirradio" es el nombre de la hoja, no una empresa externa.

#### 3.4 `ARTES-BUHO_FESTIVALES`

- **Qué hace:** **CRM de festivales** para colocar artistas representados. Una de las líneas de negocio principales.
- **Stack:** Apps Script + Gemini

#### 3.5 `ARTES-BUHO_HOJA-SYNC-MUNDO-DISCOGRAFICO`

- **Qué hace:** CRM "Mundo Discográfico" — sincronización de hojas con auditoría IA.
- **Stack:** Apps Script + Gemini

#### 3.6 `ARTES-BUHO_HOJA-WEBAPP-CORREOS`

- **Qué hace:** Webapp en Google Sheets para crear correos asistidos por IA.
- **Stack:** Apps Script

#### 3.7 `ARTES-BUHO_OBJETIVO-BANDAS`

- **Qué hace:** CRM de **objetivos de bandas** — seguimiento de redes (Instagram, TikTok, Spotify, Live) para la línea Management.
- **Stack:** Apps Script + Google Cloud Logging

#### 3.8 `ARTES-BUHO_SITUACION-BANDAS`

- **Qué hace:** Dashboard de **situación de bandas** (estado actual, próximos lanzamientos).
- **Stack:** Python + Streamlit + Apps Script
- **Deploy:** Local / Streamlit

#### 3.9 `ARTES-BUHO_VENTA-BOOKING-CRM`

- **Qué hace:** **CRM de ventas de booking**. Cierre comercial de conciertos.
- **Stack:** Apps Script + Gemini

---

### CATEGORÍA 4 — BOTS Y ASISTENTES (1)

---

#### 4.1 `ARTES-BUHO_RAMON`

Ver [sección 6](#6-ramon-bot). **PIEZA CRÍTICA.**

---

### CATEGORÍA 5 — AUTOMATIZACIONES GAS (5)

---

#### 5.1 `ARTES-BUHO_GMAIL-INBOX-ROUTER`

- **Qué hace:** **Router inteligente de Gmail**. Lee la bandeja, clasifica con Gemini (modelo `gemini-3.1-pro-preview`), aplica labels, reenvía si procede.
- **Stack:** Apps Script
- **API:** Gemini API Key

#### 5.2 `ARTES-BUHO_INFORMES-LABORALES-SEMANALES`

- **Qué hace:** Genera informes laborales semanales (mencionado en commits: empleado "Román").
- **Stack:** Apps Script
- **Salida:** Google Docs + Sheets

#### 5.3 `ARTES-BUHO_MENSAJERIA-HOJA`

- **Qué hace:** Sistema de **envío de mensajes desde Google Sheets** vía Gmail.
- **Stack:** Apps Script

#### 5.4 `ARTES-BUHO_WHATSAPP-AI`

- **Qué hace:** Bot WhatsApp Business + IA. Responde mensajes con Gemini.
- **Stack:** Node Express + WhatsApp Cloud API (Meta)
- **Phone ID:** Existe ya un número configurado (ver `.env.example`)
- **WABA ID:** Existe configurado

#### 5.5 `ARTES-BUHO_WHATSAPP-WORK-API`

- **Qué hace:** API WhatsApp Business empresarial (versión derivada de WHATSAPP-AI).
- **Stack:** Node Express + WhatsApp Cloud API (Meta)

⚠️ **WHATSAPP-AI y WHATSAPP-WORK-API comparten Phone ID en producción.** Decidir cuál mantener.

---

### CATEGORÍA 6 — DRIVE Y BACKUPS (2)

---

#### 6.1 `ARTES-BUHO_COPIAS-SEGURIDAD-DRIVE`

- **Qué hace:** **Copias de seguridad semanales** de Google Drive.
- **Stack:** PowerShell + tareas programadas Windows
- **Frecuencia:** Semanal

#### 6.2 `ARTES-BUHO_DRIVE-BOOKING`

- **Qué hace:** Gestión del Drive de `booking@artesbuhomanagement.com`.
- **Stack:** Apps Script

---

### CATEGORÍA 7 — CONTABILIDAD (7)

---

#### 7.1 `ARTES-BUHO_CONTABILIDAD-CORE`

- **Qué hace:** App de contabilidad standalone (sin dependencias externas).
- **Stack:** Node.js puro
- **Estado:** Núcleo, ampliable

#### 7.2 `ARTES-BUHO_FACTUSOL-COMPLETO`

- **Qué hace:** Pipeline completo **Factusol → Google Sheets → Streamlit + informes PDF**.
- **Stack:** Python + Streamlit
- **DB origen:** Firebird local (Factusol)
- **Service Account:** Robot Codex Key
- **Arquitectura:** 100% gratuita, open source

#### 7.3 `ARTES-BUHO_FACTUSOL-LOOKER-LEGACY`

- **Qué hace:** Versión legacy del conector Factusol-Looker.
- **Stack:** Python + Streamlit

#### 7.4 `ARTES-BUHO_FACTUSOL-STREAMLIT`

- **Qué hace:** Panel Streamlit independiente sobre datos Factusol.
- **Stack:** Streamlit Cloud
- **Deploy:** Streamlit Cloud (gratis)

#### 7.5 `ARTES-BUHO_HOLDED-LOOKER-GAS`

- **Qué hace:** Conector Holded (ERP) + Apps Script.
- **Stack:** Node ESM
- **API:** Holded API Key

#### 7.6 `ARTES-BUHO_HOLDED-LOOKER-LEGACY`

- **Qué hace:** Versión legacy Node del conector Holded.
- **Estado:** Marcado explícitamente como legacy en el propio README.

#### 7.7 `ARTES-BUHO_HOLDED-PANEL-BI`

- **Qué hace:** Panel BI Streamlit + informes PDF automatizados de Holded.
- **Stack:** Python + Streamlit + gspread
- **Sistema activo** (sustituye al legacy).

---

### CATEGORÍA 8 — MAPA MUSICAL (1)

---

#### 8.1 `ARTES-BUHO_MAPA-MUSICAL`

- **Qué hace:** Gestión del **curso online "M.A.P.A. Musical"** creado por Román (de ARTES BÚHO).
- **Stack:** Node.js
- **Funciones:** Hoja de seguimiento de alumnos, materiales del curso

---

### CATEGORÍA 9 — OTROS (1)

---

#### 9.1 `ARTES-BUHO_PROMOTORA`

- **Qué hace:** Automatización de **Outlook + Revit + scraping web** para la línea promotora.
- **Stack:** Python + VBA + Excel
- **Nota:** Confianza media — sin descripción explícita en GitHub.

---

## 8. Cómo arrancar

### Apps Node.js
```bash
cd ruta/al/proyecto
cp .env.example .env       # Rellena con credenciales propias
npm install
npm start   # o el script indicado en package.json
```

### Apps Python
```bash
cd ruta/al/proyecto
python -m venv venv
.\venv\Scripts\activate    # Windows
pip install -r requirements.txt
cp .env.example .env       # Rellena
python main.py             # o el archivo principal
```

### Apps Streamlit
```bash
cd ruta/al/proyecto
streamlit run app.py
```

### Google Apps Script (GAS)
```bash
npm install -g @google/clasp
clasp login
clasp clone <SCRIPT_ID>   # El SCRIPT_ID está en el README de cada GAS
clasp push
```

---

## 9. Despliegues activos

3 apps están **en producción ahora mismo** en VPS Hostinger gestionado por Coolify (cuenta de RUBEN COTON):

| App | URL | Acción ARTES BÚHO |
|---|---|---|
| `APP_ARTES-BUHO` | `*.artesbuhomanagement.com` | Migrar a VPS propio |
| `APP_ARTES-BUHO_BELLA-BESTIA` | `bella-bestia.artesbuhomanagement.com` | Migrar a VPS propio |
| `ARTES-BUHO_RAMON` | Bot Telegram activo | Migrar a VPS propio + nuevo bot Telegram |

**ANTES DEL 5 DE JUNIO DE 2026:** ARTES BÚHO debe migrar estos 3 deploys o se caerán cuando RUBEN COTON apague su VPS.

Ver detalles en `04_DEPLOYS_ACTIVOS.md`.

---

## 10. Cuenta y dominios

- **Cuenta Google operativa:** `booking@artesbuhomanagement.com` (de ARTES BÚHO — sigue siendo propiedad de ARTES BÚHO)
- **Dominio principal:** `artesbuhomanagement.com` (propiedad de ARTES BÚHO)
- **Subdominios activos:** `bella-bestia.artesbuhomanagement.com`, `contabilidad.artesbuhomanagement.com`, posiblemente más

⚠️ **Nada de esto se transfiere — es de ARTES BÚHO desde siempre.** Lo que se entrega es solo el código.

---

## FIN del informe denso

Si necesitas más detalle de algún proyecto, **abre su `README.md` interno** en su carpeta.
