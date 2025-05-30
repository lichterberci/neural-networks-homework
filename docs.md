## Háttér és motiváció

A problémakör egy gyakorlati alkalmazásban merült fel, amiben egy nagyon zajos és komplex nemlineáris, azonban valamelyest periodikus jelet kellett előrejelezni egy viszonylag hosszú időtávra. Intuitíven úgy tűnt, hogy a háttérben álló differenciálegyenletre való visszavezetés lenne a legjobb megoldás, amely kiértékelése megadja a jövőbeli értékeket (ez a rekurzív előrejelzés). Ez a következő formát öltené:

$$
\begin{aligned}
x\left(t + \Delta t\right) - x\left(t\right) &= f\left(x\left(t\right), x\left(t - \Delta t\right), \ldots, x\left(t - n \cdot \Delta t\right)\right) + \epsilon\left(t + \Delta t\right), \\
\implies x\left(t + m \cdot \Delta t\right) &= x\left(t\right) + \sum_{i=0}^{m-1} \left( f\left(x\left(t + i \cdot \Delta t\right), x\left(t + (i - 1) \cdot \Delta t\right), \ldots, x\left(t + (i - n) \cdot \Delta t\right)\right) + \epsilon_\text{rekurzív}\left(t + m \cdot \Delta t\right) \right) \\
&= x\left(t\right) + \sum_{i=0}^{m-1} f\left(x\left(t + i \cdot \Delta t\right), x\left(t + (i - 1) \cdot \Delta t\right), \ldots, x\left(t + (i - n) \cdot \Delta t\right)\right) + \sum_{i=0}^{m-1} \epsilon_\text{rekurzív}\left(t + m \cdot \Delta t\right)
\end{aligned}
$$

Ahogy az jól látszik, hogy a $\epsilon_\text{rekurzív}\left(t + \Delta t\right)$ hibák akkumulálódnak, így minél távolabbra szeretnénk előrejelezni, annál nagyobb hibával kell számolnunk.

Egy másik megközelítés, amelyet a gyakorlatban gyakran használnak, az egylépéses előrejelzés, amely a következő formát ölti:

$$
\begin{aligned}
x\left(t + m \cdot \Delta t\right) &= f\left(x\left(t\right), x\left(t - \Delta t\right), \ldots, x\left(t - n \cdot \Delta t\right)\right) + \epsilon_\text{egylépéses}\left(t + m \cdot \Delta t\right)
\end{aligned}
$$

Ezen megközelítésnek az az előnye, hogy a hibája nem akkumulálódik, azonban egy nehezebb feladatot old meg, így a hibája általában nagyobb ($\epsilon_\text{egylépéses}\left(t + m \cdot \Delta t\right) > \epsilon_\text{rekurzív}\left(t + m \cdot \Delta t\right)$). Ennek ellenére képes pontosabb előrejelzést adni, mivel a rekurzív előrejelzés hibája felhalmozódik, míg az egylépéses előrejelzés hibája nem:

$$
\text{az egylépéses modell jobb} \iff \epsilon_\text{egylépéses}\left(t + m \cdot \Delta t\right) < \sum_{i=0}^{m-1} \epsilon_\text{rekurzív}\left(t + m \cdot \Delta t\right)
$$

## A vizsgált idősor

A vizsgált idősor egy szint etikus, periodikus jel, amelyet a következő képlettel definiálunk:

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
