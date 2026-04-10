# Model Profile Resolution

Models are resolved programmatically by the init command. **Do NOT manually resolve models.**

## How It Works

The `init` command outputs model variables (e.g., `researcher_model`, `executor_model`) that are
already resolved from `model_overrides` in config.json, falling back to the profile table.

**Always use the model values from the init command output.** Do not look up the profile table
manually or substitute "inherit" for any tier.

## Lookup Table (reference only — init handles resolution)

@.github/get-shit-done/references/model-profiles.md

## Usage

1. Run the init command for the workflow (e.g., `gsd-tools.cjs init execute-phase`)
2. Parse the `*_model` fields from the JSON output
3. Pass those values directly in Task() calls — they are already fully resolved
4. Do NOT override, substitute, or reinterpret the model values
