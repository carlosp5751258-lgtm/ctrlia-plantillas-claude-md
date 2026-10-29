# CLAUDE.md — Proyecto Go

## Comandos
- Compilar todo: `go build ./...`
- Tests: `go test ./...` (uno: `go test ./internal/pedidos -run TestCrearPedido -v`)
- Tests con detector de carreras: `go test -race ./...`
- Análisis estático: `go vet ./...` y `golangci-lint run`
- Formato: `gofmt -w .` (o `goimports -w .` si el proyecto lo usa)
- Dependencias: `go mod tidy` después de añadir o quitar imports
- Ejecutar: `go run ./cmd/<servicio>`

## Estructura
- `cmd/<binario>/main.go`: solo arranque y cableado de dependencias.
- `internal/`: lógica de negocio, no importable desde fuera del módulo.
- `pkg/`: solo si de verdad se publica como librería.

## Convenciones
- Errores: devuélvelos, no los ignores. Envuélvelos con contexto: `fmt.Errorf("leer config: %w", err)`.
- Compara errores con `errors.Is` / `errors.As`, nunca comparando strings.
- `context.Context` es el primer parámetro en funciones que hacen E/S; respeta la cancelación.
- Interfaces pequeñas y definidas donde se **usan**, no donde se implementan.
- Tests en tabla (`tests := []struct{...}`) con `t.Run(tc.nombre, ...)`.
- Nombres cortos y en inglés en el código (convención de Go); comentarios de doc empiezan con el nombre del identificador.

## Flujo
1. Lee el paquete afectado y sus tests antes de cambiar nada.
2. Escribe o ajusta el test en tabla.
3. Implementa.
4. `gofmt` + `go vet` + `go test -race ./...` antes de dar la tarea por terminada.

## No hacer
- No usar `panic` para errores esperados (entrada inválida, red, disco).
- No lanzar goroutines sin forma de terminarlas (context, `sync.WaitGroup` o `errgroup`).
- No editar `go.sum` a mano.
- No añadir dependencias nuevas sin preguntar; la librería estándar suele bastar.
