## Motiváció

[@taiebrecursive]

## A vizsgált idősor

A vizsgált idősor egy szintetikus, periodikus jel, amelyet a következő képlettel definiálunk:

$$
\begin{aligned}
x\left(t\right) &= \sin(a \cdot t + b) \cdot \sin(c \cdot t) + \rho\left(t\right) + \epsilon\left(t\right), \\
s.t. \:\: \rho\left(t\right) &= \overset{N}{\underset{i=1}{\sum}} \nu_i \sin(2\pi \cdot i \cdot f_\text{base} \cdot t + \phi_i), \\
\epsilon\left(t\right) &\sim \mathcal{N} \left(0, \sigma^2\right)
\end{aligned}
$$

A jel egy nemlineáris, periodikus determinisztikus rész, egy periodikus interferáló zaj, és egy fehér zaj összegéből áll. Az interferáló jelet úgy állítottuk össze, hogy egy adott $f_\text{base}$ alapfrekvenciát, valamint annak felharmonikusait tartalmazza. Ez jól közelíti a valós életben előforduló periodikus zajokat, például az elektromos hálózatokból származó zajokat. A fehér zaj pedig egy normális eloszlású véletlenszerű zaj, amely az érzékelő hibáját hivatott modellezni.

A periodikus jel a következő paraméterekkel definiálható:

- $a$: a periodikus jel egyik komponensének frekvenciája
- $b$: a periodikus jel komponensei közti fáziseltolás
- $c$: a periodikus jel másik komponensének frekvenciája

Az interferáló zaj a következő paraméterekkel definiálható:

- $N$: az interferáló zaj komponenseinek száma
- $\nu_i$: az interferáló zaj komponenseinek amplitúdója
- $f_\text{base}$: az interferáló zaj komponenseinek alapfrekvenciája
- $\phi_i$: az interferáló zaj komponenseinek fáziseltolása

A fehér zaj a következő paraméterekkel definiálható:

- $\sigma$: a fehér zaj szórása

## Megvalósítás

A kódot tartalmazó jegyzetfüzetet [itt](https://colab.research.google.com/drive/1vbjXEjMGOJvdmeCx64Wp0zAvEwHeoLsO#scrollTo=KY0a9gzobvrV) lehet elérni.

## Kísérletek

## Konklúzió

## Irodalomgyűjtemény

::: {#bibliography}
:::
