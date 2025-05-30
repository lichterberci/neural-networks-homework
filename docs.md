## Háttér és motiváció

A problémakör egy gyakorlati alkalmazásban merült fel, amiben egy nagyon zajos és komplex nemlineáris, azonban valamelyest periodikus jelet kellett előrejelezni egy viszonylag hosszú időtávra. Intuitíven úgy tűnt, hogy a háttérben álló differenciálegyenletre való visszavezetés lenne a legjobb megoldás, amely kiértékelése megadja a jövőbeli értékeket (ez a rekurzív előrejelzés). Ennek a megközelítésnek az a motivációja, hogy lényegében egy differenciálegyenletet próbál megtanulni a modell, amely generálja a jelet. Ilyen módon nem csak pontos illeszkedést lehet elérni bizonyos esetekben, hanem jól magyarázható modelleket is kapunk [@Egan2024]. Ez a következő formát öltené:

$$
\begin{aligned}
\dot{x}\left(t\right) \approx x\left(t + \Delta t\right) - x\left(t\right) =&\ f\left(x\left(t\right), x\left(t - \Delta t\right), \ldots, x\left(t - n \cdot \Delta t\right)\right) \\
&+ \epsilon\left(t + \Delta t\right) \\
\implies x\left(t + m \cdot \Delta t\right) =&\ x\left(t\right) \\
&+ \sum_{i=0}^{m-1} \biggl( f\left(x\left(t + i \cdot \Delta t\right), x\left(t + (i - 1) \cdot \Delta t\right), \ldots, x\left(t + (i - n) \cdot \Delta t\right)\right) \\
&\quad + \epsilon_\text{rekurzív}\left(t + m \cdot \Delta t\right) \biggr) \\
=&\ x\left(t\right) \\
&+ \sum_{i=0}^{m-1} f\bigl(x\left(t + i \cdot \Delta t\right), x\left(t + (i - 1) \cdot \Delta t\right), \ldots, x\left(t + (i - n) \cdot \Delta t\right)\bigr) \\
&+ \sum_{i=0}^{m-1} \epsilon_\text{rekurzív}\left(t + m \cdot \Delta t\right)
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

#TODO: kép a jelről

### Miért szintetikus jel?

A feladat során vizsgált idősor egy általunk konstruált, szintetikus jel, melyet azért választottunk, hogy tetszőlegesen hosszú jelet tudjunk generálni, hogy kedvünk szerint állíthassuk a zajokat, illetve a megfigyelési frekvenciát. Mindemellett talán a legfontosabb, hogy a kiértékelésnél így össze tudjuk hasonlítani a tiszta jellel, így a lehető legpontosabb képet kapva a modellek teljesítményéről. Ezzel szemben egy valós idősor bekorlátozna bennünket, hiszen nem tudnánk tetszőlegesen hosszú jelet generálni, és a zajok is adottak lennének. A szintetikus jel segítségével így nagy szabadságot ad, amellyel a különböző kísérletek során éltünk is.

## A tanuló modellek

Mindkét megközelítéshez (rekurzív és egylépéses előrejelzés) egyaránt egy 1d konvolúciós neurális hálózatot használtunk, amely a következőképpen van felépítve:

- **Bemenet**: a bemenet egy $n$ hosszúságú ablak, amely az előző $n$ időpont értékeit tartalmazza.
- **1D konvolúciós réteg**: a bemenetet egy 1d konvolúciós rétegen keresztül továbbítjuk, amely $k$ szűrőt használ, és a kimenet mérete $n - k + 1$ lesz.
- **Aktivációs függvény**: a konvolúciós réteg kimenetét egy $\tanh$ aktivációs függvényen keresztül továbbítjuk, amely normalizálja a kimenetet, és segít a nemlineáris kapcsolatok modellezésében.
- **Laposító réteg**: a konvolúciós réteg kimenetét egy laposító rétegen keresztül továbbítjuk, amely a kimenetet egy vektorba alakítja.
- **Teljesen összekötött réteg**: a $\tanh$ aktivációs függvény kimenetét egy teljesen összekötött rétegen keresztül továbbítjuk, amely a kimenetet a kívánt méretre alakítja.
- **Kimenet**: a kimenet egy $m$ hosszúságú vektor, amely az előrejelzett értékeket tartalmazza.

Az 1d konvolúciós réteg jól rá tud tanulni a periodikus és nemlineáris kapcsolatokra [@8682194] (trend nélküli idősorok esetén), és képes kezelni a zajos adatokat is, így jól alkalmazható a feladat megoldására. 

A megvalósítás az alábbi módon történik[^1]:

```python
class Conv1DNN(nn.Module):
    def __init__(self, input_size, hidden_size, output_size, kernel_size):
        super(Conv1DNN, self).__init__()
        self.conv1 = nn.Conv1d(1, hidden_size, kernel_size)
        self.tanh = nn.Tanh()
        self.fc = nn.Linear(hidden_size * (input_size - kernel_size + 1), output_size)

    def forward(self, x):
        x = x.unsqueeze(1)
        x = self.conv1(x)
        x = self.tanh(x)
        x = x.view(x.size(0), -1)
        x = self.fc(x)
        return x
```

