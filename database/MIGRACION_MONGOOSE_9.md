# Migración de Mongoose 8.x a 9.x

Este documento describe los cambios clave realizados en los modelos de la carpeta `database/` para actualizar la sintaxis y compatibilidad de Mongoose 8.x a 9.x.

## Cambios principales

### 1. Middleware `pre('save')` sin `next()`
- **Antes (Mongoose 8.x):**
  - Los middlewares recibían un parámetro `next` y se llamaba manualmente (`next()`, `next(error)`).
- **Ahora (Mongoose 9.x):**
  - El parámetro `next` fue eliminado. Se debe usar funciones `async` o retornar una promesa. Los errores se lanzan con `throw`.

#### Ejemplo antes (8.x):
```ts
BookingSchema.pre('save', async function (next) {
  // ...
  if (!eventExists) {
    return next(new Error('No existe el evento'));
  }
  next();
});
```

#### Ejemplo después (9.x):
```ts
BookingSchema.pre('save', async function() {
  // ...
  if (!eventExists) {
    throw new Error('No existe el evento');
  }
  // No se llama a next()
});
```

### 2. Tipado de funciones middleware
- Se eliminó el tipado explícito de `next` y se usa solo `async function()`.
- Si se requiere acceso a las opciones de guardado, se puede usar `function(opts: SaveOptions)`.

### 3. Errores de compilación corregidos
- Los errores de tipo `Type 'SaveOptions' has no call signatures` desaparecen al actualizar la sintaxis.

## Archivos modificados
- `database/booking.model.ts`
- `database/event.model.ts`

## Referencias
- [Mongoose 9.x Migration Guide](https://mongoosejs.com/docs/migrating_to_9.html)
- [Mongoose Middleware Docs](https://mongoosejs.com/docs/middleware.html)

---
**Actualizado:** 20 de abril de 2026
