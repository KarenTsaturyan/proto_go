# Protos — overview and generation ⚙️

## Overview
This folder contains Protocol Buffer (`.proto`) definitions and the generated Go code used by the project. Keep your `.proto` files in `proto/` and generate Go sources into `gen/go/`.

## Layout
- `proto/` — source `.proto` files (e.g., `proto/sso/sso.proto`).
- `gen/go/` — generated Go code (e.g., `gen/go/sso/sso.pb.go`, `gen/go/sso/sso_grpc.pb.go`).
- `go.mod` — module for the generated code (keeps imports tidy).

## How to regenerate (Windows PowerShell)
Run this command from the `Protos` folder after you update a `.proto` file:

```
protoc -I .\proto --go_out=.\gen\go --go_opt=paths=source_relative --go-grpc_out=.\gen\go --go-grpc_opt=paths=source_relative .\proto\sso\sso.proto
```

> Note: the `-I .\proto` flag sets the include directory; the `paths=source_relative` option makes generated files use paths relative to the source file, which keeps package paths stable.

## Requirements & quick checks 🔍
- `protoc` (the protobuf compiler) installed and available in PATH: `protoc --version` should print `libprotoc X.Y.Z`.
- Install the Go plugins used by the command:

```
# run from a powershell or cmd that has Go in PATH
go install google.golang.org/protobuf/cmd/protoc-gen-go@latest
go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@latest
```

- Ensure `$env:GOPATH\bin` or `GOBIN` is in your `PATH` so `protoc` can find `protoc-gen-go` and `protoc-gen-go-grpc`.
- If paths contain spaces, wrap them in quotes when running the command.
- After install, verify `protoc --version` and that `protoc-gen-go` and `protoc-gen-go-grpc` are discoverable.

## Tips
- Keep `.proto` files as the source of truth and regenerate code as part of build or CI when needed. ✅
- Commit generated code if your CI/build requires it or if consumers depend on generated artifacts being present.

---

## How to update versions
### In services
go get github.com/KarenTsaturyan/proto_go@v1.0.0
go mod tidy

## Proto 
git tag v0.0.2
git push origin v0.0.2