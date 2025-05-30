- Standard pro autentizaci a autorizaci
- Slouží pro bezpečné přenášení informací mezi oběma stranami v JSON objektu
- Je digitálně podepsaný - podpis je důvěryhodný a ověřitelný
# Základní koncepty
- Malý
- Přenositelný skrze všechny možná média (URL, POST, HTTP hlavička...)
- Self-contained
	- Obsahuje všechny potřebné informace o uživateli
# Struktura
- JWT se skládá ze 3 částí oddělených tečkou
- Části jsou zakódované pomocí Base64-URL
- Části:
	- Hlavička
		- Algoritmus (HMAC, SHA256...)
		- Typ tokenu (JWT)
	- Payload
		- Tzv. *claims* - jedná se o data týkající se uživatele
			- Registrovaná podle [IANA JWT](https://www.iana.org/assignments/jwt/jwt.xhtml)
			- Privátní - custom claims
	- Signature
		- Podepsaná zakódovaná hlavička, payload a secret
# Využití
- Autentizace - uživatel nejprve získá token a poté jej připojí k dalším requestům. Server si token ověřuje
	![[Pasted image 20250529193220.png]]
	- Typicky se využívá v `Authorization` HTTP hlavičce, jako `Bearer` token
- Výměna informací - podpis JWT umožní verifikovat odesílatele a také ověřit integritu zprávy

# Expirace a odebrání tokenu
- Tokeny (access) by měly být platné po omezený čas, aby se zabránilo jejich zneužití
- Na toto slouží *claim* `exp`
	- Určuje timestamp, do kdy token platí
	- Je kontrolován při každém požadavku
- Pro obnovení neplatného tokenu může klient využít refresh token
	- Jedná se o speciální jednorázový (většinou) token, který má server uložený v DB
	- Po jeho přijetí se vygeneruje nový access token
	- Tento token má typicky delší dobu platnosti ale po využití je dobré jej vygenerovat znovu.