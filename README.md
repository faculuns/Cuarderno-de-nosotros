# Cuaderno de Nosotros — guía de instalación

Esta carpeta es una app web (PWA) instalable en iPhone y Android, sin pasar por
las tiendas de aplicaciones. Para que los dos vean los mismos recuerdos en
tiempo real, usa una base de datos gratuita de Firebase (Google).

Vas a hacer esto **una sola vez**. Te lleva entre 15 y 25 minutos.

---

## Parte 1 — Crear la base de datos en Firebase

1. Andá a **https://console.firebase.google.com** y entrá con una cuenta de Google.
2. Click en **"Agregar proyecto"**. Ponele un nombre (ej: `cuaderno-de-nosotros`) y seguí los pasos (podés desactivar Google Analytics, no hace falta).
3. Una vez creado el proyecto, en el menú izquierdo andá a **Compilación > Firestore Database**.
4. Click en **"Crear base de datos"**.
   - Elegí la ubicación más cercana (por ejemplo `southamerica-east1`).
   - Elegí **"Iniciar en modo de prueba"** (test mode). Esto te da 30 días de acceso abierto; después vas a tener que ajustar las reglas (ver Parte 3).
5. Ahora registrá una "app web": en el ícono de engranaje (⚙) junto a "Descripción general del proyecto" → **Configuración del proyecto** → abajo en "Tus apps" → click en el ícono `</>` (Web).
6. Ponele un apodo a la app (ej: `cuaderno-web`) y click en **"Registrar app"**. NO hace falta activar Firebase Hosting.
7. Firebase te va a mostrar un bloque de código con un objeto `firebaseConfig`. Copiá esos valores.

## Parte 2 — Completar la configuración en el proyecto

1. Abrí el archivo **`firebase-config.js`** de esta carpeta.
2. Reemplazá cada `"PEGA_ACA_TU_..."` con el valor correspondiente que copiaste de Firebase.
3. Guardá el archivo.

## Parte 3 — Reglas de seguridad de Firestore (importante)

Por defecto, el "modo de prueba" deja la base de datos abierta por 30 días y
después se bloquea sola. Para que la app siga funcionando después de esos 30
días, andá a **Firestore Database > Reglas** y reemplazá el contenido por:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if true;
    }
  }
}
```

Click en **"Publicar"**.

> ⚠️ Esto deja la base de datos accesible para cualquiera que conozca tu
> `projectId` (por ejemplo, si alguien mirara el código fuente de tu app). Es
> razonable para un proyecto personal como este, pero no subas la carpeta a un
> repositorio público de GitHub con el `firebase-config.js` completado si te
> preocupa la privacidad — usá un repositorio **privado** (ver Parte 4).

## Parte 4 — Publicar la app con GitHub Pages

1. Creá una cuenta en **https://github.com** si no tenés una.
2. Click en **"New repository"**. Nombre sugerido: `cuaderno-de-nosotros`.
   Marcalo como **Private** si te preocupa que el `firebase-config.js` quede visible.
3. Subí **todos los archivos de esta carpeta** (`index.html`, `manifest.json`,
   `sw.js`, `firebase-config.js`, la carpeta `icons/`) usando el botón
   **"Add file" → "Upload files"** en GitHub, arrastrando la carpeta completa.
4. Andá a **Settings → Pages** (menú izquierdo del repositorio).
5. En "Build and deployment" elegí **Source: Deploy from a branch**, branch
   **main**, carpeta **/ (root)**. Guardá.
6. Esperá 1-2 minutos. GitHub te va a dar una URL tipo:
   `https://tu-usuario.github.io/cuaderno-de-nosotros/`

   > Nota: si el repo es privado, GitHub Pages requiere un plan pago (GitHub
   > Pro) para publicarlo. Si querés mantenerlo privado y gratis, usá
   > **Firebase Hosting** en su lugar (alternativa más abajo) o simplemente
   > usá un repo público — la app en sí no expone nada sensible si aplicaste
   > las reglas de Firestore.

## Parte 5 — Instalar en el celular

**iPhone (Safari):**
1. Abrí la URL de GitHub Pages en Safari.
2. Tocá el ícono de compartir (cuadrado con flecha hacia arriba).
3. Elegí **"Agregar a pantalla de inicio"**.

**Android (Chrome):**
1. Abrí la URL en Chrome.
2. Tocá el menú (⋮) → **"Instalar app"** o **"Agregar a pantalla de inicio"**.

Ambos van a tener un ícono como cualquier app, pantalla completa, y los
recuerdos se sincronizan solos entre los dos celulares.

---

## Alternativa: Firebase Hosting (si preferís todo dentro de Firebase)

En vez de GitHub Pages, podés instalar Firebase CLI (`npm install -g
firebase-tools`) y correr `firebase init hosting` + `firebase deploy` desde
esta carpeta. Es un poco más técnico pero te permite mantener todo privado sin
pagar nada. Si querés, te puedo dar los comandos exactos.

## Si algo no funciona

- **La app dice "falta configurar"**: revisá que `firebase-config.js` tenga
  los valores reales, no los placeholders.
- **No se guardan los recuerdos**: revisá las reglas de Firestore (Parte 3).
- **No aparece el ícono al instalar**: asegurate de haber subido la carpeta
  `icons/` completa.
