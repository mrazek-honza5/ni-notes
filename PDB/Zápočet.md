# SQL Optimalizace
## Metriky
- $n_R$ - počet řádků relace $R$
- $b_R$ - blokovací faktor (počet řádků  v jednom bloku)
- $p_R$ - počet stránek
- $V(A,R)$ - variabilita $A$ v relaci $R$ (počet unikátních hodnot $A$)
- $M$ - velikost paměti (kolik stránek se tam vejde)
- $BLOCKSIZE$ - velikost bloku
- $ROWAVG$ - průměrná velikost řádky
- $b_{I(A,R)}$ - blokovací faktor listu indexového stromu (počet klíčů uložených v uzlu)
- $f_{I(A,R)}$ - faktor větvení (průměrný počet následníku vnitřního uzlu indexového stromu)
- $I(A,R)$ - hloubka indexového stromu

### Vztahy mezi metrikami
- $I(A,R) = \frac{\log{\# ne nullových hodnot v R[A]}}{\log{f_{I(A,R)}}}$ 

## Výpočet ceny ($c$)
### 1.  Výběr
```sql
SELECT * FROM R WHERE A = 'x';
```
#### Bez indexu (full table scan heap tablu)
- Pokud je $A$ unikátní: $$c = \frac{p_R}{2}$$
- Jinak: $$c = p_R$$ 

#### Unikátní index $R[A]$
- Cena závisí na typu úložiště
- Heap table + index:  $$c = I(A,R) + 1$$
- Index-only table: $$c = I(A,R)$$

#### Neunikátní index $R[A]$
- $$c = I(A,R) + n_{R(A='x')}$$

#### Složený index na $R[A,B]$
- $$c=I(R,(A,B)) ; \frac{n_{R(A='x')}}{b_{I(A,B)}} - 1$$

#### Složený index na $R[A,B]$, ale $A$ je unikátní
- $$c = I(R, (A,B))$$

### 2. Výběr
```sql
SELECT A FROM R WHERE A < 'x';
```
#### Výběr pouze indexu
$$c = I(A,R) + \frac{p_{I(A,R)}}{2}$$
#### Výběr jiných dat
- S indexem $R[A]$ $$c = I(A) + \frac{p_{I(A,R)}}{2} + \frac{n_R}{2}$$
- Bez indexu na $R[A]$: $$c = p_R$$

### 3. Nested loop join
- $M = 3$ 
