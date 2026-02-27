# Lab 03 — Backup

## Paso 02 — Importar Realm desde archivo

---

## 🎯 Objetivo

Restaurar un realm utilizando el archivo JSON exportado previamente.

---

## 🧠 Contexto

La importación permite:

* Restaurar entornos.
* Replicar configuración en otro entorno.
* Recuperarse ante pérdida de datos.

La importación puede hacerse:

* En arranque del contenedor.
* Mediante comando CLI.

---

## 📍 Opción recomendada — Importar en arranque

Copiar el archivo exportado a:

```
infra/import/
```

Crear carpeta si no existe:

```bash id="v9m2qr"
mkdir -p infra/import
```

Mover el archivo:

```bash id="k4x8tp"
mv infra/training-realm-export.json infra/import/
```

---

## 📍 Editar docker-compose

Modificar servicio `keycloak` en:

```
infra/docker-compose.yml
```

Añadir volumen:

```yaml id="p7w3sz"
      - ./import:/opt/keycloak/data/import
```

Modificar comando:

```yaml id="l2n8yx"
    command: start --import-realm
```

---

## ▶️ Reiniciar entorno

```bash id="r5z9qd"
docker compose down
docker compose up -d
```

---

## 🔎 Verificar logs

```bash id="m6q8vk"
docker logs keycloak
```

Debe aparecer algo similar a:

```
Imported realm training
```

---

## 🧠 Qué estamos validando

* El realm puede restaurarse desde JSON.
* El proceso funciona en arranque.
* Se puede reconstruir entorno completo automáticamente.
* Base preparada para recuperación ante desastres.
