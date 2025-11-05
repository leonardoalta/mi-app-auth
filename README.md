# Mi App Auth

Autenticación MFA (contraseña + OTP) y passwordless

Este proyecto es un monorepositorio que implementa un sistema moderno de autenticación utilizando Spring Boot (backend) y React + Vite (frontend). Permite autenticación tradicional mediante correo y contraseña usando OTP como segundo factor (MFA), así como inicio de sesión sin contraseña (passwordless) utilizando solamente el correo electrónico.

---

## 1. Estructura del repositorio

```
mi-app-auth/
├── backend/        → API REST (Spring Boot)
└── frontend/       → Cliente web (React + Vite)
```

---

## 2. Funcionalidades principales

* Registro de usuarios
* Login tradicional (correo + contraseña + OTP)
* Login sin contraseña (passwordless)
* Envío de código OTP a correo
* Verificación de OTP
* Reenvío de OTP
* Control de expiración OTP

---

## 3. Requisitos

* Java 17+
* Node 22.21.0 (usando NVM)
* PostgreSQL
* SMTP disponible (MailHog o Mailtrap)

---

## 4. Backend - Spring Boot

### Ejecución

```bash
cd backend
./mvnw spring-boot:run
```

Backend disponible en: [http://localhost:8080](http://localhost:8080)

### Configuración de correo para OTP

Editar archivo:

```
backend/src/main/resources/application.properties
```

Ejemplo configuración desarrollo:

```
app.auth.expose-otp-in-response=true
app.auth.otp-exp-minutes=5
app.auth.mail.from=no-reply@local.test
```

---

## 5. Frontend - React + Vite

### Ejecución

```bash
cd frontend
nvm use
npm install
npm run dev
```

Frontend disponible en: [http://localhost:5173](http://localhost:5173)

---

## 6. Flujo de autenticación

### 6.1 Registro

```bash
curl -X POST http://localhost:8080/api/auth/register ...
```

### 6.2 Login (contraseña + OTP)

1. Login genera OTP

```bash
curl -X POST http://localhost:8080/api/auth/login ...
```

2. Validar OTP

```bash
curl -X POST http://localhost:8080/api/auth/otp/verify ...
```

### 6.3 Passwordless

1. Solicitar OTP

```bash
curl -X POST http://localhost:8080/api/auth/otp/request ...
```

2. Validar OTP (igual)

---

## 7. Mensajes comunes

* Login correcto → OTP enviado
* OTP correcto → Inicio de sesión correcto
* OTP incorrecto → Código incorrecto
* OTP expirado → Código expirado
* Reenvío OTP → Nuevo código enviado

---

## 8. Seguridad

* MFA
* Passwordless
* Hash BCrypt
* Expiración OTP
* DTOs seguros
* SMTP

Producción:

```
app.auth.expose-otp-in-response=false
```

---

## 9. Commits

* feat(auth-backend): agrega endpoints OTP y login
* fix(ux): mejora mensajes de verificación
* docs(readme): agrega documentación principal
* chore(repo): configura estructura monorepo

---

## 10. Roadmap

* Refresh Token
* Roles y permisos
* Recuperación de contraseña
* JWT
* Auditoría

---

## 11. Licencia

MIT

---

## 12. Autor

Proyecto orientado a aprendizaje y práctica de autenticación moderna (MFA + passwordless)
