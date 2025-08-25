# Control de Gastos - Proyecto de Gestión Financiera

## 📋 Descripción del Proyecto

Control de Gastos es una aplicación web desarrollada en HTML, CSS y JavaScript que permite a los usuarios gestionar y visualizar sus gastos personales de manera intuitiva. La aplicación incluye:

- **Registro de gastos** con nombre, monto y categoría
- **Cálculo automático** de saldo restante
- **Visualización gráfica** de la distribución de gastos por categorías
- **Interfaz responsive** y amigable
- **Funcionalidad de eliminar** gastos registrados

## 🚀 Despliegue en AWS EC2

### Errores Encontrados y Solucionados

1. **Error 403 Forbidden**: 
   - **Causa**: Permisos incorrectos en los archivos y directorios
   - **Solución**: Configurar adecuadamente los permisos y propietario de los archivos

2. **Error de configuración Nginx**:
   - **Causa**: Sintaxis incorrecta en el archivo de configuración
   - **Solución**: Corregir la estructura del archivo de configuración

3. **Problema de puertos**:
   - **Causa**: Intento de acceso por puerto 8000 sin configuración adecuada
   - **Solución**: Configurar Nginx para escuchar en el puerto 8000 y ajustar Security Groups

### 📝 Paso a Paso para Despliegue en AWS

#### 1. Configuración Inicial de la Instancia EC2

```bash
# Conectarse a la instancia
ssh -i "tu-clave.pem" ubuntu@tu-ip-publica
2. Instalación de Dependencias
bash
# Actualizar sistema
sudo apt update && sudo apt upgrade -y

# Instalar Nginx
sudo apt install nginx -y

# Instalar Git
sudo apt install git -y
3. Configuración del Firewall
bash
# Habilitar puertos necesarios
sudo ufw allow 'Nginx Full'
sudo ufw allow 'OpenSSH'
sudo ufw enable
4. Clonación y Configuración del Proyecto
bash
# Crear directorio para la aplicación
sudo mkdir -p /var/www/control_gastos
sudo chown -R ubuntu:ubuntu /var/www/control_gastos

# Clonar el proyecto
cd /var/www/control_gastos
git clone https://github.com/bdmaderam/control_gastos.git .
5. Configuración de Nginx
bash
# Crear archivo de configuración
sudo nano /etc/nginx/sites-available/control_gastos
Contenido del archivo de configuración:

nginx
server {
    listen 8000;
    server_name _;
    root /var/www/control_gastos;
    index proyecto.html;
    
    location / {
        try_files $uri $uri/ =404;
    }
}
6. Habilitar el Sitio y Verificar Configuración
bash
# Habilitar el sitio
sudo ln -s /etc/nginx/sites-available/control_gastos /etc/nginx/sites-enabled/

# Eliminar configuración por defecto
sudo rm /etc/nginx/sites-enabled/default

# Verificar configuración
sudo nginx -t

# Reiniciar Nginx
sudo systemctl restart nginx
7. Configuración de Permisos
bash
# Dar permisos adecuados
sudo chmod -R 755 /var/www/control_gastos
sudo chown -R www-data:www-data /var/www/control_gastos
8. Configuración del Security Group en AWS
Ir a la consola de AWS EC2

Seleccionar la instancia

Editar el Security Group

Agregar regla de entrada:

Tipo: Custom TCP

Puerto: 8000

Origen: 0.0.0.0/0

9. Verificación Final
bash
# Verificar estado de Nginx
sudo systemctl status nginx

# Verificar logs en tiempo real
sudo tail -f /var/log/nginx/error.log
🌐 Acceso a la Aplicación
La aplicación estará disponible en:

text
http://tu-ip-publica:8000
🛠️ Comandos Útiles para Mantenimiento
bash
# Reiniciar Nginx
sudo systemctl restart nginx

# Ver logs de error
sudo tail -f /var/log/nginx/error.log

# Ver logs de acceso
sudo tail -f /var/log/nginx/access.log

# Verificar espacio en disco
df -h

# Ver uso de memoria
free -h
📊 Características Técnicas
Frontend: HTML5, CSS3, JavaScript ES6+

Gráficas: Chart.js para visualización de datos

Servidor: Nginx en Ubuntu

Alojamiento: AWS EC2

Puerto: 8000

🔧 Troubleshooting Común
Error 403 Forbidden: Verificar permisos de archivos

Conexión rechazada: Verificar Security Group en AWS

Página no encontrada: Verificar ruta del archivo index.html

Problemas de CORS: Configuración adecuada de Nginx

📞 Soporte
Para problemas adicionales, verificar:

Logs de Nginx: /var/log/nginx/error.log

Estado del servicio: sudo systemctl status nginx

Configuración: sudo nginx -t

Este proyecto demuestra un flujo completo de desarrollo web moderno, desde la implementación frontend hasta el despliegue en infraestructura cloud con AWS.
