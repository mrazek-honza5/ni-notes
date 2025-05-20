- Základ je předpoklad, že uzly mají soukromou paměť a potřebují komunikovat
- Připojení je přes přepínač / směrovač, který má příslušný počet vstupních a výstupních portů

# Přímé propojovací sítě

- Uvažujeme pouze jednoduché souvislé hrany bez smyček, zavádí se následující pojmy

### Topologie $G_n$
- Množina grafů (instance topologie), jejichž velikost a struktura je definovaná parametrem $n$ (velikost dimenze).
- Může nastat, že instance menších dimenzí jsou podgrafy instancí větších dimenzí (hierarchicky rekurzivní topologie)
- Když se dají postupně budovat větší a větší topologie, tak můžeme rozdělit na
	- Inkrementálně škálovatelnou topologii, která je definovaná pro $\forall N$
	- Částečně škálovatelnou - definována pro nekonečně mnoho $N$
- Topologie mohou být vícedimenzionální
- Z hlediska stupně uzlů dále omezujeme na
	- **Řídké topologie**: $|E(G_n)| = O(|V(G_n)|)$ - stupně uzlů omezeny konstantou
	- **Husté topologie**: $|E(G_n)| = \omega(|V(G_n)|)$ - stupně uzlů rostou s funkcí $n$

## Kartézsky součin grafů $G_1 \times G_2$
- Graf $G_1$ nakopírujeme místo všech uzlů $G_2$ a stejnolehlé uzly mezi kopiemi propojíme hranami podle $E(G_2)$
- Jedná se o komutativní a asociativní operaci a zachovává uzlovou symetrii (pokud porovnáváme grafy izomorfismem)

## Uzlově symetrický graf
- Pro libovolné dva vrcholy existuje automorfismus, že namapuje jeden vrchol na ten druhý (z obou vrcholů můžeme vidět stejný graf)
- Každý uzlově symetrický graf je regulární

# Grafy
## Vzdálenosti v grafu ($N$ = počet uzlů grafu $G$)
- $P(u,v)$ = délka cesty = počet hran mezi $u$ a $v$
- $dist_g(u,v)$ = délka nejkratší cesty v $G$
- $\overline{dist}(g)=\frac{1}{N*(N-1)} * \sum_{u,v, v \ne v}dist_G(u,v)$  - průměr všech nejkratších cest
- $exc(u) = max_{v \in V(G)}\;dist_G(u,v)$ - nejdelší nejkratší cesty
- $diam(G)=max_{u,v}\;dist_g(u,v)=max_u\;exc(u)$
- $r(G)=min_u\;exc(u)$
- Uzlově disjunktní cesty - cesty z $u$ do $v$ a z $x$ do $y$ spolu nesdílejí žádný vrchol, s výjimkou začátku a konce
- Hranově disjunktní cesty - cesty z $u$ do $v$ a $x$ do $y$ spolu nesdílejí žádnou hranu
## Souvislost grafů
- Uzlový řez - množina uzlů, jejichž odebráním se rozpojí graf
	- Identicky také hranový řez
- Uzlová souvislost - značí se $\kappa(G)$ a je to velikost minimálního uzlového řezu
	- Identicky pro hranovou souvislost (značí se $\lambda(G)$)
	- Uzlová je rovna nejvýše té hranové, a hranová je omezená shora minimálním stupněm v grafu ($\delta(G)$)
- Pokud jsou čísla stejná, graf má optimální souvislost ($\kappa = \lambda = \delta$)
- Pojmem k-souvislost se myslí, že $\kappa$ nebo $\lambda = k$ 

## Bisekční šířka a bipartitnost grafů
- Hranová bisekční šířka grafu $G$, $bw_e(G)$ je velikost nejmenšího hranového řezu, kterým se $G$ rozpojí na 2 poloviny
- Bipartitní graf - graf co se dá obarvit dvěma barvami. Pokud jsou množiny uzlů o obou barvách stejně velké, jedná se o vyvážený bipartitní graf

