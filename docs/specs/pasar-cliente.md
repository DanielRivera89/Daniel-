# Especificación: Pasar cliente

**Estado:** borrador v1 · **Fecha:** 2026-10-13 · **Plan relacionado:** [`docs/plan-por-fases.md`](../plan-por-fases.md)

## Problema

Cuando un fotógrafo ya tiene una fecha ocupada, el cliente que pregunta por esa fecha se pierde o se pasa a un colega por WhatsApp. Ese pase informal funciona, pero solo entre conocidos de mucha confianza. Además no queda registro, nadie sabe si el cliente terminó contratando, y el favor no se devuelve de forma visible.

El costo de no resolverlo: ingresos perdidos para el estudio y para el colega, y una relación de confianza que existe pero que ningún producto aprovecha. Para Xarpen hay un costo extra: es el comportamiento más natural para que un estudio invite a otros, y hoy ocurre fuera del producto.

**Evidencia:** los fotógrafos ya se pasan fechas entre sí, pero solo con conocidos de mayor confianza (conversación con el fundador, 2026-10-13). Falta validarlo con más estudios.

## Principio de diseño

**Círculos de confianza, no mercado abierto.** La reputación de quien pasa el cliente queda en juego. Si el colega falla, el cliente culpa a quien lo recomendó. Por eso la v1 solo permite pasar clientes a colegas que el fotógrafo agregó a su círculo y que aceptaron estar ahí. No habrá directorio de desconocidos.

## Después del pase: el cliente decide

**Decisión (fundador, 2026-10-13):** el cliente decide con quién sigue en trabajos futuros, con quien lo recomendó o con el recomendado. Xarpen no asigna un "dueño" del cliente.

Reglas que salen de esa decisión:

1. **Los dos estudios conservan su ficha.** A conserva la consulta original con el historial del pase. B tiene el cliente completo desde que C aceptó compartir sus datos.
2. **La comisión acordada aplica solo al encargo que se pasó,** salvo que A y B acuerden otra cosa al preparar el pase.
3. **Transparencia dentro del círculo.** Si A o B van a ofrecerle un nuevo trabajo al cliente, Xarpen les recuerda que el cliente trabajó con el otro colega. Solo se indica el tipo de encargo y la fecha, nunca precios ni notas privadas. Así nadie se entera por sorpresa y la confianza no se rompe.
4. **Sin mensajes dobles.** Las sugerencias automáticas del ciclo de vida (aniversario, maternidad, etc.) van por defecto al estudio que hizo el último trabajo, para que el cliente no reciba la misma oferta de dos fotógrafos. El otro estudio puede contactarlo por su cuenta. Al final, el cliente elige.

## Objetivos

1. **Recuperar clientes que hoy se pierden:** que al menos el 30% de las consultas para fechas ocupadas se pasen a un colega en lugar de rechazarse. Es una hipótesis; se mide desde el lanzamiento.
2. **Traer estudios nuevos:** que al menos el 25% de los registros nuevos en 6 meses lleguen por un pase o una invitación al círculo. Es una hipótesis.
3. **Hacer visible la reciprocidad:** que cada estudio vea cuántos clientes dio y recibió, y con quién.
4. **Dejar lista la base de la red:** conexiones entre estudios y registro de pases en el modelo de datos, para construir encima sin rehacer.

## Fuera de alcance (v1)

| Fuera de alcance | Por qué |
|---|---|
| Directorio o mercado abierto de fotógrafos | Va contra cómo funciona la confianza hoy. Se reconsidera cuando existan reputaciones con datos reales. |
| Pasar clientes a "colegas de mis colegas" (segundo grado) | Necesita un sistema de aval que no existe todavía. |
| Cobrar o pagar comisiones automáticamente desde Xarpen | Regla de Xarpen: el dinero se prepara y una persona lo confirma. En v1 solo se registra el acuerdo. |
| Calificaciones o reseñas públicas entre estudios | Riesgo de conflicto en comunidades pequeñas. Primero hay que medir resultados privados. |
| Algoritmo que elige al colega automáticamente | Esa decisión es del fotógrafo. La IA sugiere, no decide. |

