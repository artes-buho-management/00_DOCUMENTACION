# 🔑 CREDENCIALES A GENERAR — Antes de arrancar nada

> ⚠️ **TODAS LAS CLAVES DE ESTA ENTREGA SE HAN ELIMINADO.**
> ARTES BÚHO debe generar las suyas propias.

---

## ORDEN RECOMENDADO

Si las generas en este orden, no rompes dependencias:

1. Google Cloud Project + Service Account + OAuth2 Client
2. Gemini API Key
3. Anthropic API Key (solo para RAMON)
4. Holded API Key
5. WhatsApp Business (Meta) — transferir o crear nuevo
6. Telegram Bot (solo para RAMON)
7. Coolify + VPS Hostinger
8. Streamlit Cloud (opcional, para Factusol)

---

## 1. GOOGLE CLOUD PROJECT (CRÍTICO)

**Lo usan:** 37 de los 41 proyectos.

### 1.1 Crear proyecto en GCP

1. Ir a **https://console.cloud.google.com/projectcreate**
2. Nombre sugerido: `artes-buho-management`
3. Vincular a la facturación (la mayoría de APIs tienen cuota gratuita).

### 1.2 Habilitar APIs

En **APIs & Services → Library**, habilita:

- Google Sheets API
- Google Drive API
- Gmail API
- Google Calendar API
- Google Docs API
- Google Places API (New)
- Google Maps JavaScript API (si usáis mapas)
- YouTube Data API v3
- Cloud Logging API

### 1.3 Crear Service Account

1. **IAM & Admin → Service Accounts → Create**
2. Nombre: `artes-buho-sa` (o el que prefieras)
3. Rol: `Editor` (o más restrictivo según necesidad)
4. **Keys → Add Key → JSON**. Descargar el archivo.
5. Renombrar a `service-account.json` (o como indique cada proyecto).

### 1.4 Crear OAuth 2.0 Client

1. **APIs & Services → Credentials → Create Credentials → OAuth client ID**
2. Tipo: **Web application**
3. Redirect URIs: `http://localhost:3000/oauth/callback` (para desarrollo) + URI de producción
4. Anotar `CLIENT_ID` y `CLIENT_SECRET`.

### 1.5 Crear API Key (Places/Maps/YouTube)

1. **APIs & Services → Credentials → Create Credentials → API Key**
2. Restringir por API (solo Places, Maps, YouTube) y por IP de servidor.

### 1.6 Variables a rellenar

En `.env` de **ARTES-BUHO_API-GOOGLE** (el hub central):

```bash
GOOGLE_OAUTH_CLIENT_ID=xxxxxxxxxxx.apps.googleusercontent.com
GOOGLE_OAUTH_CLIENT_SECRET=GOCSPX-xxxxxxxxxxxx
GOOGLE_PLACES_API_KEY=AIzaSyxxxxxxxxxxxxx
GOOGLE_MAPS_API_KEY=AIzaSyxxxxxxxxxxxxx   # mismo o distinto
GOOGLE_YOUTUBE_API_KEY=AIzaSyxxxxxxxxxxxxx
SERVICE_ACCOUNT_PATH=./service-account.json
```

---

## 2. GEMINI API KEY

**Lo usan:** 10 proyectos (CRM-AYUDAS-LEGACY, CRM-MARKETING-CIRRADIO, FESTIVALES, VENTA-BOOKING-CRM, GMAIL-INBOX-ROUTER, HOJA-SYNC, DISTRITOS-MADRID, WHATSAPP-AI, WHATSAPP-WORK-API, RAMON).

### Generar

1. Ir a **https://aistudio.google.com/apikey**
2. **Create API Key** → vincular al proyecto GCP anterior.
3. Anotar la key.

### Configurar

- En proyectos Apps Script: **Project Settings → Script Properties → `GEMINI_API_KEY`**
- En proyectos Node/Python: en `.env` como `GEMINI_API_KEY=...`

---

## 3. ANTHROPIC API KEY

**Lo usa:** Solo `ARTES-BUHO_RAMON` (cascada IA: Gemini → Claude → Ollama).

### Generar

1. Crear cuenta en **https://console.anthropic.com**
2. **API Keys → Create Key**
3. Anotar.

### Configurar

En `.env` de RAMON:
```bash
ANTHROPIC_API_KEY=sk-ant-api03-xxxxxxxxxxxx
```

---

## 4. HOLDED API KEY

**Lo usan:** RAMON, HOLDED-LOOKER-GAS, HOLDED-LOOKER-LEGACY, HOLDED-PANEL-BI.

### Generar

