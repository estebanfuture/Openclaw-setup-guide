03. Instalación de OpenClaw
Objetivo de esta fase

En esta fase se instala OpenClaw dentro de Ubuntu y se deja operativo el entorno base.

El objetivo no es solo instalar el programa, sino dejar funcionando:

el comando openclaw
el gateway local
el dashboard web
la base para conectar modelos y canales
Dónde se instala OpenClaw en esta guía

En esta guía, OpenClaw se instala dentro de Ubuntu, que corre como máquina virtual en VirtualBox.

Esto significa que:

Windows actúa como sistema host
Ubuntu actúa como sistema invitado
OpenClaw vive dentro de Ubuntu
cuando Ubuntu se apaga, OpenClaw deja de estar disponible
Preparación previa

Antes de instalar OpenClaw, conviene actualizar Ubuntu y asegurarse de que el sistema tiene las herramientas básicas necesarias.

Abrir una terminal en Ubuntu y ejecutar:

sudo apt update
sudo apt full-upgrade -y
Instalar dependencias necesarias

Instalar paquetes básicos del sistema:

sudo apt install -y curl git build-essential cmake python3 dkms linux-headers-$(uname -r)
Qué hace este paso

Este comando instala herramientas necesarias para:

descargar scripts y recursos (curl)
manejar repositorios (git)
compilar componentes (build-essential, cmake)
disponer de Python base (python3)
preparar el sistema para módulos del kernel y componentes de VirtualBox (dkms, linux-headers-$(uname -r))
Reiniciar Ubuntu

Después de instalar dependencias, reiniciar la máquina:

sudo reboot


Instalar OpenClaw

Una vez reiniciado Ubuntu, abrir otra vez la terminal y ejecutar:

curl -fsSL --proto '=https' --tlsv1.2 https://openclaw.ai/install-cli.sh | bash -s -- --no-onboard
Qué hace este paso

Este instalador descarga e instala OpenClaw en el entorno del usuario, sin lanzar todavía el asistente completo de configuración.

Se usa esta ruta porque fue la que resultó más estable durante la instalación validada.

Añadir OpenClaw al PATH

Después de la instalación, añadir la ruta binaria de OpenClaw al entorno del usuario:

echo 'export PATH="$HOME/.openclaw/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
Qué significa esto
echo ... >> ~/.bashrc añade la ruta al archivo de arranque del shell
source ~/.bashrc recarga el archivo en la terminal actual

Sin este paso, el comando openclaw puede no ser reconocido correctamente en nuevas terminales.

Verificar la instalación

Comprobar que el binario ya funciona:

openclaw --version

Si devuelve una versión, la instalación base quedó correcta.

Lanzar el onboarding inicial

Una vez instalado OpenClaw, se ejecuta el asistente de configuración inicial:

openclaw onboard --install-daemon
Qué hace el onboarding

Este asistente guía la configuración inicial de OpenClaw, incluyendo:

proveedor de modelo
credenciales
gateway local
canales opcionales
primeras comprobaciones de salud


Recomendación durante esta fase

En la instalación validada, se siguió esta lógica:

usar Gemini como modelo rápido principal
usar Telegram como canal opcional
dejar integraciones secundarias para más adelante
completar primero una instalación base funcional
Comprobar estado del gateway

Una vez terminado el onboarding, comprobar que el gateway está activo:

openclaw gateway status

Y para una comprobación más estricta:

openclaw gateway status --require-rpc
Comprobar el entorno general

Ejecutar también:

openclaw status
openclaw doctor
Qué significa cada comando
openclaw status: muestra el estado general del sistema
openclaw doctor: revisa problemas comunes y ayuda a detectar errores de configuración
openclaw gateway status --require-rpc: confirma que el gateway está vivo y que la comunicación interna responde correctamente
Abrir el dashboard

Para abrir la interfaz web local de OpenClaw:

openclaw dashboard

Esto abre el panel web en el navegador de Ubuntu.

Desde ahí se pueden:

crear sesiones
chatear
cambiar de modelo
revisar respuestas
probar si el sistema responde correctamente
Primera prueba funcional

Una prueba simple consiste en abrir el dashboard y enviar un mensaje corto como:

Hola, responde solo con OK
Hola, responde normal en español

El objetivo de esta fase es confirmar que:

el gateway está levantado
el modelo responde
el dashboard carga bien
el sistema base está operativo


Qué debe quedar listo al terminar

Al final de esta fase, el sistema debe tener:

OpenClaw instalado en Ubuntu
el comando openclaw funcionando
el gateway activo
el dashboard accesible
el entorno listo para configurar modelos y canales
Errores frecuentes en esta fase
El comando openclaw no se reconoce

Posibles causas:

no se añadió la ruta al PATH
no se recargó ~/.bashrc
se abrió una terminal nueva sin recargar el entorno

Solución:

echo 'export PATH="$HOME/.openclaw/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
El gateway no arranca

Posibles causas:

instalación incompleta
problema temporal de entorno
proceso anterior mal cerrado

Solución inicial:

openclaw doctor
openclaw gateway status
El dashboard no abre

Posibles causas:

el gateway no está corriendo
el comando se ejecutó antes de completar la instalación
el navegador no llegó a abrir la URL local

Solución inicial:

openclaw gateway status --require-rpc
openclaw dashboard
Resultado esperado

Al terminar esta fase, OpenClaw debe estar instalado y funcionando dentro de Ubuntu, listo para continuar con la configuración de modelos y autenticación.