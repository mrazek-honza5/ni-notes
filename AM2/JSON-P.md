- Slouží k programovému odeslání requestu na stránku na jiném originu
- Jedná se o starší řešení oproti CORSu
- Server jej musí také podporovat

# Princip
- Specifikuji query-string parametr, který obsahuje jméno funkce, která načte data
- Server mi potom vrátí JavaScript, který se vloží do DOM a provede - tím se načtou data