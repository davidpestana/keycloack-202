# Lab 02 — Clustering

## Paso 01 — Ejecutar múltiples instancias de Keycloak

---

## 🎯 Objetivo

Simular un entorno de alta disponibilidad ejecutando dos instancias de Keycloak conectadas a la misma base de datos PostgreSQL.

---

## 🧠 Contexto

Para que Keycloak funcione en HA:

* Todas las instancias deben compartir la misma base de datos.
* Deben compartir estado de sesión.
* Deben usar configuración de cache distribuida.

En este laboratorio simularemos múltiples instancias en Docker.

---

## 📍 Editar docker-compose

Abrir:

```
infra/docker-compose.yml
```

Duplicar el servicio `keycloak` creando una segunda instancia:

```yaml id="r4n8zx"
  keycloak-2:
    image: quay.io/keycloak/keycloak:latest
    container_name: keycloak-2
    command: start-dev
    environment:
      KC_DB: postgres
      KC_DB_URL_HOST: postgres
      KC_DB_URL_DATABASE: keycloak
      KC_DB_USERNAME: keycloak
      KC_DB_PASSWORD: keycloak
      KC_PROXY: edge
      KC_HTTP_ENABLED: "true"
      KC_HOSTNAME_STRICT: "false"
    depends_on:
      - postgres
    expose:
      - "8080"
```

---

## 📍 Ajustar NGINX para balancear

Editar:

```
infra/nginx/nginx.conf
```

Dentro de `http` añadir upstream:

```nginx id="u2x7mk"
http {

  upstream keycloak_cluster {
    server keycloak:8080;
    server keycloak-2:8080;
  }
```

Y modificar `proxy_pass`:

```nginx id="p9k4vw"
proxy_pass http://keycloak_cluster;
```

---

## ▶️ Reiniciar entorno

Desde:

```
infra/
```

Ejecutar:

```bash id="j6p8rt"
docker compose down
docker compose up -d
```

---

## 🔎 Verificar contenedores

```bash id="s3m7qx"
docker ps
```

Debe aparecer:

* keycloak
* keycloak-2
* postgres
* nginx

---

## 🧠 Qué estamos validando

* Dos instancias conectadas a la misma base.
* Reverse proxy balanceando tráfico.
* Base preparada para alta disponibilidad.
* En el siguiente paso configuraremos cache compartida.
