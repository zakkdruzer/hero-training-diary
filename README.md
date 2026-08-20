# Hero Training Diary

Diario de entrenamiento de un héroe de anime. Registra energía diaria (1–5), nota opcional y fecha. Los datos se guardan en localStorage y se cargan al iniciar.

## Tecnologías

- Vue 3 (Composition API, `<script setup>`)
- Vite
- Bootstrap (CSS por CDN)

## Cómo levantar el proyecto

```bash
npm install
npm run dev
```

## Decisiones técnicas

- Se usa `watch` con `{ deep: true }` para guardar automáticamente los registros en localStorage cada vez que el array cambia.
- `onMounted` se usa para cargar los registros guardados al iniciar la app, manejando el caso en que localStorage esté vacío.
- Los registros se ordenan del más reciente al más antiguo usando un `computed` que crea una copia ordenada.
- El resumen (promedio, mejor y peor día) se calcula con `computed` para evitar recalcular manualmente.

## Enlace desplegado

[Ver app en GitHub Pages](https://zakkdruzer.github.io/hero-training-diary/)