## Hamiltonovy sračky
- Hamiltonovská cesta - cesta v grafu procházející každým vrcholem právě jednou
- Hamiltonovská kružnice - hamiltonovská cesta, která končí v počátku
- Hamiltonovský graf - graf obsahující Hamiltonovskou cestu


# Požadavky na propojovací sítě
- Dva primární požadavky jsou protichůdné
	- Konstantní stupeň uzlu
		- Nižší cena
	- Malý průměr a malá průměrná vzdálenost
		- Snižuje komunikační zpoždění
- Spodní mez průměru $N$-uzlové řídké sítě je $\Omega(logN)$ - když tohoto síť dosahuje, tak je optimální
- Další požadavky
	- Uzlová symetrie a hierarchická rekurzivita
		- Usnadňuje návrh komunikačních algoritmů
	- Vysoká souvislost
		- Robustnost
		- Větší propustnost
	- Bisekční šířka
		- Kontroverzní požadavek
		- Pro paralelní rozděl a panuj algoritmy chceme velkou (velká propustnost)
		- Při stavbě superpočítače ale vysoká hodnota znamená mnoho externích spojů mezi stavebními bloky
		- Horní mez je $N/2$
	- Vnořitelnost jiných a do jiných topologií
	- Podpora pro směrování a kolektivní komunikační operace
		- Příští přednáška

# Základní přímé propojovací sítě
# Ortogonální topologie
- Definovány pomocí kartézského součinu

### Binární hyperkrychle
- Souřadnicí a dimenzí je $n$ => binární vektory délky $n$
- Propojuji ty vrcholy, které se liší pouze v jednom bitu
- Vrcholů je $2^n$
- Hran je $n*2^{n-1}$ 
- Průměr je $n$ (neboť nejhůře musím invertovat $n$ bitů)
- Bisekční šířka ($bw_e$) je $2^{n-1}$, každý uzel má souseda vedle
	- Vlastně když bych měl krychli 3 a přidal jednu hodnotu na začátek, tak sousedi v druhé polovině jsou podle tohoto ti, co mají nejvíce signifikantní bit opačný a liší se právě o tenhle bit
	- Takto se dá jet dále, vždy krychle $n$ je tvořena 2 krychlemi $n-1$, které jsou spojeny na vrcholech, které se liší o jedna. Takto to jde až do $Q_1$, což jsou dva vrcholy propojené hranou
- ![[Pasted image 20250519210513.png]]
#### Vlastnosti
- Regulární graf s logaritmickým stupněm uzlu => není řídká
- Je optimální z hlediska souvislosti ($\lambda(Q_n)=\kappa(Q_n)=n$)
- Je hierarchicky rekurzivní
	- Podkrychle = boolovský výraz o délce $n$ (s možností neutrálního symbol \*)
- Bisekční šířka je nejvěší možná ($N/2$) => vhodné pro rozděl a panuj algoritmy
- Jedná se o vyvážený bipartitní graf (stačí obarvovat dle parity)
- Velmi symetrický graf (existuje $2^n \times n!$ různých automorfismů)
	- Libovolně kombinujeme dvě symetrie
		- Jedna je přes permutaci dimenzí (vždy se liší v jednom bitu) - prohazuji souřadnice ve vektoru
		- Dále můžeme přičíst k 2 uzlům (binárnímu vektoru) XORem konstantní vektor ($x$ XOR ($u$ XOR $v$)) - tohle uděláme pro všechny uzly v grafu (přeložení)
- Počet sousedů ve vzdálenosti $i$ je $n \choose i$, průměrná vzdálenost je $\lceil n/2 \rceil$  
- Počet nejkratších cest délky $k$ je $k!$ 

#### Základní směrování
- $e$-cube - bity v $n$-bitových adresách se testují zprava doleva
#### Dnešní význam
- Používá se pouze pro omezené velikosti
- Využívá se při vymýšlení paralelních algoritmů (obdobně jako PRAM)

