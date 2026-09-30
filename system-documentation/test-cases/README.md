# Test cases

Manual test cases for the system. Each test case verifies one or more requirements of the [Software Requirements Specification](../software-requirements-specification.md).

## Index

Group test cases by the modules in the system. Replace the example module below with the relevant modules.

### Module name

| ID | Title | Requirements | Priority |
|---|---|---|---|
| [TC-MODULE-001](module/TC-MODULE-001-test-title.md) | Test title | FR-001 | High |

## Creating a new test case

1. Copy [TC-MODULE-001-test-title.md](module/TC-MODULE-001-test-title.md) to `TC-<MODULE>-<NNN>-short-title.md` in a folder named for its module, using the next number in that module, starting at `001`.
2. Link the requirements the test case verifies. Describe only documented behavior.
3. Set the `Module` and add a row to the matching group in the index above.

## Running a test case

Fill in `Actual Result` for each step and set `Status` to `Passed` or `Failed`. Do not commit filled-in runs of the same test case; record the result of a run in the related ticket or pull request.
