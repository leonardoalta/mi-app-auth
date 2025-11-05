🛡️ Mi App Auth
Autenticación MFA (Contraseña + OTP) & Passwordless

Bienvenido 👋
Este es un monorepo full-stack que implementa autenticación moderna usando:

✔️ Login tradicional (correo + contraseña + OTP)

✔️ Login sin contraseña (passwordless) — solo correo → OTP

✔️ Verificación OTP + reenvío

✔️ Control de expiración del código

Ideal como base para proyectos que requieren seguridad, simplicidad de integración y buena UX.

📂 Estructura del repositorio
mi-app-auth/
├── backend/      # API – Spring Boot
│   ├── src/
│   ├── pom.xml
│   └── ...
└── frontend/     # UI – React + Vite
    ├── src/
    ├── package.json
    └── ...

✅ Características principales
Funcionalidad	Estado
Registro de usuarios	✅
Login tradicional (correo + password)	✅
Envío de OTP al correo	✅
Verificación OTP	✅
Login sin contraseña (passwordless)	✅
Reenvío de OTP	✅
Control de expiración	✅
⚙️ Instalación & ejecución
📌 Requisitos
Dependencia	Versión
Java	17+
Node	22.21.0 (NVM recomendado)
PostgreSQL	✅
SMTP	(MailHog, Mailtrap o Gmail)
🖥️ Backend – Spring Boot
▶️ Ejecutar backend
cd backend
./mvnw spring-boot:run


Servirá en:

http://localhost:8080/

⚙️ Configuración de correo (OTP)

Editar:

backend/src/main/resources/application.properties


Ejemplo (dev):

app.auth.expose-otp-in-response=true
app.auth.otp-exp-minutes=5
app.auth.mail.from=no-reply@local.test

Opciones
✅ MailHog (local)
spring.mail.host=localhost
spring.mail.port=1025
spring.mail.properties.mail.smtp.auth=false
spring.mail.properties.mail.smtp.starttls.enable=false

✅ Mailtrap
spring.mail.host=sandbox.smtp.mailtrap.io
spring.mail.port=2525
spring.mail.username=TU_USER
spring.mail.password=TU_PASS
spring.mail.properties.mail.smtp.auth=true
spring.mail.properties.mail.smtp.starttls.enable=true


⚠️ En producción
app.auth.expose-otp-in-response=false

💻 Frontend – React + Vite
▶️ Ejecutar frontend
cd frontend
nvm use
npm install
npm run dev


Disponible en:

http://localhost:5173/

🔐 Flujo: Login con contraseña ➜ OTP
✅ 1) Registrar usuario
curl -X POST http://localhost:8080/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "firstName":"Ana",
    "lastName":"Lopez",
    "email":"ana@example.com",
    "password":"Secret.123"
  }'

✅ 2) Login → genera OTP
curl -X POST http://localhost:8080/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email":"ana@example.com",
    "password":"Secret.123",
    "device":"Chrome/Linux"
  }'


Ejemplo (dev):

{
  "message": "First factor OK. OTP sent to email.",
  "otp_demo": 123456,
  "valid_minutes": 5
}

✅ 3) Verificar OTP
curl -X POST http://localhost:8080/api/auth/otp/verify \
  -H "Content-Type: application/json" \
  -d '{
    "email":"ana@example.com",
    "code":123456
  }'


Respuesta:

{
  "authenticated": true,
  "message": "MFA completed"
}

🔐 Flujo: Login Passwordless (solo correo)
✅ 1) Solicitar OTP
curl -X POST http://localhost:8080/api/auth/otp/request \
  -H "Content-Type: application/json" \
  -d '{
    "email":"ana@example.com",
    "device":"Chrome/Linux"
  }'


Ejemplo (dev):

{
  "message": "OTP sent to email",
  "valid_minutes": 5
}

✅ 2) Verificar OTP

Mismo paso del login tradicional

✅ UX — Mensajes visibles
Acción	Mensaje
Login correcto	OTP enviado
OTP validado	Inicio de sesión exitoso
OTP incorrecto	Código incorrecto
OTP expirado	Código expirado
Reenvío	Código reenviado
🔒 Seguridad implementada

✅ MFA
✅ Passwordless
✅ Expiración OTP
✅ Repositorio seguro
✅ BCrypt password
✅ Reenvío OTP
✅ SMTP
✅ DTOs seguros

⚠️ En producción:

No exponer OTP: app.auth.expose-otp-in-response=false

📦 Commits (Conventional Commits)

Ejemplos aplicados:

feat(auth-backend): agrega endpoints OTP y login
fix(ux): mejora mensajes de verificación
docs(readme): agrega documentación principal
chore(repo): configura estructura monorepo

🏁 Roadmap (Sugerencias)

Refresh Token

Roles y permisos

Recuperar contraseña

JWT

Auditoría

📄 Licencia

MIT
