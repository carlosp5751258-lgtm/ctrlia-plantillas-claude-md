# CLAUDE.md — API FastAPI

## Comandos
- Instalar: `uv sync` (alternativa: `pip install -e ".[dev]"`)
- Servidor local: `uv run fastapi dev app/main.py` (o `uv run uvicorn app.main:app --reload`)
- Tests: `uv run pytest -q` (uno: `uv run pytest tests/test_pedidos.py::test_crear_pedido`)
- Formato y lint: `uv run ruff format . && uv run ruff check --fix .`
- Tipos: `uv run mypy app/`
- Migraciones (Alembic): `uv run alembic revision --autogenerate -m "añade tabla pedidos"` y `uv run alembic upgrade head`

## Estructura
- `app/main.py`: crea la app e incluye routers.
- `app/routers/`: un `APIRouter` por recurso.
- `app/schemas/`: modelos Pydantic de entrada y salida.
- `app/models/`: modelos de base de datos (SQLAlchemy / SQLModel).
- `app/services/`: lógica de negocio, sin dependencias de FastAPI.

## Convenciones
- Pydantic v2: `model_config = ConfigDict(...)`, `model_dump()`, `field_validator`.
- Esquemas separados para entrada y salida (`PedidoCrear`, `PedidoLeer`); usa `response_model`.
- Dependencias con `Depends` (sesión de BD, usuario actual) y `Annotated`.
- Errores con `HTTPException` y códigos correctos (404, 409, 422); nada de 200 con `{"error": ...}`.
- `async def` solo si todo lo que se llama dentro es asíncrono; si usas librerías bloqueantes, usa `def`.
- Configuración con `pydantic-settings` leyendo variables de entorno.
- Tests con `TestClient` (o `httpx.AsyncClient`) y `app.dependency_overrides` para la BD.

## Flujo
1. Revisa router, esquema y modelo afectados.
2. Test del endpoint.
3. Implementa.
4. `ruff` + `mypy` + `pytest` antes de terminar.

## No hacer
- No llamar a código bloqueante (`requests`, `time.sleep`, drivers síncronos) dentro de `async def`.
- No devolver modelos ORM sin `response_model`: puedes filtrar campos sensibles por accidente.
- No editar migraciones de Alembic ya aplicadas; revisa siempre el autogenerado antes de aplicarlo.
- No commitear `.env`.
