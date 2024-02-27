---
"orphan": true
"nosearch": true
---

# Authoring

This page will outline the dos and don't of writing Markdown to render correctly.

In general, stick with common Markdown constructs. For example,

    Some **bold** text or _italic_ text

Becomes, "Some **bold** text or _italic_ text".

Use the usual `-` for lists and `##` for headings.

## Admonitions

To add a note, use a fenced block as shown below.

    ```{note}
    This is a note
    ```

This will render as:

```{note}
This is a note. Take note!
```

You may create a important blocks in a similar fashion, i.e.

    ```{important}
    Put things that need the readers attention here.
    ```

Which will render as:

```{important}
Put things that need the readers attention here.
```

See further types of blocks at
<https://myst-parser.readthedocs.io/en/latest/syntax/admonitions.html>

## Code blocks

The Sphinx rendering engine uses Pygments for code blocks. These can be quite
picky and cause build warnings if the syntax is not correct. In general, use
` ```text ` code blocks for anything that may cause issues.

## LaTeX

For compatibility with things like VS Code markdown preview and Jupyter
notebooks, LaTeX enclosed in \$inline dollars\$ or \$\$ blocks is recommended.

For example, `$(a+b)^2$` will become $(a+b)^2$, and a block like:

    $$
    \mbox{CNOT} =
    \begin{align}
        \left[\begin{matrix}
            1 & 0 & 0 & 0 \\
            0 & 1 & 0 & 0 \\
            0 & 0 & 0 & 1 \\
            0 & 0 & 1 & 0
         \end{matrix}\right]
    \end{align}
    $$

Will render as,

$$
\mbox{CNOT} =
\begin{align}
    \left[\begin{matrix}
        1 & 0 & 0 & 0 \\
        0 & 1 & 0 & 0 \\
        0 & 0 & 0 & 1 \\
        0 & 0 & 1 & 0
     \end{matrix}\right]
\end{align}
$$

More elaborate equations such as the below should also render nicely. See
<https://docs.mathjax.org/en/latest/input/tex/macros/index.html> for the
macros supported.

$$
\oint_S {E_n dA = \frac{1}{{\varepsilon _0 }}} Q_{inside}
$$
