# Overview

marimo notebooks are **reactive**: they automatically react to your code
changes and UI interactions and keep your notebook up-to-date, not unlike a
spreadsheet.

```{admonition} Creating marimo notebooks
:class: tip

Make sure to first read the [getting started](/getting_started/index.md) page,
which teaches you how to install marimo and create notebooks.
```

## Cells

A marimo notebook is made of small blocks of Python code called **cells**.
_When you run a cell, marimo automatically runs all cells that read any global
variables defined by that cell._ This is reactive execution.

> **Reactive execution guarantees that your code and program state are
> consistent.** It also gives notebooks a deterministic execution order,
> letting them double as both reproducible scripts and interactive apps.

<div align="center">
<figure>
<img src="/_static/reactive.gif" width="600px"/>
</figure>
</div>

```{admonition} Lazy evaluation
:class: tip

If you don't want cells to run automatically, the runtime can be
configured to be lazy, only running cells when you ask for them to be run and
marking affected cells as stale. Learn more in the
[runtime configuration guide](/guides/configuration/runtime_configuration.md)
```

**Execution order.**
The order of cells on the page has no bearing on the order cells are
executed in: execution order is determined by the variables
cells define and the variables they read.

You have full freedom over how to organize your code and tell your stories:
move helper functions and other "appendices" to the bottom of your notebook, or
put cells with important outputs at the top.

**No hidden state.**
marimo notebooks have no hidden state because the program state is
automatically synchronized with your code changes and UI interactions. And if
you delete a cell, marimo automatically deletes that cell's variables,
preventing painful bugs that arise in traditional notebooks.

**No magical syntax.**
There's no magical syntax or API required to opt-in to reactivity: cells are
Python and _only Python_. Behind-the-scenes, marimo statically analyzes each
cell's code just once, creating a directed acyclic graph based on the
global names each cell defines and reads. This is how data flows
in a marimo notebook.

```{admonition} Minimize variable mutation.
:class: warning

marimo's understanding of your code is based on variable definitions and
references; marimo does not track mutations to objects at runtime. For this
reason, if you need to mutate a variable (such as adding a new column to a
dataframe), you should perform the mutation in the same cell as the one that
defines it.

Learn more in our [reactivity guide](/guides/reactivity.md#reactivity-mutations).


For more on reactive execution, open the dataflow tutorial:

```bash
marimo tutorial dataflow
```

or read the [reactivity guide](/guides/reactivity.md).

## The marimo library

marimo is both a notebook and a library. The marimo library lets you use
markdown, interactive UI elements, layout elements, and more in your marimo
notebooks.

We recommend starting each marimo notebook with a cell containing a single
line of code,

```python3
import marimo as mo
```

## Outputs

marimo visualizes the last expression of each cell as its **output**. Outputs
can be any Python value, including markdown and interactive elements created
with the marimo library, _e.g._, `mo.md(...)`, `mo.ui.slider(...)`.
You can even interpolate Python values into markdown and other marimo elements
to build rich composite outputs.

<div align="center">
<figure>
<img src="/_static/outputs.gif" width="600px"/>
</figure>
</div>

> Thanks to reactive execution, running a cell refreshes all the relevant outputs in your notebook.

For more on outputs, try these tutorials:

```bash
marimo tutorial markdown
marimo tutorial plots
marimo tutorial layout
```

## Interactive elements

The marimo library comes with many interactive stateful elements in
`marimo.ui`, including simple ones like sliders, dropdowns, text fields, and file
upload areas, as well as composite ones like forms, arrays, and dictionaries
that can wrap other UI elements.

<div align="center">
<figure>
<img src="/_static/readme-ui.gif" width="600px"/>
</figure>
</div>

**Using UI elements.**
To use a UI element, create it with `marimo.ui` and **assign it to a global
variable.** When you interact with a UI element in your browser (_e.g._,
sliding a slider), _marimo sends the new value back to Python and reactively
runs all cells that use the element_, which you can access via its `value`
attribute.

> **This combination of interactivity and reactivity is very powerful**: use it
> to make your data tangible during exploration and to build all kinds of tools
> and apps.

_marimo can only synchronize UI elements that are assigned to
global variables._ You can use composite elements like `mo.ui.array` and
`mo.ui.dictionary` if the set of UI elements is not known until runtime.

For more on interactive elements, run the UI tutorial:

```bash
marimo tutorial ui
```

### Composite elements

marimo's composite UI elements let you wrap other UI
elements to create powerful UIs. For example,
`marimo.ui.form` lets you gate elements on submission, while
`marimo.ui.dictionary` and `marimo.ui.array` let you batch arbitrary
collections of elements.

<div align="center">
<figure>
<img src="/_static/readme-ui-form.gif" width="600px"/>
</figure>
</div>

## Layout

The marimo library also comes with elements for laying out outputs, including
[`mo.hstack`](#marimo.hstack), [`mo.vstack`](#marimo.vstack),
[`mo.accordion`](#marimo.accordion), [`mo.ui.tabs`](#marimo.ui.tabs), [`mo.sidebar`](#marimo.sidebar),
[`mo.nav_menu`](#marimo.nav_menu), [`mo.ui.table`](#marimo.ui.table),
and [many more](https://docs.marimo.io/api/layouts/index.html).

## SQL

marimo has built-in support for SQL: you can query Python dataframes,
databases, CSVs, Google Sheets, or anything else. After executing your query,
marimo returns the result to you as a dataframe, making it seamless
to go back and forth between SQL and Python.

<div align="center">
  <figure>
    <img src="/_static/docs-sql-df.png"/>
    <figcaption>Query a dataframe using SQL!</figcaption>
  </figure>
</div>

To create a SQL cell, click on the SQL button that appears at the bottom of the
cell array, or right click the create cell button next to a cell.

To learn more, run the SQL tutorial:

```bash
marimo tutorial sql
```

or read the [SQL guide](/guides/working_with_data/sql.md).
