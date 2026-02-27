# Lab 02 — Clustering

## Paso 03 — Validar sesión compartida entre instancias

---

## 🎯 Objetivo

Comprobar que una sesión iniciada en una instancia de Keycloak es válida en otra instancia del cluster.

---

## 🧠 Contexto

Si el cluster funciona correctamente:

* El login en una instancia debe replicarse.
* El usuario no debe perder sesión aunque cambie de nodo.
* No debe producirse relogin al cambiar de backend.

---

## 🧪 Forzar balanceo

NGINX ya balancea entre:

* keycloak
* keycloak-2

Cada petición puede ir a una instancia distinta.

---

## 🧪 Iniciar sesión

1. Acceder a:

   [https://localhost:8443](https://localhost:8443)

2. Iniciar sesión normalmente.

---

## 🔎 Verificar sesión

Ir a:

```
https://localhost:8443/realms/training/account
```

Refrescar varias veces.

Resultado esperado:

* No se pierde sesión.
* No aparece pantalla de login.
* La sesión permanece activa.

---

## 🧪 Reiniciar una instancia

Desde:

```
infra/
```

Ejecutar:

```bash id="m4z8xp"
docker restart keycloak
```

Mantener `keycloak-2` activo.

---

## 🔎 Probar nuevamente

Refrescar la página del account.

Resultado esperado:

* La sesión sigue activa.
* No se solicita relogin.
* El usuario permanece autenticado.

---

## 🧠 Qué estamos validando

* Sesiones replicadas en Infinispan.
* Cluster funcionando correctamente.
* Alta disponibilidad real.
* El sistema tolera caída de un nodo.

---

## 🧠 Estado actual

* PostgreSQL compartido.
* Múltiples instancias activas.
* Cache distribuida.
* Arquitectura lista para producción en HA.
