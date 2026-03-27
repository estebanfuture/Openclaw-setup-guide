04. Configuración de Gemini
Objetivo de esta fase

En esta fase se configura Gemini como modelo rápido principal para OpenClaw.

El objetivo es dejar una autenticación funcional y validada, de forma que OpenClaw pueda usar Gemini desde Ubuntu sin depender de configuraciones ambiguas o temporales.

Qué se usó en esta guía

Durante la instalación validada se usó:

Google Gemini API
una API key creada en Google AI Studio
OpenClaw ejecutándose dentro de Ubuntu
autenticación mediante variable de entorno
Por qué se eligió Gemini en esta fase

Gemini se usó como modelo principal para el entorno base por varias razones:

respuesta rápida
integración sencilla con OpenClaw
buena experiencia para dashboard y Telegram
configuración más estable que otras rutas improvisadas
Requisito previo

Antes de configurar Gemini en OpenClaw, hace falta una API key válida.

Crear la API key de Gemini

La clave se obtiene desde Google AI Studio.

Pasos generales:

entrar en Google AI Studio
iniciar sesión con una cuenta de Google
abrir la sección de API keys
crear una clave nueva
copiar la clave
no compartirla
no subirla al repositorio
Advertencia importante

La API key es un secreto.

No se debe:

enviar en capturas públicas
escribir en chats
subir a GitHub
dejar en documentos públicos
guardar en archivos que luego se publiquen

Guardar la clave en Ubuntu

Una vez creada la API key, la forma más estable de usarla con OpenClaw fue guardarla en un archivo .env dentro del directorio de OpenClaw.

En Ubuntu, se creó este archivo:

~/.openclaw/.env

Y dentro se añadió una línea como esta:

GEMINI_API_KEY=TU_CLAVE_REAL_AQUI
Qué significa este paso

Esto permite que OpenClaw y las pruebas manuales lean la clave desde una ubicación estable.

La idea no es escribir la clave una y otra vez, sino dejarla disponible en el entorno local de forma controlada.

Cómo editar el archivo

En Ubuntu se puede hacer así:

mkdir -p ~/.openclaw
nano ~/.openclaw/.env

Dentro del archivo se escribe:

GEMINI_API_KEY=TU_CLAVE_REAL_AQUI

Luego se guarda y se sale del editor.

Cargar la variable en la terminal actual

Para que la terminal actual lea la clave, se usó:

set -a
source ~/.openclaw/.env
set +a
Qué hace esto
source ~/.openclaw/.env carga el contenido del archivo en la shell actual
set -a hace que las variables cargadas se exporten
set +a desactiva ese comportamiento al terminar
Comprobar que la variable quedó cargada

Se puede verificar con:

