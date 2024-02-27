---
"orphan": true
"nosearch": true
---

# Authoring

The web site for the specification is generated using the [Sphinx](https://www.sphinx-doc.org/)
Python package along with the [MyST](https://myst-parser.readthedocs.io/) package for using
Markdown instead of reStructuredText (.rst) files for content.

This page will outline the dos and don't of writing Markdown to render correctly.

In general, stick with [common Markdown constructs](https://daringfireball.net/projects/markdown/syntax).
For example,

    Some **bold** text or _italic_ text

Becomes, "Some **bold** text or _italic_ text".

Use the usual `-` for lists, `##` for headings, etc.

## Metadata

There are a couple of areas where extensions to basic Markdown are required to add metadata
to content files. For example, to include a table of contents you would typically add a block
such as the below:

    ```{toctree}
    :numbered:
    1_Data_Types.md
    2_Callables.md
    3_etc...md
    ```
See the `index.md` files for examples.

You can also add metadata to specify other directives. For example, this file starts with the
below to stop Sphinx complaining that it is **NOT** in any table of contents, and to stop it
being indexed.

    ---
    "orphan": true
    "nosearch": true
    ---

See the [FrontMatter](https://myst-parser.readthedocs.io/en/latest/configuration.html#frontmatter-local-configuration)
docs for more information.

## Admonitions

To add a note, use a fenced block as shown below.

    ```{note}
    This is a note
    ```

This will render as:

```{note}
This is a note.
```

To flag open issues that need addressing, place them in an "attention" admonition,
which will make them easy to search for. For example:

    ```{attention}
    This section is deprecated and should be removed
    ```

Which will render as:

```{attention}
This section is deprecated and should be removed
```

See the [admonition docs](<https://myst-parser.readthedocs.io/en/latest/syntax/admonitions.html>) for further info.

## Code blocks

The Sphinx rendering engine uses Pygments for code blocks. Use the LLVM syntax
for LLVM IR constructs, e.g.

    ```LLVM
    %f = call %Callable* @__quantum__rt__callable_create(
      [4 x void (%Tuple*, %Tuple*, %Tuple*)*]* @someOp,
      [2 x void (%Tuple*, i32)*]* null,
      %Tuple* null)
    %g = call %__quantum__rt__callable_copy(%f)
    call %__quantum__rt__callable_make_adjoint(%g)
    ```

Will render as:

```LLVM
%f = call %Callable* @__quantum__rt__callable_create(
  [4 x void (%Tuple*, %Tuple*, %Tuple*)*]* @someOp,
  [2 x void (%Tuple*, i32)*]* null,
  %Tuple* null)
%g = call %__quantum__rt__callable_copy(%f)
call %__quantum__rt__callable_make_adjoint(%g)
```

The syntax parser can be quite picky and cause build warnings if the syntax is not correct.
In general, use ` ```text ` code blocks for anything that may cause syntax parsing issues.

## LaTeX

Sphinx and MyST support several ways of including LaTeX, which is rendered using
[MathJax](https://myst-parser.readthedocs.io/en/latest/syntax/optional.html#mathjax-and-math-parsing).
For compatibility with tools like VS Code markdown preview and Jupyter notebooks, LaTeX
enclosed in \$inline dollars\$ or \$\$ blocks are recommended.

For example, `$(a+b)^2$` will be rendered as $(a+b)^2$, and a block like:

    $$
    CNOT = \begin{bmatrix}
        1 & 0 & 0 & 0 \\
        0 & 1 & 0 & 0 \\
        0 & 0 & 0 & 1 \\
        0 & 0 & 1 & 0
    \end{bmatrix}
    $$

Will render as,

$$
CNOT = \begin{bmatrix}
    1 & 0 & 0 & 0 \\
    0 & 1 & 0 & 0 \\
    0 & 0 & 0 & 1 \\
    0 & 0 & 1 & 0
\end{bmatrix}
$$

More elaborate equations such as the below should also render nicely.

$$
\sigma = \sqrt{ \frac{1}{N} \sum_{i=1}^N (x_i -\mu)^2}
$$

See the [MathJax macros docs](https://docs.mathjax.org/en/latest/input/tex/macros/index.html)
and [MathJax samples](https://www.mathjax.org/#samples) for more info.
