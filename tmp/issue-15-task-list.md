# Issue #15: Add `jlserve build` Command for Containerized Deployments

We are working on this issue. https://github.com/svishnu88/jlserve/issues/15

## High-Level Task List

### Phase 1: Environment Variable Support ✅
- [x] Add environment variable validation utilities
  - [x] `JLSERVE_CACHE_DIR` validator (required for both build and dev)
  - [x] `UV_CACHE_DIR` - decided not needed (UV handles internally)
- [x] Create configuration module for cache directory management
- [x] Add tests for environment variable validation

**Implementation:**
- Created `jlserve/config.py` with `get_jlserve_cache_dir()` function
- Added `CacheConfigError` exception to `jlserve/exceptions.py`
- Created comprehensive tests in `jlserve/config_test.py`
- All tests passing (4/4 config tests, 105/105 total tests)

### Phase 2: Download Weights Lifecycle Method ✅
- [x] Update decorator/validator to recognize `download_weights()` method
  - [x] Add validation that method exists on app class
  - [x] Add validation that method has correct signature (no params, no return)
  - [x] Make it required for all apps (not just build)
- [x] Document `download_weights()` pattern in docstrings
- [x] Add tests for method presence/signature validation

**Implementation:**
- Created shared `_validate_lifecycle_method()` helper in `jlserve/validator.py`
- Added `validate_setup_method()` and `validate_download_weights_method()` validators
- Both methods now required for all apps (enforced by `validate_app()`)
- Added 24 comprehensive tests covering all validation scenarios
- Updated all existing test apps (38 apps) to include both lifecycle methods
- Validators ensure: method exists, is callable, only accepts 'self', returns None
- All tests passing (129/129 total tests)

### Phase 3: Build Command Implementation
- [ ] Create `jlserve build` CLI command in cli.py
  - [ ] Add Typer command definition
  - [ ] Accept file path argument
  - [ ] Validate JLSERVE_CACHE_DIR environment variable
- [ ] Implement build workflow:
  - [ ] Extract requirements from Python file (scan imports/app)
  - [ ] Install dependencies using `uv pip install`
  - [ ] Load app class and call `download_weights()`
  - [ ] Optionally validate by calling `setup()`
- [ ] Add proper error handling and user feedback
- [ ] Add tests for build command

### Phase 4: Requirement Extraction
- [ ] Create utility to extract Python dependencies from app file
  - [ ] Option 1: Parse imports using AST
  - [ ] Option 2: Use existing requirements.txt if present
  - [ ] Option 3: Infer from pyproject.toml dependencies
- [ ] Handle edge cases (missing dependencies, version conflicts)
- [ ] Add tests for requirement extraction

### Phase 5: Update Dev Command
- [ ] Modify `jlserve dev` to check JLSERVE_CACHE_DIR
  - [ ] Log warning if not set (weights may be re-downloaded)
  - [ ] Skip calling `download_weights()` - rely on cache
- [ ] Update lifespan handler to work with cached weights
- [ ] Add tests for dev command with/without cache

### Phase 6: Integration & Testing
- [ ] End-to-end test: build → dev workflow
- [ ] Test with actual model weights (mock HuggingFace download)
- [ ] Test cache reuse across multiple builds
- [ ] Test error cases (missing cache dir, failed downloads)
- [ ] Add example app demonstrating the pattern

### Phase 7: Documentation
- [ ] Update README with `jlserve build` usage
- [ ] Document environment variables (JLSERVE_CACHE_DIR, UV_CACHE_DIR)
- [ ] Add example showing download_weights() implementation
- [ ] Add Dockerfile example for containerized deployment
- [ ] Update CLAUDE.md with new commands and patterns

## Key Design Decisions to Validate

1. **Should `download_weights()` be required for all apps or only when using `build`?**
   - Proposal: Optional in general, but required validation when `jlserve build` is used

2. **How to handle requirements extraction?**
   - Need to decide between AST parsing vs. requirements.txt vs. pyproject.toml

3. **Should `setup()` validation be mandatory or optional in build?**
   - Proposal: Optional with --validate flag

4. **Error handling for missing cache directory** ✅ DECIDED
   - Both Build and Dev: Hard error (required)
   - Rationale: Simpler, more predictable behavior. Cache dir should always be set.

## Success Criteria

- [ ] `jlserve build app.py` successfully downloads dependencies and weights to cache
- [ ] `jlserve dev app.py` reuses cached weights without re-downloading
- [ ] Validation catches missing `download_weights()` method when using build
- [ ] Documentation clearly explains the containerized deployment pattern
- [ ] All tests pass including integration tests
