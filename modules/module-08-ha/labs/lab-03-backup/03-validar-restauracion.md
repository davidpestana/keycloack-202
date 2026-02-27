# Lab 03 — Backup

## Paso 03 — Validar restauración completa

---

## 🎯 Objetivo

Comprobar que el realm restaurado funciona correctamente y que todos los elementos críticos están presentes.

---

## 🧪 Verificar existencia del realm

Acceder a:

```
https://localhost:8443
```

Confirmar que el realm:

```
training
```

existe tras reinicio.

---

## 🧪 Verificar configuración clave

Comprobar:

* Clients → spa-client
* Clients → backend-service
* Roles configurados
* Identity Providers (si existían)
* Authentication flows personalizados

---

## 🧪 Verificar usuarios

Ir a:

```
Users
```

Confirmar que usuarios creados previamente existen.

Intentar iniciar sesión con uno de ellos.

---

## 🧪 Verificar integraciones

Probar:

* Login desde Angular.
* Acceso a API protegida.
* Flujo MFA si estaba habilitado.
* Identity brokering si estaba configurado.

---

## 🧠 Validación técnica

El entorno debe comportarse exactamente igual que antes de la exportación.

Si todo funciona:

* El backup es válido.
* La restauración es completa.
* El entorno puede reconstruirse desde cero.

---

## 🧠 Estado final del módulo

* PostgreSQL persistente.
* Cluster multi-instancia.
* Cache distribuida.
* Reverse proxy con HTTPS.
* Backup e importación automatizable.
* Arquitectura lista para entornos empresariales reales.
