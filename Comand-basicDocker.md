01 - Docker I - Fundamentos y Comandos Básicos
¿Qué es Docker?
1. ¿Qué es Docker? - Conceptos Fundamentales
Contenedores vs Máquinas Virtuales
Antes de empezar con comandos, entendamos qué hace especial a Docker.

Verificar que Docker está instalado
docker --version
docker info
¡Perfecto! Ya tenés Docker funcionando.

Concepto clave: ¿Qué es un contenedor?
Un contenedor es como una "caja" que empaqueta todo lo necesario para que una aplicación funcione:

📦 Código de la aplicación
🔧 Dependencias y librerías
⚙️ Configuración del entorno
🗂️ Archivos del sistema necesarios
Diferencias con Máquinas Virtuales
echo "=== MÁQUINAS VIRTUALES ==="
echo "✗ Incluyen SO completo (GB)"
echo "✗ Arranque lento (minutos)"
echo "✗ Consumen muchos recursos"
echo ""
echo "=== CONTENEDORES DOCKER ==="
echo "✅ Comparten el kernel del host"
echo "✅ Arranque ultra-rápido (segundos)"
echo "✅ Ligeros (MB en lugar de GB)"
echo "✅ Mayor densidad por servidor"
Tu primer contenedor
Vamos a ejecutar tu primer contenedor con una aplicación web simple:

docker run hello-world
¡Felicitaciones! Acabás de ejecutar tu primer contenedor Docker.

¿Qué pasó internamente?
echo "🔍 Lo que hizo Docker:"
echo "1. 📥 Buscó la imagen 'hello-world' localmente"
echo "2. 🌐 Como no la encontró, la descargó de Docker Hub"
echo "3. 🚀 Creó un contenedor desde esa imagen"
echo "4. ▶️  Ejecutó el contenedor"
echo "5. 📄 Mostró el mensaje"
echo "6. 🛑 El contenedor terminó automáticamente"
Verificar qué contenedores corrieron
docker ps -a
Podés ver tu contenedor hello-world que terminó exitosamente.

🎯 ¡Acabás de ejecutar tu primera aplicación en contenedor! En el próximo paso vas a aprender los comandos fundamentales.

///////////////////////////////////////////////////////////////////////////////////////////////

2. Primeros Comandos Docker
Ahora vas a aprender los comandos más importantes de Docker que todo desarrollador debe conocer.

docker run - Crear y ejecutar contenedores
Ejecutar un servidor web simple
docker run -d -p 8080:80 nginx
¿Qué significan estos parámetros?

-d = detached (ejecuta en segundo plano)
-p 8080:80 = mapea puerto 8080 del host al puerto 80 del contenedor
nginx = imagen del servidor web Nginx
Verificar que está corriendo
docker ps
¡Tu servidor web ya está funcionando!

Probar el servidor web
curl http://localhost:8080
También podés acceder desde el navegador en la pestaña "Traffic" → puerto 8080.

docker ps - Ver contenedores activos
echo "📋 Contenedores ejecutándose:"
docker ps
echo "📋 Todos los contenedores (activos + detenidos):"
docker ps -a
docker images - Ver imágenes locales
echo "📦 Imágenes descargadas en este sistema:"
docker images
Podés ver las imágenes nginx y hello-world que ya descargaste.

docker stop - Detener contenedores
Primero obtené el ID o nombre del contenedor nginx:

docker ps
Luego detenelo (reemplazá CONTAINER_ID con el ID real):

NGINX_CONTAINER=$(docker ps -q --filter ancestor=nginx)
echo "Deteniendo contenedor nginx: $NGINX_CONTAINER"
docker stop $NGINX_CONTAINER
Verificar que se detuvo
docker ps
El contenedor nginx ya no aparece en la lista de activos.

docker start - Reiniciar contenedores
echo "Reiniciando el contenedor nginx..."
docker start $NGINX_CONTAINER
docker ps
¡El servidor está funcionando nuevamente!

Resumen de comandos básicos
echo "🐳 === COMANDOS DOCKER BÁSICOS ==="
echo ""
echo "docker run <imagen>     # Crear y ejecutar contenedor"
echo "docker ps              # Ver contenedores activos"
echo "docker ps -a           # Ver todos los contenedores"
echo "docker images          # Ver imágenes locales"
echo "docker stop <id>       # Detener contenedor"
echo "docker start <id>      # Reiniciar contenedor"
echo ""
echo "🎯 ¡Ya conocés los comandos fundamentales!"
En el próximo paso vas a aprender a ejecutar contenedores interactivos donde podés trabajar directamente dentro del contenedor.

//////////////////////////////////////////////////////////////////////////////////////
3. Ejecutar Contenedores Interactivos
Los contenedores no solo sirven para ejecutar aplicaciones, sino que también podés usarlos como entornos de desarrollo temporales.

