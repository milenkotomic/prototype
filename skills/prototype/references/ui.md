# UI variants

Several structurally different versions of one page or component, switched
from a small bar at the bottom of the screen. The user flips between them in
the browser, picks one or takes parts of each, and the rest is removed.

## Where they live

**On the existing page, whenever there is one.** A design is far easier to
judge next to the real header, the real sidebar, real data and real density;
on an empty route every variant looks fine. The route stays, the data
fetching, parameters and auth stay, and only the rendered part swaps on
`?variant=`. Something new that would naturally sit inside an existing page
(a section, a card, a step in a flow) is mounted inside that page too.

**A new route only as a last resort**, when the thing has no page to live in.
Follow the project's routing convention, put `prototype` in the path, and use
the same `?variant=` switch.

## Build them

### 1. The question and the count

Three variants by default, five at most; beyond that they stop being
different. Write the plan as one comment at the top of the switcher file:
"Three variants of the settings page on `/settings`, switched with
`?variant=`."

### 2. Variants that disagree

Each variant gets the page's purpose, the data available, and the project's
own component library and styling. Name them `VariantA`, `VariantB`,
`VariantC`, each with a short label ("Sidebar layout").

They must differ in structure: layout, what is most prominent, what the main
action is. Three card grids with different colours are one design. When two
come out alike, redo one with an explicit ban on what they share.

Share little between them. A common header is fine; a common layout defeats
the purpose.

### 3. The switch

One switcher component on the route (adapt to the project's framework):

```tsx
const variant = searchParams.get('variant') ?? 'A';
return (
  <>
    {variant === 'A' && <VariantA {...data} />}
    {variant === 'B' && <VariantB {...data} />}
    {variant === 'C' && <VariantC {...data} />}
    <PrototypeSwitcher variants={['A', 'B', 'C']} current={variant} />
  </>
);
```

### 4. The bar

A small fixed bar at the bottom centre: previous, the current key and label
(`B (Sidebar layout)`), next. Both arrows wrap around.

- An arrow updates the URL parameter through the project's router, so a
  variant can be linked and survives a reload.
- The left and right arrow keys do the same, except while an input, a
  textarea or an editable element has focus.
- It looks unlike the page (a high-contrast pill), so nobody mistakes it for
  part of the design.
- It does not render in production builds: gate it on the project's own
  development check, so a stray merge cannot ship it.

### 5. Check, then hand over

Start the project's dev server if it is not running, load each variant once
and confirm it renders with real data and no console errors, when the session
has a browser tool; otherwise run the type check and the build and say the
page was not opened. Then give the URL with each `?variant=` key. The usual
answer is "the header from B with the list from C", which is the design.

## Do not

- Vary only colour or copy. That is a tweak.
- Wire a variant to a real write. Point mutations at a stub.
- Promote variant code as it is. It was written without tests or error
  handling; the winner gets rewritten.
- Leave variants or the bar behind. `SKILL.md` "Finish" says how they go.
