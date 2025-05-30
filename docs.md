# Rekurzív és egylépéses idősorelőrejelzők pontosságának vizsgálata

*Készült: 2025 tavasza*  
*Készítők: Szakos Máté, Lichter Bertalan*

## Motiváció

A gyakorlatban használt előrejelző modellektől az esetek többségében egy előre ismert előrejelzési horizonton szeretnénk tudni az idősorok jövőbeli értékeit. Ez elsősorban abból adódik, hogy a legtöbb alkalmazásban a döntések fix jövőbeli értékekre épülnek, például egy napra, hétre előrejelzett értékekre.
Ha megvizsgáljuk a legelterjedtebb idősorelőrejelző modelleket, azt láthatjuk, hogy a legtöbb esetben rekurzív előrejelzést alkalmaznak, azaz a jövőbeli értékeket a múltbeli értékekből, pontonként számítják ki. Ez sokszor jó illeszkedést tesz lehetővé, azonban megjelenhet az úgynevezett drift jelenség. Ennek lényege, hogy a modell kis hibával előrejelzi a jövőbeli értékeket, azonban a hibák összeadódnak, így a jövőbeli előrejelzések pontossága csökkenhet. Ezzel szemben az egylépéses előrejelzés esetén a modell minden egyes jövőbeli értéket külön számít ki, így elkerülhető a drift jelenség. Utóbbi megközelítés hátránya azonban, hogy a legtöbb esetben rosszabb illeszkedést eredményez, mivel a függvény, amelyet megközelít, lényegesen bonyolultabb, mint a rekurzív előrejelzés esetén.
Jelen házi feladat célja, hogy a két megközelítés pontosságát összehasonlítsa, és megvizsgálja, hogy melyik esetben érhető el jobb előrejelzési teljesítmény.

## A vizsgált idősor

A vizsgált idősor egy szintetikus, periodikus jel, amelyet a következő képlettel definiálunk:

$$
\begin{align}
x\left(t\right) &= \sin(a \cdot t + b) \cdot \sin(c \cdot t) + \rho\left(t\right) + \epsilon\left(t\right), \\
s.t. \:\: \rho\left(t\right) &= \overset{N}{\underset{i=1}{∑}} \nu_i \sin(2\pi \cdot i \cdot f_\text{base} \cdot t + \phi_i), \\
\epsilon\left(t\right) &\sim 𝓝\left(0, σ^2\right)
\end{align}
$$

A jel egy nemlineáris, periodikus determinisztikus rész (1), egy periodikus interferáló zaj (2), és egy fehér zaj (3) összegéből áll. Az interferáló jelet úgy állítottuk össze, hogy egy adott $f_\text{base}$ alapfrekvenciát, valamint annak felharmonikusait tartalmazza. Ez jól közelíti a valós életben előforduló periodikus zajokat, például az elektromos hálózatokból származó zajokat. A fehér zaj pedig egy normális eloszlású véletlenszerű zaj, amely az érzékelő hibáját hivatott modellezni.

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

- $σ$: a fehér zaj szórása

## Megvalósítás

A kódot tartalmazó jegyzetfüzetet [itt](https://colab.research.google.com/drive/1vbjXEjMGOJvdmeCx64Wp0zAvEwHeoLsO#scrollTo=KY0a9gzobvrV) lehet elérni.

## Kísérletek

## Konklúzió

## Irodalomgyűjtemény
