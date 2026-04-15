# 🚀 Lab Docker Compose - Contenedores y Microservicios

Repositorio correspondiente al laboratorio de Docker Compose donde se implementan diferentes escenarios prácticos de contenedores, redes, persistencia y configuración.

---

## 📁 Estructura del Proyecto

```
lab-docker-compose/
├── ejercicio1/   # Nginx + HTML estático
├── ejercicio2/   # Node.js + PostgreSQL
├── ejercicio3/   # MySQL + Volúmenes (persistencia)
├── ejercicio4/   # .env + Healthchecks
```

---

## ⚙️ Requisitos

Antes de ejecutar los ejercicios, asegúrate de tener:

* Docker Desktop instalado y en ejecución
* Docker Compose v2 (`docker compose version`)
* Git
* VS Code (opcional pero recomendado)
* iTerm2 (opcional)

---

# 🧪 Ejercicio 1 — Servicio Web con Nginx

## 📌 Descripción

Se levanta un servidor web con Nginx que sirve contenido HTML estático usando Docker Compose.

## ▶️ Cómo ejecutarlo

```bash
cd ejercicio1
docker compose up -d
```

## 🔍 Verificar

```bash
docker compose ps
docker compose logs web
```

Abrir en navegador:

```
http://localhost:8080
```

## ✏️ Prueba en caliente

Edita el archivo:

```
html/index.html
```

Recarga el navegador → verás los cambios sin reiniciar el contenedor.

## 🛑 Detener

```bash
docker compose down
```

---

# 🧪 Ejercicio 2 — Multi-servicio (Node.js + PostgreSQL)

## 📌 Descripción

Se crea una aplicación Node.js que se comunica con una base de datos PostgreSQL dentro de la red interna de Docker Compose.

## ▶️ Cómo ejecutarlo

```bash
cd ejercicio2
docker compose up -d
```

## 🔍 Verificar

```bash
docker compose ps
docker compose logs app
docker compose logs db
```

Abrir en navegador:

```
http://localhost:3000
```

## 🧠 Probar conexión entre servicios

```bash
docker compose exec app ping db -c 3
```

## 🗄️ Acceder a PostgreSQL

```bash
docker compose exec db psql -U admin -d miapp
```

Dentro:

```sql
\l
\dt
\q
```

## 🛑 Detener

```bash
docker compose down
```

---

# 🧪 Ejercicio 3 — Persistencia con Volúmenes (MySQL)

## 📌 Descripción

Se utiliza un volumen nombrado para almacenar datos de MySQL y garantizar que no se pierdan al reiniciar contenedores.

## ▶️ Cómo ejecutarlo

```bash
cd ejercicio3
docker compose up -d
```

## 🔍 Verificar logs

```bash
docker compose logs db
```

## 🗄️ Crear datos de prueba

```bash
docker compose exec db mysql -uroot -prootpass tienda
```

Dentro:

```sql
CREATE TABLE productos (
  id INT AUTO_INCREMENT PRIMARY KEY,
  nombre VARCHAR(100),
  precio DECIMAL(10,2)
);

INSERT INTO productos (nombre, precio) VALUES ('Laptop', 1200);
INSERT INTO productos (nombre, precio) VALUES ('Mouse', 25.50);

SELECT * FROM productos;
EXIT;
```

## 🔁 Probar persistencia

```bash
docker compose down
docker volume ls
docker compose up -d
```

Verificar:

```bash
docker compose exec db mysql -uroot -prootpass tienda -e "SELECT * FROM productos;"
```

## 🧹 Eliminar todo (incluyendo datos)

```bash
docker compose down -v
```

---

# 🧪 Ejercicio 4 — Variables de entorno y Healthchecks

## 📌 Descripción

Se usa un archivo `.env` para manejar configuración y se implementa un `healthcheck` para controlar el orden de inicio de servicios.

## ▶️ Cómo ejecutarlo

```bash
cd ejercicio4
docker compose up -d
```

## 🔍 Verificar estado

```bash
docker compose ps
docker compose logs db
docker compose logs app
```

Abrir en navegador:

```
http://localhost:4000
```

## 🔧 Ver variables aplicadas

```bash
docker compose config
```

## 🧪 Ver variables dentro del contenedor

```bash
docker compose exec app env | grep DB_
```

## ⚠️ Probar fallo de healthcheck

Modificar temporalmente:

```yaml
healthcheck:
  test: ["CMD", "false"]
```

Luego:

```bash
docker compose up -d
docker compose ps
```

Verás estado `unhealthy`.

## 🛑 Detener

```bash
docker compose down -v
```

---

# 📌 Buenas prácticas

* No subir archivos `.env` al repositorio
* Usar `.env.example` para compartir configuración
* Evitar credenciales reales en desarrollo
* Usar volúmenes para bases de datos

---

# ✅ Conclusiones

1. Docker Compose permite gestionar múltiples servicios de forma sencilla y eficiente, facilitando el desarrollo de arquitecturas basadas en microservicios.

2. El uso de volúmenes garantiza la persistencia de datos, evitando pérdidas al reiniciar contenedores.

3. Los archivos `.env` y los healthchecks mejoran la configuración y el control del ciclo de vida de los servicios.

---
