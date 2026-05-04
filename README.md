# REIS Logística — Web Repo

Repositorio estático para campañas y landings de **REIS Logística**, desplegado en **Vercel**.

> **Estado actual:** la landing principal en producción es `landing/warehousing`. El resto del repo es scaffolding para próximas landings y migración futura del sitio principal (actualmente en Wix).

---

## 🗂 Estructura

```
reis-logistica/
├── index.html                         ← Hub interno (noindex)
├── vercel.json                        ← Config de Vercel + redirects + headers
├── robots.txt
├── sitemap.xml
├── .gitignore
├── README.md
│
├── landing/
│   ├── warehousing/
│   │   └── index.html                 ← LANDING ACTIVA (rediseñada)
│   └── comercio-exterior/
│       └── index.html                 ← Placeholder
│
├── nosotros/                          ← Reservado para migración futura
├── servicios/                         ← Reservado para migración futura
├── ubicaciones/                       ← Reservado para migración futura
├── contacto/                          ← Reservado para migración futura
│
└── assets/
    ├── img/
    │   ├── hero.avif                   ← Hero aerial
    │   ├── about-facility.jpg          ← Sección "Somos REIS"
    │   ├── service-3pl.webp
    │   ├── service-immex.jpg
    │   ├── service-logistica.jpg
    │   └── almacenes/                  ← Carrusel (5 fotos)
    │       ├── 01-slp-aurora.jpg
    │       ├── 02-mty-e1a.jpg
    │       ├── 03-juarez-eastlake.jpg
    │       ├── 04-slp-docks.jpg
    │       └── 05-slp-green.jpg
    └── video/
        └── operaciones.mp4             ← Reel operaciones (autoplay)
```

---

## 🎨 Sistema de diseño — REIS (Light Tech)

Tokens en línea dentro de cada landing (auto-contenido para Vercel `@vercel/static`).

| Token | Valor | Uso |
|---|---|---|
| `--bg` | `#F4F7F8` | Background principal (off-white frío) |
| `--bg-alt` | `#EAF0F2` | Background alterno |
| `--bg-card` | `#FFFFFF` | Cards y superficies elevadas |
| `--bg-dark` | `#0b1a22` | Secciones oscuras de contraste (CTA, footer) |
| `--green` | `#2ea73a` | CTA primario, acento principal |
| `--green-deep` | `#248a2f` | Hover / texto sobre claro |
| `--green-2` | `#37ae40` | Acento sobre fondo oscuro |
| `--blue` | `#145474` | Acento secundario |
| `--ink` | `#0b1a22` | Texto principal sobre claro |
| `--ink-mute` | `#5c7884` | Texto secundario |
| `--ink-dim` | `#8aa0a8` | Labels / fineprint |
| `--line` | `#dee5e8` | Borders sutiles |

**Tipografía:** Titillium Web (300/400/600/700/900) + JetBrains Mono (datos, labels técnicos).

**Estética:** light tech profesional. Off-white con cards blancas, texto navy, acentos verdes en CTAs, una sección oscura de contraste para el form, grid sutil de fondo, números grandes con counters animados, bento grid para diferenciadores y carrusel con imágenes reales de bodegas.

---

## 🖼 Assets (Warehousing)

| Archivo | Uso |
|---|---|
| `assets/img/hero.avif` | Imagen del hero (vista aérea anochecer) |
| `assets/img/about-facility.jpg` | Sección "Somos REIS" (fachada) |
| `assets/img/service-3pl.webp` | Card servicio 3PL |
| `assets/img/service-immex.jpg` | Card servicio IMMEX |
| `assets/img/service-logistica.jpg` | Card servicio Logística |
| `assets/img/almacenes/01-slp-aurora.jpg` | Carrusel — SLP Aurora |
| `assets/img/almacenes/02-mty-e1a.jpg` | Carrusel — MTY E1-A |
| `assets/img/almacenes/03-juarez-eastlake.jpg` | Carrusel — Cd. Juárez |
| `assets/img/almacenes/04-slp-docks.jpg` | Carrusel — SLP andenes |
| `assets/img/almacenes/05-slp-green.jpg` | Carrusel — SLP acceso verde |
| `assets/video/operaciones.mp4` | Video reel sección operaciones (autoplay muted loop) |

> **Nota de performance:** el video pesa ~20 MB y `about-facility.jpg` ~1.65 MB. Antes de prod conviene comprimir (`ffmpeg -crf 28` para video, redimensionar a 2400px máx el JPG).

---

## 🚀 Deploy en Vercel

### Setup inicial

1. Crear repo en GitHub:
   ```bash
   git init
   git add .
   git commit -m "feat: initial REIS Logística repo with warehousing landing"
   git branch -M main
   git remote add origin git@github.com:<tu-usuario>/reis-logistica.git
   git push -u origin main
   ```

2. En Vercel:
   - **Import Project** → seleccionar el repo
   - Framework preset: **Other**
   - Build command: *(dejar vacío)*
   - Output directory: *(dejar vacío)*
   - Deploy

3. Dominio sugerido (ya existe `landing.reislogistica.com` para el flow GHL — usar uno distinto):
   - `warehousing.reislogistica.com` → `/landing/warehousing`
   - O genérico: `reislogistica.vercel.app`

### Redirects ya configurados en `vercel.json`

| URL corta | Destino |
|---|---|
| `/warehousing` | `/landing/warehousing` |
| `/comercio-exterior` | `/landing/comercio-exterior` |

---

## 🔌 Integraciones pendientes (warehousing)

El form actual usa **WhatsApp como fallback**. Para producción, conectar con GHL:

```js
// En landing/warehousing/index.html, dentro del submit handler:
const ghlPayload = {
  nombre:           data.nombre,
  email:            data.email,
  phone:            data.telefono,
  source:           'warehousing landing',
  empresa:          data.empresa,
  tipo_de_servicio: data.servicio,
  ubicacion:        data.ubicacion,
  mensaje:          data.mensaje,
};

await fetch('<GHL_WEBHOOK_URL>', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify(ghlPayload)
});

window.location.href = '/gracias';  // crear /gracias/index.html después
```

### Meta Pixel
Si se va a correr campaña en Meta para esta landing, agregar el snippet del Pixel en `<head>` y disparar `Lead` al submit. (Patrón ya usado en `landing.reislogistica.com` con Pixel ID `930475916097890`.)

---

## 📋 Checklist próximas iteraciones

- [ ] Conectar form a webhook GHL (warehousing)
- [ ] Agregar Meta Pixel + evento `Lead`
- [ ] Crear `/gracias/index.html` con confirmación
- [ ] Migrar fotos de bodegas a `assets/img/` (evitar dependencia de Wixstatic)
- [ ] Construir landing `comercio-exterior` (mismo sistema de diseño)
- [ ] Considerar migración del sitio principal de Wix → Vercel

---

## 📞 Contactos REIS

| Área | Persona | Contacto |
|---|---|---|
| Ventas SLP / MTY | Juan Manuel Nava | `juan.n@reisbl.com` · +52 444 140 6149 |
| Comercio Exterior | Rafael Alvarado | `rafael.a@reisbl.com` · +52 656 352 3565 |
| Cd. Juárez | Orlando Rubio | `orlando.r@reisbl.com` · +52 444 719 7812 |

**Dirección general:** Eje 114 No. 590, Zona Industrial, San Luis Potosí, S.L.P. 78395

---

© 2026 REIS Logistics
