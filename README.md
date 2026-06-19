# Wizzi · Postiz (deploy)

Configuración de despliegue de [Postiz](https://github.com/gitroomhq/postiz-app) para el VPS de Wizzi, vía **Coolify**.

> Este repo contiene **solo la config de despliegue**, no el código fuente de Postiz.

## Contenido

- `docker-compose.yaml` — stack completo (Postiz v2.21.8 + Temporal + Postgres + Redis).
- `docker-patches/instagram.provider.js` — parche de Instagram (se monta sobre el `dist` de la imagen). **No borrar**: sin esto Instagram deja de funcionar.
- `.env.example` — plantilla de variables. Los valores reales van en Coolify, nunca en Git.

## Despliegue

1. En Coolify: nuevo recurso **Docker Compose** apuntando a este repo.
2. Cargar las variables reales (`JWT_SECRET`, `FACEBOOK_APP_ID`, `FACEBOOK_APP_SECRET`, `POSTIZ_PUBLIC_URL`) en la UI de Coolify.
3. Asignar el dominio `postwiz.wizzi.com.ar` al servicio `postiz` (HTTPS automático por Traefik).

## ⚠️ Seguridad

- **Nunca** commitear el `.env` real (está en `.gitignore`).
- Si un secreto se filtra, rotarlo en el panel de Meta y regenerar `JWT_SECRET`.
- Los puertos administrativos (Temporal UI/gRPC) no se exponen al internet.

## Configuración en Meta

Para que Facebook/Instagram conecten, en la app de Meta (`developers.facebook.com`):
- **App Domain** y **redirect URI** → `https://postwiz.wizzi.com.ar` (`.../integrations/social/facebook` y `.../instagram`).
- **URL de Política de privacidad** cargada (obligatoria).
- En el caso de uso de Páginas, el permiso `pages_read_user_content` debe estar **añadido** (si no, el login de Facebook se rompe).
