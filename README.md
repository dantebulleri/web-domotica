# DomotiCA Rosario — Landing B2B

Landing page B2B para captación de reuniones con instaladores eléctricos, constructoras y empresas de seguridad en Rosario, Argentina.

**Sin carrito. Sin precios públicos. Objetivo único: generar contactos calificados.**

---

## Por qué Astro + Tailwind

Se eligió **Astro (App Router estático) + Tailwind CSS** por las siguientes razones:

- **Cero JS en el cliente por defecto** — ideal para una landing donde el contenido es estático. Menor bundle = mayor velocidad.
- **SEO perfecto** — el HTML se genera en build time, indexable inmediatamente.
- **Deploy trivial** — genera una carpeta `dist/` con HTML/CSS/JS estático compatible con cualquier CDN.
- **Sin overhead de framework** — a diferencia de Next.js o Vite+React, no se necesita hidratación para esta use case.
- **Tailwind integrado nativamente** vía `@astrojs/tailwind`.

---

## Estructura del proyecto

```
web-domotica/
├── public/
│   ├── favicon.svg              # Favicon del sitio
│   ├── og-image.png             # TODO: Reemplazar con imagen OG real (1200x630px)
│   ├── catalogo.pdf             # TODO: Reemplazar con catálogo PDF real
│   └── images/
│       ├── kit-starter.jpg      # TODO: Foto real del Kit Starter
│       ├── kit-riego.jpg        # TODO: Foto real del Kit Riego
│       └── kit-depto.jpg        # TODO: Foto real del Kit Departamento
├── src/
│   ├── layouts/
│   │   └── Layout.astro         # Layout base: SEO, meta OG, Google Fonts
│   ├── pages/
│   │   └── index.astro          # Página principal (ensambla todos los componentes)
│   └── components/
│       ├── Header.astro         # Nav sticky con menú mobile
│       ├── Hero.astro           # Sección principal con CTAs
│       ├── ParaQuienEs.astro    # Cards por perfil de cliente
│       ├── Kits.astro           # Los 3 kits/soluciones
│       ├── ComoFunciona.astro   # 3 pasos del proceso
│       ├── ProgramaInstaladores.astro  # Beneficios del programa partner
│       ├── Compatibilidad.astro # Ecosistemas y tabla de compatibilidad
│       ├── FAQ.astro            # 6 preguntas frecuentes (accordion)
│       ├── Contacto.astro       # Formulario + info de contacto
│       ├── Disclaimer.astro     # Aviso honesto de stock y condiciones
│       ├── Footer.astro         # Pie de página
│       └── WhatsAppButton.astro # Botón flotante de WhatsApp
├── astro.config.mjs
├── tailwind.config.mjs
├── tsconfig.json
├── package.json
└── README.md
```

---

## Instalación y desarrollo

### Requisitos
- Node.js 18+ (recomendado: 20 LTS)
- npm, pnpm o yarn

### Instalar dependencias

```bash
npm install
```

### Servidor de desarrollo

```bash
npm run dev
```

