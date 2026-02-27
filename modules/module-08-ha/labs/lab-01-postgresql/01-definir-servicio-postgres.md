# Lab 01 — PostgreSQL

## Paso 01 — Definir servicio PostgreSQL

---

## 🎯 Objetivo

Añadir un servicio PostgreSQL para que Keycloak deje de usar base de datos en memoria y pase a almacenamiento persistente.

---

## 🧠 Contexto

En modo desarrollo:

* Keycloak usa base en memoria (H2).
* No es persistente.
* No es apto para producción.

En producción:

* Se usa PostgreSQL.
* Se garantiza persistencia.
* Se habilita clustering real.

---

## 📍 Editar docker-compose

Abrir:

```
infra/docker-compose.yml
```

Añadir dentro de `services:`:

```yaml id="m3z7ke"
  postgres:
    image: postgres:15
    container_name: postgres
    environment:
      POSTGRES_DB: keycloak
      POSTGRES_USER: keycloak
      POSTGRES_PASSWORD: keycloak
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data
    restart: always
```

---

## 📍 Añadir volumen persistente

Al final del archivo añadir:

```yaml id="y5v2lx"
volumes:
  postgres_data:
```

---

## 💾 Guardar cambios

Verificar que la indentación YAML es correcta.

---

## 🧠 Estado actual

* Servicio PostgreSQL definido.
* Volumen persistente configurado.
* Preparado para conectar Keycloak a esta base de datos en el siguiente paso.
