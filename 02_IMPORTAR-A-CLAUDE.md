# 🤖 Cómo continuar este trabajo con Claude Code

> Guía para que el siguiente desarrollador siga el trabajo donde Rubén lo dejó, usando **Claude Code** (la herramienta CLI de Anthropic).

---

## 1. Por qué Claude Code

El proyecto se desarrolló íntegramente con **Claude Code**. Si lo continúas con la misma herramienta, tendrás:

- Contexto automático de la arquitectura (Claude lee `CLAUDE.md` de cada proyecto).
- Convenciones de naming respetadas (`MARCA_NOMBRE`).
- Sub-agentes para tareas grandes (búsqueda, audit, despliegue).
- Modelo Opus 4.7 / Sonnet 4.6 / Haiku 4.5.

---

## 2. Instalar Claude Code

### Windows (PowerShell)

```powershell
winget install --id Anthropic.ClaudeCode -e
```

### macOS / Linux

```bash
curl -fsSL https://claude.ai/install.sh | sh
```

### Login

```bash
claude login
```

Inicia sesión con cuenta Anthropic (suscripción Claude Code).

---

## 3. Abrir la entrega en Claude Code

```bash
cd "C:\Users\elrub\Desktop\ARTES-BUHO-MANAGEMENT"
claude
```

> Si la carpeta se ha movido, abre la nueva ubicación.

---

## 4. Prompts iniciales recomendados

Pega estos prompts a Claude **en orden** para arrancar:

### Prompt 1 — Onboarding

```
Soy [TU NOMBRE], voy a continuar el trabajo de Rubén Cotón con los proyectos
de ARTES BÚHO MANAGEMENT. Lee primero el archivo
`00_DOCUMENTACION/00_LEEME_PRIMERO.md` y luego
`00_DOCUMENTACION/01_INFORME_TECNICO_DENSO.md`.

Después dime:
1. Cuántos proyectos hay y cómo están organizados.
2. Cuáles son los 3 más críticos.
3. Qué credenciales hay que generar antes de arrancar nada.
```

### Prompt 2 — Generar credenciales

```
Lee `00_DOCUMENTACION/03_CREDENCIALES_A_GENERAR.md` y dame el orden
recomendado para generar todas las credenciales sin romper dependencias.
```

### Prompt 3 — Migrar despliegues activos

```
Lee `00_DOCUMENTACION/04_DEPLOYS_ACTIVOS.md`. Necesito migrar los 3
deploys activos del VPS de Rubén Cotón a un VPS propio de ARTES BÚHO
antes del 5 de junio de 2026. Hazme un plan paso a paso.
```

### Prompt 4 — Arrancar un proyecto concreto

```
Quiero arrancar en local `repos/01_apps_principales/APP_ARTES-BUHO_BELLA-BESTIA/`.
Lee su README y dame los comandos exactos para correrlo, indicando qué
variables de entorno necesito rellenar en `.env`.
```

---

## 5. Reglas de oro al trabajar con estos repos

1. **NO ejecutar nada en producción sin tener un backup primero.**
2. **NO commitar `.env` ni claves API.** Ya se eliminaron — no volverlos a meter.
3. **Respeta el naming:** todo nuevo proyecto debe seguir `ARTES-BUHO_NOMBRE` o `APP_ARTES-BUHO_NOMBRE`.
4. **Cuenta operativa única:** `booking@artesbuhomanagement.com`. No mezclar con otras.
5. **El hub `ARTES-BUHO_API-GOOGLE` lo invocan casi todas las apps Node.** Antes de cambiar algo ahí, asegúrate de no romper a los demás.
6. **RAMON nunca envía correos**, solo lee y propone borradores. Mantener esa regla.
7. **Idioma del proyecto:** Español (frases cortas, formato visual).

---

## 6. Estructura de la entrega (recordatorio)

```
ARTES-BUHO-MANAGEMENT/
├── 00_DOCUMENTACION/
│   ├── 00_LEEME_PRIMERO.md            ← Empezar aquí
│   ├── 01_INFORME_TECNICO_DENSO.md    ← Detalle de los 41 proyectos
│   ├── 02_IMPORTAR-A-CLAUDE.md        ← Este archivo
│   ├── 03_CREDENCIALES_A_GENERAR.md   ← Lista de claves a generar
│   ├── 04_DEPLOYS_ACTIVOS.md          ← Migración deploys VPS
│   └── WHATSAPP-DAVID.txt             ← Mensaje para David
└── repos/                              41 proyectos organizados
```

---

## 7. Modelos recomendados según tarea

| Tarea | Modelo |
|---|---|
| Búsqueda simple, renombrado, leer un archivo | Haiku 4.5 |
| Crear/editar funciones, bugs con error claro | Sonnet 4.6 |
| Arquitectura, refactor multi-archivo | Opus 4.7 |
| Migración deploy, decisiones críticas | Opus 4.7 (esfuerzo max) |

---

## 8. Contacto urgente

Para dudas durante la transición:

- **Rubén Cotón:** manager@rubencoton.com / +34 613 00 93 36

---

> Esta entrega caduca el **5 de junio de 2026**. Después, los repos GitHub se eliminarán.
