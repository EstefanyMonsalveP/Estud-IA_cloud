# Reto de red · sesión 3

El mismo proyecto de la solución, roto a propósito. No mires `main` hasta haber intentado las cuatro etapas. La clave del `.env.example` sigue siendo ficticia.

## Etapa 1 · el entorno no funciona

`docker compose up --build` no deja el servicio sano. Hay tres causas a la vez:

- El backend habla con `localhost`. Dentro de su contenedor, eso es él mismo, no la base.
- El puerto publicado del frontend es uno que suele estar ocupado.
- La base no tiene healthcheck, y otro servicio la espera con `condition: service_healthy`.

Arregla esas tres y consigue que `/health` responda.

## Etapa 2 · aísla la base

Ahora mismo los tres servicios comparten una sola red y la base publica `5432` en el host. Separa las redes para que el frontend no alcance la base, y quita ese puerto publicado. Compruébalo sin instalar `ping` ni `curl` en las imágenes: `getent` en el frontend, o `python comprobar_nombre.py` en el backend. `docker network ls` y `docker network inspect` dicen quién está en cada red.

## Etapa 3 · el dato y la clave

- Inserta un ítem, haz `docker compose down` y vuelve a levantar. El ítem tiene que seguir.
- Prueba `docker compose down -v` y anota qué desaparece.
- La clave vive en `.env`, copiado desde `.env.example`. `docker compose config` muestra el archivo ya interpolado. Confirma con `git status` que `.env` no se va a commitear.

## Etapa 4 · README de cinco líneas

Sustituye el `README.md` de esta carpeta por cinco líneas: qué es, cómo se copia el entorno, cómo se levanta, cómo se comprueba `/health` y cómo se apaga sin borrar los datos.
