# Project Guidelines

## Build and Test

- Run commands from the repository root.
- Use `task validate -- --name <server>` for `server.yaml` changes, `task build -- --tools <server>` when build or tool discovery behavior matters, and `task unittest` when changing Go code or shared validation logic.
- Prefer `task wizard` or `task remote-wizard` when creating new registry entries instead of hand-rolling files.
- This repo relies on Trunk-managed checks such as `gofmt`, `golangci-lint`, `markdownlint`, `yamllint`, `shellcheck`, and `hadolint`. Keep edits compatible with those tools even when you do not run the full stack.

## Architecture

- `cmd/` contains the CLI entrypoints, `pkg/` contains shared packages, `internal/` contains non-exported implementation details, and `servers/` contains the registry data that most contributions modify.
- Treat `servers/<name>/server.yaml` as the source of truth for a registry entry. `tools.json` is optional for local servers and is standard for remote servers with dynamic tool discovery.

## Conventions

- Keep the `servers/<name>/` directory name aligned with `server.yaml:name`, and use lowercase hyphenated names unless you are following an existing documented exception.
- Know the three supported entry types before editing definitions: `server` for containerized local servers, `remote` for hosted HTTP/SSE servers, and `poci` for plain CLI wrappers.
- Local server entries must pin `source.commit`.
- If a server cannot list tools until after configuration, add `tools.json` instead of relying on live tool discovery during `task build -- --tools <server>`.
- Remote server entries should usually include `server.yaml`, `tools.json`, and `readme.md` with the upstream documentation link.
- CI validates changed servers with tooling built from `main`, so do not assume a metadata-only PR can rely on simultaneous unmerged changes to the validator or builder.

## Docs

- Link to `../CONTRIBUTING.md` for contribution workflow and PR expectations.
- Link to `../docs/configuration.md` for `server.yaml` schema details.
- Link to `SECURITY.md` for vulnerability reporting instead of duplicating security process details.