Abre [http://localhost:4321](http://localhost:4321) en el navegador.

### Build para producción

```bash
npm run build
```

Genera la carpeta `dist/` lista para deploy.

### Preview del build local

```bash
npm run preview
```

---

## Configuración obligatoria antes del deploy

### 1. Número de WhatsApp

Buscar y reemplazar el placeholder en los siguientes archivos:

```
src/components/Header.astro        → const WA_NUMBER = '5493413000000';
src/components/Hero.astro          → const WA_NUMBER = '5493413000000';
src/components/ProgramaInstaladores.astro → const WA_NUMBER = '5493413000000';
src/components/Contacto.astro      → const WA_NUMBER = '5493413000000';
src/components/WhatsAppButton.astro → const WA_NUMBER = '5493413000000';
src/components/Footer.astro        → const WA_NUMBER = '5493413000000';
```

**Formato correcto para Argentina:**
```
54 + código de área sin el 0 inicial + número sin el 15
Ejemplo Rosario: 54 + 341 + 1234567 → 5493411234567
```

### 2. Formulario de contacto (Formspree)

1. Crear cuenta gratuita en [formspree.io](https://formspree.io)
2. Crear un nuevo formulario
3. Copiar el ID (ejemplo: `xpwzaabc`)
4. En `src/components/Contacto.astro`, reemplazar:

```js
const FORMSPREE_ID = 'YOUR_FORM_ID'; // → reemplazar con tu ID
```

**Alternativa sin cuenta (mailto):**

Cambiar la línea de `FORM_ACTION` en `Contacto.astro`:
```js
const FORM_ACTION = 'mailto:tu@email.com?subject=Contacto Web B2B';
```

> ⚠️ La versión `mailto:` abre el cliente de correo del usuario. Funciona sin backend pero la experiencia es peor. Formspree es la opción recomendada.

### 3. Dominio en astro.config.mjs

```js
// astro.config.mjs
export default defineConfig({
  site: 'https://tu-dominio-real.com', // Reemplazar
});
```

Esto es necesario para las URLs canónicas y los meta OpenGraph correctos.

### 4. Catálogo PDF

Reemplazar `public/catalogo.pdf` con el PDF real del catálogo.

El archivo actual es un placeholder. El botón "Descargar catálogo" lo descarga directamente desde el navegador.

### 5. Imágenes de productos

Reemplazar los archivos en `public/images/`:
- `kit-starter.jpg` — Foto del Kit Starter Iluminación Smart
- `kit-riego.jpg` — Foto del Kit Riego Inteligente WiFi
- `kit-depto.jpg` — Foto del Kit Departamento Smart

**Especificaciones recomendadas:**
- Formato: JPG o WebP
- Tamaño: 800×600px mínimo (relación 4:3)
- Peso: menos de 200 KB por imagen
- Fondo neutro (blanco o gris claro)

> Mientras no haya imágenes reales, la web muestra un placeholder gris con icono. El fallback está implementado en `Kits.astro` con el atributo `onerror`.

### 6. Imagen OpenGraph

Reemplazar `public/og-image.png` con una imagen real de 1200×630px.

Esta imagen aparece cuando el enlace de la web se comparte en WhatsApp, LinkedIn, etc.

**Opción rápida:** Crear la imagen en [Canva](https://canva.com) con el logo, nombre y tagline del negocio.

---

## Deploy

### Opción A: Vercel (recomendado)

1. Importar el repositorio en [vercel.com](https://vercel.com)
2. Vercel detecta Astro automáticamente
3. Deploy automático en cada push a `main`

```bash
# O desde CLI:
npm i -g vercel
vercel
```

### Opción B: Netlify

1. Importar el repositorio en [netlify.com](https://netlify.com)
2. Configurar:
   - **Build command:** `npm run build`
   - **Publish directory:** `dist`
3. Deploy automático en cada push

### Opción C: GitHub Pages

Requiere configurar `base` en `astro.config.mjs` si se usa en subdirectorio:

```js
export default defineConfig({
  site: 'https://usuario.github.io',
  base: '/nombre-repo', // solo si no es el repo principal
});
```

Usar GitHub Actions con el workflow oficial de Astro:
- [Guía oficial Astro + GitHub Pages](https://docs.astro.build/en/guides/deploy/github/)

---

## TODOs antes de lanzar

### Contenido obligatorio
- [ ] Reemplazar número de WhatsApp en todos los componentes
- [ ] Configurar Formspree (o definir mailto alternativo)
- [ ] Actualizar `astro.config.mjs` con el dominio real
- [ ] Reemplazar `public/catalogo.pdf` con el catálogo real
- [ ] Reemplazar imágenes en `public/images/` con fotos reales de los productos
- [ ] Reemplazar `public/og-image.png` con imagen 1200×630px para redes

### Contenido opcional
- [ ] Agregar testimonios o casos de uso reales (sección nueva o en Hero)
- [ ] Agregar nombre/datos de contacto real en Footer
- [ ] Conectar dominio propio (ver guía de deploy)
- [ ] Configurar Google Analytics o Plausible (analytics sin cookies)
- [ ] Agregar Google Search Console tras indexar

### Técnico
- [ ] Verificar velocidad en [PageSpeed Insights](https://pagespeed.web.dev/) tras deploy
- [ ] Verificar meta OG con [og.xyz](https://og.xyz/) o [opengraph.xyz](https://www.opengraph.xyz/)
- [ ] Agregar `robots.txt` si es necesario restringir indexación
- [ ] Revisar formulario de contacto en producción (enviar test desde Formspree)

---

## Paleta de colores y tipografía

| Token | Valor | Uso |
|-------|-------|-----|
| `primary` | `#1E2A38` | Fondo header, nav, cards |
| `accent` | `#2DBE7E` | CTAs, highlights, checks |
| `bgalt` | `#F2F4F7` | Secciones alternas |
| `ink` | `#111111` | Texto principal |

**Fuentes (Google Fonts):**
- `Inter` — texto corrido, labels, botones
- `Montserrat` — títulos (H1-H3)

---

## Notas de desarrollo

- **Sin backend** — todo el formulario va a Formspree o mailto.
- **Sin carrito ni pasarela** — por diseño de negocio.
- **Sin precios públicos** — por diseño de negocio.
- El botón de WhatsApp flotante está en `WhatsAppButton.astro` y se expande al hover en desktop.
- El menú mobile tiene toggle JS mínimo en `Header.astro`.
- El FAQ accordion está en `FAQ.astro` con JS vanilla.
- El formulario maneja submit/error/success states sin recarga de página.
