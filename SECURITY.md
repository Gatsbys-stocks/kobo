# Seguridad — Firebase Realtime Database

Esta app usa Firebase (autenticación anónima + Realtime Database) para
sincronizar los pedidos entre los móviles del personal. No hay contraseñas:
cualquiera que abra la página entra como usuario anónimo autenticado.

## Reglas de la base de datos

El archivo [`database.rules.json`](./database.rules.json) en este repo es
la versión que debería estar publicada en Firebase. Hace dos cosas:

1. Deniega todo por defecto, y solo permite leer/escribir dentro de
   `sessions/{código}` (en vez de toda la base de datos, como estaba antes).
2. Valida que lo que se guarda en los pedidos tenga la forma esperada
   (cantidades numéricas, notas de texto cortas, etc.), para evitar que una
   escritura rara corrompa los datos.

Como la app no pide login, estas reglas no distinguen "quién" escribe —
solo que sea alguien que ha cargado la página (usuario anónimo válido) y
que escriba dentro de la zona de `sessions`. Es una mejora grande frente a
dejar toda la base abierta, pero no sustituye a tener un control de acceso
real si en el futuro se necesita más seguridad (por ejemplo, Firebase App
Check, para bloquear bots que solo conocen la URL del proyecto).

## Cómo publicar un cambio en las reglas

Firebase no lee este archivo automáticamente: hay que pegarlo a mano en la
consola cada vez que cambie.

1. Abre la consola de Firebase del proyecto:
   https://console.firebase.google.com/project/kobo-2aea4/database/kobo-2aea4-default-rtdb/rules
2. Borra el contenido del editor y pega el contenido de `database.rules.json`.
3. Pulsa **Publicar**.

## Reportar una vulnerabilidad

Si detectas un problema de seguridad en esta app, abre un issue en este
repositorio describiéndolo.