Modo interactivo con Ubuntu
Vamos a crear un contenedor Ubuntu donde podés ejecutar comandos interactivamente:

docker run -it ubuntu bash
¿Qué significan estos parámetros?

-i = interactive (mantiene la entrada estándar abierta)
-t = tty (asigna una pseudo-terminal)
ubuntu = imagen de Ubuntu
bash = comando a ejecutar (shell de Bash)
Explorar el contenedor desde adentro
Ahora estás dentro del contenedor Ubuntu. Probá estos comandos:

whoami
cat /etc/os-release
ls /
apt update && apt install -y curl
curl -s https://httpbin.org/ip
Salir del contenedor
Para salir del contenedor interactivo:

exit
¡Ya estás de vuelta en el sistema host!

Diferencias entre el host y el contenedor
Compará la información del sistema:

echo "=== EN EL HOST ==="
cat /etc/os-release | head -3
echo "=== EJECUTANDO EN CONTENEDOR TEMPORAL ==="
docker run ubuntu cat /etc/os-release | head -3
Contenedores temporales vs persistentes
Contenedor temporal (se destruye al salir)
docker run -it --rm ubuntu bash -c "
echo 'Creando archivo temporal...'
echo 'Hola Docker!' > /tmp/archivo.txt
cat /tmp/archivo.txt
echo 'Al salir, este archivo se perderá'
"
Verificar que el contenedor se eliminó
docker ps -a | grep ubuntu
No queda ningún contenedor porque usamos --rm .

Caso práctico: Probar diferentes versiones de Python
Sin instalar Python en tu sistema, podés probar diferentes versiones:

Python 3.9
docker run python:3.9 python --version
Python 3.12
docker run python:3.12 python --version
Ejecutar código Python temporalmente
docker run python:3.12 python -c "
import sys
print(f'Python {sys.version}')
print('¡Hola desde contenedor Python!')
print(f'Ejecutándose en: {sys.platform}')
"
Resumen: Modos de ejecución
echo "🐳 === MODOS DE EJECUCIÓN DOCKER ==="
echo ""
echo "docker run imagen               # Ejecutar y terminar"
echo "docker run -d imagen            # Ejecutar en segundo plano"  
echo "docker run -it imagen bash      # Modo interactivo"
echo "docker run --rm imagen          # Eliminar al terminar"
echo "docker run -p 8080:80 imagen    # Mapear puertos"
echo ""
echo "🎯 ¡Ya sabés crear entornos temporales!"
En el próximo paso vas a aprender a gestionar el ciclo de vida completo de los contenedores.

///////////////////////////////////////////////////////////////////////////////

4. Gestionar Contenedores
Ahora vas a aprender el ciclo de vida completo de los contenedores: crear, monitorear, parar y eliminar.

Crear múltiples contenedores para práctica
Vamos a crear varios contenedores para practicar su gestión:

