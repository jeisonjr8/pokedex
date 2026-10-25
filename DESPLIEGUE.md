# Despliegue de la aplicación PokeDex

Este documento detalla el proceso de publicación de la aplicación PokeDex en Azure Static Web Apps, incluyendo las configuraciones de seguridad requeridas por Pueblo Paleta Inc.

## 1. Preparación del Repositorio
1. Creé un repositorio público en GitHub llamado `pokedex`.
2. Subí el código fuente de la aplicación proporcionado por PumasLab al repositorio.

## 2. Configuración de Azure Static Web Apps
1. En el Portal de Azure, busqué el servicio **Static Web Apps** y seleccioné "Crear".
2. Configuré los detalles del proyecto:
   - **Suscripción:** Azure for Students.
   - **Grupo de recursos:** Creé un nuevo grupo llamado `pokedex3_group`.
   - **Nombre de la aplicación:** `pokedex3`
   - **Región:** East US 2
3. **Detalles de implementación:** Seleccioné **GitHub** como origen y vinculé mi cuenta.
4. Seleccioné la organización, el repositorio (`pokedex`) y la rama (`main`).
5. **Detalles de la compilación:** Seleccioné los valores predeterminados de HTML/Estático. Configuré la "Ubicación de la aplicación" (App location) en `/` (o la carpeta donde está tu index.html).
6. Hice clic en "Revisar y crear" y luego en "Crear". Esto generó automáticamente un flujo de trabajo de GitHub Actions en mi repositorio para desplegar la app.
7. Pagina pokedex-jz
(https://ashy-stone-02946e60f.3.azurestaticapps.net/).

## 3. Configuración de Encabezados de Seguridad (HTTP Headers)
Para cumplir con las políticas de seguridad y prevenir ciberataques, fue necesario configurar encabezados HTTP. En Azure Static Web Apps, esto se logra creando un archivo de configuración.

1. En la raíz de mi repositorio, creé un archivo llamado `staticwebapp.config.json`.
2. Añadí la siguiente configuración para habilitar CSP, HSTS, X-Frame-Options, entre otros:
<img width="1199" height="309" alt="image" src="https://github.com/user-attachments/assets/441afec9-3c75-4d10-82ee-d38d7f0f04e7" />


```json
{
"navigationFallback": {
"rewrite": "/index.html",
"exclude": ["/assets/*", "*.{js,css,png,jpg,ico,woff,woff2,ttf,eot,svg}"]
},

"globalHeaders": {
"Strict-Transport-Security": "max-age=63072000; includeSubDomains; preload",
"Content-Security-Policy": "default-src 'self'; script-src 'self'; style-src 'self' 'unsafe-inline' https://fonts.googleapis.com; font-src 'self' https://fonts.gstatic.com data:; img-src 'self' data: https://assets.pokemon.com https://raw.githubusercontent.com; connect-src 'self' https://pokeapi.co https://beta.pokeapi.co; object-src 'none'; base-uri 'self'; form-action 'self'; frame-ancestors 'none'; upgrade-insecure-requests",
"X-Content-Type-Options": "nosniff",
"X-Frame-Options": "DENY",
"Referrer-Policy": "strict-origin-when-cross-origin",
"Permissions-Policy": "accelerometer=(), camera=(), geolocation=(), gyroscope=(), magnetometer=(), microphone=(), payment=(), usb=()",
"Cross-Origin-Opener-Policy": "same-origin",
"Cross-Origin-Resource-Policy": "same-origin"
}
}

