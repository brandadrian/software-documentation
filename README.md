# Software Documentation Templates

Templates for documenting software systems in a consistent and structured way.

The templates are based on the [C4 model](https://c4model.com/) and focus on three guiding questions:

- **What** does the system need to do? Requirements and system responsibilities.
- **How** is the system structured? Architecture and component relationships.
- **Why** were important design decisions made? Architecture Decision Records (ADRs).

## Contents

The main template is located in [`system-documentation`](system-documentation/README.md) and includes:

- **What:** System overview, responsibilities, and software requirements specification
- **How:** Software architecture specification
- **Why:** Architecture Decision Records (ADRs)

## Repository Structure

```text
system-documentation/
├── README.md
├── software-requirements-specification.md
├── software-architecture-specification.md
├── adr/
│   ├── README.md
│   └── NNNN-decision-title.md
└── assets/
```

## Usage

1. Copy the `system-documentation` directory into your project.
2. Replace the placeholder content with project-specific information.
3. Add new architecture decisions using the [ADR instructions](system-documentation/adr/README.md).