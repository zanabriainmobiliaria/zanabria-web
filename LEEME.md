# Zanabria Inmobiliaria — web y panel de carga

Sitio estático con panel de administración propio. No hay que programar nada:
todo el contenido se carga desde `tusitio.com/admin`.

---

## 1. Publicar el sitio (se hace una sola vez, unos 20 minutos)

**Paso 1 — Subir los archivos a GitHub**
1. Creá una cuenta en [github.com](https://github.com) si no tenés.
2. Botón verde **New repository** → nombre `zanabria-web` → **Private** → **Create**.
3. En la pantalla siguiente, **uploading an existing file** → arrastrá *todos* los
   archivos y carpetas de este paquete → **Commit changes**.

**Paso 2 — Conectar Netlify**
1. Entrá a [netlify.com](https://netlify.com) y registrate **con tu cuenta de GitHub**.
2. **Add new site** → **Import an existing project** → **GitHub** → elegí `zanabria-web`.
3. Dejá todo como viene (sin build command, publish directory `.`) → **Deploy**.
4. En un minuto tenés la web andando en una dirección tipo `algo-random.netlify.app`.
   Podés cambiarla en **Site configuration → Change site name**.

**Paso 3 — Activar el panel**

> Netlify marcó **Git Gateway** como deprecado: sigue andando pero no lo recomiendan
> para configuraciones nuevas. Probá este paso; si el botón de Git Gateway no aparece
> o da error, saltá al **Plan B** que está al final de este archivo.

1. En el panel de Netlify: **Site configuration → Identity → Enable Identity**.
2. Dentro de Identity: **Registration preferences → Invite only**.
   (Importante: si queda abierto, cualquiera puede registrarse.)
3. Más abajo: **Services → Git Gateway → Enable Git Gateway**.
4. Pestaña **Identity → Invite users** → poné tu mail → **Send**.
5. Te llega un mail, tocás el link, creás tu contraseña.
6. Listo: entrás a `tusitio.netlify.app/admin`.

Para darle acceso a Ignacio, repetí el paso 4 con su mail.

---

## 2. Usar el panel

Entrás a `/admin` y ves tres secciones:

### Propiedades
Todas juntas en una lista. **Add Propiedad** para sumar una nueva, la flecha para
desplegarla, y se arrastran para cambiar el orden.

- **Los campos vacíos no se muestran.** Si no cargás la antigüedad, esa línea
  simplemente no aparece en la ficha. Nunca queda un dato en blanco.
- **Fotos:** subilas del tamaño que sean, se recortan solas. La primera es la portada.
- **Precio vacío** → en la web dice "Consultar".
- **Publicada en la web:** apagá el interruptor para sacarla sin borrar nada.

**Marcar una propiedad como vendida o alquilada:**
1. Cambiá *Estado de la operación* a **Vendida** o **Alquilada**.
2. Cargá la *Fecha en que se cerró*.
3. Guardá.

La propiedad queda a la vista con el cartel, la ficha cambia el formulario por un
"busco algo parecido", y a los **12 días se retira sola**. Ese plazo se cambia en
**Sitio → Ajustes**.

### Proyectos en pozo
Renders, fotos de la obra, números destacados, ventajas y financiación. Mientras el
link del recorrido 3D esté vacío, la web muestra "Próximamente" en todos lados.

### Sitio
Contacto, matrícula, videos, agenda, imágenes de búsqueda personalizada, estadísticas
y ajustes. Es lo que casi nunca vas a tocar.

**Guardar y publicar:** el botón **Publish** de arriba a la derecha. El cambio tarda
un minuto en verse (Netlify vuelve a publicar el sitio solo).

---

## 3. Cargar los videos

Subilos a YouTube como **no listados** y pegá en el panel solo el ID:
en `youtube.com/watch?v=**dQw4w9WgXcQ**`, el ID es `dQw4w9WgXcQ`.

No los subas como archivo salvo que pesen menos de 20 MB: GitHub no está hecho para
video y la web se pondría lenta.

---

## 4. Agenda de Google Calendar

1. Google Calendar → **Crear** → **Programación de citas**.
2. Duración **15 minutos**.
3. Disponibilidad: **lunes a viernes y sábado, de 9 a 13**.
4. Activá que pida nombre, mail y teléfono.
5. **Opciones de uso compartido → Insertar en el sitio web**.
6. Del código que te da, copiá lo que va después de `/schedules/` y pegalo en
   **Sitio → Agenda de llamadas → ID de la agenda de Google**.

Hasta que hagas esto, el formulario de la web deriva a WhatsApp.

---

## 5. Tipografía

La web está preparada para **Helvena Grotesk**. Cuando tengas la licencia, creá una
carpeta `fuentes` junto a `index.html` y poné adentro los archivos `.woff2` con estos
nombres exactos:

```
fuentes/HelvenaGrotesk-Regular.woff2
fuentes/HelvenaGrotesk-Medium.woff2
fuentes/HelvenaGrotesk-SemiBold.woff2
fuentes/HelvenaGrotesk-Bold.woff2
fuentes/HelvenaGrotesk-ExtraBold.woff2
fuentes/HelvenaGrotesk-Black.woff2
```

La toma sola. Mientras tanto usa Inter, que es la más parecida disponible gratis.

---

## 6. Qué archivo es qué

| Archivo | Para qué sirve |
|---|---|
| `index.html` | La web. No hace falta abrirlo. |
| `admin/` | El panel de carga. |
| `data/*.json` | El contenido. Lo escribe el panel solo. |
| `assets/img/` | Las fotos que subís desde el panel. |
| `logo-zanabria-*.png` | Los logos con fondo transparente. |
| `netlify.toml` | Configuración de publicación. |

**Nunca edites `data/*.json` a mano.** Para eso está el panel.


---

## 7. Plan B: si no podés activar Git Gateway

En vez de crear usuarios del panel, entrás con tu propia cuenta de GitHub.

1. **GitHub** → foto de perfil arriba a la derecha → **Settings** → abajo de todo en el
   menú izquierdo **Developer settings** → **OAuth Apps** → **New OAuth App**.
2. Completá:
   - *Application name*: `Panel Zanabria`
   - *Homepage URL*: la dirección de tu sitio en Netlify
   - *Authorization callback URL*: `https://api.netlify.com/auth/done`
3. **Register application** → copiá el **Client ID** → **Generate a new client secret**
   → copiá el secret (se muestra una sola vez).
4. En **Netlify**, en tu sitio: **Site configuration** → **Access & security** →
   **OAuth** → **Install provider** → GitHub → pegá Client ID y Client Secret.
5. Abrí `admin/config.yml`, borrá las 6 líneas del bloque `backend: name: git-gateway`
   y descomentá el bloque de la Opción B (sacale el `#` a cada línea), cambiando
   `USUARIO/zanabria-web` por tu usuario y el nombre de tu repositorio.
6. Subí el archivo cambiado a GitHub. Entrás a `/admin` y ahora te pide login con GitHub.

## 8. Plan C: editar sin panel

Si algún día el panel falla, el contenido nunca queda encerrado: entrá a tu repositorio
en GitHub, abrí `data/propiedades.json`, tocá el lápiz de arriba a la derecha, editás y
**Commit changes**. Es feo pero funciona siempre.
