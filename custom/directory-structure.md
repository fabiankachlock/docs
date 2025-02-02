# Ordner Struktur

Slidev verwendet einige Verzeichnisstruktur-Konventionen, um die Konfigurationsoberfläche zu minimieren und die Erweiterungen flexibel und intuitiv zu gestalten.


Der Grundaufbau ist wie folgt:

```bash
your-slidev/
  ├── components/       # eigene Komponenten
  ├── layouts/          # eigene Layouts
  ├── public/           # statische Assets
  ├── setup/            # eigenes Setup / Hooks
  ├── styles/           # eigene Styles
  ├── index.html        # injektionen in index.html
  ├── slides.md         # Haupt-Eingangspunkt
  └── vite.config.ts    # vite-Konfiguration erweitern
```

Alle sind optional.

## Components

Konventionen: `./components/*.{vue,js,ts,jsx,tsx,md}`

Komponenten innerhalb dieses Ordners können direkt in den Folien Markdown mit dem gleichen Komponentennamen wie der Dateiname verwendet werden.

Zum Beispiel:

```bash
your-slidev/
  ├── ...
  └── components/
      ├── MeineKomponente.vue
      └── HalloWelt.ts
```

```md
<!-- slides.md -->

# My Slide

<MeineKomponente :count="4"/>

<!-- beide Namen funktionieren -->

<hallo-welt foo="bar">
  Slot
</hallo-welt>
```

Diese Funktion wird von [`vite-plugin-components`](https://github.com/antfu/vite-plugin-components) bereitgestellt, erfahre dort mehr darüber.

Slidev stellt auch einige [eingebaute Komponenten](/builtin/components) zur Verfügung, die man verwenden kann.

## Layouts

Konventionen: `./layouts/*.{vue,js,ts,jsx,tsx}`

```
your-slidev/
  ├── ...
  └── layouts/
      ├── cover.vue
      └── mein-tolles-theme.vue
```

Man kann einen beliebigen Dateinamen für das Layout verwenden. Verwende das Layout dann in dem YAML-Header über den Dateinamen.


```yaml
---
layout: mein-tolles-theme
---
```

Wenn das eigene Layout den gleichen Namen, wie ein integriertes Layout oder ein Themenlayout hat, dann hat das benutzerdefinierte Layout den Vorrang vor deM Integrierten- oder Themenlayout. Die Prioritätsreihenfolge ist `Lokal > Theme > Integriert`.

Verwende `<slot />` in der Layoutkomponente für den Folieninhalt. Zum Beispiel:

```html
<!-- default.vue -->
<template>
  <div class="slidev-layout default">
    <slot />
  </div>
</template>
```

## Public

Konvetionen: `./public/*`

Assets in diesem Verzeichnis werden nach der Umwandlung in eine SPA im Wurzelpfad `/` bereitgestellt und in das Wurzelverzeichnis des Dist-Verzeichnisses kopiert. Lesen mehr über [Vites `public`-Verzeichnis](https://vitejs.dev/guide/assets.html#the-public-directory).

## Style

Koventionen: `./style.css` | `./styles/index.{css,js,ts}`

Dateien, die dieser Konvention folgen, werden in das Stammverzeichnis der App injiziert. Wenn man mehrere css-Einträge importieren möchte, kann man die folgende Struktur erstellen und die Importreihenfolge selbst verwalten.

```bash
your-slidev/
  ├── ...
  └── styles/
      ├── index.ts
      ├── base.css
      ├── code.css
      └── layouts.css
```

```ts
// styles/index.ts

import './base.css'
import './code.css'
import './layouts.css'
```

Stile werden von [Windi CSS](http://windicss.org/) und [PostCSS](https://postcss.org/) verarbeitet, so dass man css-Schachtelungen und [at-Direktiven](https://windicss.org/features/directives.html) out-of-box verwenden kann. Zum Beispiel:


```less
.slidev-layout {
  @apply px-14 py-10 text-[1.1rem];

  h1, h2, h3, h4, p, div {
    @apply select-none;
  }

  pre, code {
    @apply select-text;
  }

  a {
    color: theme('colors.primary');
  }
}
```

[Erfahre mehr über den Syntax](https://windicss.org/features/directives.html).

## `index.html`

Konvetionen: `index.html`

Die `index.html` bietet die Möglichkeit, Meta-Tags und/oder Skripte in die Haupt-`index.html` zu injizieren.

Zum Beispiel für die folgende eigene `index.html`:

```html
<!-- ./index.html -->
<head>
  <link rel="preconnect" href="https://fonts.gstatic.com">
  <link href="https://fonts.googleapis.com/css2?family=Fira+Code:wght@400;600&family=Nunito+Sans:wght@200;400;600&display=swap" rel="stylesheet">
</head>

<body>
  <script src="./your-scripts"></script>
</body>
```

Die fertig gehostete `index.html` wird sein:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link rel="icon" type="image/png" href="https://cdn.jsdelivr.net/gh/slidevjs/slidev/assets/favicon.png">
  <!-- injected head -->
  <link rel="preconnect" href="https://fonts.gstatic.com">
  <link href="https://fonts.googleapis.com/css2?family=Fira+Code:wght@400;600&family=Nunito+Sans:wght@200;400;600&display=swap" rel="stylesheet">
</head>
<body>
  <div id="app"></div>
  <script type="module" src="__ENTRY__"></script>
  <!-- injected body -->
  <script src="./your-scripts"></script>
</body>
</html>
```

## Globale Ebenen

Konvetionen: `global-top.vue` | `global-bottom.vue`

Erfahre mehr: [Globale Ebenen](/custom/global-layers)

