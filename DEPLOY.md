# Publicar NAUTI para el jurado

Render Web Service ejecuta Express y sirve la aplicación compilada y /api en el mismo enlace HTTPS. render.yaml solicita exclusivamente plan free. No requiere claves NASA ni Gemini para las funciones actuales. No contratar un plan pago ni agregar tarjeta para esta prueba.

## Antes de crear el servicio

Confirmar las condiciones vigentes en https://render.com/docs/free y en el panel. Esas páginas fueron bloqueadas por el proxy del entorno de preparación, por lo que no se pudieron verificar sus condiciones actuales. Como referencia histórica, Free se suspende tras 15 minutos sin tráfico, dispone de 750 horas mensuales por espacio de trabajo y usa disco efímero; estas cifras no deben tratarse como límites actuales confirmados. Un servicio único durante 30 días consume como máximo 720 horas si está siempre encendido. Esto no garantiza disponibilidad ni un SLA: pueden aplicar otros límites de tráfico, compilación o políticas del proveedor. Verificar también que no haya caducidad de prueba ni tarjeta obligatoria. Si la interfaz requiere pagar, detenerse.

El primer acceso después de una suspensión puede tardar aproximadamente un minuto o más. Abrir la página antes de la evaluación. La biblioteca inicial se restaura con el repositorio; las ediciones a data/nasa_scientific_library.json se pueden perder al reiniciar o desplegar. Las rutas de edición de biblioteca no tienen autenticación: no cargar datos privados en esta demostración. El contenido y narrativa no se modificaron.

## Pasos en Render

1. Entrar en https://dashboard.render.com/register e iniciar sesión con GitHub.
2. Elegir New + → Web Service → Git Provider → GitHub. Autorizar solo palomadelvalleworking-dev/NAUTIMISSION si se solicita.
3. Seleccionar el repositorio, rama main; Root Directory vacío; Language/Runtime Node.
4. Build Command: npm install --prefix /tmp/nauti-tools --no-package-lock --no-audit --no-fund bun@1.4.4 && /tmp/nauti-tools/node_modules/.bin/bun install --frozen-lockfile && npm run build
5. Start Command: npm start. Seleccionar instancia Free.
6. En Environment agregar NODE_VERSION=24.22.1 y NODE_ENV=production. En Advanced configurar Health Check Path=/api/health.
7. Revisar que el costo sea US$0 y confirmar los límites anteriores. Pulsar Deploy Web Service (o Create Web Service, según la interfaz).
8. Esperar estado Live y copiar la dirección HTTPS asignada por Render. Compartir esa dirección en el chat para verificarla. No compartir secretos ni credenciales.

Alternativa cuando render.yaml esté en GitHub: New + → Blueprint → seleccionar repositorio → comprobar que solo crea el servicio Free → Deploy Blueprint. Los nombres de botones pueden variar con la versión del panel.

## Verificación pendiente tras publicar

Abrir el enlace en incógnito sin iniciar sesión. Comprobar el menú, entrar a Efecto Dominó, avanzar la misión y probar decisiones. Comprobar también biblioteca, /api/health, /api/library/records y búsquedas /api/nasa/search?q=water&source=NASA_NTRS&size=1 y /api/nasa/search?q=water&source=NASA_OSDR&size=1. Un 200 con hits vacíos no prueba que existan resultados: inspeccionar ok, total y hits. La publicación no se considera validada hasta comprobar estos pasos en la URL pública.

No usar npm run preview como servidor final: solo sirve Vite y no sustituye las rutas Express.
