# CLAUDE.md — Proyecto Laravel

## Comandos
- Instalar: `composer install && npm install`
- Servidor local: `php artisan serve` + `npm run dev` (Vite) — o `composer run dev` si existe ese script
- Tests: `php artisan test` (uno: `php artisan test --filter=CrearPedidoTest`)
- Formato: `./vendor/bin/pint` (verificar: `./vendor/bin/pint --test`)
- Análisis estático: `./vendor/bin/phpstan analyse` (si el proyecto tiene Larastan)
- Migraciones: `php artisan make:migration create_pedidos_table` y `php artisan migrate`
- Rutas: `php artisan route:list`

## Convenciones
- Validación en Form Requests (`php artisan make:request`), no en el controlador.
- Autorización con Policies y `$this->authorize()` / `Gate`.
- Controladores delgados; lógica de negocio en Actions o Services en `app/`.
- Eloquent: evita N+1 con `with()`; usa `$fillable` explícito en cada modelo.
- Respuestas de API con API Resources (`JsonResource`).
- Configuración leída con `config()`; `env()` solo dentro de `config/*.php`.
- Tests de feature con `RefreshDatabase` y factories.

## Flujo
1. Revisa rutas (`routes/web.php`, `routes/api.php`) y el modelo afectado.
2. Escribe o ajusta el test de feature.
3. Implementa (migración → modelo → request → controlador → resource).
4. `pint` + `phpstan` + `php artisan test` antes de terminar.

## No hacer
- No ejecutar `php artisan migrate:fresh`, `db:wipe` ni `migrate:rollback` sin permiso explícito.
- No modificar migraciones que ya se ejecutaron en producción: crea una nueva.
- No commitear `.env` ni usar `DB::raw` con datos del usuario sin bindings.
- No desactivar la protección CSRF ni el mass-assignment (`$guarded = []`).
