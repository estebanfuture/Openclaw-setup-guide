# 01. Requisitos

## Objetivo de esta fase

Antes de instalar nada, hay que preparar el entorno correcto.

Esta guía está pensada para montar un sistema híbrido con:

- Windows como sistema principal (host)
- Ubuntu 24.04 en VirtualBox como máquina virtual
- OpenClaw ejecutándose dentro de Ubuntu
- Gemini API como modelo en la nube
- Ollama ejecutándose en Windows host
- conexión entre Ubuntu VM y Ollama del host

## Qué se necesita

### Hardware recomendado

Para reproducir esta guía con comodidad, se recomienda:

- Procesador moderno multinúcleo
- Mínimo 16 GB de RAM
- Recomendado: 32 GB de RAM
- Espacio libre en disco: al menos 50 GB
- GPU NVIDIA opcional para modelos locales
- Recomendado para Ollama local: una GPU con suficiente VRAM y RAM general abundante

### Sistema operativo usado en esta guía

Esta instalación fue validada en un entorno con:

- Windows como sistema host
- Ubuntu 24.04 dentro de VirtualBox

## Software necesario

### En Windows host

- VirtualBox
- VS Code
- Git
- Ollama
- Navegador web
- Cuenta de GitHub

### En Ubuntu VM

- Ubuntu 24.04
- Terminal
- curl
- Node.js (instalado indirectamente por OpenClaw en algunos pasos)
- OpenClaw

## Cuentas necesarias

Para seguir esta guía hacen falta estas cuentas o accesos:

- Cuenta de GitHub
- Cuenta de Google AI Studio o acceso a Gemini API
- Cuenta de Telegram si se quiere integrar el bot
- Acceso a BotFather si se quiere crear un bot de Telegram

## Qué parte corre en cada sitio

### En Windows host

En Windows se ejecuta:

- Ollama
- VS Code
- Git
- GitHub
- el navegador
- el almacenamiento principal del proyecto

### En Ubuntu VM

En Ubuntu se ejecuta:

- OpenClaw
- la configuración del gateway
- el dashboard local
- la conexión hacia Ollama del host
- la configuración del bot y del entorno local

## Requisitos de red y arquitectura

La arquitectura usada en esta guía es:

- Windows = host principal
- Ubuntu = guest en VirtualBox
- Ollama escucha en Windows host
- Ubuntu accede a Ollama por la IP especial del host en VirtualBox

## Advertencias importantes

### No subir secretos al repositorio

No se deben subir nunca a GitHub:

- API keys
- tokens de Telegram
- archivos `.env`
- URLs del dashboard con token
- capturas con credenciales visibles

### No mezclar documentación validada con pruebas fallidas

Este repositorio debe contener:

- pasos correctos
- pasos validados
- soluciones a errores
- pero no pruebas rotas sin contexto

## Qué se validó en esta guía

Durante el proceso se validó:

- instalación de OpenClaw
- configuración funcional de Gemini
- instalación de Ollama en Windows
- conexión entre Ubuntu VM y Ollama host
- uso híbrido entre Gemini y Ollama
- preparación de documentación en GitHub

## Resultado esperado al terminar esta fase

Al terminar esta fase, el lector debe tener claro:

- qué necesita
- dónde se instala cada componente
- qué partes del sistema viven en Windows
- qué partes del sistema viven en Ubuntu
- qué riesgos debe evitar