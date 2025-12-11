## Relevant Files

- `jarvis/__init__.py` - Main module entry point, exports the `endpoint` decorator
- `jarvis/decorator.py` - Contains the `@jarvis.endpoint()` decorator implementation
- `jarvis/decorator_test.py` - Unit tests for the decorator
- `jarvis/validator.py` - Validation logic for endpoint classes (run method, type hints)
- `jarvis/validator_test.py` - Unit tests for validation logic
- `jarvis/server.py` - FastAPI server integration and request handling
- `jarvis/server_test.py` - Unit tests for server integration
- `jarvis/cli.py` - CLI implementation for `jarvis dev` command
- `jarvis/cli_test.py` - Unit tests for CLI
- `jarvis/exceptions.py` - Custom exception classes for error handling
- `pyproject.toml` - Project configuration and dependencies
- `README.md` - Usage documentation and examples

### Notes

- Unit tests should typically be placed alongside the code files they are testing (e.g., `decorator.py` and `decorator_test.py` in the same directory).
- Use `pytest` to run tests. Running without a path executes all tests found by the pytest configuration.

## Instructions for Completing Tasks

**IMPORTANT:** As you complete each task, you must check it off in this markdown file by changing `- [ ]` to `- [x]`. This helps track progress and ensures you don't skip any steps.

Example:
- `- [ ] 1.1 Read file` → `- [x] 1.1 Read file` (after completing)

Update the file after completing each sub-task, not just after completing an entire parent task.

## Tasks

- [ ] 0.0 Create feature branch
  - [ ] 0.1 Create and checkout a new branch for this feature (e.g., `git checkout -b feature/jarvis-sdk-v0`)

- [ ] 1.0 Set up project structure and dependencies
  - [ ] 1.1 Create the `jarvis/` package directory with `__init__.py`
  - [ ] 1.2 Create `pyproject.toml` with project metadata and dependencies (FastAPI, Pydantic, Uvicorn, Typer/Click)
  - [ ] 1.3 Set up a virtual environment and install dependencies
  - [ ] 1.4 Configure pytest in `pyproject.toml` for running tests
  - [ ] 1.5 Create `jarvis/exceptions.py` with custom exception classes (e.g., `EndpointValidationError`)

- [ ] 2.0 Implement the `@jarvis.endpoint()` decorator
  - [ ] 2.1 Create `jarvis/decorator.py` with the `endpoint` function that accepts a `name` parameter
  - [ ] 2.2 Implement the decorator to store endpoint metadata on the decorated class (e.g., `_jarvis_endpoint_name`)
  - [ ] 2.3 Create a registry to track all decorated endpoint classes for later discovery
  - [ ] 2.4 Export the `endpoint` decorator from `jarvis/__init__.py` so users can call `@jarvis.endpoint()`
  - [ ] 2.5 Write unit tests in `jarvis/decorator_test.py` to verify decorator behavior

- [ ] 3.0 Implement endpoint validation logic
  - [ ] 3.1 Create `jarvis/validator.py` with validation functions
  - [ ] 3.2 Implement check for `run()` method existence on the endpoint class
  - [ ] 3.3 Implement check for type hints on `run()` method (input parameter and return type)
  - [ ] 3.4 Implement check that input type hint is a Pydantic `BaseModel` subclass
  - [ ] 3.5 Implement check that return type hint is a Pydantic `BaseModel` subclass
  - [ ] 3.6 Raise clear error messages for each validation failure (as specified in PRD)
  - [ ] 3.7 Write unit tests in `jarvis/validator_test.py` for all validation scenarios

- [ ] 4.0 Implement the FastAPI server integration
  - [ ] 4.1 Create `jarvis/server.py` with a function to create a FastAPI app from an endpoint class
  - [ ] 4.2 Implement instantiation of the endpoint class when the server starts
  - [ ] 4.3 Implement calling `setup()` method (if defined) during server startup using FastAPI lifespan
  - [ ] 4.4 Create a POST route at `/` that calls the endpoint's `run()` method
  - [ ] 4.5 Wire up request body validation using the input Pydantic model from type hints
  - [ ] 4.6 Wire up response serialization using the output Pydantic model from type hints
  - [ ] 4.7 Ensure OpenAPI docs are auto-generated at `/docs` with correct schema
  - [ ] 4.8 Write unit tests in `jarvis/server_test.py` using FastAPI TestClient

- [ ] 5.0 Implement the `jarvis dev` CLI command
  - [ ] 5.1 Create `jarvis/cli.py` using Typer (or Click) for CLI framework
  - [ ] 5.2 Implement the `dev` command that accepts a file path argument
  - [ ] 5.3 Implement dynamic loading of the user's Python file to discover endpoint classes
  - [ ] 5.4 Add `--port` option with default value of `8000`
  - [ ] 5.5 Start Uvicorn server programmatically with the generated FastAPI app
  - [ ] 5.6 Print startup message: "Serving {name} at http://localhost:{port}" and "Docs at http://localhost:{port}/docs"
  - [ ] 5.7 Configure the CLI entry point in `pyproject.toml` so `jarvis` command is available after install
  - [ ] 5.8 Write unit tests in `jarvis/cli_test.py` for CLI argument parsing

- [ ] 6.0 Add error handling and validation responses
  - [ ] 6.1 Ensure FastAPI returns 422 with validation details for invalid input JSON
  - [ ] 6.2 Implement exception handling in the route to catch exceptions from `run()` and return 500 with error message
  - [ ] 6.3 Implement error handling for `setup()` failures that prevents server from starting and shows error message
  - [ ] 6.4 Add integration tests for error scenarios (invalid JSON, exceptions in run/setup)

- [ ] 7.0 Write tests and documentation
  - [ ] 7.1 Create end-to-end integration test with a sample endpoint (like the Greeter example from PRD)
  - [ ] 7.2 Create integration test with ML-like endpoint (mocking the model)
  - [ ] 7.3 Verify all success criteria from PRD are met (< 10 lines, < 2 seconds startup, typo detection, autocomplete, OpenAPI)
  - [ ] 7.4 Write `README.md` with installation instructions, quick start example, and API reference
  - [ ] 7.5 Run full test suite and ensure all tests pass
