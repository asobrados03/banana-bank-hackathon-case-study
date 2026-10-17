# Banana Bank — experiencia de hackathon

> Caso de estudio personal desarrollado durante un reto técnico de [**T·dots**](https://tdots.es/).
> Banana Bank, sus clientes, documentos y operaciones son completamente ficticios.

Este repositorio documenta mi aportación durante varios días de trabajo sobre una aplicación entregada como base por la organización. No es un proyecto oficial de T·dots ni redistribuye el código ni los datos del reto.

## Contexto

La propuesta partía de una aplicación bancaria ficticia con un asistente de IA, datos de prueba y un simulador de banca. El objetivo era doble: mejorar su preparación para lanzamiento y construir una capacidad distintiva integrada en el flujo del agente.

Mi enfoque fue priorizar riesgos que afectaban a operaciones monetarias y, a la vez, diseñar una respuesta más fiable para consultas sobre cambios de condiciones bancarias.

## Mi aportación

### 1. Transferencias: confirmación explícita e idempotencia

Detecté que envíos repetidos podían ejecutar varias transferencias equivalentes y que el flujo no ofrecía una revisión explícita antes de mover dinero simulado.

Implementé un flujo de dos pasos:

- La primera acción genera una propuesta con origen, destinatario, importe y concepto para revisión.
- Solo la confirmación de esa propuesta ejecuta la transferencia.
- La propuesta caduca a los 15 minutos y no puede consumirse más de una vez.
- Los reintentos reutilizan la misma referencia bancaria, evitando duplicados incluso ante respuestas ambiguas del servicio.
- La interfaz bloquea clics repetidos mientras se procesa la petición.

Verifiqué el comportamiento con pruebas automatizadas y con los escenarios de fallo proporcionados por el simulador, incluido el caso de respuesta perdida tras realizar la operación.

### 2. Recuperación al volver a una conversación nueva

Al volver desde el historial a la opción de conversación nueva, la interfaz intentaba tratar una conversación vacía como un historial existente y podía terminar en una pantalla de error.

Corregí el restablecimiento del estado de la vista y añadí una comprobación para aceptar únicamente colecciones de mensajes válidas al cargar un historial. Con ello, el usuario puede alternar entre conversaciones anteriores y una nueva sin perder la interacción.

### 3. Navegador de cambios de políticas para el asistente

Construí una capacidad para consultas como: _«¿Han cambiado las comisiones de esta cuenta este año?»_.

En lugar de confiar únicamente en que una búsqueda semántica recuperase por casualidad una versión actual y otra histórica, el flujo:

- detecta consultas sobre cambios de condiciones;
- identifica el producto relevante;
- selecciona de forma determinista las fuentes históricas y vigentes con sus fechas;
- conserva la evidencia junto con la respuesta del agente; y
- muestra las fuentes consultadas en la conversación.

Así, la respuesta diferencia condiciones vigentes de operaciones ya contabilizadas y hace visible de dónde procede cada afirmación. Añadí pruebas para la selección de versiones y para la detección de preguntas tanto en español como en inglés.

## Cómo trabajé

1. Exploré la aplicación y reproduje los comportamientos problemáticos.
2. Priorizé el riesgo de pagos duplicados por su impacto directo en una operación financiera.
3. Diseñé una mejora del agente centrada en trazabilidad: no solo responder la política actual, sino comparar versiones con evidencia fechada.
4. Validé los cambios con comprobaciones de tipos, pruebas automatizadas y recorridos manuales en la interfaz.

## Tecnologías y conceptos aplicados

- TypeScript, React y Next.js.
- Flujos de aprobación explícita para acciones sensibles.
- Idempotencia y recuperación ante fallos de red.
- Pruebas de invariantes de negocio.
- Recuperación de información y respuestas de IA con fuentes verificables.
- Diseño de interfaz para revisión, confirmación y trazabilidad.

## Demo y documentación

- [Demo en vídeo](demo.mp4): recorrido narrado de las correcciones y de la funcionalidad del agente.
- [Documentación previa a la resolución del hackathon](documentacion-previa-hackathon.pdf): material de apoyo que acompaña al caso de estudio.

## Alcance y atribución

La base de la aplicación, el simulador, los documentos de prueba y el enunciado pertenecen a T·dots. Este caso de estudio explica únicamente mi proceso y mis aportaciones sobre aquel entorno. Incluye el vídeo de demostración y la documentación de apoyo que he decidido compartir, pero no incluye código fuente, datos, credenciales ni sesiones de IA.

Si T·dots autoriza la publicación de una versión del código, este repositorio puede enlazarla o incorporar exclusivamente los archivos cuya redistribución esté expresamente permitida.
