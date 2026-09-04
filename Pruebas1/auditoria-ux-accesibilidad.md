# Auditoria UX y accesibilidad

**Proyecto:** Pruebas1  
**Fecha:** 2026-09-04  
**Alcance:** `index.html`, `styles.css` y `script.js`

> Este documento contiene una revision estatica del proyecto. OpenCode esta instalado localmente, pero no pudo generar una auditoria con IA porque no tiene ningun proveedor autenticado (`0 credentials`). Los hallazgos de abajo se pueden comprobar directamente en los archivos.

## Resumen ejecutivo

La pagina tiene una identidad visual clara, buena jerarquia editorial y una estructura semantica inicial correcta. Las prioridades son mejorar la navegacion por teclado, el movimiento respetuoso con usuarios con sensibilidad al movimiento, la estabilidad visual de la imagen principal y la independencia del recurso fotografico externo.

## Hallazgos priorizados

### Alta prioridad

#### A11Y-01: Falta un enlace para saltar al contenido

- **Evidencia:** la primera navegacion por teclado entra en los enlaces del encabezado y no ofrece un enlace "Saltar al contenido".
- **Impacto:** las personas que usan teclado o lector de pantalla deben recorrer toda la cabecera antes de llegar al contenido principal.
- **Recomendacion:** agregar un enlace al inicio del `body` con `href="#contenido"` y asignar `id="contenido"` al `main`. Mostrarlo al recibir foco.

#### A11Y-02: No hay estilos de foco visibles

- **Evidencia:** existen estados `:hover` para enlaces y botones, pero no hay reglas `:focus-visible`.
- **Impacto:** un usuario de teclado puede perder la ubicacion actual dentro de la pagina.
- **Recomendacion:** definir un foco visible y de alto contraste, por ejemplo un contorno de 3px en color lima con separacion suficiente.

### Media prioridad

#### A11Y-03: El desplazamiento suave no respeta movimiento reducido

- **Evidencia:** `html { scroll-behavior: smooth; }` esta activo globalmente y los botones tienen transiciones.
- **Impacto:** puede causar mareo o incomodidad a personas que solicitan menos movimiento en su sistema operativo.
- **Recomendacion:** envolver el desplazamiento suave y las transiciones no esenciales en `@media (prefers-reduced-motion: reduce)` para desactivarlos o reducirlos.

#### UX-01: La foto principal depende de un servidor externo

- **Evidencia:** la imagen se carga desde `upload.wikimedia.org`.
- **Impacto:** sin conexion, con bloqueo de terceros o si cambia la URL, la primera pantalla pierde su elemento visual principal.
- **Recomendacion:** descargar una imagen con licencia compatible dentro de `Pruebas1/assets/`, conservar la atribucion correspondiente y usar la copia local como `src`.

#### UX-02: La imagen no declara dimensiones

- **Evidencia:** el elemento `img` no tiene `width` ni `height`; el layout se controla solo mediante CSS.
- **Impacto:** el navegador puede reajustar el espacio durante la carga y producir cambio de layout.
- **Recomendacion:** declarar la proporcion o dimensiones del recurso y mantener `object-fit: cover` para conservar el tratamiento visual.

#### A11Y-04: La ficha tecnica mezcla contenido fuera de la estructura del `dl`

- **Evidencia:** en algunos elementos, la descripcion secundaria (`Delantero centro`, `Suecia`, etc.) es un `small` hermano de `dd`, no parte del par termino/descripcion.
- **Impacto:** lectores de pantalla pueden anunciar la informacion de forma fragmentada.
- **Recomendacion:** integrar el texto secundario dentro de `dd`, o convertir cada bloque en un grupo semantico consistente de `dt` y `dd`.

### Baja prioridad

#### UX-03: La navegacion movil oculta enlaces secundarios

- **Evidencia:** debajo de 760px se ocultan `Perfil` y `Trayectoria`, dejando solo `Ficha tecnica`.
- **Impacto:** las secciones siguen siendo accesibles por scroll, pero la orientacion y el acceso directo empeoran en movil.
- **Recomendacion:** conservar los enlaces en un menu compacto o mantener una navegacion horizontal desplazable.

#### A11Y-05: Elementos decorativos no estan marcados como decorativos

- **Evidencia:** las lineas de `.eyebrow` y la marca visual `ZI` son `span` sin `aria-hidden`.
- **Impacto:** normalmente es menor, pero puede introducir ruido en algunas tecnologias de asistencia.
- **Recomendacion:** marcar como `aria-hidden="true"` los elementos que no aporten informacion.

#### UX-04: El contenido editorial usa caracteres sin tildes

- **Evidencia:** textos como `Navegacion`, `tecnica`, `logica` y `futbol` no llevan diacriticos.
- **Impacto:** reduce la calidad editorial y puede afectar pronunciacion en lectores de pantalla en espanol.
- **Recomendacion:** corregir la ortografia y mantener `lang="es"`, que ya esta correctamente declarado.

## Aspectos positivos observados

- `lang="es"`, `charset` y `viewport` estan declarados.
- Existe un `title` y una meta descripcion.
- Hay un solo `h1` y los bloques principales usan encabezados asociados con `aria-labelledby`.
- La imagen principal tiene texto alternativo descriptivo.
- Los enlaces internos permiten recorrer las secciones principales.
- El layout incluye un breakpoint movil y evita depender exclusivamente del color para transmitir la estructura.
- La lista de trayectoria usa `ol`, apropiado para una secuencia temporal.

## Plan recomendado

1. Resolver `A11Y-01` y `A11Y-02`.
2. Añadir soporte para `prefers-reduced-motion`.
3. Corregir la estructura de la ficha tecnica y los textos con tildes.
4. Fijar dimensiones de la imagen y valorar una copia local.
5. Repetir la auditoria con OpenCode despues de autenticar un proveedor.
6. Validar con Lighthouse o axe y probar el recorrido completo usando solo teclado.

## Comprobaciones pendientes

- Navegacion completa con teclado y foco visible en un navegador real.
- Zoom al 200% y reflow en 320px de ancho.
- Contraste medido con una herramienta automatica, incluyendo estados de foco.
- Carga de la pagina sin red.
- Prueba con lector de pantalla.
- Auditoria IA de OpenCode una vez configuradas sus credenciales.

## Cambios implementados

- Se agrego un enlace "Saltar al contenido" para la navegacion por teclado.
- Se agregaron estilos `:focus-visible` de alto contraste.
- Se incorporo `prefers-reduced-motion` para reducir desplazamientos y transiciones.
- Se corrigio la estructura de la ficha tecnica para que cada dato use `dt` y `dd` de forma consistente.
- Se declararon dimensiones de la imagen principal para reducir cambios de layout.
- Se conservaron los enlaces de Perfil y Trayectoria en la navegacion movil.
- Se marcaron elementos visuales decorativos con `aria-hidden`.
- Se corrigieron tildes y textos editoriales en espanol.
