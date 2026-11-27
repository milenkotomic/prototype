# prototype

A Claude Code skill that builds a throwaway prototype to answer one design
question, then keeps the answer and discards the code.

- **Logic demo**: one self-contained HTML file. The logic is a pure module;
  the page around it shows the current state, a button per action, guided
  scenarios for the awkward cases and the history of what was pressed. It
  opens by double-click, loads nothing from the network, and can be handed
  to someone who does not code.
- **UI variants**: three structurally different versions of a page or
  component on the real route, with real data, switched with `?variant=` and
  a small bar that never renders in production.
- **An answer at the end**: the question, what the prototype showed, what is
  worth keeping and what gets removed.

Logic demos are stored outside the repo, under
`~/.claude/prototypes/<project>/`. UI variants live in the repo while the
question is open, with `prototype` in every name, and are removed afterwards.

## Install

```
claude plugin marketplace add milenkotomic/claude-plugins
claude plugin install prototype@milenkotomic
```

## Use

In Claude Code: "prototype this state machine", "what happens if a refund
arrives after the order is cancelled, let me try it", "show me three options
for the settings page", or in Spanish "prototipa esto", "muéstrame opciones
para esta pantalla". Or `/prototype:prototype [question | logic | ui]`.

## Layout

- `skills/prototype/SKILL.md`: picking the shape, the shared rules, the finish
- `skills/prototype/references/logic.md`: the HTML logic demo
- `skills/prototype/references/ui.md`: the UI variants and the switcher

## Credits

Adapted from the prototype skill in
[mattpocock/skills](https://github.com/mattpocock/skills) (MIT): the two
shapes, the pure module behind a throwaway page, and the variant switcher
come from there. This version stores logic demos outside the repo, loads
nothing from the network and ends with a written answer.

## License

MIT
