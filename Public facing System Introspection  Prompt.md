
Facts only.
Inspect only the provided workspace/repository.
Cite file and line numbers for important claims.
Do not assume behavior from names alone.
Classify implemented vs implied.
Separate active runtime code from tests, experiments, demos, and dormant code.
Report unknowns explicitly.
Do not suggest changes unless the prompt asks for recommendations.
````

Evidence levels:

```text
DIRECT_CODE
DIRECT_TEST
DIRECT_DOC
DIRECT_CONFIG
INFERRED_FROM_CALL_CHAIN
UNKNOWN_FROM_WORKSPACE
```

Runtime status labels:

```text
ACTIVE_RUNTIME_PATH
TEST_ONLY
EXPERIMENT_ONLY
DOCS_ONLY
CONFIG_ONLY
DEFINED_NO_CALLER_FOUND
UNKNOWN_FROM_WORKSPACE
```

Required evidence block for every important claim:

```text
file:
line/range:
evidence:
evidence_level:
runtime_status:
```

Final line for every report:

```text
END OF FACTUAL REPORT
```

---

## 1. Repository Factual Map

Task:

Produce a factual map of this repository/workspace only.

Report only:

* repository/workspace root
* major folders/modules/packages
* runtime entrypoints
* CLI entrypoints
* API/server entrypoints
* UI entrypoints if present
* test entrypoints
* experiment/demo entrypoints
* docs/contracts/specs
* implemented behavior
* implied-only behavior
* not implemented behavior
* references to files/systems outside the workspace
* unknowns

Rules:

* Do not suggest improvements.
* Do not evaluate quality.
* Do not infer future architecture.
* Every major claim needs file/line evidence.

Required sections:

1. Workspace root and boundaries
2. Module/package inventory
3. Entrypoint inventory
4. Runtime status inventory
5. Implemented behavior
6. Implied-only behavior
7. Not implemented behavior
8. Tests and verification commands found
9. External references and dependencies
10. Open factual questions

---

## 2. Security Surface Inventory

Task:

Produce a facts-only inventory of security-relevant surfaces in this workspace.

Report only:

* path handling
* file writes/deletes/overwrites
* secrets/credentials/API keys
* network ingress/egress
* process/shell execution
* serialization/deserialization
* input trust boundaries
* logging/audit traces
* concurrency/locking
* permissions/authorization/authentication

Rules:

* Do not recommend fixes.
* Do not call something secure/insecure unless the code/docs directly state it.
* If none are found, use exact `NO_*_FOUND` markers.

Required sections:

1. Workspace boundary / path safety
2. Write/delete/mutation authority
3. Destructive command inventory
4. Secrets and sensitive data
5. Network/external egress
6. Process/shell/tool execution
7. Serialization/deserialization
8. Input trust boundaries
9. Logging/audit/trace behavior
10. Concurrency/locking/atomicity
11. Authentication/authorization/permissions
12. Open factual questions

---

## 3. Route / Command / UI-to-Behavior Audit

Task:

For every public route, CLI command, UI action, public handler, or exported operation, prove whether it reaches real backend behavior.

Report only:

* public entrypoint exists
* input shape
* output shape
* backend call chain
* backend write/return behavior
* tests proving behavior
* gaps marked as unknown or implied-only

Rules:

* Do not count something as implemented only because a route/function/name exists.
* Distinguish UI/route shell from backend behavior.
* Mark unreachable or uncalled handlers as `DEFINED_NO_CALLER_FOUND`.

Required sections:

1. Public route/command/action inventory
2. Handler evidence
3. Backend call-chain evidence
4. Artifact/write/return evidence
5. Tests proving behavior
6. Runtime status classification
7. Missing or implied-only behavior
8. Open factual questions

---

## 4. Data Authority / Storage Lane Audit

Task:

Identify every data store or authority lane used by this workspace and what reads/writes it.

Generic lane tags:

```text
source_data
application_state
user_data
configuration
cache
index
database
log
report
artifact
model_or_ml_data
test_fixture
debug_export
credential
temporary_file
unknown
```

Report only:

* lane name
* files/directories/schemas/tables
* readers
* writers
* mutators
* deletion paths
* precedence rules
* promotion/approval gates if present
* debug/export paths

Rules:

* Every write must have a lane.
* Every authority claim must say whether it is direct code, direct test, direct doc, direct config, or inferred.
* Do not merge debug/export lanes with authority lanes.

Required sections:

1. Data lane inventory
2. Authority stores
3. Derived/debug/export stores
4. Readers by lane
5. Writers by lane
6. Promotion/mutation gates
7. Precedence rules
8. Untested lane behavior
9. Open factual questions

---

## 5. Generated Artifact Audit

Task:

Inventory generated files, reports, caches, build outputs, experiment outputs, sidecars, and derived data.

Report only:

* producer command/function
* output path
* committed vs ignored vs temporary
* deterministic vs timestamped/random/hash-derived/unknown
* rebuild command
* cleanup command
* source inputs
* authority/data lane

Rules:

* Do not recommend cleanup.
* Do not assume an artifact can be regenerated unless code/docs prove it.
* Mark external or missing outputs explicitly.

Required sections:

