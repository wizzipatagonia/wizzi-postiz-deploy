# Wizzi Postiz deploy

Configuracion de despliegue de Postiz para el VPS de Wizzi via Coolify.

Este repo contiene solo la config de despliegue, no el codigo fuente de Postiz.

## Contenido

- `docker-compose.yaml` - stack completo de Postiz, Temporal, Postgres y Redis.
- `docker-patches/instagram.provider.js` - parche de Instagram. No borrar: sin esto Instagram deja de funcionar.
- `.env.example` - plantilla de variables. Los valores reales van en Coolify, nunca en Git.

## Despliegue

1. En Coolify: nuevo recurso Docker Compose apuntando a este repo.
2. Cargar las variables reales en la UI de Coolify: `JWT_SECRET`, `FACEBOOK_APP_ID`, `FACEBOOK_APP_SECRET`, `INSTAGRAM_APP_ID`, `INSTAGRAM_APP_SECRET`, `POSTIZ_PUBLIC_URL`.
3. Asignar el dominio `postwiz.wizzi.com.ar` al servicio `postiz` con HTTPS por Traefik.
4. Mantener `DISABLE_REGISTRATION=true` salvo durante una ventana corta de alta de usuarios.

## Seguridad

- Nunca commitear el `.env` real.
- Si un secreto se filtra, rotarlo en Meta/Coolify y regenerar `JWT_SECRET`.
- Los puertos administrativos de Temporal no deben exponerse a internet.
- Revisar que el puerto host `4007` no quede expuesto directamente si Traefik ya enruta el servicio.
- No usar `NOT_SECURED=true` en produccion.

## Configuracion en Meta

Para que Facebook/Instagram conecten, en la app de Meta:

- App Domain: `postwiz.wizzi.com.ar`.
- Redirect URI Facebook: `https://postwiz.wizzi.com.ar/integrations/social/facebook`.
- Redirect URI Instagram standalone: `https://postwiz.wizzi.com.ar/integrations/social/instagram-standalone`.
- URL de politica de privacidad cargada.
- URL de terminos cargada.
- URL o instrucciones de eliminacion de datos cargadas.
- Site URL en HTTPS: `https://postwiz.wizzi.com.ar/`.
- App icon cargado.
- Permisos de Pages/Instagram agregados y enviados a revision si la app pasa a produccion.
