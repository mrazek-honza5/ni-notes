- Jazyk pro komplexní dotazování nad [[XML]] dokumentem
- Obsahuje [[XPath]] a tedy i veškeré 

# Výrazy
- [[XPath#Path expressions|Path expression]]
- FLWOR výrazy (for, let, where, order by, return)
- Podmínky (if, then, else)
- Kvantifikátory (some, every, satisfies...)

# Konstruktory
- Umožňují vytvářet nové nody, které v původním [[XML]] neexistují
## Direct konstruktor
- Dobře formovaný XML fragment 
- Obsahuje navíc XQuery výraz (**buď v hodnotě atributu, nebo obsahu elementu, nesmí být jinde**)
- Příklad:
```xquery
<movies>
	<count>{ count(//movie) }</count>
	{
		for $m in //movie
		return <movie year="{ data($m/@year) }">{ $m/title/text() }</movie>
	}
</movies>
```

## Computed constructor
- Umožňuje dynamické elementy a atributy
- Má jinou syntax
```xquery
element movie {
	element count { count(//movie) }
	for $m in //movie
	return 
		element movie {
			attribute year { data($m/@year) }
			text { $m/title/text() }
		}
}
```
# FLWOR konstrukty
- Zkratka pro for, let, where, order by, return
- Řeší usecases pro dotazování, joiny, seskupení a agregace, transformace, validace a další
- Příklad:
```xquery
for $m in //movie
let $r := $m/@rating
where $r >= 75
order by $m/@year
return $m/title/text()
```
