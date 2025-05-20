* Knihovna pro paralelní výpočty
* Paradigma SPMD - Single Process Multiple Data
* Pracuje nad sdílenou pamětí s volnější konzistencí
    * Není zaručeno, že všechna vlákna vidí stejné hodnoty ve sdílených proměnných
    * Pokud to chceme zaručit, musí se provést *flush*
* Programovací model je typu **fork-join**
    * Začíná úvodní vlákno
    * Z kódu se **explicitně**, pomocí direktiv, vytváří vlákna
    * Po vykonání úlohy se vlákna uchovávají do thread poolu pro recyklaci
    * Vlákna sdílejí paměť, knihovny, přístup k I/O. Mají jen vlastní zásobníky

# Direktivy

## parallel
* Inicializuje paralelní oblast, ve které pracují vlákna současně
* Základní direktiva pro využití více vláken
* Uvnitř této direktivy jsou povolené další direktivy
* Počet vláken pro zpracování je popsán [[#Algoritmus přiřazení počtu vláken|zde]]
* Na konci bloku je implicitní bariéra, kde na sebe vlákna čekají
* Dále pokračuje opět hlavní vlákno a ostatní se uloží do thread poolu
* Oblasti se do sebe mohou zanořovat, ale zanoření je limitované, viz [[#Interní řídící proměnné (ICV)|Proměnné]]

## barrier
* Místo, kde vlákna čekají na dokončení všech ostatních

## master
* Kód, který provádí **pouze hlavní vlákno**
* Ostatní vlákna kód **přeskakují** a rovnou pokračují dále

## single
* Kód, který provádí **pouze jedno vlákno**
* Vlákon provádějící kód je libovolné, nemusí být hlavní

## critical
* Kód provádí současně **maximálně jedno vlákno**
* Další vlákna čekají na dokončení a poté mohou kritickou sekci uzmout pro sebe
* Kritické sekce mohou být pojmenované a nemusí být pospolu. I tak bude vyloučení fungovat
    ```cpp
    char[] a;

    #pragma omp critical updateA
    {
        // Code
    }

    #pragma omp critical updateA
    {
        // Code
    }
    ```

## atomic
* Elementární atomická operace **nad skalární hodnotou**
* Zaručeno na kterémkoliv HW
* Operace
    * `read`
    * `write`
    * `update`
    * `capture`
        * Rozšiřuje update o získání hodnoty proměnné, před nebo po provedení update části
            ```cpp
            int *ptr = (int*) malloc(....); 
            
            #pragma omp parallel shared(ptr) num_threads(3) 
            {
                int *my_ptr; 
                
                #pragma omp atomic capture
                { my_ptr = ptr; ptr += BLOCK_SIZE;} 
            }
            ```

## cancel
* Předčasné ukončení paralelního výpočtu
* Více možností, co ukončit:
    * parallel - ukončení celé paralelní sekce
    * for - ukončení foru
    * taskgroup - dojde ke zrušení všech těchto tasků
* Vlákna kontrolují ukončení na:
    * bariérách
    * na direktivě cancel
    * ve speciálních stornovacích bodech
        * `#pragma omp cancellation point {parallel | for | taskgroup}`

## flush
* Synchronizuje obsah sdílené proměnných mezi všemi vlákny

## taskwait
* Synchronizace dceřiných úloh s rodičovskou v **task** paralelismu

# Direktivy proměnných
* Až na výjimky mají jako parametr seznam proměnných
* Proměnné musí být předem alespoň deklarovány

## Shared
* Označuje sdílené proměnné, které vidí všechna vlákna (eventuálně) stejně
* Je na programátorovi, aby ohlídal přístup a zápis do paměti, aby nedošlo k časově závislým chybám (race conditions)

## Private 
* Označují proměnné, které jsou lokální pro každé vlákno
* Každé vlákno má novou, **neinicializovanou** instanci
* Po dokončení výpočtu nabývají proměnné stejných hodnot, které měly před paralelní oblastí

## Firstprivate
* Stejné jako private, ale proměnná je inicializovaná na hodnotu, kterou měla před paralelním blokem

## Default (shared | none)
* Výchozí nastavení pro všechny proměnné, které nebyly specifikovány v jiné direktivě

## Reduction(operator : seznam)
* Proměnné uvedené v seznamu se chovají, jako private, ale jsou inicializovány na **nulovou** hodnotu daného datového typu
* Po skončení výpočtu se hodnoty ze všech vláken spojí do výsledné proměnné pomocí specifikovaného operátoru
* Implementace redukce
    * Vybírá jí automaticky systém
    * Lineární
        * Vlákna ukládají svoje výsledky do sdíleného pole
        * Jedno vlákno poté vypočítá výsledek
        * Typicky využívaná varianta
    * Logaritmická
        * Má větší režii, vyžaduje bariéru v každém kroku

# Direktivy definující chování paralelismu

## For
* Představuje iterační datový paralelismus
* Jednotlivé iterace cyklu **musí být datově nezávislé** (prohození výpočtu iterací nesmí ovlivnit výsledek)
* Na konci je opět implicitní bariéra

### Klazule upravující chování
* `schedule` - určuje plánování práce, více níže
* `collapse(i)` - určuje, kolik úrovní vnořených cyklů zpracuje, více níže
* `ordered` - prikáže sekvenční provádění iterací
* `nowait` - odstraní implicitní bariéru

### Klauzule proměnných, platné jen pro for
* `lastprivate(seznam)` - stejné jako private, ale proměnné ze seznamu nabývají hodnoty ze **syntakticky poslední** smyčky

### Schedule
* Syntaxe `schedule(typ[, chunkSize])`
* Určuje, jak bude přidělována práce jednotlivým vláknům
* V následujících příkladech definujme
    * $n$ = počet iterací
    * $p$ = počet vláken
* Může výrazně zrychlit či zpomalit dobu běhu
* Nabývá následujících hodnot:
    * **static**
        * Přiděluje každému vláknu cyklicky, $chunkSize$ po sobě jdoucích iterací
        * Není-li hodnota udělena, vezme se výchozí $\frac{n}{p}$

    * **dynamic**
        * Vláknům jsou dynamicky přidělovány bloky velikosti $chunkSize$ po sobě jdoucích iterací
        * Není-li hodnota udělena, vezme se výchozí $chunkSize=1$

    * **guided**
        * Dynamicky přiřazuje bloky velikosti $x = max(\lceil \frac{t}{p}\rceil, chunkSize)$, kde $t$ = počet dosud nepřidělených iterací
    * **runtime** - rozhodnutí je odloženo až do spuštění, kde nabývá hodnoty podle proměnné `OMP_SCHEDULE`

    * **auto** - rozhodnutí je ponecháno na kompilátoru a OS

### Víceúrovňové cykly
* Pokud máme dva vnořené cykly, tak je by default paralelizovaná pouze první úroveň
* Toto lze ovlivnit klauzulí `collapse(depth)`, kde `depth` určuje, kolik cyklů bude zpracováno
* Collapse dělá to, že cykly vlastně rozvine do jednoho, a přepočítá indexy > je zde nějaká režie
* Alternativou je vložení dalšího paralelního regionu

### Možnosti paralelizace víceúrovňových cyklů

1. **Paralelizace pouze vnějšího cyklu**
    ```cpp
    #pragma omp parallel for
    for (int y = 0; y < YM; y++) 
        for (int x = 0; x < XM; x++) 
            computeElement(x,y);
    ```
    * Vnější cyklus provádí hlavní vlákno
    * Jedná se o nejčastější způsob
    * Každé vlákno dostane $\lfloor \frac{YM}{p} \rfloor * XM$ iterací
    * S thread poolem se manipuluje pouze jednou
2. **Dvouúrovňový cyklus se pomocí collapse transformuje na jednoúrovňový**
    ```cpp
    #pragma omp parallel for collapse(2) 
    for (int y = 0; y < YM; y++) 
        for (int x = 0; x < XM; x++) 
            computeElement(x,y);
    ```
    * Obsahuje režii potřebnou na zlinearizování vnitřního cyklu
    * Každé vlákno dostane buď $\lfloor \frac{YM*XM}{p} \rfloor$ nebo $\lceil \frac{YM*XM}{p} \rceil$ iterací
    * Pokud $p$ nedělí $YM$ a pokud je $XM$ velké, tak poskytuje rovnoměrnější rozdělení iterací mezi vlákna a je teoreticky rychlejší, než předešlý
3. **Paralelizace pouze vnitřních cyklů**
    ```cpp
    for (int y = 0; y < YM; y++) 

        #pragma omp parallel for 
        for (int x = 0; x < XM; x++) 
            computeElement(x,y);
    ```
    * Vnější cyklus provádí hlavní vlákno
    * Pokud neumí kompilátor optimalizace, může mít velkou režii kvůli opakovanému vytváření paralelního regionu
4. **Iterace vnitřního cyklu se vláknům přidělují pouze 1x, na konci těla bariéra.**
    ```cpp
    #pragma omp parallel 
    for (int y = 0; y < YM; y++) 
    
        #pragma omp for
        for (int x = 0; x < XM; x++)
            computeElement(x,y);
    ```
    * Může mít o něco menší režii, než příklad 3 a tím pádem může být rychlejší
    * Obsahuje pouze jedno vytvoření paralelní oblasti

* Způsoby 3 a 4 jsou jediné správné způsoby, jak paralelizovat smyčky, pokud máme vnitřní smyčku datově závislou na té vnější. Jako například u Gaussovy eliminace:
    1. **Paralelizace GEM 1**
        ```cpp
        int ge1(float **A, float *y, int n)
        {
            for (int i = 0; i < n; i++) {
                float d = A[i][j];

                #pragma omp parallel for
                for (int j = i + 1; j < n; j++) {
                    float p = A[j][i] / d;
                    for (int k = 0; k < n; k+++)
                        A[j][k] -= p * A[i][k];
                    y[j] -= p * y[i];
                }
            }
        }
        ```
        
    2. **Paralelizace GEM 2**
        ```cpp
        int ge2(float **A, *y, int n) 
        {
            #pragma omp parallel 
            for (int i = 0; i < n; i++) {
                float d = A[i][i]; 
                
                #pragma omp for 
                for (int j = i + 1; j < n; j++) {
                    float p = A[j][i] / d; 
                    for (int k = 0; k < n; k++) 
                        A[j][k]-= p*A[i][k]; 
                    y[j]-= p*y[i]; 
                }
            }
        }
        ```
        * Kvůli režii s paralelní oblastí by měl být trochu rychlejší, než 1. 

---

## Task
* Mechanismus pro funkční paralelismus
* Vhodný pro úlohy typu **rozděl a panuj**
* Mechanismus přidělování je klasický producent - konzument
    1. Vlákno narazí na direktivu `task`
    2. Vytvoří úlohu a vloží jí do zásobárny úloh
    3. Konzument vezme úlohu ze zásobárny 
    4. Zpracuje úlohu
    5. Během zpracování se může konzument stát producentem
* Je potřeba hlídat granularitu úloh, abychom nevytvářeli příliš malé úlohy (hodí se to do ifu, nebo do direktivy `if`)
    * Tento mechanismus funguje tak, že když vyjde podmínka v `if` na `false`, odloží se práce, vytvoří se úloha pro aktuální vlákno a totéž vlákno si jí vezme ihned ke zpracování
        ```cpp
        #pragma omp task if (i > THRESHOLD)
        DoWork();
        ```
    * Tzn. i použití `if` klauzule má tady nějakou režii.
    * Proto je lepší celé to obalit klasickým `if` takto: 
        ```cpp
        if (i > THRESHOLD) {
            #pragma omp task
            DoWork();
        } else {
            DoWork();
        }
        ```
* Hlavní vlákno (první producent), musí být proveden v režimu **single**, jinak je hned vytvořeno $p$ stejných úloh.

### Úloha
* Úloha je jednotka práce zpracovávaná konzumentem
* Obsahuje:
    * Ukazatel na začátek prováděného kódu
    * Vstupní data
    * Datovou strukturu, kam ukládá ID vlákna, zpracovávajícího tuto úlohu

### Klauzule
* `if(podmínka)` - pokud je `true`, kód je předán ke zpracování do task poolu. Pokud ne, tak je proveden tím stejným vláknem
* `final(výraz)` - pokud je výraz pravdivý, nebude úloha generovat další úlohy
* `priority(výraz)` - přiřadí dceřiné úloze prioritu

# OpenMP funkce
* `omp_get_num_procs()` - vrací počet CPU jader, která máme k dispozici
* `omp_get_thread_num()` - vrací index aktuálního vlákna (0 = hlavní)
* `omp_get_num_threads()` - vrací počet vláken v aktuální paralelní oblasti
* `omp_set_num_threads(#vláken)` - nastaví počet vláken. Musí se volat ze sekvenční části
* `omp_get_wtime()` - vrátí číslo udávající čas od okamžiku v minulosti. Používá se pro zjištění doby trvání cyklu

# Interní řídící proměnné (ICV)
* Definují konfiguraci vícevláknového výpočtu
* Tyto jsou důležité
    * `dyn-var` - určuje, zda se může měnit počet vláken v paralelní oblasti
    * `nthreads-var` - počet vláken, který se má v paralelní oblasti využít
    * `thread-limit-var` - maximální počet vláken, který se má v dané oblasti provádět
    * `max-active-levels-var` - maximální hloubka zanoření paralelních oblastí

# Algoritmus přiřazení počtu vláken
![thread count](attachments/image.png)


# Výkon a typické problémy v OpenMP

