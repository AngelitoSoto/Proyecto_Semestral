# Proyecto DevOps – Sistema de Ventas y Despachos

## Integrantes
- Samuel Urzua
- Angel Soto

## Docente
Miguel David Acuña Narváez

## Asignatura
INTRODUCCION A HERRAMIENTAS DEVOPS_304D

---

# Descripción del Proyecto

Este proyecto corresponde a una arquitectura basada en microservicios desarrollada utilizando Spring Boot, Docker, Nginx y AWS EC2.

El sistema permite gestionar ventas y despachos mediante APIs REST independientes, consumidas desde un frontend web desarrollado en React/Vite.

La solución implementa:

- Microservicio de Ventas
- Microservicio de Despachos
- Frontend Web
- Base de Datos MySQL
- Docker y Docker Compose
- CI/CD con GitHub Actions
- Reverse Proxy con Nginx
- Despliegue en AWS EC2

---

# Arquitectura del Sistema

El proyecto está compuesto por los siguientes servicios:

## Frontend

Aplicación desarrollada en React + Vite.

### Responsabilidades
- Mostrar ventas
- Mostrar despachos
- Consumir APIs REST
- Interfaz gráfica del usuario

### Puerto
```bash
5173
```

---

## Microservicio Ventas

API REST desarrollada en Spring Boot.

### Responsabilidades
- Gestión de ventas
- CRUD de ventas
- Persistencia en MySQL

### Puerto
```bash
8080
```

### Endpoint Principal
```bash
/api/v1/ventas
```

---

## Microservicio Despachos

API REST desarrollada en Spring Boot.

### Responsabilidades
- Gestión de despachos
- CRUD de despachos
- Persistencia en MySQL

### Puerto
```bash
8081
```

### Endpoint Principal
```bash
/api/v1/despachos
```

---

## Base de Datos

### Motor
```bash
MySQL 8.4
```

### Puerto
```bash
3306
```

### Contenedor
```bash
mysql-citt
```

---

# Tecnologías Utilizadas

## Backend
- Java 17
- Spring Boot 3
- Spring Data JPA
- Hibernate
- Maven

## Frontend
- React
- Vite
- Axios
- Nginx

## DevOps
- Docker
- Docker Compose
- GitHub Actions
- AWS EC2

## Base de Datos
- MySQL 8.4

---

# Estructura del Proyecto

```bash
/
├── back-Ventas_SpringBoot/
├── back-Despachos_SpringBoot/
├── front_despacho/
├── docker-compose.yml
└── .github/workflows/
```

---

# Variables de Entorno

Los microservicios utilizan las siguientes variables:

```yaml
environment:
  DB_ENDPOINT: mysql
  DB_PORT: 3306
  DB_NAME: cittdb
  DB_USERNAME: root
  DB_PASSWORD: root
```

---

# Docker Compose

El sistema se levanta mediante Docker Compose.

## Servicios incluidos

- mysql
- ventas-api
- despachos-api
- despacho-front

## Levantar el proyecto

```bash
docker compose up -d
```

## Detener el proyecto

```bash
docker compose down
```

---

# Configuración de Nginx

Nginx funciona como reverse proxy para enrutar solicitudes hacia los microservicios.

```nginx
server {
    listen 80;

    location / {
        root /usr/share/nginx/html;
        index index.html;
        try_files $uri /index.html;
    }

    location /api/ventas {
        proxy_pass http://10.0.6.83:8080/api/v1/ventas;
    }

    location /api/despachos {
        proxy_pass http://10.0.6.83:8081/api/v1/despachos;
    }
}
```

---

# CI/CD con GitHub Actions

El proyecto implementa automatización mediante GitHub Actions.

## Funcionalidades

- Build automático
- Construcción de imágenes Docker
- Push automático a Docker Hub
- Automatización del despliegue

## Branch utilizada

```bash
deploy
```

---

# Contenedores Docker

## Ver contenedores activos

```bash
docker ps
```

## Contenedores esperados

- mysql-citt
- ventas-api
- despachos-api
- despacho-front

---

# Comandos Utilizados en AWS

## Conexión SSH

```bash
ssh -i clave.pem ec2-user@IP_PUBLICA
```

## Ir al proyecto

```bash
cd /home/ec2-user/app
```

## Levantar servicios

```bash
sudo docker compose up -d
```

## Ver logs

```bash
sudo docker logs ventas-api
```

---

# Verificación de APIs

## Ventas

```bash
curl localhost:8080/api/v1/ventas
```

## Despachos

```bash
curl localhost:8081/api/v1/despachos
```

---

# Despliegue en AWS EC2

El sistema fue desplegado utilizando:

- Amazon EC2
- Amazon Linux
- Docker Engine
- Docker Compose
- GitHub Actions
- Docker Hub

La comunicación entre servicios se realiza mediante Docker Networking.

---

# Problemas Solucionados Durante el Desarrollo

## Error 502 Bad Gateway

### Causa
- APIs no disponibles
- Configuración incorrecta de Nginx

### Solución
- Verificación de puertos
- Corrección de proxy_pass

---

## Error MySQL

### Error

```bash
Host is not allowed to connect to this MySQL server
```

### Solución

```sql
ALTER USER 'root'@'%' IDENTIFIED BY 'root';
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

---

## Variables de Entorno No Reconocidas

### Error

```bash
${DB_ENDPOINT}:${DB_PORT}
```

### Solución
- Corrección de variables en docker-compose.yml
- Reinicio de contenedores

---

# Funcionamiento General del Sistema

1. El usuario accede al frontend.
2. El frontend consume las APIs REST.
3. Nginx redirecciona las solicitudes.
4. Los microservicios procesan la información.
5. Spring Boot accede a MySQL.
6. La información se devuelve al frontend.

---

# Endpoints Disponibles

## Ventas

```http
GET /api/v1/ventas
POST /api/v1/ventas
PUT /api/v1/ventas/{id}
DELETE /api/v1/ventas/{id}
```

---

## Despachos

```http
GET /api/v1/despachos
POST /api/v1/despachos
PUT /api/v1/despachos/{id}
DELETE /api/v1/despachos/{id}
```

---

# Conclusión

El proyecto implementa correctamente una arquitectura basada en microservicios utilizando tecnologías modernas de desarrollo y despliegue.

Se logró:

- Separación de responsabilidades
- Contenerización completa
- Automatización CI/CD
- Integración frontend/backend
- Despliegue en AWS
- Comunicación entre servicios mediante Docker Networking

El sistema quedó completamente funcional y preparado para futuras mejoras y escalabilidad.
