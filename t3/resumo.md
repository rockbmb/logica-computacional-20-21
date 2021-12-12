---
id: CvwPWj4YY9WiJiEJqaOFm
title: Trabalho 3
desc: ''
updated: 1639250365794
created: 1638886485674
export_on_save:
    html: True
---

$$
% Comandos auxiliares
\newcommand{\mode}[1]{\textrm{#1}}
\newcommand{\untimed}[1]{\operatorname{{untimed}_{#1}}}
\newcommand{\timed}[1]{\operatorname{{timed}_{#1}}}
% Modos
\newcommand{\START}{\mode{START}}
\newcommand{\FREE}{\mode{FREE}}
\newcommand{\BLOCKED}{\mode{BLOCKED}}
\newcommand{\STOPPING}{\mode{STOPPING}}
\newcommand{\STOPPED}{\mode{STOPPED}}
% Eventos
\newcommand{\INIT}{\mode{𝗂𝗇𝗂𝗍}}
\newcommand{\BREAK}{\mode{𝖻𝗋𝖾𝖺𝗄}}
\newcommand{\UNBREAK}{\mode{unbreak}}
\newcommand{\BLOCK}{\mode{𝖻𝗅𝗈𝖼𝗄}}
\newcommand{\UNBLOCK}{\mode{𝗎𝗇𝖻𝗅𝗈𝖼𝗄}}
\newcommand{\STOP}{\mode{𝗌𝗍𝗈𝗉}}
% Transições: Timed
\newcommand{\TSTART}{\timed{\small\START}}
\newcommand{\TFREE}{\timed{\small\FREE}}
\newcommand{\TBLOCKED}{\timed{\small\BLOCKED}}
\newcommand{\TSTOPPING}{\timed{\small\STOPPING}}
\newcommand{\TSTOPPED}{\timed{\small\STOPPED}}
% Transições: Untimed
\newcommand{\TINIT}{\untimed{\INIT}}
\newcommand{\TBREAK}{\untimed{\BREAK}}
\newcommand{\TUNBREAK}{\untimed{\UNBREAK}}
\newcommand{\TBLOCK}{\untimed{\BLOCK}}
\newcommand{\TUNBLOCK}{\untimed{\UNBLOCK}}
\newcommand{\TSTOP}{\untimed{\STOP}}
%
\DeclareMathOperator{\trans}{trans}
$$

## Problem description

- [Enunciado](https://paper.dropbox.com/doc/LC-2021-2022-Trabalhos-Praticos-NZEwyS6N5YQQTw1XsYimE)

### Variables

#### Constantes

$$
\begin{aligned}
&a      &&\textrm{constante de atrito}\\
&b      &&\textrm{atrito no contacto corpo/ar}\\
&P      &&\textrm{Peso}\\
&f = aP &&\textrm{Força de atrito ao solo (constante)}\\
\end{aligned}
$$

#### Outras

$$
\begin{aligned}
&v            &&\textrm{Velocidade linear das rodas em relação ao solo}\\
&V            &&\textrm{Velocidade do corpo em relação ao solo}\\
&F = c(V-v)   &&\textrm{Força de travagem}\\
\end{aligned}
$$

## Descrição do Sistema

Estado
: $X ≡ (m,t,V,v)$

Predicado $\INIT(X)$
: $\INIT(X) ≡ (m = \START) ∧ (𝑡 = 0) ∧ (𝑉 = 𝑣 = 𝑉₀)$

Predicado $\trans(X,X')$
: <!--  -->

### Transições

#### Untimed

As transições "**untimed**" estão associadas aos eventos $e ∈ \{\INIT, \BREAK, \UNBREAK, \BLOCK, \UNBLOCK, \STOP\}$

//∧ (V=v)

$$
\begin{aligned}
&\TINIT(X,X')     &≡& (m = \START)    &∧& (m' = \FREE)     &∧& (𝑡' = 𝑡) &∧& (𝑉' = 𝑉) &∧& (𝑣' = 𝑣)\\
&\TBREAK(X,X')    &≡& (m = \FREE)     &∧& (m' = \STOPPING) &∧& (𝑡' = 𝑡) &∧& (𝑉' = 𝑉) &∧& (𝑣' = 𝑣)\\
&\TUNBREAK(X,X')  &≡& (m = \STOPPING) &∧& (m' = \FREE)     &∧& (𝑡' = 𝑡) &∧& (𝑉' = 𝑉) &∧& (𝑣' = 𝑣)\\
&\TBLOCK(X,X')    &≡& (m = \STOPPING) &∧& (m' = \BLOCKED)  &∧& (𝑡' = 𝑡) &∧& (𝑉' = 𝑉) &∧& (𝑣' = 𝑣) \\
&\TUNBLOCK(X,X')  &≡& (m = \BLOCKED)  &∧& (m' = \FREE)     &∧& (𝑡' = 𝑡) &∧& (𝑉' = 𝑉) &∧& (𝑣' = 𝑣)\\
&\TSTOP(X,X')     &≡& (m = \BLOCKED)  &∧& (m' = \STOPPED)     &∧& (𝑡' = 𝑡) &∧& (𝑉' = 𝑉) &∧& (𝑣' = 𝑣)\\
\end{aligned}
$$

#### Timed

As transições "**timed**" estão associadas aos modos $m ∈ \{\FREE, \STOPPING, \BLOCKED\}$

Seja $X ≡ (m, t, V, v)$

$$
\begin{aligned}
\TFREE(X,X')
&\equiv
\begin{cases}
\dot{V} &= -c\cdot(V - v) - b)\\
\dot{v} &= -a \cdot P + c \cdot (V - v)
\end{cases}
%
\\[1.5em]
%
\TSTOPPING(X,X')
&\equiv
\begin{cases}
\dot{V} = -c\cdot(V - v) - b)\\
\dot{v} = -a \cdot P + c \cdot (V - v)
\end{cases}
%
\\[1.5em]
%
\TBLOCKED(X,X')
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

```txt
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
