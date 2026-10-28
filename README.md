# Gracia Escuelita (Gracia Cartelera)

Web app **phone-first** de estrellas de la escuelita bíblica infantil **Gracia** (también publicada como Gracia Cartelera).

Un solo archivo de app, sin build: [`index.html`](./index.html).

**Demo en vivo:** https://gracia-cartelera.web.app

## Flujo de padres (punto de integración)

Esto es lo que suele reutilizar otro sistema o otra IA:

1. Pantalla de bienvenida  
2. Abrir la hoja (sheet)  
3. Cuatro casillas para un código de **4 dígitos**  
4. Continuar  
5. Ficha del niño (estrellas, progreso hacia la meta, registros)

**No hay correo ni registro.** El código lo asignan los maestros.

## Modo maestro

Código `7777`: lista de niños reales, buscador, ver ficha y volver a la lista. Existe y funciona; el foco de integración para padres es el flujo de 4 dígitos + ficha.

## Modelo de datos

En el JS de `index.html`:

- `NINOS`: mapa `código → { nombre, layout: "tablero", saldoInicial?: { estrellas, hasta }, registros: [{ fecha, asistencia, comportamiento, versiculo }] }`
- `META = 50` (meta de estrellas)
- Cualquier marca del cartel físico (estrella, sticker redondo o ✓) cuenta como **1 estrella**

### Fuente viva vs. exportación

1. **Fuente viva de estrellas hoy:** el objeto `NINOS` dentro de [`index.html`](./index.html). Ahí se editan saldos y registros semanales.
2. **Exportación para otra base de datos:** [`datos-ninos.json`](./datos-ninos.json) es la misma información de los niños reales, lista para que otro sistema (por ejemplo el del pastor) la importe o la copie.
3. **Cómo se interpreta cada niño:**
   - `saldoInicial`: estrellas acumuladas en el cartel físico **hasta** una fecha (sin desglose por domingo).
   - `registros`: filas semanales (domingo a domingo) desde que empezó el sistema digital; cada campo `asistencia` / `comportamiento` / `versiculo` es `0` o `1`.
   - `total` = `saldoInicial.estrellas` (si existe) + suma de todas las marcas en `registros`.
   - En el cartel físico, cualquier casilla marcada = **1 estrella**.

## Cómo probar en local

Abre `index.html` en el navegador, o sirve la carpeta con un servidor estático simple, por ejemplo:

```bash
python3 -m http.server 8080
```

Luego visita `http://localhost:8080`.

## Nota de integración

Hoy la UI y los datos viven juntos en un solo HTML. Para unir esto con otro sistema, lo natural es:

1. Usar o sincronizar desde [`datos-ninos.json`](./datos-ninos.json) (o extraer `NINOS` a API/Firebase)
2. Reutilizar el flujo de acceso de 4 dígitos + ficha del niño

## Stack

HTML + CSS + JS vanilla · tipografías Fredoka / Nunito · sin framework
