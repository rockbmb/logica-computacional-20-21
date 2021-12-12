---
id: CvwPWj4YY9WiJiEJqaOFm
title: Trabalho 3
desc: ''
updated: 1639250365794
created: 1638886485674
---

$
\newcommand{\START}{\textrm{START}}
\newcommand{\FREE}{\textrm{FREE}}
\newcommand{\BLOCKED}{\textrm{BLOCKED}}
\newcommand{\STOPPING}{\textrm{STOPPING}}
\newcommand{\STOP}{\textrm{STOP}}
\DeclareMathOperator{\𝗂𝗇𝗂𝗍}{𝗂𝗇𝗂𝗍}
\DeclareMathOperator{\𝖻𝗋𝖾𝖺𝗄}{𝖻𝗋𝖾𝖺𝗄}
\DeclareMathOperator{\unbreak}{unbreak}
\DeclareMathOperator{\𝖻𝗅𝗈𝖼𝗄}{𝖻𝗅𝗈𝖼𝗄}
\DeclareMathOperator{\𝗎𝗇𝖻𝗅𝗈𝖼𝗄}{𝗎𝗇𝖻𝗅𝗈𝖼𝗄}
\DeclareMathOperator{\𝗌𝗍𝗈𝗉}{𝗌𝗍𝗈𝗉}
\DeclareMathOperator{\trans}{trans}
\newcommand{\timed}[1]{\operatorname{timed}_{#1}}
$

## Problem description

- [Enunciado](https://paper.dropbox.com/doc/LC-2021-2022-Trabalhos-Praticos-NZEwyS6N5YQQTw1XsYimE)

### Variables

#### Constants

- $a$: constante de atrito
- $b$: atrito no contacto corpo/ar
- $P$: Peso
- $f = aP$: Força de atrito ao solo (constante)

#### Other

$F = c(V-v)$ - Força de travagem
$V$ - Velocidade do corpo em relação ao solo
$v$ - Velocidade linear das rodas em relação ao solo

### Transitions

#### Caracterização do Estado

**Estado** $𝑋 ≡ (𝑚,𝑡,𝑉,𝑣)$

#### Estado Inicial

Predicado $\operatorname{init}(𝑋) = (𝑚 = \textrm{START}) ∧ (𝑡 = 0) ∧ (𝑉 = 𝑣 = 𝑉₀)$

#### Transições

Predicado trans(𝑋,𝑋')

##### Untimed

As transições untimed estão associadas aos eventos 𝑒 ∈ {𝗂𝗇𝗂𝗍, 𝖻𝗋𝖾𝖺𝗄, unbreak, 𝖻𝗅𝗈𝖼𝗄 𝗎𝗇𝖻𝗅𝗈𝖼𝗄 𝗌𝗍𝗈𝗉}

//∧ (V=v)

$$
\begin{aligned}
&\operatorname{init}(𝑋,𝑋')     &≡& (𝑚 = \START)    &∧& (𝑚' = \FREE)     &∧& (𝑡' = 𝑡) &∧& (𝑉' = 𝑉) &∧& (𝑣' = 𝑣)\\
&\operatorname{break}(X,X')    &≡& (𝑚 = \FREE)     &∧& (𝑚' = \STOPPING) &∧& (𝑡' = 𝑡) &∧& (𝑉' = 𝑉) &∧& (𝑣' = 𝑣)\\
&\operatorname{unbreak}(X,X')  &≡& (𝑚 = \STOPPING) &∧& (𝑚' = \FREE)     &∧& (𝑡' = 𝑡) &∧& (𝑉' = 𝑉) &∧& (𝑣' = 𝑣)\\
&\operatorname{block}(X,X')    &≡& (𝑚 = \STOPPING) &∧& (𝑚' = \BLOCKED)  &∧& (𝑡' = 𝑡) &∧& (𝑉' = 𝑉) &∧& (𝑣' = 𝑣) \\
&\operatorname{unblock}(X,X')  &≡& (𝑚 = \BLOCKED)  &∧& (𝑚' = \FREE)     &∧& (𝑡' = 𝑡) &∧& (𝑉' = 𝑉) &∧& (𝑣' = 𝑣)\\
&\operatorname{stop}(X,X')     &≡& (𝑚 = \BLOCKED)  &∧& (𝑚' = \STOP)     &∧& (𝑡' = 𝑡) &∧& (𝑉' = 𝑉) &∧& (𝑣' = 𝑣)\\
\end{aligned}
$$

##### Timed

As transições timed estão associadas aos modos 𝑚 ∈ {FREE STOPPING BLOCKED}

Seja $X ≡ (m, t, V, v)$

$$
\begin{aligned}
\timed{\FREE}(X,X')
&\equiv
\begin{cases}
\dot{V} &= -c\cdot(V - v) - b)\\
\dot{v} &= -a \cdot P + c \cdot (V - v)
\end{cases}
%
\\[1.5em]
%
\timed{\STOPPING}(X,X')
&\equiv
\begin{cases}
\dot{V} = -c\cdot(V - v) - b)\\
\dot{v} = -a \cdot P + c \cdot (V - v)
\end{cases}
%
\\[1.5em]
%
\timed{\BLOCKED}(X,X')
&\equiv
\begin{cases}
V &= v\\
\dot{V} &= -a\cdot P - b\\
\end{cases}
\end{aligned}
$$

### Diagram

#### Convenções

$ΔT = t' - t$

https://plantuml-editor.kkeisuke.com/

```
@startuml
hide empty description

note as n1
Let
<latex>\Delta T = t' - t</latex>
end note

Start --> Free : init
Start:<latex>V=v=V_0</latex>
Start:<latex>t=0</latex>

Free --> Stopping : break
Free:<latex>\dot{V} = -c \cdot (V-v) - b</latex>
Free:<latex>\dot{v} = -a \cdot P + c \cdot (V-v)</latex>
Free:<latex>t < t' \le t + \tau</latex>

Stopping --> Free : unstop
Stopping --> Blocked : block
Stopping:<latex>\dot{V} = -(\uparrow\!\!\!c) \cdot (V-v) - b</latex>
Stopping:<latex>\dot{v} = -a \cdot P + (\uparrow\!\!\!c) \cdot (V-v)</latex>
Stopping:<latex>t' > t</latex>

Blocked --> Stopped : stop
Blocked --> Free : unblock
Blocked:<latex>V=v</latex>
Blocked:<latex>\dot{V} = -a \cdot P - b</latex>

Stopped:<latex>V=0</latex>
Stopped:<latex>V=0</latex>

@enduml
```

## Tools

### SMT-LIB

- [Home](https://smtlib.cs.uiowa.edu/)
- [Logics](https://smtlib.cs.uiowa.edu/logics.shtml)
- [Language/Documentation](https://smtlib.cs.uiowa.edu/language.shtml)
- [Examples](https://smtlib.cs.uiowa.edu/examples.shtml)
- [Utilities](https://smtlib.cs.uiowa.edu/utilities.shtml)

### PySMT

- [Home](https://github.com/pysmt/pysmt)
- [Examples](https://github.com/pysmt/pysmt/tree/master/examples)

### MathSMT

- [Home](https://mathsat.fbk.eu/)
- [Documentation](https://mathsat.fbk.eu/documentation.html)
  - [Model Checking](https://github.com/pysmt/pysmt/blob/master/examples/model_checking.py)

### Z3 Solver

Z3 is a theorem prover from Microsoft Research.

- [Home](https://github.com/Z3Prover/z3/wiki)
- [Programming Z3](https://theory.stanford.edu/~nikolaj/programmingz3.html)
- [Z3 API in Python examples](https://ericpony.github.io/z3py-tutorial/guide-examples.htm)
- [Python examples](https://github.com/Z3Prover/z3/tree/master/examples/python)
- [SAT/SMT by Example Book](https://sat-smt.codes/SAT_SMT_by_example.pdf)
- [Online Tutoril](https://jfmc.github.io/z3-play/)
- [API Documentation](https://z3prover.github.io/api/html/index.html)
