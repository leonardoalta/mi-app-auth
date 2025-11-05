# ✅ Mi App Auth – Login con MFA (Contraseña + OTP) y Passwordless  

Monorepo con backend (Spring Boot) y frontend (React + Vite) que implementa autenticación moderna con:

✅ Login tradicional  
> Correo + contraseña → envío OTP → verificación → acceso  

✅ Login passwordless  
> Solo correo → envío OTP → verificación → acceso  

✅ Funciones clave
- Envío de código OTP por correo
- Verificación de código OTP
- Reenvío de OTP
- Control de expiración
- Doble factor de autenticación (MFA)
- Flujo passwordless

Este proyecto sirve como base para sistemas que requieran mayor seguridad, sin depender únicamente de contraseñas.  
Gracias a su enfoque modular, puede extenderse a perfiles de usuario, refresco de tokens, o integración con apps móviles.

------------------------------------------------------------
📁 ESTRUCTURA DEL REPOSITORIO
------------------------------------------------------------

mi-app-auth/
├── backend/       # API REST con Spring Boot
│   ├── src/
│   ├── pom.xml
│   └── ...
└── frontend/      # UI con React + Vite
    ├── src/
    ├── package.json
    └── ...

------------------------------------------------------------
🚀 INSTRUCCIONES PARA EJECUTAR
------------------------------------------------------------

✅ Requisitos

| Componente | Versión |
|------------|---------|
| Java       | 17+     |
| Node       | 22.21.0 (nvm) |
| PostgreSQL | Requerido |
| SMTP       | MailHog / Mailtrap |

------------------------------------------------------------
⚙️ BACKEND (SPRING BOOT)
------------------------------------------------------------

1) Entrar
cd backend

2) Ejecutar
./mvnw spring-boot:run

El backend queda disponible en:
http://localhost:8080

------------------------------------------------------------
⚙️ CONFIGURACIÓN SMTP
------------------------------------------------------------

Editar:
backend/src/main/resources/application.properties

app.auth.expose-otp-in-response=true
app.auth.otp-exp-minutes=5

# OPCIÓN 1 – MailHog local
# spring.mail.host=localhost
# spring.mail.port=1025
# spring.mail.properties.mail.smtp.auth=false
# spring.mail.properties.mail.smtp.starttls.enable=false
# app.auth.mail.from=no-reply@local.test

# OPCIÓN 2 – Mailtrap
# spring.mail.host=sandbox.smtp.mailtrap.io
# spring.mail.port=2525
# spring.mail.username=TU_USER
# spring.mail.password=TU_PASS
# spring.mail.properties.mail.smtp.auth=true
# spring.mail.properties.mail.smtp.starttls.enable=true
# app.auth.mail.from=no-reply@miapp.com

⚠️ NOTA PRODUCTION:
No exponer OTP:
app.auth.expose-otp-in-response=false

------------------------------------------------------------
💻 FRONTEND (REACT + VITE)
------------------------------------------------------------

1) Entrar
cd frontend

2) Seleccionar nodo
nvm use

3) Instalar
npm install

4) Ejecutar
npm run dev

Frontend disponible en:
http://localhost:5173

------------------------------------------------------------
🔐 FLUJO DE LOGIN TRADICIONAL (CONTRASEÑA + OTP)
------------------------------------------------------------

1) Registrar usuario

curl -X POST http://localhost:8080/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "firstName": "Ana",
    "lastName": "Lopez",
    "email": "ana@example.com",
    "password": "Secret.123"
  }'

2) Login → genera OTP

curl -X POST http://localhost:8080/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "ana@example.com",
    "password": "Secret.123",
    "device": "Chrome/Linux"
  }'

Respuesta (DEV):
{
  "message": "First factor OK. OTP sent to email.",
  "otp_demo": 123456,
  "valid_minutes": 5
}

3) Verificar OTP

curl -X POST http://localhost:8080/api/auth/otp/verify \
  -H "Content-Type: application/json" \
  -d '{
    "email": "ana@example.com",
    "code": 123456
  }'

✅ Respuesta:
Inicio de sesión completado

------------------------------------------------------------
🔐 LOGIN PASSWORDLESS (SIN CONTRASEÑA)
------------------------------------------------------------

1) Solicitar OTP solo con email

curl -X POST http://localhost:8080/api/auth/otp/request \
  -H "Content-Type: application/json" \
  -d '{
    "email": "ana@example.com",
    "device": "Chrome/Linux"
  }'

Respuesta (DEV):
{
  "message": "OTP sent to email.",
  "otp_demo": 654321,
  "valid_minutes": 5
}

2) Verificar OTP

curl -X POST http://localhost:8080/api/auth/otp/verify \
  -H "Content-Type: application/json" \
  -d '{
    "email": "ana@example.com",
    "code": 654321
  }'

✅ Resultado:
Inicio de sesión completado

------------------------------------------------------------
✅ MENSAJES EN UI
------------------------------------------------------------

| Acción | Mensaje |
|--------|--------|
| Login correcto | OTP enviado |
| OTP correcto | Inicio de sesión exitoso |
| OTP incorrecto | Código incorrecto |
| OTP expirado | Código expirado |
| Reenvío | Código reenviado |

------------------------------------------------------------
🔒 SEGURIDAD IMPLEMENTADA
------------------------------------------------------------

✅ MFA (contraseña + OTP)  
✅ Passwordless  
✅ Control de expiración  
✅ Prevención de user enumeration  
✅ Reenvío OTP  
✅ Logs  
✅ SMTP

⚠️ En producción desactivar:
app.auth.expose-otp-in-response=true

------------------------------------------------------------
📄 LICENCIA
------------------------------------------------------------

MIT

