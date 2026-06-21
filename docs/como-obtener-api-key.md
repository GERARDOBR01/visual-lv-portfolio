# Cómo obtener tu API key (guía para colegas no técnicos)

El asistente necesita una "llave" (API key) para conectarse a un modelo de IA. Es gratis y toma menos de 5 minutos. Recomendamos **GitHub Models** porque es gratuito y no requiere tarjeta de crédito.

## Opción recomendada: GitHub Models (gratis)

1. Si no tienes cuenta de GitHub, crea una en [github.com/signup](https://github.com/signup) (gratis).
2. Ve a [github.com/settings/tokens](https://github.com/settings/tokens).
3. Haz clic en **"Generate new token"** → **"Generate new token (classic)"**.
4. Dale un nombre (ej. "asistente-lv") y no marques ningún permiso adicional — los permisos por defecto bastan para GitHub Models.
5. Genera el token y **cópialo de inmediato** (no podrás verlo de nuevo).
6. Abre el asistente, ve a la pestaña ⚙️ **Config**, pega el token en el campo de API key del proveedor "GitHub Models" y guarda.

## Opción alternativa: Google Gemini (gratis con límites)

1. Ve a [aistudio.google.com/app/apikey](https://aistudio.google.com/app/apikey).
2. Inicia sesión con tu cuenta de Google.
3. Haz clic en **"Create API key"** y cópiala.
4. Pégala en la pestaña ⚙️ Config, proveedor "Google Gemini".

## Opción alternativa: OpenAI (de pago)

1. Ve a [platform.openai.com/api-keys](https://platform.openai.com/api-keys).
2. Crea una cuenta si no tienes, y agrega un método de pago (esta opción no es gratuita).
3. Genera una nueva key y pégala en la pestaña ⚙️ Config, proveedor "OpenAI".

## Notas de seguridad

- Tu API key se guarda **solo en tu navegador** (sesión actual), nunca se envía a ningún servidor de Gerardo ni de terceros más allá del proveedor de IA elegido.
- Si cierras la pestaña o el navegador, deberás volver a ingresarla.
- Nunca compartas tu API key con nadie.
