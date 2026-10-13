# Xarpen: plan por fases

**Fecha:** 2026-10-13 · **Relacionado:** [`xarpen-module-map.md`](xarpen-module-map.md), [`specs/pasar-cliente.md`](specs/pasar-cliente.md)

## La tesis

Xarpen gana el juego vertical y el horizontal a la vez:

- **Vertical:** profundidad real en fotografía. Datos del estudio, reglas de confianza (la IA prepara y una persona confirma) y conocimiento del oficio.
- **Horizontal:** un CRM polímata que combina módulos (Personas, Tiempo, Encargo, Dinero, Papeles, Mensajes, Trabajo y Memoria) para crear CRMs para oficios nuevos.

Los módulos se copian en meses. Lo que no se copia:

1. **La red:** muchos estudios conectados en un mismo modelo de datos.
2. **Entender imágenes:** el producto del fotógrafo son fotos.
3. **La confianza:** la IA puede tocar dinero y contratos porque una persona siempre confirma.

## Reglas para construir

1. **Todo lo que se construye debe servirle a un estudio solo, y además dejar lista la pieza de la red.** Primero herramienta, después red.
2. **Si faltan usuarios, primero las funciones que traen usuarios.**
3. **Círculos de confianza, no mercado abierto.** Los fotógrafos se pasan fechas solo con conocidos de mucha confianza. La red crece por esos círculos.
4. **La IA sugiere, la persona decide.** Dinero, citas, contratos y recibos siempre quedan pendientes hasta que alguien los confirma.

## Fase 0: bases (decisiones de modelo de datos)

Hay que tomarlas ya, porque cambiarlas después es caro.

| Decisión | Para qué sirve después |
|---|---|
| "Evento" pasa a ser **"Encargo"** con un ciclo de etapas configurable | La boda es una configuración. Así se abre la fábrica de verticales. |
| **"Lugar"** como entidad propia | El mapa compartido de lugares de evento. |
| **Fechas de vida** en el cliente (boda, aniversario, nacimientos) | El ciclo de vida del cliente. |
| **Conexiones entre estudios** (`StudioConnection`) | Pasar cliente, segundos fotógrafos, la red. |
| **Historial completo de cambios** | El simulador del estudio (gemelo). |
| **Permiso para usar datos anónimos** en los términos desde el día uno | Mapa de lugares y referencias de precios por ciudad. |
| **La fotografía como primera plantilla** (configuración, no código) | Demostrar que el motor de síntesis funciona. |

## Fase 1: herramienta que crece sola (ahora)

| Función | Valor para un estudio solo | Cómo trae usuarios |
|---|---|---|
| **Pasar cliente** ([especificación](specs/pasar-cliente.md)) | Recupera clientes de fechas ocupadas y hace visible la reciprocidad. | Cada pase invita a un colega de confianza. |
| **Ciclo de vida del cliente** | Ingresos de clientes que ya tiene: aniversario, maternidad, recién nacido, familia. | Clientes felices que vuelven y recomiendan. |
| **Fichas de lugares** | Notas propias del estudio sobre luz, reglas y contactos de cada lugar. | Indirecto: datos que alimentarán el mapa compartido. |
| **Asistente de cara al cliente** | Responde consultas y cotiza a cualquier hora en la voz del estudio. | "Con tecnología de Xarpen" llega a cada pareja y a sus amigos fotógrafos. |

**Señal para pasar a la Fase 2:** círculos activos y densos en al menos una ciudad (por ejemplo, 30 o más estudios conectados entre sí).

## Fase 2: la red de confianza

- Libro de reciprocidad y comisiones registradas.
- Disponibilidad libre/ocupado dentro del círculo.
- Reputación privada basada en resultados reales (entregas a tiempo, clientes que vuelven).
- Segundo grado con aval: "colega de mi colega".
- Círculos por rol: segundos fotógrafos, editores y alquiler de equipo.

## Fase 3: inteligencia de la red

- **Mapa de lugares** compartido de forma anónima: luz por mes, reglas, experiencias.
- **Referencias de mercado:** precios y tasa de cierre por ciudad, siempre anónimas.
- **Gemelo del estudio:** "¿qué pasa si subo precios?" o "¿si acepto esta boda, cómo queda mi mes?".
- **CRM que ve:** favoritas del cliente → propuesta de álbum o impresión, portafolio organizado por luz y lugar.

## Fase 4: fábrica de verticales (lo horizontal)

1. Un profesional describe su negocio en un párrafo.
2. Xarpen genera su CRM combinando módulos.
3. La persona revisa y aprueba la configuración.
4. Puede publicarla como plantilla, y otros del mismo oficio la usan y le pagan una parte.
5. La red de confianza se repite dentro de cada vertical.

Las reglas de confianza viajan con los módulos: dinero, papeles y citas siempre necesitan confirmación humana en cualquier vertical.

## Riesgos principales

| Riesgo | Cómo lo mitigamos |
|---|---|
| Abrir lo horizontal demasiado pronto y terminar genérico | No abrir la Fase 4 hasta tener profundidad y densidad en fotografía. |
| Red vacía al inicio | Fase 1 con funciones que sirven a un estudio solo y que invitan a colegas. |
| Que no quieran pasar clientes fuera de su círculo | No forzarlo: círculos cerrados y, más adelante, segundo grado solo con aval. |
| Protección de datos entre estudios | Consentimiento del cliente antes de compartir y revisión legal por mercado. |

## Próximos pasos

1. Validar "Pasar cliente" con 5 a 10 fotógrafos: ¿cómo se pasan fechas hoy?, ¿cobran comisión?, ¿de quién es el cliente después?
2. Revisar el modelo actual de Xarpen frente a las decisiones de la Fase 0.
3. Definir el formato de especificación de dominio (la salida del motor de síntesis) usando la fotografía como primer ejemplo.
