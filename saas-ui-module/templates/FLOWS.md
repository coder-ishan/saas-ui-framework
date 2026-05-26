# <Module name> — Flows

## Flow enumeration

| # | Flow name | Pattern | Entry points | Goal | Priority |
|---|---|---|---|---|---|
| 1 | | linear / branching / save-resume / single-action / continuous | | | P0 / P1 / P2 |

### Pareto: the 80/20

- **Flows getting 80% of polish:** <P0 list>
- **Everything else:** functional, not delightful, by design.

### Dropped candidates

- <flow> — dropped because <reason>

### Cross-module hand-offs

| Flow start | Hand-off point | Continues in | Hand-off contract |
|---|---|---|---|

---

## Flow specs

### Flow: <name>

- **Pattern:** ...
- **Goal:** ...
- **Priority:** ...
- **Composites used:** ...

#### Pre-conditions
- ...

#### Steps

| # | Step | What user does | What system does | Composite | State |
|---|---|---|---|---|---|
| 1 | Entry | | | | idle |
| 2 | | | | | |

#### Branches (if branching)

```text
<branch tree>
```

#### Save-and-resume contract (if save-resume)

- What's persisted: ...
- When: ...
- Resume entry point: ...
- Resume UI: ...

#### Decision affordances (if branching)
- ...

#### Keyboard path
1. ...
2. ...

#### Latency budget
- ...

#### Success exit
- Confirmation: ...
- Where: ...
- What next: ...

#### Abandon exit
- Preserved: ...
- Lands: ...
- Indication: ...

#### Error exits per step
- Step <N>: ...

---

### Flow: <next flow>

<repeat>
