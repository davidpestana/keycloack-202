# Lab 03 — Backup

## Paso 01 — Exportar Realm

---

## 🎯 Objetivo

Exportar la configuración completa de un realm para poder realizar backup o migración.

---

## 🧠 Contexto

Keycloak permite:

* Exportar realms a JSON.
* Restaurarlos posteriormente.
* Migrarlos entre entornos.
* Versionar configuración.

En producción es crítico tener backups periódicos.

---

## 📍 Exportar desde contenedor

Entrar al contenedor:

```bash id="k7p3xz"
docker exec -it keycloak bash
```

---

## 📤 Ejecutar exportación

Ejecutar:

```bash id="n4v8qt"
/opt/keycloak/bin/kc.sh export \
--realm training \
--file /opt/keycloak/data/training-realm-export.json
```

---

## 📁 Copiar archivo al host

Salir del contenedor y ejecutar:

```bash id="m2z9pl"
docker cp keycloak:/opt/keycloak/data/training-realm-export.json \
infra/training-realm-export.json
```

---

## 🔎 Verificar archivo

Confirmar que existe:

```
infra/training-realm-export.json
```

Abrirlo y verificar que contiene:

* Usuarios
* Clientes
* Roles
* Identity Providers
* Configuración del realm

---

## 🧠 Qué estamos validando

* Se puede exportar configuración completa.
* El backup es portable.
* El archivo JSON contiene todo el estado del realm.
* Preparado para probar restauración en el siguiente paso.
