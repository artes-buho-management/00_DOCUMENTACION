# 🚀 DEPLOYS ACTIVOS — Migración urgente

> ⏰ **DEADLINE: 5 de junio de 2026.** Después, el VPS de Rubén Cotón se apagará y estos 3 servicios se caerán.

---

## 3 APPS EN PRODUCCIÓN AHORA MISMO

| App | URL actual | Stack | Criticidad |
|---|---|---|---|
| `APP_ARTES-BUHO` | `*.artesbuhomanagement.com` | Node Express | ALTA |
| `APP_ARTES-BUHO_BELLA-BESTIA` | `bella-bestia.artesbuhomanagement.com` | React 19 + Node | ALTA |
| `ARTES-BUHO_RAMON` | Bot Telegram (sin URL pública) | Python FastAPI | ALTA |

VPS actual: **Hostinger** + **Coolify** (cuenta de Rubén Cotón, no transferible).

---

## PLAN DE MIGRACIÓN — 4 PASOS

### PASO 1 — Aprovisionar VPS propio de ARTES BÚHO

1. Contratar VPS en **Hostinger** (KVM2 o superior — 4 GB RAM mínimo).
2. SO: Ubuntu 22.04 LTS.
3. Acceso SSH habilitado.

### PASO 2 — Instalar Coolify

```bash
ssh root@<IP_NUEVO_VPS>
curl -fsSL https://cdn.coollabs.io/coolify/install.sh | bash
```

Accede al panel: `http://<IP_NUEVO_VPS>:8000`

### PASO 3 — Configurar DNS

Cambiar los registros DNS de `artesbuhomanagement.com` en el proveedor del dominio (probablemente Hostinger):

| Subdominio | Tipo | Apunta a |
|---|---|---|
| `app.artesbuhomanagement.com` | A | IP nuevo VPS |
| `bella-bestia.artesbuhomanagement.com` | A | IP nuevo VPS |
| `contabilidad.artesbuhomanagement.com` | A | IP nuevo VPS |

⚠️ **El cambio DNS tarda 1-24h en propagarse.** Hacedlo con margen.

### PASO 4 — Desplegar en Coolify

Para cada app:

1. **New Resource → GitHub** (conectar la org `artes-buho-management`).
2. Seleccionar el repo correspondiente.
3. Configurar **Environment Variables** con las nuevas credenciales (ver `03_CREDENCIALES_A_GENERAR.md`).
4. Configurar dominio.
5. Deploy.

#### Específico de cada app:

##### APP_ARTES-BUHO

- **Tipo:** Node.js Express
- **Build command:** `npm install`
- **Start command:** `npm start`
- **Variables:** Ver `.env.example` interno del repo
- **Dominio:** `app.artesbuhomanagement.com` (o el subdominio elegido)

##### APP_ARTES-BUHO_BELLA-BESTIA

- **Tipo:** Docker (lleva Dockerfile)
- **Build:** Automático con Dockerfile
- **Variables:**
  - `GOOGLE_SERVICE_ACCOUNT_BASE64` (Service Account en base64)
  - `SHEETS_SPREADSHEET_ID` (ID de la hoja de Bella Bestia — conservar el existente)
- **Dominio:** `bella-bestia.artesbuhomanagement.com`
- **Volumen persistente:** `/app/data` (uploads, exports)

##### ARTES-BUHO_RAMON

- **Tipo:** Docker (lleva Dockerfile)
- **Build:** Automático
- **Variables (críticas):**
  - `GMAIL_OAUTH_*` (tokens OAuth para `booking@`)
  - `GEMINI_API_KEY`
  - `ANTHROPIC_API_KEY`
  - `TELEGRAM_BOT_TOKEN` (bot NUEVO — el actual es de Rubén)
  - `TELEGRAM_CHAT_ID`
  - `HOLDED_API_KEY`
  - `DATABASE_URL` (PostgreSQL)
- **PostgreSQL:** Coolify puede provisionar uno nuevo. Migrar datos del original con `pg_dump`.

---

## ⚠️ COSAS A NO OLVIDAR

1. **Bot Telegram de RAMON:** El bot actual es propiedad de Rubén Cotón. Hay que crear uno nuevo con `@BotFather`. Los usuarios actuales **tendrán que cambiar de bot manualmente** (no es transparente).

2. **PostgreSQL de RAMON:** Si quieres conservar el histórico de emails clasificados / preferencias del bot, hay que migrar la DB:
   ```bash
   # Desde el VPS antiguo
   pg_dump ramon_db > ramon_backup.sql
   # En el VPS nuevo
   psql ramon_db < ramon_backup.sql
   ```
   Pedir el dump a Rubén Cotón antes del deadline.

3. **Cookies / sesiones activas:** Si APP_ARTES-BUHO emite cookies de dominio `.artesbuhomanagement.com`, esas cookies seguirán funcionando si el dominio es el mismo. Si cambias dominio, los usuarios tendrán que loguearse de nuevo.

4. **Coolify backups:** Configurar backups automáticos del nuevo VPS desde el primer día.

5. **Service Account Google:** No reutilizar el del VPS antiguo (es de Rubén). Crear uno nuevo y compartir las hojas con su email.

---

## 📅 CRONOGRAMA SUGERIDO

| Día | Acción |
|---|---|
| 1 | Contratar VPS Hostinger + instalar Coolify |
| 2-3 | Generar todas las credenciales (ver `03_CREDENCIALES_A_GENERAR.md`) |
| 4 | Cambiar DNS (propagación 24h) |
| 5 | Desplegar `APP_ARTES-BUHO` y verificar |
| 6 | Desplegar `APP_ARTES-BUHO_BELLA-BESTIA` y verificar contabilidad |
| 7-8 | Migrar DB PostgreSQL de RAMON + desplegar |
| 9 | Pruebas integrales con la cuenta `booking@` |
| 10-14 | Margen para resolver bugs / Apps Script clasp push |
| **15 (5 jun)** | **Rubén apaga su VPS** — todo debe estar funcionando solo |

---

## ❓ CONSULTAS URGENTES

Si algo no arranca durante la migración:

- **Rubén Cotón:** manager@rubencoton.com / +34 613 00 93 36
- Horario de respuesta: días laborables 10:00-19:00
