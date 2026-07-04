### Task 1: Project scaffolding + health check

**Files:**
- Create: `go.mod`
- Create: `cmd/server/main.go`
- Create: `internal/httpserver/router.go`
- Test: `internal/httpserver/router_test.go`

**Interfaces:**
- Produces: `httpserver.NewRouter() *chi.Mux` — used by `main.go` and every later task that registers routes.

- [ ] **Step 1: Initialize the module and dependencies**

```bash
go mod init businesscore
go get github.com/go-chi/chi/v5@v5.1.0
```

- [ ] **Step 2: Write the failing test**

`internal/httpserver/router_test.go`:
```go
package httpserver

import (
	"net/http"
	"net/http/httptest"
	"testing"
)

func TestHealthEndpoint(t *testing.T) {
	r := NewRouter()
	req := httptest.NewRequest(http.MethodGet, "/health", nil)
	rec := httptest.NewRecorder()

	r.ServeHTTP(rec, req)

	if rec.Code != http.StatusOK {
		t.Fatalf("expected 200, got %d", rec.Code)
	}
	if rec.Body.String() != "ok" {
		t.Fatalf("expected body %q, got %q", "ok", rec.Body.String())
	}
}
```

- [ ] **Step 3: Run test to verify it fails**

Run: `go test ./internal/httpserver/... -v`
Expected: FAIL — `NewRouter` undefined.

- [ ] **Step 4: Write minimal implementation**

`internal/httpserver/router.go`:
```go
package httpserver

import (
	"net/http"

	"github.com/go-chi/chi/v5"
)

func NewRouter() *chi.Mux {
	r := chi.NewRouter()
	r.Get("/health", func(w http.ResponseWriter, req *http.Request) {
		w.WriteHeader(http.StatusOK)
		w.Write([]byte("ok"))
	})
	return r
}
```

`cmd/server/main.go`:
```go
package main

import (
	"log"
	"net/http"
	"os"

	"businesscore/internal/httpserver"
)

func main() {
	port := os.Getenv("PORT")
	if port == "" {
		port = "8080"
	}
	r := httpserver.NewRouter()
	log.Printf("listening on :%s", port)
	log.Fatal(http.ListenAndServe(":"+port, r))
}
```

- [ ] **Step 5: Run test to verify it passes**

Run: `go test ./internal/httpserver/... -v`
Expected: PASS

- [ ] **Step 6: Commit**

```bash
git add go.mod go.sum cmd internal/httpserver
git commit -m "feat: project scaffolding with health check endpoint"
```

---

### Task 5: Module contract + registry

**Interfaces:**
- Produces: `module.Module` interface, `module.MenuItem`, `module.Permission`, `module.RoleSeed`, `module.Registry` with `Register`, `Get`, `All` — every domain module (Tasks 8, 12, 13, 14) implements this interface; the provisioning service (Task 15) iterates `Registry.All()`.
