# Lab 02 — Clustering

## Paso 02 — Configurar caché distribuida (Infinispan)

---

## 🎯 Objetivo

Configurar correctamente el modo de caché para que múltiples instancias compartan estado de sesión.

---

## 🧠 Contexto

Keycloak usa **Infinispan** como motor de caché.

En modo cluster:

* Las sesiones deben replicarse.
* Las instancias deben descubrirse entre sí.
* Se debe usar modo `cluster` (no local).

En Docker podemos usar descubrimiento por DNS interno.

---

## 📍 Editar docker-compose

Abrir:

```
infra/docker-compose.yml
```

En ambos servicios `keycloak` y `keycloak-2` añadir:

```yaml id="k3v9zd"
      KC_CACHE: ispn
      KC_CACHE_STACK: tcp
```

---

## 📍 Añadir configuración de red compartida

Asegurarse de que todos los servicios estén en la misma red (Docker lo hace por defecto si están en el mismo compose).

No es necesario exponer puertos adicionales si están en red interna.

---

## 📍 Confirmar modo producción

Para clustering real, usar:

```yaml id="z7m2qp"
    command: start
```

En lugar de `start-dev`.

---

## ▶️ Reiniciar entorno

```bash id="v4x8tn"
docker compose down
docker compose up -d
```

---

## 🔎 Validar logs

```bash id="y2n7rk"
docker logs keycloak
docker logs keycloak-2
```

Debe aparecer información relacionada con:

```
Infinispan
cluster
node joined
```

---

## 🧠 Qué estamos validando

* Cache distribuida activa.
* Instancias reconocen cluster.
* Preparado para validar sesión compartida en el siguiente paso.
