# .NET Project Structure Guideline

Status: **Draft**

Derived from the conventions of the translation-processor project. Applies
to new .NET projects. Deviations are recorded as an ADR in the project.

## Repository layout

```text
<repository-name>/
├── src/
│   ├── <Product>.sln
│   ├── <Product>/                  Host (CLI, API, worker)
│   ├── <Product>.Core/             Logic, models, interfaces
│   └── <Product>.<Adapter>/        One project per external system
├── tests/
│   └── <Product>.Tests/
├── files/                          Runtime config, inputs, outputs (if file-based)
│   ├── config/
│   ├── inputs/
│   └── outputs/<command>/
├── docs/
│   ├── README.md
│   ├── software-requirements-specification.md
│   ├── software-architecture-specification.md
│   ├── adr/
│   └── test-cases/
├── .gitignore
└── README.md
```

- `src/` contains only source projects and the solution.
- `tests/` contains test projects; never inside `src/`.
- `logs/` and `data/` are runtime directories, not source code, and are
  Git-ignored.

## Naming

| Element | Convention | Example |
|---|---|---|
| Repository | kebab-case | `translation-processor` |
| Solution | `<Product>.sln` in `src/`, PascalCase | `src/TranslationProcessor.sln` |
| Host project | `<Product>` | `TranslationProcessor` |
| Core project | `<Product>.Core` | `TranslationProcessor.Core` |
| Adapter project | `<Product>.<ExternalSystem>` | `TranslationProcessor.Shopware`, `TranslationProcessor.OpenAI` |
| Test project | `<Product>.Tests` | `TranslationProcessor.Tests` |
| Root namespace | Same as project name | `TranslationProcessor.Core.Models` |
| Interfaces | `I<Purpose>` in `Core/Abstractions/` | `ITranslationProvider` |
| Adapter interfaces | Named for the external system | `IShopwareTranslationRepository` |

The product name is the same in repository (kebab-case), solution and
projects (PascalCase).

## Projects and references

```text
<Product>             → Core, all adapters
<Product>.<Adapter>   → Core
<Product>.Core        → no adapter projects
<Product>.Tests       → Core, adapters as needed
```

- Dependencies point inward. Core contains no provider-, database- or
  SDK-specific types.
- Core defines the interfaces; adapters implement them.
- Only the host wires adapters to interfaces (dependency injection at
  startup).
- Adapters are libraries, not separately deployable services.
- Add a new project only for a new external system or a concrete need.

## Folders inside `src/`

### Host (`<Product>/`)

```text
<Product>/
├── Commands/                One class per command (CLI) — or Endpoints/ (API)
├── Program.cs               Startup, configuration, DI, logging
├── appsettings.json         Local, Git-ignored
└── appsettings.example.json Committed template
```

The host contains no business logic and no adapter-specific logic
(e.g. no SQL or field mapping).

### Core (`<Product>.Core/`)

```text
<Product>.Core/
├── Abstractions/    Interfaces implemented by adapters
├── Models/          Shared models and file/document contracts
├── Configuration/   Options classes and configuration loading
├── Files/           File reading and writing (if file-based)
└── <Feature>/       Logic per feature, e.g. Translation/
```

### Adapter (`<Product>.<Adapter>/`)

Contains only the access to its external system (SQL, HTTP client, SDK)
and the mapping to Core models. Folder structure as needed.

## appsettings

- `appsettings.json` is local and Git-ignored. It is the only place for
  secrets (API keys, connection strings).
- `appsettings.example.json` is committed, has the same structure and
  contains placeholders instead of secrets.
- Setup creates the local file from the template without overwriting it:

  ```sh
  cp -n src/<Product>/appsettings.example.json src/<Product>/appsettings.json
  ```

- One top-level section per external system, named like the adapter;
  keys in PascalCase:

  ```json
  {
    "OpenAI": {
      "ApiKey": "<your-api-key>",
      "Model": "gpt-4o-mini"
    },
    "Shopware": {
      "ConnectionString": "Server=<host>;Port=3306;Database=<db>;User=<user>;Password=<password>",
      "LanguageIds": {
        "fr": "<32-hex-id>"
      }
    }
  }
  ```

- Optional values have a documented default in code; required values
  produce a clear error at startup if missing.
- The settings file is copied to the output directory during the build.
  A CLI offers `--settings <file.json>` to select another file.
- Editable non-secret configuration that is not a setting (e.g. prompts)
  goes into `files/config/` as a separate JSON file.
- Secrets never appear in output files or logs.
- Every setting is documented in the README with key, meaning and default.

## Logging

- Technical logs are kept separate from data output (e.g. JSON result files).
- Line format:

  ```text
  timestamp;loglevel;message
  ```

- The message consists of `key=value` fields. Every message contains the
  context needed to correlate it:

  | Field | Content |
  |---|---|
  | `command` | Command or operation name |
  | `id` | Execution ID; matches the ID in output filenames and metadata |
  | `event` | Event in kebab-case, e.g. `item-translated`, `completed` |

  Example:

  ```text
  command=translate id=<id> event=item-translated input=<file> itemId=<id> processed=1 total=10
  command=translate id=<id> event=completed files=3 fileFailures=0 durationMs=15432
  ```

- Long-running work logs progress (`processed`/`total`).
- Completion, failure, timeout and cancellation log `durationMs`.
- Not logged: secrets, connection strings, full payloads or content texts.
- Errors per file or item are logged and processing continues where
  possible; the exit code is `1` if anything failed.

## README and docs

- The README contains: short description, status, overview diagram,
  setup, usage per command, configuration, logging, links to `docs/`.
- `docs/` is created from the
  [system-documentation](../../system-documentation/README.md) template:
  SRS, SAS, ADRs (`NNNN-short-title.md`) and test cases
  (`TC-<MODULE>-<NNN>-short-title.md`).

## .gitignore (minimum)

```text
bin/
obj/
src/**/appsettings.json
logs/
data/
```

## Open points

Not defined by the existing projects; to be decided:

- Timestamp format in log lines (e.g. ISO 8601 UTC).
- Log destination (console only or additional file in `logs/`) and logging
  library.
- `.sln` or `.slnx` for .NET 10.
- Whether `files/outputs/` is Git-ignored.
- Structure for web APIs and Angular frontends in the same repository.
