# Měřítka složitosti sekvenčních algoritmů

- $T^{K}_{A}(n)$ = časová složitost / doba výpočtu sekvenčního algoritmu $A$, který řeší problém $K$ na vstupních datech velikosti $n$
- $SL^K(n)$ = spodní mez sekvenční časové složitosti řešení problému $K$ (nejhorší časová složitost nejlepšího možného sekvenčního algoritmu pro $K$)
	- Triviální spodní mez je dána velikostí množiny vstupních dat $n$
	- Jedná se o vlastnost problému
- $SU^K(n)$ = horní mez časové složitosti pro řešení problému $K$ (nejhorší časová složitost nejrychlejšího existujícího sekvenčního algoritmu pro $K$)
	- S nejlepšími existujícími sekvenčními algoritmy porovnáváme naše paralelní řešení

# Optimální sekvenční algoritmy

* $A$ je asymptoticky optimální sekvenční algoritmus pro řešení $K$ právě když $$T_A(n) = \Theta(SU^K(n)) = \Theta(SL^K(n))$$
* Například řazení posloupnosti má již optimální algoritmus
* Naopak, například pro násobení matic, je známá triviální spodní mez $\Omega(n^2)$, ale zatím nemáme takový algoritmus, který to v tomto čase dokáže

# Paralelní časová složitost $T(n,p)$

* Paralelní časová složitost závisí nejen na $n$, ale i na počtu procesorů $p$
	* Pro jednoduchost máme zajištěno, že $p$  = \#procesorů = \#procesů = \#jader = \#vláken
* Je to definováno jako:
	>Čas, který uplynul od začátku výpočtu do okamžiku, kdy poslední procesor ukončil výpočet
* Měří se typicky sčítáním, jelikož hodně závisí na architektuře počítače
* Srovnává se se sekvenčním za stejných podmínek, pouze s tím, že sekvenční běží na jednom vlákně

# Paralelní cena $C(n, p)$ 

* Je definovaná jako $$C(n,p) = p \times T(n,p)$$
* Zcela určitě nemůže být paralelní cena pro paralelní algoritmus nižší, než sekvenční řešení $$C(n,p) = \Omega(SU(n))$$
* Můžeme ale dosáhnout cenově optimální řešení, pokud $$C(n,p) = O(SU(n))$$
# Paralelní zrychlení

* Je definováno jako $$S(n,p) = \frac{SU(n)}{T(n,p)}$$
* Paralelní zrychlení je lineární, právě když asymptoticky $$S(n,p) = \Omega(p)$$tedy pokud dokážeme držet zrychlení s libovolně narůstajícím počtem procesorů
	* Lineární zrychlení je svatý grál paralelního programování - pokud $p$ stoupne $k$ krát, chceme aby $T(n,p)$ $k$ krát klesl
	* Jedná se však o velmi obtížně splnitelný cíl

## Superlineární zrychlení

* Vyšší, než lineární zrychlení
* Za férových podmínek jej nelze dosáhnout
	* Technické důvody - distribuovaný systém má vyšší kapacitu paměti a nemusí docházet ke swapování
	* Anomálie při prohledávání stavového prostoru

# Spodní mez na paralelní čas $L^K(n,p)$

* Pro daný problém $K$ je spodní mez paralelního času pro $p$ vláken rovna $$L^K(n,p)=\frac{SL(n)}{p}$$

# Paralelní efektivnost $E(n,p)$

* Jedná se o vyjádření vytížení výpočetních zdrojů na samotný výpočet $$E(n,p)=\frac{SU(n)}{C(n,p)} \le 1$$
* Kvůli režiím bude vždy menší, než 100%
* Vyjadřuje vlastně zrychlení na jádro

# Amdahlův zákon

* Každý sekvenční algoritmus se proporčně skládá z
	* Inherentně sekvenčního podílu $f_s$, který může provést pouze jedno vlákno
	* Paralelizovatelného podílu $f_p=1-f_s$ 
* Když poté paralelizuju pomocí $p$ vláken, tak ideálně platí $$S(n,p)=\frac{T_A(n)}{f_s * T_A(n) + \frac{f_p}{p} * T_A(n)} = \frac{1}{f_s+\frac{f_p}{p}} \le \frac{1}{f_s}$$
* Zrychlení je tedy vždy saturováno velikostí proporce sekvenční a paralelizovatelné části

# Gustafsonův zákon

* [[#Amdahlův zákon]] říká, že problém fixní velikosti poskytuje omezené množství paralelismu 
* Gustafsonův říká, že když zvětšujeme $p$, máme úměrně navyšovat velikost problému $n$
* To vede na to, že $f_s$ trvá stále konstantní čas, ale $f_p$ bude v čase lineárně škálovat s $p$
* Potom $$S(n,p)=\frac{t_s+t_p(n,1)}{t_s+t_p(n,p)}$$

# Škálovatelnost

* Schopnost paralelního počítače se zvětšit, pokud narůstá problém
* Vyjadřuje, že větší problémy lze řešit v témže čase, pokud mám k dispozici dost procesorů

## Silná škálovatelnost
* Měří schopnost paralelního algoritmu dosáhnout pro fixní $n$ lineárního zrychlení s rostoucím $p$
* Jinak lze říci, že je to měřítko rychlosti poklesu efektivnost, pokud $p$ roste a $n$ se nemění

## Slabá škálovatelnost
* Definuje, jak se mění paralelní čas s $p$ pro fixní $n / p$ 
* Je to měřítko růstu $n$ takového, že při rostoucím $p$ zůstává efektivita stejná

# Izoefektivní funkce $\psi_1, \psi_2$ 

> Je-li dána konstanta $0 < E_0 < 1$, pak izoefektivní funkce
> 	- $\psi_1(p)$ je asymptoticky **minimální** funkce taková, že vrací jaké nejmenší musí být $n$, aby byla konstantní efektivnost $E_0$ 
> 	- $\psi_2(n)$ je asymptoticky **minimální** funkce taková, že vrací nejvyšší počet procesorů, který můžeme použít, abychom měli konstantní efektivnost $E_0$
 