### n-rozměrná mřížka
- Kartézský součin 1-rozměrných mřížek
	- Není regulární (různé stupně vrcholů) => není uzlově symetrická
- Sousedi jsou takové vrcholy, které se v jedné souřadnici liší o jedna
- Uzel je identifikován jako $n$ rozměrný souřadnicový vektor
	- Uzel má maximálně $2n$ sousedů
- Mřížka má velikost $k_1 \times k_2 \times \dots \times k_n$ kde $k_i$ značí počet uzlů rozměru $i$
- ![[Pasted image 20250519215350.png]]
#### Vlastnosti
- Jednoduché
- Hierarchicky rekurzivní
- **Manhattanská vzdálenost**
	- Délka cesty při pohybu po mřížce
	- Např. pro 2- rozměrnou se pohybuji jen nahoru dolů, či doleva doprava
		- Mějme $A = (0,0),\;B=(1,3)$
		- Manhattanská vzdálenost $dist(A,B) = |0-1|+|0-3|=4$
- Optimální souvislost
- Bipartitní graf (součet souřadnic modulo 2 = skupina vrcholu)


#### Základní směrovací algoritmus
- Dimenzně uspořádané směrování

#### Topologicky optimální algoritmy
- Existuje jich pro mnoho 
#### Metriky 
![[Pasted image 20250519215708.png]]

## n-rozměrný toroid
- "Zabalená mřížka"
	- Kartézský součin jednorozměrných kružnic
- Uděláme to, že z každé lineární mřížky (každá úcečka v mřížce) přidáme hranu mezi koncovými vrcholy
	- Na obrázku teda např mezi (0,0,0) a (0,0,3)
	- ![[Pasted image 20250519220720.png]]

### Vlastnosti
- Uzlově symetrická
- Využívá také Manhattanskou vzdálenost, ale musíme vzít tu kratší cestu z okrajů (zhlednit přidanou hranu)
- Bipartitní, pokud máme všechny délky stran sudé (pak je i vyvážený)
	- Také součet souřadnic modulo 2, jako u mřížky
- Je méně hierarchicky rekurzivní (nelze rozložit na podtoroidy)
### Metriky
![[Pasted image 20250519220834.png]]
### Využítí dnes
- Velmi úspěšná pro dnešní masivně paralelní počítače

## Porovnání ortogonálních sítí
![[Pasted image 20250519221504.png]]
# Hyperkubické topologie
## Řídké hyperkubické
- Odvozené z hyperkrychle rozvinutím každého uzlu do více uzlů, tím vzniká **motýlek**
- Máme celkem dva druhy motýlků (reálně více, ale probíráme 2), co mají společné vlastnosti
	- Stupeň $O(1)$, průměr $O(logN)$
	- Škálují hůře, než hyperkrychle
	- Bisekční šířka $\Omega(N/logN)$
	- Přirozené pro řadu paralelních algoritmů (rychlá Fourierova transformace)
### Zabalený motýlek dimenze $n$, $wBF_n$
- Každý vrchol 3 rozměrné krychle nahradíme kružnicí velikosti 3 (zobecněno i na $n$)
	- Na obrázku - žluté shluky jsou uzly původní hyperkrychle - zde nahrazeny kružnicí o $n = 3$ vrcholech
	- Tím, si poté každý uzel vezme $n$ hyperkubických dimenzí a díky tomu mám konstantní počet sousedů
		- Pro kružnici vždy potřebujeme minimálně 2
		- A 2 sousedy v hyperkubické části (ty kružnice, kde se nám uzly původní hyperkrychle liší právě o jeden bit)
	- ![[Pasted image 20250519222831.png]]

#### Vlastnosti
- Uzlově symetrický
- Jedná se o řídký graf s optimálním průměrem
- Není hierarchicky rekurzivní
- Je vyvážený bipartitní graf <=> $n$ je sudé
- Jedná se o Hamiltonovský graf

#### Metriky
![[Pasted image 20250519223139.png]]

### Obyčejný motýlek dimenze $n$, $BF_n$

## Closovy topologie

## Dragonfly topologie