# CLAUDE.md — Pulso

## Producto

**Pulso** es un CRM para **Norte Consultores**, una consultora de 8 personas que vende **capacitaciones** y **proyectos**. Sirve para seguir oportunidades comerciales desde el primer contacto hasta el cierre.

## Reglas no negociables

1. **Un solo archivo `index.html`.** HTML, CSS y JS van dentro del mismo archivo. Nada de servidores, backend, build steps ni bundlers. Debe funcionar abriéndolo con doble clic.
2. **Los datos viven en el navegador** (`localStorage`). Envuelve cada lectura y escritura en `try/catch`; si el almacenamiento falla o viene vacío, la app igual debe cargar.
3. **20 oportunidades de prueba inventadas** se cargan la primera vez (cuando no hay datos guardados). Empresas, personas y montos ficticios pero verosímiles para Chile. Nunca uses empresas o personas reales.
4. **Español de Chile** en toda la interfaz, textos y datos de ejemplo (p. ej. "RUT", "cotización", "región"). Tutea al usuario con naturalidad; evita modismos excesivos.
5. **Pesos chilenos (CLP):** sin decimales, punto como separador de miles, signo `$` delante. Ejemplo: `$1.250.000`. Usa `Intl.NumberFormat('es-CL', { style: 'currency', currency: 'CLP' })`. Fechas en formato `dd-mm-aaaa`.
6. **Lo visual sigue `DESIGN.md`.** Colores, tipografía, espaciados y componentes salen de ahí. Si algo no está definido en `DESIGN.md`, pregunta o propón una adición antes de inventar un estilo nuevo.

## Tareas

- Las tareas viven en **`.project/tasks.md`**. Léelo al empezar cada sesión.
- Trabaja una tarea a la vez; al terminarla, márcala como hecha en ese archivo.
- Si descubres trabajo nuevo, agrégalo a `.project/tasks.md` en vez de hacerlo sin avisar.

## Dependencias externas

- Evítalas. Si una librería es imprescindible, cárgala desde CDN con versión exacta y verifica que la app no se rompa sin conexión en lo esencial.

## Antes de dar algo por terminado

- `index.html` abre sin errores en la consola.
- Los 20 datos de prueba aparecen en una instalación limpia (borra `localStorage` para probar).
- Los cambios persisten al recargar la página.
- Todos los montos se ven en CLP y todos los textos en español de Chile.
- Lo nuevo respeta `DESIGN.md`.
