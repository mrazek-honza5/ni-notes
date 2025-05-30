![[Pasted image 20250529181618.png]]
- JS kód může pouze přistoupit k HTTP zdrojům by default pouze na stejné doméně
- Systém rozlišuje i "bezpečnost" HTTP metody
- Omezení se **NEPROJEVÍ** u načítání zdrojů (GET metoda)
- Zabraňují útokům 
	- Například CSRF 

# CORS - Cross Origin Resource Sharing Protocol
- Mechanismus, kde server přidá hlavičky do odpovědi
- Prohlížeč přijme odpověď a poté se na bázi hlaviček rozhodne, zda obsah zablokuje či nikoliv
## Hlavičky
- Origin - identifikuje origin požadavku
- Access-Control-Allow-Origin - definuje, jaké originy mohou přistupovat ke zdroji
	- Lze využít i wildcard

## Preflight
![[Pasted image 20250529184939.png]]
- Pro unsafe metody (PUT, DELETE apod.) se provádí jeden předrequest
	- Pro POST se provádí pouze pokud nemá payload, nebo má custom hlavičky
	- Provedení rozhoduje browser
- Tento request má metodu `OPTIONS` a zjišťuje konfiguraci. Tím zjistí hlavičky CORS a buď provede unsafe request, nebo ne
	- Odpověď potom může zacachovat podle hlavičky `Access-Control-Max-Age`

# Další způsob řešení
- Historicky se používalo k řešení same origin policies také [[JSON-P]]