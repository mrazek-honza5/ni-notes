- Společný datový model pro [[XPath]] a [[XQuery]]
- Určuje:
	- XML se skládá z uzlů s různými typy
	- Document order
		- Pořadí uzlů je takové, jak se objevují v dokumentu (preorder DFS průchod)
	- Reverse document order
- Výsledek dotazu je reprezentován jako **sekvence**

## Sekvence
- Seřazená kolekce uzlů a / nebo atomických hodnot
- Prvky můžou mít různé typy
- Jsou automaticky flattened: `(2, (), (4, 1, (3)), (1)) => (2, 4, 1, 3, 1)`
- Mohou být prázdné
- Může obsahovat duplikáty
- 