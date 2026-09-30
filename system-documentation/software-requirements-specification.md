# System name Software Requirements Specification

Status: **Draft** | **Accepted**

## Introduction

This document specifies what the system must do. How it is built is described in the [Software Architecture Specification](software-architecture-specification.md).

Weights used to categorize the requirements:

- **Shall:** Indicates a mandatory requirement that must be implemented to meet the system's objectives.
- **Should:** Represents a highly desirable requirement that is not mandatory but would significantly enhance the system if included.
- **May:** Denotes an optional requirement that could be implemented if resources and time allow, but is not critical to the system's success.

## Customer Problems or Needs

- Problem or need, with only verified facts.

## Stakeholders

| Id | Stakeholder | Role |
|---|---|---|
| S-01 | Business owner | What this stakeholder does or decides. |
| S-02 | External partner | What this stakeholder does or decides. |
| S-03 | Development | Implements and operates the system. |

## Overall description

One short paragraph: what the system does, from input to result.

## Scope

**In scope**
- What the system covers.

**Out of scope**
- What is explicitly not covered.

## Use case specification

- UC-01 Action phrase

## Functional Requirements

Describe what the system does, not how it is built.

| ID | Name | Description | Weight | Stakeholder | Use case |
|---|---|---|---|---|---|
| FR-001 | Name | What the system must do. | Shall | S-01 | UC-01 |

## Non Functional Requirements

Describe a quality, not a solution.

| ID | Name | Description | Weight | Stakeholder |
|---|---|---|---|---|
| NFR-001 | Name | Quality the system must have. | Shall | S-03 |

## Assumptions and open issues

| # | Topic | Affects | Owner | Status |
|---:|---|---|---|---|
| A-01 | Assumption the requirements rely on. | FR-001 | S-01 | Assumption, with source |
| O-01 | Open question. | FR-001 | S-02 | Open |
