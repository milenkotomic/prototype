# Logic demo

One self-contained HTML file that lets anyone drive a model by pressing
buttons. Use it when the question is about business rules, state transitions
or the shape of data: things that look fine on paper and only feel wrong once
real cases go through them.

Because it is one file with nothing to install, it can go to a designer, a
product person or a domain expert. So it speaks their language, not the
code's.

## It fits when

- "I am not sure this state machine handles X followed by Y."
- "Can this data model even represent the case where..."
- "I want to feel what the API should be before writing it."
- Anyone wants to press buttons and watch state change.

## Build it

### 1. The question, visible

One paragraph at the top of the page, in plain words: what model this is and
what the reader should find out by using it.

### 2. The logic, as a pure module

Put the logic in one `<script>` block, written as a small module that could
be lifted into the real codebase: no DOM, no `document`, no handlers reaching
into it. The page calls the module; nothing flows the other way. Pick the
form the question calls for, not the one that is easiest to wire to buttons:

- a **reducer** `(state, action) => state`, when actions are separate events
  over one state value;
- a **state machine** with named states and transitions, when "which actions
  are allowed right now" is part of the question;
- a few **pure functions** over plain data, when there is no current state,
  only transformations;
- a **class** with a clear method surface, only when the logic really owns
  state over time.

An action that is not allowed returns the state unchanged with a reason; the
page shows the reason. Seeing what is refused is half of the answer.

### 3. The page

Plain HTML, CSS and JavaScript in one file. No framework, no bundler, no
server, and nothing loaded from the network: no CDN scripts, no web fonts, no
remote images. It must open by double-click and still work when it is
attached to an email.

Every label uses the words of the domain, not of the code: buttons and fields
read like the business. Lay it out top to bottom:

1. **Title and the question** from step 1.
2. **Current state**: a panel of labelled fields, not a JSON dump, redrawn
   after every action, with what just changed marked.
3. **Free play**: one button per action, always there, so the model can be
   poked in any order. A refused action shows why.
4. **Guided scenarios**: one tab per scenario. Each has two or three lines on
   the situation and what to watch for, then the steps as real buttons in
   order; pressing one performs the action and moves to the next. Starting a
   scenario resets to a known state, so it runs the same every time.
5. **History**: the list of actions taken so far, so a surprising state can
   be retraced and reported.

Pick scenarios that show the awkward cases: the normal path, one hard edge
case, and one attempt at something that should be refused. Three to five are
enough.

Keep it clean and quiet: readable type, generous spacing, one accent colour,
readable in light and dark. No animation. Nothing competes with the state and
the buttons.

### 4. Check it, then hand it over

Before showing it, run each scenario's actions through the module (with
`node` on the script block, or by reading it step by step) and confirm the
end states are what the scenario text claims. A demo that contradicts its own
description answers nothing.

Open the file and give the path. The useful reactions are "that should not be
possible" and "I assumed that would be different": those are faults in the
idea, which is the point. New actions or scenarios the user asks for go into
the same file.

## Do not

- Add tests. A prototype that needs tests has stopped being one.
- Connect it to a real database or a real service.
- Generalise. It answers one question.
- Let the module touch the page. Once it does, it cannot be lifted out.
- Carry the page into production. Only the module is worth keeping.
