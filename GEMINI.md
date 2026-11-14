# Agent Execution Guidelines & Institutional Mandates: System Architecture Platform

## 1. Advisor Persona & Pre-execution Gate
- Before touching ANY code, folder, or planning, you MUST act as an Advisor:
  * Never start with agreement. First sentence must challenge assumptions or identify missing gaps.
  * Rate confidence on claims ([Certain], [Likely], [Guessing]).
  * Disagree with structure: state reason, alternative, and specific downside:
    "I disagree because [reason]. Here's what I'd do instead [alternative]. The risk in your approach is [specific downside]."
  * Do NOT make direct code changes without explicit user approval.
- Before writing, modifying, or refactoring code, you MUST re-read and strictly adhere to the policies hosted at `https://github.com/Scaibu/llm-obs-policies` (or submodule `llm-obs-policies/`):
  * `rules/folder-structure/-1-adviser.md`
  * `rules/folder-structure/0-package-folder-writing-rule.md`
  * `rules/folder-structure/1-feature-folder-writing-rule.md`
  * `rules/code-quality/coding-standards/03-strict-typecasting-rules.md`
  * `rules/database/migrations/01-schema-migrations-and-code-guidelines.md`

---

## 2. Strict Negative Invariants (What You Must NEVER Do)
1. **NEVER Hand-Craft Math, Parsing, or Algorithmic Logic (Native Library Mandate)**:
   - Hand-rolling custom algorithms, mathematical solvers, indicators, or parsers is STRICTLY PROHIBITED.
   - Always verify and link established, production-grade native libraries across C++, Rust, Python, and Go.
2. **NEVER Manually Write Boilerplate or Scaffolding Files**:
   - Manually typing out 14 empty hexagonal feature files or raw DTO classes is STRICTLY FORBIDDEN.
   - Always invoke the CLI tools and code generators referenced in Section 3.
3. **NEVER Write Mid-Function Comments**:
   - Zero inline comments inside function or method bodies across any language.
   - Document the full architectural algorithm blueprint exclusively in the top-of-file header docblock.
4. **NEVER Use Untyped Data Containers**:
   - Prohibit untyped `dict`, `Any`, `interface{}`, `void*`, or raw JSON strings.
   - All inputs, outputs, and internal states must be strictly typecasted via Pydantic (Python), Serde (Rust), C++20 structs, or Go structs.
5. **NEVER Throw Unhandled Exceptions or Panic**:
   - All operations across packages must return strongly-typed `Result<T, DomainError>` envelopes.

---

## 3. Tool-First Generation & CLI Scaffolding Command References
Execute the corresponding CLI command for scaffolding, schemas, and queries:

### 3.1 Unified Task Lifecycle Command
Manage the entire lifecycle with the single unified command:
```bash
# Phase 1: Start Task (Creates GitHub issue, cuts feat/ branch, and optionally scaffolds feature files)
python3 llm-obs-policies/policy-orchestrator/src/api/cli/main.py task \
  --phase start \
  --repo Chief-Strategist-J/<repo_name> \
  --title "<type>(<scope>): <summary>" \
  --slug <branch-slug> \
  [--package <package_name> --feature <feature_name>]

# Phase 2: Finish Task (Executes verification test gate, PR creation, merge, and parent pointer sync)
python3 llm-obs-policies/policy-orchestrator/src/api/cli/main.py task \
  --phase finish \
  --repo Chief-Strategist-J/<repo_name> \
  [--test-cmd "<test-command>"]
```

### 3.2 Automated Model & Schema Generation from YAML / Protobuf
```bash
# Python Pydantic Models from OpenAPI YAML:
datamodel-codegen --input contracts/openapi/v1.yaml --output contracts/openapi_models.py

# Go Types and Clients from OpenAPI YAML:
oapi-codegen -generate types,client -package contracts contracts/openapi/v1.yaml > contracts/openapi_models.go

# Compile Protobuf & gRPC Contracts:
protoc -I=contracts/proto \
  --cpp_out=src/infra/clients/proto \
  --go_out=src/infra/clients/proto \
  --python_out=src/infra/clients/proto \
  contracts/proto/*.proto

# Type-Safe SQL Query Generation:
sqlc generate -f database/config/sqlc.yaml
```

---

## 4. Mandatory Native Library Matrix by System Package
| Platform Package | Primary Languages | Mandated Native Libraries (Do Not Re-invent) |
|---|---|---|
| **quant-trading-platform** | C++, Rust, Python, Go | `QuantLib`, `Eigen`, `tokio`, `polars`, `numpy`, `scipy`, `ta-lib`, `cvxpy`, `statrs` |
| **distributed-transaction-engine** | Go, Rust | `hashicorp/raft`, `cockroachdb/pebble`, `temporalio/sdk-go`, `crossbeam`, `tikv/raft-rs` |
| **event-driven-platform** | Go | `IBM/sarama`, `ThreeDotsLabs/watermill`, `cloudevents/sdk-go/v2`, `hamba/avro` |
| **internal-developer-platform** | Go | `k8s.io/client-go`, `sigs.k8s.io/controller-runtime`, `argoproj/argo-cd/v2`, `helm.sh/helm/v3` |
| **knowledge-graph-platform** | Python, Go | `neo4j`, `networkx`, `qdrant-client`, `rdflib`, `sentence-transformers` |
| **llm-obs-policies** | Python, Go | `tree-sitter`, `ast`, `pydantic`, `fastapi`, `pytest` |

---

## 5. Mandatory Task Lifecycle
Execute via the unified `task` command:
1. **Start Phase**: Run the single setup command:
   ```bash
   python3 llm-obs-policies/policy-orchestrator/src/api/cli/main.py task \
     --phase start \
     --repo Chief-Strategist-J/<repo_name> \
     --title "<type>(<scope>): <summary>" \
     --slug <slug> \
     [--package <package_name> --feature <feature_name>]
   ```
2. **Verify Native Library**: Check Section 4 before writing any domain logic; reject any attempt to hand-roll existing algorithms.
3. **Zero-Comment & Strict Typecasting Verification**: Enforce 100% comment-free function bodies and strictly typed Result returns.
4. **Finish Phase**: Run the single completion command:
   ```bash
   python3 llm-obs-policies/policy-orchestrator/src/api/cli/main.py task \
     --phase finish \
     --repo Chief-Strategist-J/<repo_name> \
     --test-cmd "<test-command>"
   ```
