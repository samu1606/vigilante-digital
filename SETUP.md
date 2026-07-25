# 🛡️ Vigilante Digital — Setup Guide

> Suite de MonitorPro + CambioTrack + WhatsApp Alerts

---

## Arquitectura

```
MonitorPro (Uptime Kuma) ──webhook──▶ n8n ──Evolution API──▶ WhatsApp
CambioTrack (Changedetection) ──webhook──▶ n8n ──Evolution API──▶ WhatsApp
                                           │
                               Landing: vigilante.elingesmartgrids.cloud
```

---

## Paso 1: Obtener API Key de Evolution

1. Entrar a http://148.230.90.171:8096/manager
2. Ir a la instancia `flujodigital`
3. Copiar la **API Key** (Global API Key o Instance Key)
4. Guardarla — la necesitamos para los workflows

## Paso 2: Importar workflows en n8n

1. Entrar a http://148.230.90.171:5678
2. Click en **Import Workflow** (o arrastrar el JSON)
3. Importar `001-monitorpro-whatsapp.json`
4. Importar `002-cambiotrack-whatsapp.json`

En cada workflow, reemplazar:
- `{{EVO_API_KEY}}` → la API key de Evolution
- `{{PHONE_NUMBER}}` → número de WhatsApp (ej: `573173662752`)

## Paso 3: Activar webhooks en n8n

1. Abrir workflow 001 → click ⚡ **Activate** (webhook production URL)
2. Copiar la URL del webhook (ej: `http://148.230.90.171:5678/webhook/monitorpro-alert`)
3. Repetir para workflow 002

## Paso 4: Conectar MonitorPro

1. Entrar a MonitorPro (Uptime Kuma)
2. Settings → Notifications → **Add Notification**
3. Tipo: **Webhook**
4. URL: la que copiaste del workflow 001
5. Probar con un monitor

## Paso 5: Conectar CambioTrack

1. Entrar a CambioTrack (Changedetection.io)
2. Settings → Notifications → **Add Notification**
3. Tipo: **Webhook (JSON)**
4. URL: la del workflow 002
5. Probar con un watch

## Paso 6: Deployar la landing (opcional)

```bash
# Si quieres deployar la landing como servicio en Dokploy
# Compose, Git → este mismo repo, path: landing/
```

O simplemente agregar la landing como página estática en FlujoDigital.
