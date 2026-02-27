# Lab 01 — PostgreSQL

## Paso 03 — Validar persistencia tras migración

---

## 🎯 Objetivo

Comprobar que Keycloak está utilizando PostgreSQL y que los datos persisten tras reiniciar contenedores.

---

## 🧪 Crear dato de prueba

1. Acceder a Keycloak:

   [https://localhost:8443](https://localhost:8443)

2. Crear un usuario nuevo en el realm `training`:

   Username: test-persist

Asignar contraseña permanente.

---

## 🔄 Reiniciar solo Keycloak

Desde:

```
infra/
```

Ejecutar:

```bash id="v3k9qp"
docker compose restart keycloak
```

---

## 🔎 Verificar persistencia

Volver a:

```
Users
```

Confirmar que el usuario:

```
test-persist
```

sigue existiendo.

---

## 🔄 Reiniciar todo el entorno

Ejecutar:

```bash id="n7z4xd"
docker compose down
docker compose up -d
```

Esperar a que Keycloak esté operativo.

---

## 🔎 Verificar nuevamente

Comprobar que:

* El usuario sigue existiendo.
* Los realms siguen configurados.
* Clientes siguen configurados.
* Identity Providers siguen presentes.

---

## 🧠 Validación técnica

El volumen:

```
postgres_data
```

Mantiene la base de datos incluso si el contenedor se elimina.

---

## 🧠 Qué estamos confirmando

* La migración a PostgreSQL es efectiva.
* Los datos ya no se pierden al reiniciar.
* El entorno está preparado para clustering.
* Base de datos apta para producción real.