echo "🚀 Creando múltiples contenedores..."
docker run -d --name web1 nginx
docker run -d --name web2 nginx  
docker run -d --name db1 postgres:13 -e POSTGRES_PASSWORD=mi_password
docker ps - Inspeccionar contenedores activos
echo "📋 Contenedores ejecutándose:"
docker ps
echo "📋 Información resumida:"
docker ps --format "table {{.Names}}\t{{.Image}}\t{{.Status}}\t{{.Ports}}"
docker logs - Ver logs de contenedores
Logs del servidor web
echo "📄 Logs del contenedor web1:"
docker logs web1
Logs de la base de datos
echo "📄 Logs del contenedor db1:"
docker logs db1
Seguir logs en tiempo real
echo "📺 Siguiendo logs de nginx (presiona Ctrl+C para parar):"
timeout 10 docker logs -f web1 || echo "✅ Timeout completado"
docker exec - Ejecutar comandos en contenedores corriendo
Acceder al contenedor nginx
echo "🔧 Ejecutando comandos dentro del contenedor web1:"
docker exec web1 nginx -v
Modo interactivo dentro del contenedor
echo "🖥️ Accediendo al shell del contenedor:"
docker exec -it web1 bash -c "
echo 'Estoy dentro del contenedor nginx'
ls /etc/nginx/
cat /etc/os-release | head -3
exit
"
docker inspect - Información detallada
echo "🔍 Información detallada del contenedor web1:"
docker inspect web1 --format '{{json .State}}' | python3 -m json.tool
echo "🌐 Dirección IP del contenedor:"
docker inspect web1 --format '{{.NetworkSettings.IPAddress}}'
docker stats - Monitoreo de recursos
echo "📊 Uso de recursos de los contenedores (5 segundos):"
timeout 5 docker stats --no-stream
Gestión del ciclo de vida
Pausar y reanudar contenedores
echo "⏸️ Pausando contenedor web2:"
docker pause web2
docker ps --format "table {{.Names}}\t{{.Status}}"
echo "▶️ Reanudando contenedor web2:"
docker unpause web2
docker ps --format "table {{.Names}}\t{{.Status}}"
Detener contenedores
echo "🛑 Deteniendo contenedores:"
docker stop web1 web2 db1
echo "📋 Estado después de detener:"
docker ps -a --format "table {{.Names}}\t{{.Status}}"
Reiniciar contenedores
echo "🔄 Reiniciando web1:"
docker start web1
docker ps --format "table {{.Names}}\t{{.Status}}"
Eliminar contenedores
Eliminar contenedores detenidos
echo "🗑️ Eliminando contenedores detenidos:"
docker rm web2 db1
Eliminar contenedor activo (forzado)
echo "💥 Eliminando contenedor activo (forzado):"
docker rm -f web1
Verificar limpieza
echo "📋 Contenedores restantes:"
docker ps -a
Comandos de limpieza útiles
echo "🧹 === COMANDOS DE LIMPIEZA ==="
echo ""
echo "# Eliminar todos los contenedores detenidos:"
echo "docker container prune"
echo ""
echo "# Eliminar contenedores, redes e imágenes no usadas:"
echo "docker system prune"
echo ""
echo "# Eliminar TODO (¡cuidado!):"
echo "docker system prune -a"
Resumen de gestión
echo "🐳 === GESTIÓN DE CONTENEDORES ==="
echo ""
echo "docker ps                    # Ver activos"
echo "docker ps -a                 # Ver todos"
echo "docker logs <nombre>         # Ver logs"
echo "docker exec -it <nombre> sh  # Acceder al shell"
echo "docker inspect <nombre>      # Información detallada"
echo "docker stats                 # Monitoreo de recursos"
echo "docker pause/unpause <nombre># Pausar/reanudar"
echo "docker stop <nombre>         # Detener"
echo "docker rm <nombre>           # Eliminar"
echo "docker rm -f <nombre>        # Eliminar forzado"
echo ""
echo "🎯 ¡Ya podés gestionar contenedores como un pro!"
En el próximo paso vas a aprender a trabajar con imágenes Docker.

///////////////////////////////////////////////////////////////////////////////

5. Trabajar con Imágenes Docker
Las imágenes son las "plantillas" que se usan para crear contenedores. En este paso vas a aprender a buscar, descargar y gestionar imágenes.

