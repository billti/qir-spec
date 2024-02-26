# Test page

This page exists to try out some rendering options and markdown syntax.

## This is another heading

Some **bold** text or _italic_ text works fine.

## Next level heading

Let's try out some LaTeX. Using inline math tags we get {math}`a^2 + b^2 = c^2`.

Inserting some LaTex in a math block results in:

```{math}
(a + b)^2 = a^2 + 2ab + b^2

(a + b)^2  &=  (a + b)(a + b) \\
           &=  a^2 + 2ab + b^2
```

How about a note

```{note}
This is a note. Take note!
```

There are also important tags.

```{important}
Put things that need the readers attention here.
```

## Using traditional LaTeX

For compatibility with things like VS Code markdown preview and Jupyter
notebooks, LaTeX enclosed in \$inline dollars\$ or \$\$ blocks is recommended.

Let's try some inline via $(a+b)^2$ and some math blocks with asmmath:

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

Verify something more fancy like one of Maxwell's equations renders nicely.

$$
\oint_S {E_n dA = \frac{1}{{\varepsilon _0 }}} Q_{inside}
$$
