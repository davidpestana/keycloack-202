# Lab 01 — PostgreSQL

## Paso 02 — Configurar Keycloak para usar PostgreSQL

🎯 **Objetivo**
Configurar el contenedor de Keycloak para que utilice **PostgreSQL como base de datos persistente** en lugar de la base en memoria H2.

---

# 🧠 Contexto

Keycloak soporta múltiples bases de datos:

* PostgreSQL
* MySQL
* MariaDB
* Oracle
* SQL Server

En entornos reales **PostgreSQL es la opción más utilizada**.

Para que Keycloak use PostgreSQL debemos configurar:

* tipo de base de datos
* host
* usuario
* contraseña
* nombre de base

Esto se hace mediante **variables de entorno** en el contenedor.

---

# 📍 Editar docker-compose

Abrir:

```
infra/docker-compose.yml
```

Localizar el servicio `keycloak`.

Ejemplo actual simplificado:

```yaml
keycloak:
  image: quay.io/keycloak/keycloak:latest
  command:
    - start-dev
    - --hostname-strict=false
    - --proxy-headers=xforwarded
  environment:
    KEYCLOAK_ADMIN: admin
    KEYCLOAK_ADMIN_PASSWORD: admin
```

---

# ✏️ Añadir configuración de base de datos

Modificar el servicio `keycloak` para añadir las variables de PostgreSQL.

```yaml
keycloak:
  image: quay.io/keycloak/keycloak:latest
  command:
    - start-dev
    - --hostname-strict=false
    - --proxy-headers=xforwarded

  environment:
    KEYCLOAK_ADMIN: admin
    KEYCLOAK_ADMIN_PASSWORD: admin

    KC_DB: postgres
    KC_DB_URL: jdbc:postgresql://postgres:5432/keycloak
    KC_DB_USERNAME: keycloak
    KC_DB_PASSWORD: keycloak

  depends_on:
    - postgres

  ports:
    - "8080:8080"
```

---

# 🔎 Explicación de las variables

| Variable         | Función                               |
| ---------------- | ------------------------------------- |
| `KC_DB`          | Motor de base de datos                |
| `KC_DB_URL`      | URL JDBC de conexión                  |
| `KC_DB_USERNAME` | Usuario de PostgreSQL                 |
| `KC_DB_PASSWORD` | Password                              |
| `depends_on`     | Arranca PostgreSQL antes que Keycloak |

---

# 📦 Arquitectura resultante

```
┌──────────────┐
│   Browser    │
└──────┬───────┘
       │
       │ HTTP
       ▼
┌──────────────┐
│   Keycloak   │
│  Port 8080   │
└──────┬───────┘
       │ JDBC
       ▼
┌──────────────┐
│ PostgreSQL   │
│ Port 5432    │
└──────────────┘
```

Keycloak ahora **persistirá usuarios, realms, clientes y sesiones en PostgreSQL**.

---

# ▶️ Levantar el entorno

Desde la carpeta `infra` ejecutar:

```bash
docker compose up -d
```

---

# 🔎 Verificar conexión

Comprobar logs de Keycloak:

```bash
docker logs -f keycloak
```

Buscar mensajes similares a:

```
Database: PostgreSQL
Connected to jdbc:postgresql://postgres:5432/keycloak
```

---

# 🧠 Estado tras este paso

El entorno ahora tiene:

✔ PostgreSQL persistente
✔ Keycloak conectado a base externa
✔ datos guardados entre reinicios

Esto nos permite:

* mantener **usuarios**
* mantener **realms**
* mantener **clientes**
