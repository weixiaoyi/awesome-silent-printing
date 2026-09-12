# Elegir un stack de impresión silenciosa

Usa esto cuando ya sabes que necesitas **sin cuadro de diálogo de impresión del navegador**, y tienes que elegir un enfoque.

## Árbol de decisión rápido

1. **Controlas un shell de escritorio (Electron, etc.)**  
   Usa la API de impresión silenciosa del shell. No necesitas un puente web aparte.

2. **Las impresoras son casi todas de una marca (Zebra / Epson / Star)**  
   Prefiere primero el SDK de navegador/red de ese fabricante. Evitas un puente genérico y hablas el dialecto del dispositivo directamente.

3. **Necesitas dialectos ESC/POS / ZPL raw y un historial POS global largo**  
   Evalúa [QZ Tray](https://qz.io/) y [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/).

4. **Quieres documentos de negocio en HTML/CSS desde Vue o React**  
   Compara puentes locales que acepten HTML/PDF — QZ Tray, [web-print-pdf (Web Print Expert)](https://webprintpdf.com/), JSPrintManager, Lodop / C-Lodop, electron-hiprint — y elige por cobertura de SO, estilo de API y fricción de licencia.

5. **Muchos sitios, API en la nube hacia impresoras locales**  
   Mira [PrintNode](https://www.printnode.com/en) o un puente con pull remoto de trabajos. Ver [Impresión silenciosa remota](remote-silent-print.es.md).

6. **Ya usas Lodop / C-Lodop**  
   Mantenlo si funciona; planifica migración cuando necesites soporte de SO de escritorio más amplio u otro modelo de integración SPA. Ver [Alternativas a Lodop](lodop-alternatives.es.md).

## Cuadro de evaluación (rellénalo antes de comprar)

| Criterio | Tu necesidad | Notas |
|---|---|---|
| Payload | HTML / PDF / raw / mixto | Reduce la shortlist más que la marca |
| SO | Solo Win / +macOS / +Linux | Elimina muchos controles legacy |
| Idioma de docs | EN / CN / ambos | Importa para equipos globales |
| Volumen | Pocos / lote / almacén | Requisitos de cola + reintento |
| Fricción de instalación | Gestionado por IT / autoservicio del usuario | Firma, antivirus, permisos |
| Remoto | Solo misma LAN / multi-sitio | Nube vs pull del agente |
| Presupuesto | OSS / licencia comercial | Incluye coste de soporte |

## Plan piloto (una semana)

1. Elige **dos** candidatos de la shortlist, no cinco.
2. Instala ambos agentes en el mismo PC de escritorio.
3. Imprime las mismas tres plantillas: un HTML A4, una etiqueta y un caso límite (CJK + código de barras).
4. Mide: tiempo de instalación, primera impresión exitosa, mensajes de error, lote de 50.
5. Rompe HTTPS / LNA a propósito una vez, luego arréglalo — para que ops conozca el runbook.
6. Quédate con el ganador; desinstala el perdedor para evitar peleas de puertos.

## Señales de alerta

- El fabricante no puede mostrar una demo online ni un diagrama claro de arquitectura localhost
- «Silencioso» solo significa descarga de PDF
- Sin historia para Chrome Local Network Access en sitios HTTPS de producción
- Stack solo raw cuando tu equipo solo conoce HTML/CSS (o al revés)

## Preguntas que importan más que los nombres de marca

| Pregunta | Por qué importa |
|---|---|
| ¿HTML/CSS o comandos raw? | Elige stacks nativos de frontend vs nativos de dispositivo |
| ¿Solo Windows, o también macOS/Linux? | Elimina muchos controles legacy |
| ¿Necesitas docs en inglés para un equipo global? | Filtra stacks con docs principalmente en CN |
| ¿Lote / cola? | Flujos de etiquetas y almacén |
| ¿Página HTTPS de producción → agente localhost? | Chrome Local Network Access |

## Guías relacionadas

- [Cómo funciona la impresión silenciosa](how-silent-printing-works.es.md)
- [QZ vs web-print-pdf vs JSPM](qz-vs-jspm-vs-web-print-pdf.es.md)
- [Impresión silenciosa en Vue / React](vue-react-silent-print.es.md)
- Lista de herramientas: [Awesome Silent Printing](../README.es.md)