## Personas

- **Estudio que pasa (A):** tiene la fecha ocupada y quiere ayudar a su cliente y a su colega.
- **Colega que recibe (B):** está en el círculo de A y puede usar Xarpen o no.
- **Cliente final (C):** la pareja o familia que pidió la fecha.

## Historias de usuario

**Estudio que pasa (A)**
1. Como fotógrafo con la fecha ocupada, quiero que Xarpen me avise del conflicto al llegar una consulta y me sugiera colegas de mi círculo, para no perder al cliente.
2. Como fotógrafo, quiero elegir a quién paso el cliente y revisar el mensaje antes de enviarlo, para cuidar mi reputación.
3. Como fotógrafo, quiero acordar una comisión, un intercambio de favores o nada, para que cada relación funcione a su manera.
4. Como fotógrafo, quiero saber si el cliente contrató al colega, para saber si mi recomendación funcionó.
5. Como fotógrafo, quiero cancelar un pase antes de que el cliente lo acepte, por si me equivoqué.

**Colega que recibe (B)**
6. Como colega sin Xarpen, quiero recibir el cliente con un enlace y aceptarlo con un registro rápido, sin pagar para empezar.
7. Como colega, quiero aceptar o rechazar el pase, para no comprometerme con fechas que no puedo cubrir.
8. Como colega en Xarpen, quiero que el cliente aceptado aparezca en mis clientes con todo el contexto, para no pedirle los datos de nuevo.

**Cliente final (C)**
9. Como cliente, quiero recibir la recomendación de mi fotógrafo y decidir si comparto mis datos con el colega, para mantener el control de mi información.

**Casos límite**
10. Si B no responde en 48 horas, A recibe un aviso y puede pasar el cliente a otra persona.
11. Si C no acepta compartir sus datos, el pase se cierra y B nunca ve los datos de C.
12. Si A pasa la misma fecha a dos colegas, el sistema lo advierte y no lo permite al mismo tiempo.

## Flujo

```
Consulta de C para fecha ocupada
  → Xarpen detecta el conflicto y sugiere colegas del círculo de A (con disponibilidad si B usa Xarpen)
  → A elige a B, el acuerdo y el mensaje        [A confirma]
  → C recibe la recomendación                   [C acepta compartir sus datos]
  → B recibe el cliente (si no usa Xarpen: se registra para aceptarlo)
  → B acepta o rechaza
  → Estado visible para A: enviado · aceptado por C · aceptado por B · contratado · entregado
```

## Requisitos

### P0: sin esto no se lanza

| # | Requisito | Criterios de aceptación |
|---|---|---|
| R1 | **Círculo de confianza** | A puede invitar a un colega por correo o teléfono. La conexión existe solo cuando los dos aceptan. A puede quitar a alguien de su círculo en cualquier momento. |
| R2 | **Detectar el conflicto** | Dado que A tiene un encargo confirmado el día D, cuando llega una consulta para D, el resumen diario y la ficha de la consulta muestran "Fecha ocupada · Pasar cliente". |
| R3 | **Preparar el pase** | A elige un colega de su círculo (nunca alguien de fuera) y el tipo de acuerdo: porcentaje, monto fijo, favor o nada. La IA redacta el mensaje para C en la voz de A. **Nada se envía hasta que A lo confirma.** |
| R4 | **Consentimiento del cliente** | C recibe la recomendación con el perfil de B. B no ve el nombre, el contacto ni los detalles de C hasta que C acepta. Si C no acepta, el pase se cierra. |
| R5 | **Recepción con o sin Xarpen** | Si B no tiene cuenta, el enlace lleva a un registro gratuito y después al pase. Si B ya tiene cuenta, el pase aparece en su bandeja. B puede aceptar o rechazar. |
| R6 | **Estado y seguimiento** | A ve el estado del pase en la ficha original de la consulta. Si B no responde en 48 horas, A recibe un aviso. |
| R7 | **Deshacer** | A puede cancelar el pase hasta que C lo acepte. Cada cambio devuelve un `change_id` compatible con `undo_last_change`. |
| R8 | **Texto del cliente es dato** | Los mensajes de C se copian al pase como contenido. La IA nunca los ejecuta como instrucciones. |

