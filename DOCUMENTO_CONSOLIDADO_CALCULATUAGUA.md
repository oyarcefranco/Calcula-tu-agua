# 📘 Documento Consolidado — calculatuagua.cl

## Especificación Completa del Producto

**Versión:** 2.0  
**Fecha:** Abril 2026  
**Proyecto:** Calcula Tu Agua — Herramienta de cálculo y educación sobre consumo de agua para familias vulnerables en Chile  
**Contexto académico:** Proyecto de Liderazgo — Negociación y Equipos, Universidad San Sebastián  
**Dominio planificado:** calculatuagua.cl (NIC Chile)

---

## Índice

1. [Descripción General](#1-descripción-general)
2. [Público Objetivo](#2-público-objetivo)
3. [Stack Tecnológico](#3-stack-tecnológico)
4. [Arquitectura y Flujo de Usuario](#4-arquitectura-y-flujo-de-usuario)
5. [Datos Utilizados](#5-datos-utilizados)
6. [Metodología de Cálculos](#6-metodología-de-cálculos)
7. [Funcionalidades Detalladas](#7-funcionalidades-detalladas)
8. [Diseño Visual y UX](#8-diseño-visual-y-ux)
9. [Accesibilidad](#9-accesibilidad)
10. [Integraciones](#10-integraciones)
11. [Fuentes y Referencias](#11-fuentes-y-referencias)
12. [Limitaciones](#12-limitaciones)
13. [Roadmap](#13-roadmap-futuro)

---

## 1. Descripción General

**Calcula Tu Agua** (anteriormente "HidroHogar") es una herramienta web gratuita que permite a familias chilenas, especialmente aquellas en situación de vulnerabilidad, calcular su consumo mensual de agua potable a partir de sus hábitos diarios.

### ¿Qué problema resuelve?

Muchas familias vulnerables en Chile:
- No comprenden cómo se calcula su boleta de agua
- No conocen el sistema de tramos tarifarios que penaliza el sobreconsumo
- Desconocen la existencia del Subsidio SAP al que podrían acceder
- No tienen visibilidad sobre qué hábitos generan mayor gasto de agua
- No saben cuánto podrían ahorrar con cambios simples

### ¿Qué entrega la herramienta?

1. **Estimación personalizada** del consumo diario, mensual (en litros y m³) y costo en pesos chilenos
2. **Comparación** con el promedio chileno para dimensionar si su consumo es alto o bajo
3. **Información del Subsidio SAP** con cálculo personalizado del ahorro según tramo RSH
4. **Proyección de ahorro** si adoptan hábitos eficientes (mensual, anual, a 5 años)
5. **Consejos priorizados** ordenados por impacto real en litros/mes
6. **Equivalencias económicas** del ahorro en bienes cotidianos (pan, pasajes, recargas)
7. **Información sobre programas de apoyo** (convenios de pago, kits municipales, revisión de medidor)

---

## 2. Público Objetivo

### Perfil primario
- **Familias chilenas vulnerables** (RSH 0-90%)
- Hogares de 3-6 personas
- Acceso principal a internet vía **celular** (smartphone Android)
- Nivel educacional variable, requiere lenguaje simple y visual
- Potenciales beneficiarios del Subsidio SAP

### Perfil secundario
- Cualquier hogar chileno que quiera entender y reducir su consumo de agua
- Profesionales de oficinas sociales municipales que asesoran a familias
- Educadores y facilitadores de talleres sobre ahorro de recursos

---

## 3. Stack Tecnológico

### Arquitectura: SPA — Single Page Application

| Componente | Tecnología | Justificación |
|-----------|------------|--------------|
| **Estructura** | HTML5 semántico | Compatibilidad universal |
| **Estilos** | CSS3 con Variables CSS | Mantenibilidad, tematización |
| **Lógica** | JavaScript puro (ES6+) | Sin dependencias, carga instantánea |
| **Tipografía** | Google Fonts (Inter) | Moderna, legible, gratis |
| **Persistencia** | localStorage | Offline, sin servidor |
| **Hosting planificado** | Cualquier hosting estático | GitHub Pages, Netlify, Hostinger, etc. |

### Dependencias externas
- **Google Fonts CDN** (`fonts.googleapis.com`) — Tipografía Inter
- **WhatsApp API** (`wa.me`) — Compartir resultados (solo al hacer clic)
- **Ningún framework JS** — 0 dependencias
- **Ningún backend** — 100% cliente

### Tamaño del archivo
- **1 archivo HTML** autocontenido (~42 KB)
- **0 imágenes** (todo SVG inline + emojis)
- **Carga estimada**: < 1 segundo en 3G

---

## 4. Arquitectura y Flujo de Usuario

### Flujo SPA de dos páginas

```
┌──────────────────────────────────────────────┐
│              PÁGINA DE ENTRADA               │
│                                              │
│  ┌─────────────────────────────────────────┐ │
│  │  🏢 Seleccionar empresa sanitaria       │ │
│  │  👥 Número de personas (1-8)            │ │
│  │  🚿 Ajustar 7 hábitos de consumo       │ │
│  │     · Ducha (min/día)                   │ │
│  │     · WC (veces/día)                    │ │
│  │     · Cepillado (veces/día)             │ │
│  │     · Lavado platos (veces/día)         │ │
│  │     · Lavadora (cargas/semana)          │ │
│  │     · Cocina (litros/día)               │ │
│  │     · Riego (veces/semana)              │ │
│  └─────────────────────────────────────────┘ │
│                                              │
│        [ 📊 Calcular mi consumo ]            │
│                                              │
└──────────────────┬───────────────────────────┘
                   │ click
                   ▼
┌──────────────────────────────────────────────┐
│              PÁGINA DE RESULTADOS            │
│                                              │
│  [← Volver a editar]                         │
│                                              │
│  📊 Stats: Litros/día | m³/mes | $ Costo    │
│  📶 Tramo tarifario (visual)                 │
│  📊 Comparativa vs promedio chileno          │
│  🏛️ Subsidio SAP (13/15 m³, 3 tramos RSH)  │
│  📋 Tabla: Tu consumo vs Eficiente           │
│  💰 Ahorro mensual/anual/5 años              │
│  🍞 Equivalencias (pan, micro, recarga)      │
│  💡 Tips personalizados (ordenados x impacto)│
│  🤝 Programas de apoyo (SAP, medidor, etc.)  │
│  [💾 Guardar] [📲 WhatsApp] [📂 Cargar]     │
│                                              │
└──────────────────────────────────────────────┘
```

### Navegación SPA
- **No recarga la página**. Se oculta/muestra con `display:none` + clase `.active`
- **Animación de entrada** con CSS `@keyframes pageIn` (fade + slide up)
- **Scroll al tope** automático al cambiar de vista
- **El hero banner** se oculta en la vista de resultados y reaparece al volver

---

## 5. Datos Utilizados

### 5.1 Empresas sanitarias (7 empresas)

| ID | Empresa | Región | Cargo Fijo | T1 ($/m³) | T2 ($/m³) | T3 ($/m³) |
|----|---------|--------|:----------:|:---------:|:---------:|:---------:|
| andinas | Aguas Andinas | RM Santiago | $3.500 | $680 | $980 | $1.450 |
| esval | ESVAL | Valparaíso | $3.200 | $720 | $1.040 | $1.560 |
| essbio | ESSBIO | Biobío | $3.000 | $650 | $940 | $1.400 |
| nuevosur | Nuevosur | Maule | $2.900 | $630 | $910 | $1.360 |
| altiplano | Aguas del Altiplano | Norte | $2.800 | $590 | $850 | $1.270 |
| araucania | Aguas Araucanía | Araucanía | $3.100 | $660 | $950 | $1.420 |
| magallanes | Aguas Magallanes | Magallanes | $3.300 | $700 | $1.010 | $1.500 |

**Fuente:** Resoluciones tarifarias de la SISS (Superintendencia de Servicios Sanitarios) — Decretos tarifarios vigentes 2025. Valores referenciales simplificados.

### 5.2 Actividades de consumo (7 actividades)

| Actividad | Icono | Consumo por unidad | Tipo | Frecuencia |
|-----------|:-----:|:-----------------:|:----:|:----------:|
| Ducha | 🚿 | 10 L/min | Per cápita | Diaria |
| Descarga WC | 🚽 | 8 L/descarga | Per cápita | Diaria |
| Cepillado | 🪥 | 6 L/vez (llave abierta) | Per cápita | Diaria |
| Lavado platos | 🍽️ | 15 L/lavada | Hogar | Diaria |
| Lavadora | 👕 | 65 L/carga | Hogar | Semanal |
| Cocinar/beber | 🍳 | 1 L/L declarado | Hogar | Diaria |
| Riego jardín | 🌱 | 30 L/vez | Hogar | Semanal |

**Fuentes:** SISS, Aguas Andinas (material educativo), fabricantes de artefactos sanitarios (Fanaloza, Roca), Andess.

### 5.3 Valores eficientes (meta de ahorro)

| Actividad | Valor por defecto | Meta eficiente | Mecanismo |
|-----------|:-----------------:|:--------------:|-----------|
| Ducha | 8 min × 10 L/min | 5 min × 10 L/min | Reducir tiempo |
| WC | 5 × 8 L | 5 × **4 L** | WC doble descarga |
| Cepillado | 3 × 6 L | 3 × **1 L** | Cerrar la llave |
| Platos | 2 × 15 L | 2 × **5 L** | Usar recipiente |
| Lavadora | 3 × 65 L | **2** × 65 L | Carga completa |
| Cocina | 8 L | **6 L** | Reutilizar agua |
| Riego | 0 × 30 L | 0 × **20 L** | Riego eficiente |

**Fuente:** Recomendaciones SISS, programas de ahorro de Aguas Andinas, guías del MOP.

### 5.4 Subsidio SAP

| Tramo RSH | Descuento |
|:---------:|:---------:|
| 0–40% | **85%** |
| 41–70% | **50%** |
| 71–90% | **25%** |

- **Tope SAP estándar:** 13 m³/mes
- **Tope Chile Seguridades y Oportunidades:** 15 m³/mes (hasta 100%)
- **Fuente legal:** Ley N° 18.778, ChileAtiende, BCN

### 5.5 Promedio chileno

- **3.8 m³/persona/mes** (~127 L/persona/día)
- **Fuente:** SISS, Andess (tendencia a la baja reportada en 2023-2025)

### 5.6 Precios de equivalencias

| Bien | Precio | Fuente |
|------|:------:|--------|
| Pan (marraqueta) | $2.300/kg | Odepa 2025 |
| Pasaje micro | $795 | Tarifa bus RED adulto, feb 2026 |
| Recarga celular | $5.000 | Promedio operadores chilenos |

---

## 6. Metodología de Cálculos

### 6.1 Consumo diario por actividad

```
Para cada actividad:
  Si es per cápita (ducha, WC, cepillado):
    consumo = valor × litros_por_unidad × número_personas
  
  Si es del hogar (platos, lavadora, cocina, riego):
    consumo = valor × litros_por_unidad × 1
  
  Si la frecuencia es semanal (lavadora, riego):
    consumo = resultado ÷ 7
```

### 6.2 Conversión a m³/mes

```
litros_dia = Σ consumo de todas las actividades
m3_mes = (litros_dia × 30) ÷ 1.000
```

### 6.3 Costo mensual (tarifa escalonada)

```
Si m³ ≤ 15:     variable = m³ × tarifa_T1
Si m³ ≤ 30:     variable = (15 × T1) + ((m³ − 15) × T2)
Si m³ > 30:     variable = (15 × T1) + (15 × T2) + ((m³ − 30) × T3)

costo_total = cargo_fijo + variable
```

**Ejemplo:** 18.5 m³ con Aguas Andinas:
- T1: 15 × $680 = $10.200
- T2: 3.5 × $980 = $3.430
- Fijo: $3.500
- **Total: $17.130**

### 6.4 Cálculo del ahorro

```
ahorro_mensual = costo_actual − costo_eficiente
ahorro_anual = ahorro_mensual × 12
ahorro_5_años = ahorro_anual × 5
```

### 6.5 Subsidio SAP

```
costo_subsidiable = tarifa(mínimo(consumo, 13 m³))
descuento = costo_subsidiable × porcentaje_RSH

Alertas:
  - consumo ≤ 13 m³ → ✅ Cubierto por SAP estándar
  - 13 < consumo ≤ 15 m³ → ⚠️ Solo cubierto por Chile Seg. y Oportunidades
  - consumo > 15 m³ → 🔴 Excede todo subsidio
```

### 6.6 Semáforo de eficiencia

Cada actividad tiene dos umbrales `[verde_max, amarillo_max]`:

| Actividad | 🟢 ≤ | 🟡 ≤ | 🔴 > |
|-----------|:----:|:----:|:----:|
| Ducha | 5 min | 12 min | 12 min |
| WC | 4 veces | 8 veces | 8 veces |
| Cepillado | 2 veces | 4 veces | 4 veces |
| Platos | 2 veces | 4 veces | 4 veces |
| Lavadora | 3 cargas | 7 cargas | 7 cargas |
| Cocina | 8 L | 15 L | 15 L |
| Riego | 2 veces | 5 veces | 5 veces |

### 6.7 Equivalencias de ahorro

```
kg_pan = ahorro_anual ÷ 2.300
pasajes = ahorro_anual ÷ 795
recargas = ahorro_anual ÷ 5.000
```

### 6.8 Tips dinámicos

Los consejos se generan automáticamente calculando `consumo_actual − consumo_eficiente` para cada actividad, se filtran los que tienen diferencia > 0, y se ordenan de mayor a menor impacto en litros/día. Cada tip muestra el ahorro mensual en L/mes.

---

## 7. Funcionalidades Detalladas

### 7.1 Página de Entrada

| Función | Descripción |
|---------|-------------|
| **Selector de empresa** | Lista desplegable con 7 empresas sanitarias chilenas. Cambia las tarifas aplicadas a los cálculos. |
| **Selector de personas** | Botones del 1 al 8. Resalta el activo con gradiente cyan. Afecta actividades per cápita. |
| **Sliders de actividad** | 7 sliders con rango configurable, botones +/− para ajuste fino, semáforo visual en tiempo real. |
| **Tags de tipo** | Cada actividad muestra "× PERSONA" (cyan) o "HOGAR" (amber) para indicar cómo se multiplica. |
| **Botón CTA** | "Calcular mi consumo" — botón grande redondeado con gradiente, hover con efecto ripple. |

### 7.2 Página de Resultados

| Sección | Descripción |
|---------|-------------|
| **Stats principales** | 3 cards: Litros/día, m³/mes (con tooltip), Costo mensual (con color semáforo). |
| **Tramo tarifario** | Barra degradada con punto indicador que se mueve según el consumo. Muestra el tramo activo y su precio. |
| **Comparativa** | Barra visual con pin del promedio chileno. Muestra diferencia en m³. |
| **Subsidio SAP** | Card con gradiente violeta. 3 tramos RSH con descuento calculado. Alerta contextual (verde/amarillo/rojo). |
| **Tabla comparativa** | 7 actividades + total. Columnas: nombre, tu consumo, eficiente, ahorro/día. |
| **Stats eficientes** | 3 cards: Litros eficientes, m³ eficientes, ahorro mensual en pesos. |
| **Banner de ahorro** | Card verde con ahorro anual, equivalencias en pan/micro/recarga, proyección a 5 años. |
| **Tips personalizados** | Lista dinámica ordenada por impacto. Cada tip con icono, texto y badge "Ahorra X L/mes". |
| **Programas de apoyo** | 4 cards: SAP, revisión de medidor, kit municipal, convenio de pago. |
| **Acciones** | 4 botones: Guardar, WhatsApp, Cargar guardado, Restablecer. |

### 7.3 Persistencia (localStorage)

| Acción | Comportamiento |
|--------|---------------|
| **Guardar** | Almacena en `cta_data`: personas, empresa, valores de todos los sliders, timestamp |
| **Cargar** | Lee `cta_data`, restaura todos los valores, regresa a la página de entrada |
| **Restablecer** | Resetea a valores por defecto (4 personas, Aguas Andinas, hábitos promedio) |
| **Auto-carga** | Al abrir la página, si existe un escenario guardado, lo restaura automáticamente |

---

## 8. Diseño Visual y UX

### 8.1 Identidad visual

| Elemento | Valor |
|----------|-------|
| **Nombre** | Calcula Tu Agua |
| **Dominio** | calculatuagua.cl |
| **Emoji principal** | 💧 |
| **Color primario** | `#0E7490` (cyan-700) |
| **Color secundario** | `#06B6D4` (cyan-400) |
| **Acento** | `#F59E0B` (amber) para tags "hogar" |
| **Éxito** | `#059669` (green) para ahorro |
| **Error** | `#DC2626` (red) para alertas |
| **Tipografía** | Inter (Google Fonts) — 400, 500, 600, 700, 800, 900 |

### 8.2 Componentes visuales

| Componente | Técnica CSS |
|-----------|------------|
| **Hero banner** | Degradado 3 pasos (dark cyan → cyan → light cyan), burbujas flotantes con `radial-gradient` animado |
| **Ola SVG** | `<svg>` inline con curva Bézier, transición entre hero y contenido |
| **Cards** | Bordes redondeados 20px, sombra tenue, hover con sombra media |
| **Botones de personas** | Gradiente activo, sombra con color, elevación al hover |
| **Semáforos** | Círculos 10px con `box-shadow` de color para efecto glow |
| **CTA** | Pill shape (60px radius), efecto ripple con pseudo-elemento, sombra profunda |
| **Tramo tarifario** | Gradiente tri-color con punto arrastrable CSS |
| **Subsidio** | Card con glassmorphism (backdrop-filter blur) |
| **Stats** | Cards con degradado según estado (blue/amber/red/green) |
| **Toast** | Fixed bottom center, slide-in con cubic-bezier |

### 8.3 Responsividad (Mobile-first)

| Breakpoint | Cambios |
|-----------|---------|
| `>640px` | Grid de 3 columnas en stats, layout completo |
| `≤640px` | Grid de 2 columnas, sliders más cortos (60px), acciones apiladas verticalmente, hero más compacto |

### 8.4 Animaciones

| Animación | Uso | Duración |
|----------|-----|:--------:|
| `float` | Burbujas decorativas del hero | 8-10s loop |
| `drip` | Emoji de gota rebotando | 3s loop |
| `pageIn` | Transición entre páginas | 0.5s |
| Hover cards | Elevación -3px + sombra | 0.2s |
| CTA ripple | Círculo blanco expandiéndose | 0.5s |
| Toast | Slide-up suave | 0.4s cubic-bezier |

---

## 9. Accesibilidad

| Aspecto | Implementación |
|---------|---------------|
| **Semántica** | `<button>` reales (no `<div>`), `<select>`, `<input type="range">` |
| **ARIA** | `aria-label` en todos los controles, `aria-pressed` en botones de personas, `role="group"` |
| **Navegación** | `focus-visible` con outline 3px en botones |
| **Contraste** | Colores principales ≥ 4.5:1 contra fondo blanco (WCAG AA) |
| **Tooltips** | Activados por hover Y focus (accesible por teclado) |
| **Botones ±** | Permiten ajuste preciso sin slider (ideal para pantallas táctiles y lectores de pantalla) |
| **Idioma** | `<html lang="es">` declarado |
| **Emojis** | Usados como apoyo visual, no como contenido único |

---

## 10. Integraciones

### 10.1 WhatsApp (compartir resultados)

- **API:** URL scheme `https://wa.me/?text=...`
- **Activación:** Solo al hacer clic en botón "📲 WhatsApp"
- **Contenido compartido:**
  ```
  💧 *Calcula Tu Agua — Mi consumo*
  
  👥 4 personas
  📊 Consumo: 18.5 m³/mes ($17.130)
  ✅ Eficiente: 9.8 m³/mes ($10.164)
  💰 Ahorro mensual: $6.966
  💰 Ahorro anual: $83.592
  
  ¡Calcula el tuyo en calculatuagua.cl! 🌊
  ```
- **No requiere API key** ni autenticación

### 10.2 localStorage (persistencia)

- **Key:** `cta_data`
- **Formato:** JSON con campos `{ppl, cIdx, V, ts}`
- **No expira** (persiste hasta que el usuario borre datos del navegador)
- **Auto-restauración** al abrir la página

### 10.3 Google Fonts CDN

- **Font:** Inter (400-900)
- **Fallback:** `system-ui, sans-serif`
- **Carga:** `preconnect` + `swap` para evitar FOIT

---

## 11. Fuentes y Referencias

| # | Fuente | URL | Dato utilizado |
|:-:|--------|-----|----------------|
| 1 | **SISS** | [siss.gob.cl](https://www.siss.gob.cl) | Consumo per cápita, regulación tarifaria, resoluciones |
| 2 | **Aguas Andinas** | [aguasandinas.cl](https://www.aguasandinas.cl) | Tarifas 2025, DS N°47 (VIII Proceso Tarifario), guías de ahorro |
| 3 | **Andess** | [andess.cl](https://www.andess.cl) | Estadísticas de consumo, tendencia a la baja del 30% |
| 4 | **BCN** | [bcn.cl/leychile](https://www.bcn.cl/leychile) | Ley N° 18.778 (Subsidio SAP) |
| 5 | **ChileAtiende** | [chileatiende.gob.cl](https://www.chileatiende.gob.cl) | Ficha SAP, requisitos, tramos RSH |
| 6 | **Ventanilla Única Social** | [ventanillaunicasocial.gob.cl](https://www.ventanillaunicasocial.gob.cl) | Porcentajes de subsidio, procedimiento |
| 7 | **Odepa** | [odepa.gob.cl](https://www.odepa.gob.cl) | Precio marraqueta $2.300/kg (2025) |
| 8 | **DTPM / Red** | [red.cl](https://www.red.cl) | Tarifa bus RED adulto $795 (feb 2026) |
| 9 | **MOP** | [mop.gob.cl](https://www.mop.gob.cl) | Guías de uso eficiente del agua |
| 10 | **Fabricantes** | Fanaloza, Roca | Fichas técnicas: consumo WC, duchas |

---

## 12. Limitaciones

### Lo que la herramienta SÍ hace
- ✅ Estima consumo basado en hábitos declarados
- ✅ Usa estructura tarifaria real (3 tramos escalonados)
- ✅ Distingue consumo per cápita vs hogar
- ✅ Informa sobre subsidio SAP con 3 tramos RSH
- ✅ Compara con promedio nacional
- ✅ Funciona 100% offline después de cargar

### Lo que la herramienta NO hace
- ❌ **No incluye alcantarillado** — Las empresas cobran adicionalmente por este servicio (~50% del cargo de agua)
- ❌ **No incluye IVA** — El 19% no se aplica en el cálculo
- ❌ **No considera período punta/no punta** — Algunas empresas tienen tarifas estacionales (dic-mar)
- ❌ **No detecta fugas** — Las fugas pueden representar 10-20% del consumo real
- ❌ **No reemplaza la boleta** — Es un cálculo referencial educativo
- ❌ **No tiene datos en tiempo real** — Las tarifas se actualizan manualmente

### Sesgos y consideraciones
- Las tarifas son **simplificadas** (un valor por empresa). En realidad cada empresa tiene múltiples grupos tarifarios por zona.
- El promedio de 3.8 m³/persona/mes es una **estimación conservadora** — el dato real varía por región y época del año.
- Los litros por actividad son **promedios de referencia**, no mediciones reales del hogar específico.

---

## 13. Roadmap Futuro

### Prioridad Alta
| Mejora | Descripción | Impacto |
|--------|-------------|---------|
| **PWA** | Convertir en Progressive Web App para instalación y uso 100% offline | Alto — muchas familias tienen conectividad intermitente |
| **Alcantarillado** | Agregar cálculo del cargo por alcantarillado (~50% adicional) | Alto — el costo real es significativamente mayor |
| **IVA** | Agregar toggle para incluir 19% IVA en el cálculo | Medio — más realista |

### Prioridad Media
| Mejora | Descripción |
|--------|-------------|
| **Más empresas** | Incluir las ~50 empresas sanitarias restantes de Chile (Aguas Chañar, SMAPA, APR) |
| **Tarifas dinámicas** | Archivo JSON externo actualizado periódicamente con tarifas vigentes |
| **Periodo punta** | Switch para calcular con tarifa de verano (dic-mar) |
| **Idioma mapudungun** | Opción de idioma para comunidades mapuche en regiones del sur |

### Prioridad Baja
| Mejora | Descripción |
|--------|-------------|
| **Analytics** | Google Analytics o Plausible para medir uso real |
| **Testing de usuario** | Pruebas con familias reales para validar comprensión del lenguaje |
| **PDF export** | Generar resumen en PDF para imprimir |
| **Historial** | Guardar múltiples escenarios con fecha para trackear progreso |

---

## Apéndice: Estructura del Archivo HTML

```
calculadora_agua.html (42 KB, autocontenido)
├── <head>
│   ├── Meta tags (charset, viewport, SEO)
│   ├── Google Fonts (Inter)
│   └── <style> (~880 líneas CSS)
│       ├── Variables CSS (colores, radios, sombras)
│       ├── Reset y base
│       ├── Hero + wave SVG
│       ├── Sistema de cards
│       ├── Controles (select, buttons, sliders)
│       ├── Stats, tramo, comparativa
│       ├── Subsidio (glassmorphism)
│       ├── Tabla comparativa
│       ├── Ahorro y tips
│       ├── Programas y acciones
│       ├── Toast y tooltips
│       └── @media (≤640px responsive)
│
├── <body>
│   ├── Hero section (logo, subtítulo, badge, wave SVG)
│   ├── Container (max-width: 800px)
│   │   ├── PAGE 1: Input
│   │   │   ├── Empresa sanitaria (select)
│   │   │   ├── Personas (buttons 1-8)
│   │   │   ├── Actividades (7 rows con semáforo + slider + ±)
│   │   │   └── CTA "Calcular mi consumo"
│   │   │
│   │   ├── PAGE 2: Results (hidden por defecto)
│   │   │   ├── Stats hero (3 cards)
│   │   │   ├── Tramo tarifario
│   │   │   ├── Comparativa promedio
│   │   │   ├── Subsidio SAP
│   │   │   ├── Tabla comparativa
│   │   │   ├── Stats eficientes
│   │   │   ├── Banner ahorro + equivalencias
│   │   │   ├── Tips dinámicos
│   │   │   ├── Programas de apoyo
│   │   │   └── Acciones (guardar, WA, cargar, reset)
│   │   │
│   │   └── Footer (branding + disclaimers)
│   │
│   ├── Toast (notificaciones)
│   │
│   └── <script> (~150 líneas JS)
│       ├── DATA: empresas, actividades, subsidio, constantes
│       ├── HELPERS: tariff(), actDaily(), totalDaily(), semaphore(), equivs()
│       ├── BUILD: buildCompany(), buildPeople(), buildActs()
│       ├── NAVIGATION: showResults(), goBack()
│       ├── RENDER: renderResults() (stats, tramo, avg, subsidy, table, tips, programs)
│       ├── ACTIONS: saveScen(), loadScen(), resetAll(), shareWA()
│       └── INIT: build + auto-load saved scenario
```

---

*Documento consolidado de calculatuagua.cl — Versión 2.0, Abril 2026*  
*Proyecto de Liderazgo — Universidad San Sebastián*