1. Login en **https://app.holded.com**
2. **Ajustes → Cuenta → API Key** (Plan ≥ Plus)
3. Anotar.

### Configurar

En `.env` de los 4 proyectos:
```bash
HOLDED_API_KEY=xxxxxxxxxxxxxx
HOLDED_BASE_URL=https://api.holded.com/api/
```

---

## 5. WHATSAPP BUSINESS (META)

**Lo usan:** WHATSAPP-AI, WHATSAPP-WORK-API.

### Opción A — Transferir el WABA actual

Si el WABA (`1280805887302111`) está a nombre de ARTES BÚHO, simplemente cambia el token de acceso:

1. **https://business.facebook.com → Configuración → Cuentas de WhatsApp Business**
2. Selecciona la WABA → **Tokens → Crear token nuevo**
3. Permisos: `whatsapp_business_messaging`, `whatsapp_business_management`

### Opción B — Crear nueva WABA

1. Crear App en **https://developers.facebook.com/apps**
2. Añadir producto "WhatsApp"
3. Crear nuevo número de teléfono (Phone ID) en el dashboard WhatsApp
4. Token con permisos arriba indicados.

### Configurar

En `.env` de ambos proyectos:
```bash
WHATSAPP_TOKEN=EAAxxxxxxxxxxxxx
WHATSAPP_PHONE_ID=xxxxxxxxxxxxxx
WABA_ID=xxxxxxxxxxxxxx
WHATSAPP_VERIFY_TOKEN=cualquier_string_aleatorio   # para el webhook
```

---

## 6. TELEGRAM BOT

**Lo usa:** Solo RAMON.

### Generar

1. Abrir chat con **@BotFather** en Telegram.
2. `/newbot` → nombre: `Ramón ARTES BÚHO`, username: `@artesbuho_ramon_bot` (o similar).
3. Anotar el TOKEN.
4. Crear chat de grupo y añadir el bot → obtener `CHAT_ID` con `@userinfobot`.

### Configurar

En `.env` de RAMON:
```bash
TELEGRAM_BOT_TOKEN=1234567890:ABCxxxxxxxxxxxxxxxx
TELEGRAM_CHAT_ID=-1001234567890
```

---

## 7. COOLIFY + VPS HOSTINGER

**Lo usan:** 3 deploys activos (APP_ARTES-BUHO, BELLA-BESTIA, RAMON).

### Pasos

1. Contratar VPS en **Hostinger** (mínimo plan KVM2 con 4GB RAM).
2. Instalar Coolify: `curl -fsSL https://cdn.coollabs.io/coolify/install.sh | bash`
3. Crear nuevo proyecto en Coolify.
4. Conectar repositorio GitHub (ya en la org `artes-buho-management`).
5. Desplegar.

Detalles en `04_DEPLOYS_ACTIVOS.md`.

---

## 8. STREAMLIT CLOUD (opcional)

**Lo usa:** ARTES-BUHO_FACTUSOL-STREAMLIT.

### Pasos

1. Login en **https://share.streamlit.io** con GitHub.
2. **New app → Selecciona repo** (necesitan estar en GitHub).
3. Configurar secrets en el dashboard (variables que normalmente irían en `.env`).

---

## 📋 CHECKLIST FINAL

Antes de arrancar **CUALQUIER** proyecto:

- [ ] Google Cloud Project creado
- [ ] APIs habilitadas (Sheets, Drive, Gmail, etc.)
- [ ] Service Account creado y JSON descargado
- [ ] OAuth Client creado (Client ID + Secret)
- [ ] API Key (Places/Maps/YouTube) creada
- [ ] Gemini API Key creada
- [ ] Anthropic API Key creada (si vais a usar RAMON)
- [ ] Holded API Key creada (si vais a usar contabilidad)
- [ ] WhatsApp Business token (si vais a usar WhatsApp)
- [ ] Telegram Bot creado (si vais a usar RAMON)
- [ ] VPS contratado + Coolify instalado (si vais a desplegar)

---

## 🔒 DÓNDE GUARDAR ESTAS CLAVES

**NO commitar nunca en GitHub.** Usar:

- **Local:** `.env` por proyecto (ya en `.gitignore`)
- **Coolify:** **Environment Variables** dentro de cada app
- **Apps Script:** **Project Settings → Script Properties**
- **Gestor de contraseñas:** Bitwarden, 1Password, etc. para el equipo

---

> 💡 Si una sola clave queda comprometida, **rótala inmediatamente.** El proceso siempre es: borrar la vieja en el dashboard del servicio → crear nueva → actualizar `.env` y deploy.