echo "$GEMINI_API_KEY"
echo ${#GEMINI_API_KEY}

La primera línea intenta mostrar el valor.
La segunda muestra la longitud de la clave.

Si la longitud es mayor que 0, entonces la variable está cargada.


Validar la API key fuera de OpenClaw

Antes de dar por buena la configuración dentro de OpenClaw, se hizo una prueba directa contra la API de Gemini.

Esto fue importante porque permitió separar dos problemas distintos:

si la clave de Google era inválida
o si el problema estaba dentro de OpenClaw
Prueba directa con curl

En Ubuntu, con la variable ya cargada en la terminal, se usó una llamada directa a Gemini.

Comando usado:

curl "https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash:generateContent" -H "x-goog-api-key: $GEMINI_API_KEY" -H "Content-Type: application/json" -X POST -d '{"contents":[{"parts":[{"text":"Reply with OK only"}]}]}'
Qué se esperaba como resultado

Si la clave funcionaba, la API devolvía una respuesta válida del modelo.

En la instalación validada, la respuesta correcta confirmó que:

la clave era válida
la variable estaba bien cargada
Google Gemini respondía correctamente
el problema ya no estaba en la API externa
Qué significaba un error 403

Cuando apareció un error 403 PERMISSION_DENIED, eso indicó que la llamada estaba llegando sin una clave válida cargada en esa terminal.

La causa real fue que la variable GEMINI_API_KEY no estaba cargada en esa shell concreta.

Qué significaba un error 400 con API key not valid

Cuando apareció API key not valid, eso apuntó a otra causa distinta:

clave mal copiada
clave regenerada y no actualizada
clave vieja expuesta y ya no válida
proyecto de Google AI Studio incorrecto
Comprobación adicional del modelo

También se podía consultar el modelo directamente con una llamada simple:

curl "https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash" -H "x-goog-api-key: $GEMINI_API_KEY"

Esto servía para confirmar que la autenticación funcionaba incluso antes de generar contenido.

Conclusión de esta validación

La prueba directa con curl fue clave porque permitió demostrar si el fallo estaba en:

Google
la clave
la variable de entorno
o la configuración de OpenClaw

En esta guía, la validación correcta con curl confirmó que la clave de Gemini era buena y que el problema posterior estaba en la configuración o en la sesión de OpenClaw, no en la API de Google.


Uso de Gemini dentro de OpenClaw

Una vez validada la clave fuera de OpenClaw, el siguiente paso fue asegurarse de que OpenClaw la leyera correctamente.

Para ello, se comprobó el estado de modelos con:

openclaw models status
Qué se esperaba ver

La salida correcta debía indicar que el proveedor de Google estaba usando la clave desde el entorno.

La señal más importante era ver algo equivalente a:

source=env: GEMINI_API_KEY

Eso confirmaba que OpenClaw estaba leyendo la clave desde el archivo .env y no desde una configuración vieja o rota.

Reiniciar el gateway después de cambios en la clave

Cada vez que se cambiaba la clave o el archivo .env, fue necesario reiniciar el gateway:

openclaw gateway restart

Después de eso, se comprobaba:

openclaw gateway status --require-rpc
openclaw models status
openclaw doctor
Qué significaba cada comprobación
openclaw gateway restart: reinicia el servicio local de OpenClaw
openclaw gateway status --require-rpc: confirma que el gateway está activo y que la comunicación interna responde
openclaw models status: muestra qué modelo y qué fuente de autenticación está usando OpenClaw
openclaw doctor: revisa problemas comunes del entorno
Problema real que apareció durante la configuración

Aunque la clave de Gemini funcionaba correctamente con curl, OpenClaw seguía fallando en algunas sesiones antiguas.

La causa real fue que ciertas sesiones o perfiles de autenticación arrastraban configuración vieja.

Esto significa que el problema no estaba en Gemini, sino en el estado interno de OpenClaw.

Cómo se resolvió

La solución práctica fue:

validar primero la clave con curl
asegurarse de que GEMINI_API_KEY estaba bien cargada
reiniciar el gateway
abrir una sesión nueva y limpia en OpenClaw
cambiar explícitamente al modelo correcto si era necesario
Qué modelo se usó como principal

En la instalación validada, el modelo rápido principal quedó como:

google/gemini-2.5-flash

Se usó porque ofrecía una experiencia más rápida y más cómoda para:

dashboard
Telegram
tareas rápidas
preguntas normales
uso general con herramientas
Errores frecuentes en esta fase
La variable existe en el archivo pero no en la terminal

Causa:

el archivo .env existe, pero la shell actual no lo cargó

Solución:

set -a
source ~/.openclaw/.env
set +a
echo ${#GEMINI_API_KEY} devuelve 0

Causa:

la variable no está cargada en esa terminal

Solución:

volver a cargar .env
comprobar que el nombre de la variable es correcto
revisar que no haya errores al escribir la línea
OpenClaw sigue fallando aunque curl funciona

Causa probable:

sesión vieja
gateway sin reiniciar
perfil interno arrastrando configuración anterior

Solución:

openclaw gateway restart
openclaw models status
openclaw doctor

Y después abrir una sesión nueva en el dashboard.

Se expuso la API key en una captura o chat

Causa:

mala práctica de seguridad durante la configuración

Solución:

revocar la clave expuesta
generar una nueva en Google AI Studio
actualizar ~/.openclaw/.env
reiniciar el gateway
Resultado esperado

Al terminar esta fase, Gemini debe quedar configurado y validado como modelo rápido principal de OpenClaw, con autenticación funcional, variable de entorno correcta y capacidad de responder tanto en el dashboard como en sesiones limpias del sistema.