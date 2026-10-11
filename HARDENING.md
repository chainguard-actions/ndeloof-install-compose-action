<!-- markdownlint-disable -->

# Hardening Report: ndeloof--install-compose-action/v0.0.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **ndeloof--install-compose-action/v0.0.1** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): The `install` step in action.yml directly interpolates `${{ inputs.version }}` three times inside a `run:` shell script. Because this is a composite action, `inputs.version` is fully caller-controlled. GitHub Actions substitutes the expression value into the shell script text before the shell parses it, so a malicious caller can supply a value containing shell metacharacters (`;`, `$(...)`, backticks, `|`, etc.) to execute arbitrary commands on the runner.

Offending lines:
  1. `DOWNLOAD_URL="https://github.com/docker/compose/releases/download/${{ inputs.version }}/docker-compose-${flavor}"`
  2. `if [[ "${{ inputs.version }}" == "latest" ]]; then`
  3. `echo "Download Compose ${{ inputs.version }} for ${flavor} from ${DOWNLOAD_URL}"`

Fix: move the input into an `env:` variable and reference it as a double-quoted shell variable:
```yaml
env:
  COMPOSE_VERSION: ${{ inputs.version }}
run: |
  DOWNLOAD_URL="https://github.com/docker/compose/releases/download/${COMPOSE_VERSION}/docker-compose-${flavor}"
  if [[ "$COMPOSE_VERSION" == "latest" ]]; then
    ...
  fi
  echo "Download Compose $COMPOSE_VERSION for ${flavor} from ${DOWNLOAD_URL}"
```

Locations:

- `action.yml:22`
- `action.yml:23`
- `action.yml:27`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection

**Notes:**

Fixed script injection in action.yml by adding an `env:` block to the `install` step with `COMPOSE_VERSION: ${{ inputs.version }}`, and replacing all three inline `${{ inputs.version }}` expressions in the `run:` script with the shell variable `$COMPOSE_VERSION`/`${COMPOSE_VERSION}`. This ensures the caller-controlled input is treated as data by the shell rather than being interpolated into the script text before parsing.