1. Generated artifact inventory
2. Producers
3. Inputs
4. Output paths
5. Git tracking/ignore status
6. Rebuild commands
7. Cleanup behavior
8. Determinism evidence
9. Open factual questions

---

## 6. Runtime Write-Path Audit

Task:

Trace all runtime write, overwrite, delete, move, rename, truncate, and append paths.

Report only:

* writer function
* caller chain
* path construction
* authority/data lane
* write mode
* atomicity
* target constraints
* tests

Rules:

* Include direct file writes and helper-mediated writes.
* Include APIs such as:

  * `open(..., "w")`
  * `open(..., "a")`
  * `write_text`
  * `write_bytes`
  * `Path.unlink`
  * `os.remove`
  * `os.replace`
  * `rename`
  * `shutil.move`
  * `shutil.rmtree`
  * database writes
  * object store writes
  * subprocesses that write files
* Separate production runtime writes from tests and experiments.

Required sections:

1. Runtime write inventory
2. Experiment/test write inventory
3. Delete/truncate inventory
4. Atomic replace/temp-file behavior
5. Path boundary checks
6. Authority/data lanes
7. Tests proving constraints
8. Open factual questions

---

## 7. Refactor No-Behavior-Change Prompt

Task:

Prepare or review a refactor while preserving behavior exactly.

Report only:

* current behavior receipts
* public interfaces that must remain stable
* tests that currently prove behavior
* files that may be touched
* files that must not be touched
* verification commands
* behavior changes detected after refactor

Rules:

* Do not introduce new features.
* Do not change public output shapes unless explicitly requested.
* If behavior changes, list the exact changed behavior with evidence.

Required sections:

1. Current behavior receipts
2. Stable interfaces
3. Allowed edit scope
4. Forbidden edit scope
5. Verification commands
6. Post-refactor behavior comparison
7. Open factual questions

---

## 8. Implementation Acceptance Audit

Task:

Decide whether an implementation satisfies a stated plan, ticket, or acceptance contract.

Report only:

* acceptance item
* evidence found
* evidence level
* pass/fail/unknown
* verification command output
* unverified claims

Rules:

* Do not accept claims without receipts.
* Do not treat passing tests as proof of untested acceptance items.
* Mark any item not directly verified as `UNKNOWN_FROM_WORKSPACE`.

Required sections:

1. Acceptance checklist
2. Code evidence
3. Test evidence
4. Command evidence
5. Missing evidence
6. Final acceptance status
7. Open factual questions

---

## 9. Binary / File Format Contract Audit

Task:

Audit binary formats, custom file formats, sidecars, manifests, indexes, caches, readers, and writers.

Report only:

* file format names
* magic/version/header fields
* row/record sizes
* metadata shape
* checksums/CRC/integrity checks
* readers/writers
* corrupt-data behavior
* tests
* debug/export parity paths

Rules:

* Do not infer binary safety from file extension names.
* Every format claim needs reader/writer evidence.
* Distinguish binary/cache artifacts from source-of-truth data.

Required sections:

1. File/binary format inventory
2. Writer paths
3. Reader paths
4. Header/version/integrity checks
5. Sidecar/manifest relationships
6. Debug/export relationships
7. Corruption/error behavior
8. Tests
9. Open factual questions

---

## 10. UI Truth / Backend Contract Audit

Task:

Check whether UI surfaces truthfully reflect backend behavior.

Report only:

* UI label/text/action
* backend endpoint/command/function
* data returned by backend
* UI state derived from backend
* placeholder/mock/static text
* tests or screenshots if present
* mismatches marked as facts

Rules:

* Do not critique design.
* Do not recommend UX changes.
* A UI label is factual only if backend behavior supports it.
* Mark static/demo/mock behavior explicitly.

Required sections:

1. UI surface inventory
2. Backend contract for each surface
3. Route-to-backend evidence
4. Static/mock/demo behavior
5. Tests or visual verification
6. Mismatch facts
7. Open factual questions

---

## 11. Dependency and External Service Audit

Task:

Inventory dependencies and external services used by the workspace.

Report only:

* package/dependency name
* where declared
* where imported/called
* runtime vs dev/test usage
* external network/service usage
* credentials/config required
* version constraints
* known optional/fallback behavior

Rules:

* Do not recommend dependency changes.
* Do not assume a dependency is used just because it is declared.
* Separate declared dependencies from actually imported/called dependencies.

Required sections:

1. Declared dependencies
2. Imported/called dependencies
3. Runtime dependency paths
4. Dev/test-only dependency paths
5. External services and network calls
6. Credential/config requirements
7. Optional/fallback behavior
8. Open factual questions

---

## 12. Startup / Boot / Readiness Audit

Task:

Trace how the application starts and how readiness is determined.

Report only:

* startup commands
* startup files/functions
* config loading
* required directories/files
* service binding/listening
* health/readiness checks
* warmup behavior
* failure behavior
* tests

Rules:

* Do not propose new startup behavior.
* Do not assume readiness unless code/docs prove it.
* Distinguish server started from system ready.

Required sections:

1. Startup commands
2. Startup entrypoints
3. Config loading
4. Required filesystem/data dependencies
5. Service/network binding
6. Health/readiness checks
7. Warmup behavior
8. Failure/error behavior
9. Tests
10. Open factual questions