### P1: mejora clara, poco después del lanzamiento

| # | Requisito |
|---|---|
| R9 | **Libro de reciprocidad:** por cada colega, cuántos clientes diste, cuántos recibiste y cuánto valieron. |
| R10 | **Disponibilidad del círculo:** si B usa Xarpen y lo permite, A ve solo "libre/ocupado" en la fecha D, nunca los detalles de la agenda de B. |
| R11 | **Recordatorio de comisión:** cuando B marca el encargo como contratado, Xarpen prepara el recordatorio del acuerdo. El pago lo confirma una persona. |
| R12 | **Pasar un encargo completo:** por ejemplo, por una emergencia el día del evento, con contrato y pagos ya registrados. |
| R13 | **Aviso de cliente compartido:** al preparar una oferta para un cliente que pasó por un pase, Xarpen muestra con qué colega trabajó (tipo de encargo y fecha) y a quién le tocan las sugerencias del ciclo de vida. |

### P2: no se construye ahora, pero el diseño no debe impedirlo

- Segundo grado con aval: "B responde por D".
- Reputación privada basada en resultados reales: entregas a tiempo y clientes que volvieron.
- Círculos para otros roles: segundo fotógrafo, editor, alquiler de equipo.
- Lo mismo en otras verticales a través de la fábrica de verticales.

## Base de datos que deja lista

Estas entidades son genéricas. Ninguna lleva la palabra "fotografía".

- **`StudioConnection`:** `studio_a`, `studio_b`, `status` (invitado · activo · eliminado), `created_at`, `roles` (para P2).
- **`Referral`:** `from_studio`, `to_studio`, `client_snapshot`, `request` (fecha, tipo de encargo, lugar, presupuesto), `agreement` (tipo, valor), `client_consent` (fecha, canal), `status`, `outcome`, `change_id`.
- **`Invite`:** `from_studio`, `contact`, `source` (círculo · pase), `accepted_at`. Sirve para medir el crecimiento que trae la red.

## Métricas

| Tipo | Métrica | Meta inicial (hipótesis) |
|---|---|---|
| Rápida | Consultas de fecha ocupada que se pasan | ≥ 30% |
| Rápida | Estudios activos con al menos 1 colega en su círculo | ≥ 50% a los 60 días |
| Rápida | Invitación de pase → registro de B | ≥ 40% |
| Rápida | Pases aceptados por C y por B | ≥ 50% |
| Lenta | Registros nuevos que llegan por un pase o una invitación al círculo | ≥ 25% a los 6 meses |
| Lenta | Retención de estudios con círculo frente a estudios sin círculo | Medir la diferencia |
| Lenta | Pases que terminan en contrato | Línea base en los primeros 3 meses |

## Preguntas abiertas

| Pregunta | Quién responde | ¿Bloquea? |
|---|---|---|
| ¿Qué exige la ley de protección de datos de cada mercado para compartir datos entre estudios? | Legal | Sí |
| ¿Las comisiones entre estudios requieren factura o recibo? | Legal / contador | No |
| ¿El colega invitado entra con un plan gratuito? ¿Por cuánto tiempo? | Negocio | Sí |
| ¿Por qué canal recibe C la recomendación (WhatsApp, correo, enlace)? | Diseño + ingeniería | No |

## Fases

1. **v1 (P0):** círculo, pase, consentimiento, registro del colega y estado.
2. **v1.1 (P1):** reciprocidad, disponibilidad libre/ocupado y recordatorio de comisión.
3. **Más adelante (P2):** según lo que muestren los datos de los primeros meses.

**Dependencia:** las decisiones de base de la Fase 0 del plan, en especial "Encargo" genérico en lugar de "Evento" y las conexiones entre estudios en el modelo de datos.