docker images - Ver imágenes locales
echo "📦 Imágenes disponibles localmente:"
docker images
echo "📊 Espacio usado por las imágenes:"
docker images --format "table {{.Repository}}\t{{.Tag}}\t{{.Size}}"
docker search - Buscar imágenes en Docker Hub
echo "🔍 Buscando imágenes de Node.js:"
docker search node --limit 5
echo "🔍 Buscando bases de datos:"
docker search mysql --limit 3
docker pull - Descargar imágenes
Descargar imagen específica
echo "📥 Descargando Node.js versión 18:"
docker pull node:18-alpine
Descargar múltiples versiones
echo "📥 Descargando diferentes versiones de Python:"
docker pull python:3.9-slim
docker pull python:3.11-slim
Verificar las nuevas imágenes
echo "📦 Imágenes después de las descargas:"
docker images --format "table {{.Repository}}\t{{.Tag}}\t{{.Size}}"
Entender las etiquetas (tags)
echo "🏷️ === ETIQUETAS DE IMÁGENES ==="
echo ""
echo "node:18          # Versión específica"
echo "node:18-alpine   # Versión + distribución ligera"
echo "node:latest      # Última versión (por defecto)"
echo "python:3.9-slim  # Versión + variante slim"
echo ""
Probar diferentes imágenes
Comparar tamaños: Ubuntu vs Alpine
echo "📏 Comparando tamaños de distribuciones:"
docker pull alpine:latest
docker images --format "table {{.Repository}}\t{{.Tag}}\t{{.Size}}" | grep -E "(alpine|ubuntu)"
Probar Alpine (distribución ultra-ligera)
echo "🏔️ Probando Alpine Linux:"
docker run --rm alpine sh -c "
echo 'Hola desde Alpine!'
cat /etc/os-release | head -3
du -sh / 2>/dev/null | head -1
"
docker history - Ver capas de una imagen
echo "📚 Capas de la imagen nginx:"
docker history nginx --format "table {{.CreatedBy}}\t{{.Size}}"
Casos prácticos con diferentes imágenes
Servidor web ligero con Alpine
echo "🚀 Ejecutando nginx-alpine (más ligero):"
docker run -d --name nginx-alpine -p 8081:80 nginx:alpine
Probar aplicación Node.js
echo "🟢 Ejecutando aplicación Node.js simple:"
docker run --rm node:18-alpine node -e "
console.log('¡Hola desde Node.js!');
console.log('Versión:', process.version);
console.log('Plataforma:', process.platform);
"
Base de datos temporal
echo "🗄️ Ejecutando base de datos Redis temporal:"
docker run -d --name redis-temp redis:alpine
echo "🔌 Probando conexión a Redis:"
docker run --rm --link redis-temp:redis redis:alpine redis-cli -h redis ping
docker rmi - Eliminar imágenes
Ver uso de espacio antes
echo "💾 Espacio usado antes de limpieza:"
docker system df
Eliminar imagen específica
echo "🗑️ Eliminando imagen Python 3.9:"
docker rmi python:3.9-slim
Eliminar imágenes no usadas
echo "🧹 Limpiando imágenes sin usar:"
docker image prune -f
Ver uso de espacio después
echo "💾 Espacio usado después de limpieza:"
docker system df
Buenas prácticas con imágenes
echo "✅ === BUENAS PRÁCTICAS ==="
echo ""
echo "1. Usar tags específicos (no 'latest') en producción"
echo "2. Preferir imágenes 'slim' o 'alpine' para menor tamaño"
echo "3. Limpiar imágenes no usadas regularmente"
echo "4. Verificar imágenes oficiales en Docker Hub"
echo "5. Considerar la seguridad de las imágenes base"
echo ""
Limpiar todo para finalizar
echo "🧹 Limpieza final del laboratorio:"
docker stop $(docker ps -q) 2>/dev/null || echo "No hay contenedores corriendo"
docker rm $(docker ps -aq) 2>/dev/null || echo "No hay contenedores para eliminar"
echo "📊 Estado final del sistema:"
docker ps -a
docker images --format "table {{.Repository}}\t{{.Tag}}\t{{.Size}}"
Resumen de comandos de imágenes
echo "🐳 === COMANDOS DE IMÁGENES ==="
echo ""
echo "docker images                # Ver imágenes locales"
echo "docker search <término>      # Buscar en Docker Hub"
echo "docker pull <imagen>         # Descargar imagen"
echo "docker history <imagen>      # Ver capas de imagen"
echo "docker rmi <imagen>          # Eliminar imagen"
echo "docker image prune           # Limpiar imágenes no usadas"
echo "docker system df             # Ver uso de espacio"
echo ""
echo "🎯 ¡Completaste Docker I - Fundamentos!"
echo "🚀 ¡Listo para Docker II - Dockerfile y Compose!"
¡Felicitaciones! Ya conocés los fundamentos de Docker. En el próximo laboratorio vas a aprender a crear tus propias imágenes con Dockerfile y orquestar múltiples contenedores.

///////////////////////////////////////////////////////////////////////////////

🎉 ¡Felicitaciones!
✅ Lo que aprendiste en Docker I
Has completado exitosamente el laboratorio Docker I - Fundamentos y Comandos Básicos. Ahora conocés:

🧠 Conceptos fundamentales:
✅ Diferencias entre contenedores y máquinas virtuales
✅ Ventajas de la virtualización ligera
✅ El ecosistema Docker
🐳 Comandos esenciales:
✅ docker run - Crear y ejecutar contenedores
✅ docker ps - Listar contenedores
✅ docker stop/start - Controlar el ciclo de vida
✅ docker logs - Monitorear aplicaciones
✅ docker exec - Acceder a contenedores
✅ docker images - Gestionar imágenes
🚀 Habilidades prácticas:
✅ Ejecutar aplicaciones web en contenedores
✅ Trabajar en modo interactivo
✅ Gestionar recursos y monitoreo
✅ Buscar y descargar imágenes de Docker Hub
✅ Limpieza y optimización del sistema
🎯 Próximos pasos
Docker II - Dockerfile y Compose
En el siguiente laboratorio vas a aprender:

🔨 Crear imágenes personalizadas con Dockerfile
📁 Gestionar volúmenes para persistir datos
🌐 Configurar redes entre contenedores
🎼 Orquestar múltiples servicios con Docker Compose
🔐 Mejores prácticas de seguridad
Aplicá lo aprendido
💡 Probá ejecutar tus aplicaciones favoritas en contenedores
🌍 Explorá Docker Hub para encontrar imágenes útiles
📖 Leé la documentación oficial para profundizar
💪 Desafío personal
Antes del próximo laboratorio, intentá:

Ejecutar una base de datos (MySQL, PostgreSQL, MongoDB)
Probar diferentes stacks (LAMP, MEAN, Python+Flask)
Experimentar con comandos que no vimos en detalle
🔗 Recursos útiles
📚 Docker Documentation
🐳 Docker Hub
💡 Docker Best Practices
📺 Docker Blog
🚀 ¡Estás listo para el siguiente nivel! Continuá con Docker II para convertirte en un experto en contenedores.

 
