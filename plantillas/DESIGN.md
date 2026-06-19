---
version: alpha
name: <Nombre del sistema>
description: <una línea sobre la dirección visual>
colors:
  primary: "#1A1C1E"
  secondary: "#6C7278"
  tertiary: "#B8422E"
  neutral: "#F7F5F2"
typography:
  h1:
    fontFamily: Public Sans
    fontSize: 3rem
    fontWeight: 700
  body-md:
    fontFamily: Public Sans
    fontSize: 1rem
  label-caps:
    fontFamily: Space Grotesk
    fontSize: 0.75rem
rounded:
  sm: 4px
  md: 8px
spacing:
  sm: 8px
  md: 16px
components:
  button-primary:
    backgroundColor: "{colors.tertiary}"
    textColor: "{colors.neutral}"
    rounded: "{rounded.sm}"
    padding: 12px
---

<!--
Orden OBLIGATORIO de secciones (omitir está ok, repetir rechaza el archivo):
Overview → Colors → Typography → Layout → Elevation & Depth → Shapes → Components → Do's and Don'ts
Tokens válidos en components: backgroundColor, textColor, typography, rounded, padding, size, height, width.
Referencias entre tokens: {ruta.al.token}
-->

## Overview
La experiencia general que debería crear el producto (1-3 frases con carácter).

## Colors
La paleta y para qué sirve cada color.
- Primary (#...): ...
- Secondary (#...): ...
- Tertiary (#...): el único motor de interacción.
- Neutral (#...): base.

## Typography
Escala, fuentes, pesos, interlineado y jerarquía.

## Layout
Grillas, ancho de página, escala de espaciado, densidad y responsive.

## Components
Botones, cards, formularios, navegación, tablas, modales, alertas, estados vacíos y de carga.

## Do's and Don'ts
**La sección más importante.** Los patrones, tratamientos y comportamientos a EVITAR.
Ej.: "Nunca usar un acento para un botón primario", "nunca más de un color de acento por vista".
