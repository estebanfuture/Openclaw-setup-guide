# 02. Instalación de VirtualBox y Ubuntu

## Objetivo de esta fase

En esta fase se prepara la base del entorno.

El objetivo es crear una máquina virtual Ubuntu dentro de Windows para ejecutar OpenClaw de forma separada del sistema host.

## Por qué se usa una máquina virtual

Se usa Ubuntu en VirtualBox para:

- aislar OpenClaw del sistema principal
- mantener una configuración más limpia
- evitar mezclar herramientas del host y del guest
- poder documentar una arquitectura más reproducible

## Componentes de esta fase

En esta fase se instala y prepara:

- VirtualBox en Windows
- Ubuntu 24.04 como sistema invitado
- una VM con recursos suficientes
- una red funcional entre Ubuntu y Windows host

## Paso 1. Instalar VirtualBox

Descargar e instalar VirtualBox en Windows.

Recomendaciones:

- usar la versión estable más reciente compatible
- aceptar los componentes de red que instala VirtualBox
- reiniciar Windows si el instalador lo recomienda

## Paso 2. Descargar Ubuntu

Descargar una imagen ISO de Ubuntu 24.04 Desktop.

Recomendación:

- usar la edición Desktop si se quiere una experiencia más cómoda
- guardar la ISO en una ubicación fácil de recordar

## Paso 3. Crear la máquina virtual

Crear una nueva máquina virtual en VirtualBox.

Valores recomendados base:

- Nombre: `Ubuntu-OpenClaw`
- Tipo: Linux
- Versión: Ubuntu (64-bit)

## Paso 4. Asignar memoria RAM

Recomendación base:

- mínimo usable: 8 GB
- recomendado: 12 GB o más si el host lo permite

Importante:

si el host va justo de RAM y además se quiere usar Ollama local pesado, asignar demasiada RAM a la VM puede perjudicar al sistema completo.

## Paso 5. Crear disco virtual

Recomendación:

- tipo VDI
- almacenamiento dinámico
- tamaño recomendado: 40 GB o más

## Paso 6. Configurar procesadores

Asignar varios núcleos si el equipo lo permite.

Recomendación:

- no usar todos los núcleos del host
- dejar margen para Windows y otras herramientas

## Paso 7. Montar la ISO de Ubuntu

Antes del primer arranque:

- abrir configuración de la VM
- ir a almacenamiento
- montar la ISO de Ubuntu en la unidad óptica virtual

## Paso 8. Revisar la red de la VM

La red debe permitir que Ubuntu vea al host Windows.

Configuración típica usada en esta guía:

- adaptador NAT

Esto permite que Ubuntu use una ruta especial para comunicarse con el host.

## Paso 9. Iniciar la instalación de Ubuntu

Arrancar la VM y seguir el instalador de Ubuntu.

Opciones recomendadas:

- instalación normal
- zona horaria correcta
- usuario fácil de recordar
- contraseña segura
- permitir instalación en el disco virtual creado

## Paso 10. Primer arranque y comprobaciones

Tras terminar la instalación:

- iniciar sesión en Ubuntu
- abrir una terminal
- comprobar que el sistema responde correctamente

Comandos útiles de verificación:

```bash
uname -a
lsb_release -a
pwd
```

## Paso 11. Actualizar Ubuntu

Actualizar paquetes básicos del sistema:

```bash
sudo apt update && sudo apt upgrade -y
```

## Paso 12. Verificar acceso a Internet

Comprobar que la VM tiene conectividad:

```bash
ping -c 4 google.com
```

Si no responde:

- revisar red de VirtualBox
- revisar que el adaptador esté activo
- reiniciar la VM

## Qué debe quedar listo al terminar

Al final de esta fase, el sistema debe tener:

- VirtualBox instalado en Windows
- una VM Ubuntu funcional
- acceso a red desde Ubuntu
- terminal operativa
- base lista para instalar OpenClaw

## Errores frecuentes en esta fase

### La VM va demasiado lenta

Posibles causas:

- poca RAM asignada
- pocos núcleos
- disco lento
- demasiadas aplicaciones abiertas en Windows

### Ubuntu no tiene Internet

Posibles causas:

- adaptador mal configurado
- NAT deshabilitado
- problema temporal de red del host

### La VM consume demasiada memoria

Solución:

- reducir RAM asignada si el host va justo
- cerrar programas pesados en Windows
- no combinar VM pesada con modelos locales exigentes al mismo tiempo

## Resultado esperado

Al terminar esta fase, el lector tendrá una VM Ubuntu funcional y lista para continuar con la instalación de OpenClaw.