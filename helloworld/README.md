# Hello World

The simplest possible XanoScript workspace — a single function that returns the string `"hello"`. Use this as a starting point to understand the structure of a XanoScript project and how to pull it into Xano.

## Structure

```
helloworld/
├── workspace/
│   └── helloworld.xs       # Workspace configuration
├── function/
│   └── hello.xs             # A function that returns "hello"
└── README.md
```

## Files

### `workspace/helloworld.xs`

Every XanoScript project starts with a **workspace block**. This defines the workspace name and its configuration preferences.

```xs
workspace helloworld {
  acceptance = {ai_terms: false}
  preferences = {
    internal_docs    : false
    track_performance: true
    sql_names        : false
    sql_columns      : true
  }
}
```

| Preference | Value | Description |
|------------|-------|-------------|
| `internal_docs` | `false` | Hide internal documentation endpoints |
| `track_performance` | `true` | Enable performance tracking on API requests |
| `sql_names` | `false` | Don't enforce SQL-safe names for tables |
| `sql_columns` | `true` | Enforce SQL-safe names for table columns |

### `function/hello.xs`

A **function** is a reusable block of logic. This one takes no inputs and returns the string `"hello"`.

```xs
function hello {
  input {
  }

  stack {
  }

  response = "hello"
}
```

| Block | Purpose |
|-------|---------|
| `input { }` | Defines the function's input parameters (none in this case) |
| `stack { }` | Contains the function's logic — variable assignments, conditionals, loops, API calls, etc. (empty here) |
| `response = "hello"` | The value returned when the function is called |

When called from another function or API endpoint, you would use:

```xs
function.run "hello" {
} as $result
// $result = "hello"
```

## Getting Started

### Pull into a new workspace

Use the [Xano CLI](https://github.com/xano-inc/cli) to pull this example into a fresh workspace:

```bash
# 1. Install the CLI (if you haven't already)
npm install -g @xano/cli

# 2. Authenticate
xano auth

# 3. Create a new workspace
xano workspace create helloworld

# 4. Pull from this repo
xano workspace git pull ./helloworld -r https://github.com/xano-inc/xanoscript-examples --path helloworld

# 5. Push to your workspace
xano workspace push ./helloworld -w <workspace_id>
```

### Pull into an existing workspace

To add this function to an existing workspace without overwriting other objects:

```bash
xano workspace git pull ./helloworld -r https://github.com/xano-inc/xanoscript-examples --path helloworld
xano workspace push ./helloworld -w <workspace_id> --partial
```

The `--partial` flag tells the push command that a workspace block is not required and existing objects should be kept.

## What to Try Next

Once pushed to your workspace, you can:

1. **Call the function** from the Xano dashboard or via the API
2. **Add an input** — try adding `text name` inside the `input { }` block and returning `"hello " + $name`
3. **Add logic to the stack** — use the `stack { }` block to perform operations before returning a response
4. **Create an API endpoint** — expose the function as an HTTP endpoint by creating an `api_group` and `query`

## XanoScript Concepts

This example demonstrates two of the core XanoScript constructs:

| Construct | File Pattern | Description |
|-----------|-------------|-------------|
| `workspace` | `workspace/{name}.xs` | Workspace-level configuration and preferences |
| `function` | `function/{name}.xs` | Reusable logic blocks with inputs, a stack, and a response |

XanoScript supports many more constructs including `table` (database schemas), `query` (API endpoints), `task` (scheduled jobs), `agent` (AI agents), `tool` (agent tools), and more. See the [XanoScript documentation](https://docs.xano.com) for the full reference.