[^1]: A kódot saját implementációként mutatjuk be, de a PyTorch dokumentációja és példái alapján készült. Lásd: [PyTorch Conv1d dokumentáció](https://pytorch.org/docs/stable/generated/torch.nn.Conv1d.html).

### Hiperparaméterek megválaszása

A kísérletek során korán világossá vált, hogy mindkét megközelítés esetén nagy mértékben függ a modell teljesítménye a hiperparaméterek beállításától. A hiperparaméterek közé tartozik a konvolúciós réteg szűrőinek száma, a tanulási ráta, az ablak mérete, a teljesen összekötött réteg mérete és a tanulási ciklusok (epoch-ok) száma.

Megválasztásuk először pár manuális próbálkozással kezdődött, amellyel felmértük, hogy mely paraméter milyen tartományban működik jól. Ezt követően egy rácskereséses módszert alkalmaztunk, amelyben a hiperparaméterek különböző kombinációit teszteltük, és a legjobb teljesítményt nyújtó kombinációt választottuk ki.

## Megvalósítás

A kódot tartalmazó jegyzetfüzetet [itt](https://colab.research.google.com/drive/1vbjXEjMGOJvdmeCx64Wp0zAvEwHeoLsO#scrollTo=KY0a9gzobvrV) lehet elérni. A megvalósítás során erősen építettünk a `pytorch` könyvtárra, amely lehetővé teszi a neurális hálózatok egyszerű és hatékony megvalósítását. A kiértékeléshez a `permetrics` könyvtárat használtuk, amely segít az idősorok előrejelzésének kiértékelésében. A kirajzoláshoz pedig a `matplotlib` és `seaborn` könyvtárakat használtuk, amelyek lehetővé teszik a vizualizációk egyszerű és szép elkészítését.

## Kísérletek

A kísérletek során minden esetben alkalmazva voltak a következő paraméterek:

| Paraméter         | Érték         |
|-------------------|--------------|
| $a$               | $0.12123123$ |
| $b$               | $2.132123$   |
| $c$               | $5$          |
| $\sigma$          | $0.1$        |
| $f_\text{base}$   | $5$          |
| $f_s$             | $2$          |

Utóbbi ($f_s$) a megfigyelési frekvencia, amely a jel mintavételezésének gyakoriságát határozza meg.

A generált jel 700 másodperc hosszú, amelyből az első 40%-ot tanulásra, 10%-ot validációra, és a maradék 50%-ot tesztelésre használtuk. Fontos megjegyezni, hogy mindenhol a zajos jelet használtuk, kivéve a tesztelésnél a kiértékeléshez; ezzel mimikálva a valós életben előforduló helyzeteket (ahol nem áll rendelkezésre a tiszta jel, azonban a modell helyességét a tiszta jelhez viszonyított hibája határozza meg).

#TODO: kép a felosztásról

### Az eredmények kiértékelése

Az egyes modellek tanítását (hiperparaéterek optimalizációjával) követően a teljes teszt adathalmazon kiértékeltük (minden olyan ablakon, amely a visszatekintési ablakot figyelembe véve belefér a tesztelési adathalmazba) a modellek teljesítményét. Minden modell minden előrejezési horizonton ki volt értékelve [három pontossági metrika](#a-pontossági-metrikák) szerint is. Így minden előrejelzési horizonthoz kapunk egy eloszlást. Minket ebből a kétdimenziós eloszlásból az utolsó időpillanathoz (maximális előrejelzési horizonthoz) tartozó eloszlások fontosak, illetve az előrejelzési horizont függvényében a metrikák átlaga és 95%-os konfidencia intervalluma.

### A pontossági metrikák

#### Determinisztikus együttható ($R^2$)

$$
R^2 = 1 - \frac{\sum_{i=1}^{n} (y_i - \hat{y}_i)^2}{\sum_{i=1}^{n} (y_i - \bar{y})^2}
$$

#### Átlagos Arkusztangens Abszolút Százalékos Hiba (MAAPE)

$$
\text{MAAPE} = \frac{1}{n} \sum_{i=1}^{n} \left| \frac{\arctan(y_i) - \arctan(\hat{y}_i)}{\arctan(y_i)} \right| \cdot 100
$$

#### Normalizált Gyökös Középérték Négyzetes Hiba (NRMSE)

$$
\text{NRMSE} = \sqrt{\frac{\frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{y}_i)^2}{\text{var}\left(y\right)}}
$$

### Alapeset (baseline)

**A vizsgált jel paraméterei:**

| Paraméter         | Érték         |
|-------------------|--------------|
| $N$               | $8$          |
| $\nu_i$           | $e^{-i}$     |
| $\phi_i$          | $0$          |

#### Eredmények

TODO: eredmények beszúrása

![fig:baseline_histograms](./figures/baseline_histograms.png){#fig:baseline_histograms}

TODO: szöveges kiértékelés (1-2 mondat)

### Másik esetek... (TODO)

TODO: másik 3 eset bemutatása

## Konklúzió

A kísérletek során sikerült bemutatni, hogy a rekurzív és egylépéses előrejelzés is képes jól teljesíteni a vizsgált idősor előrejelzésében, azonban legtöbb esetben az egylépéses előrejelzés volt a jobb teljesítményű (különösen a hosszú távú előrejelzésnél). A rekurzív előrejelzés esetén a hibák felhalmozódása miatt a hosszú távú előrejelzés pontossága csökkent, míg az egylépéses előrejelzés esetén a hibák nem halmozódtak fel, így a hosszú távú előrejelzés is pontosabb volt.

### Limitációk

A házi feladat során csupán egy architektúrát vizsgáltunk, egyetlen idősoron. Előfordulhat, hogy más architektúrák vagy idősorok esetén más eredmények születnének. Elképzelhetőek olyan modellek is, amelyek nem tisztán az egyik vagy más megközelítést használják, hanem egyesítik a rekurzív és egylépéses előrejelzést, így kihasználva mindkét megközelítés előnyeit [@taiebrecursive].

## Irodalomgyűjtemény

::: {#bibliography}
:::
