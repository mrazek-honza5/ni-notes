- Pokrývá široké aspekty komunikace mezi klientem a serverem
- Zabezpečení přenosu, autentizace, autorizace, šifrování a podepisování zpráv...
# Zabezpečení přenosu
- SSL a TLS 
	- TLS je novější, ale termíny se používají zaměnitelně
	- Jedná se o protokol na 6. vrstvě ISO/OSI modelu
- Základem je TLS handshake - následuje po TCP handshaku
	1. ClientHello - verze TLS, list cifer, TLS nastavení
	2. ServerHello - verze TLS, vybraná cifra, certifikát
	3. RSA nebo Diffie-Hellman výměna klíčů
	4. Kontrola integrity zpráv, odeslání zašifrovaného textu "Finished"
	5. Odeslání dat
- Znázornění TLS handshaku: ![[Pasted image 20250529191752.png]]
## Módy TLS
- Využívané v kombinaci s Proxy servery
- Určují, jak se bude chovat přenos a zdali například Proxy do přenosu uvidí
### TLS Offloading
- Zabezpečené spojení je jen mezi proxy serverem a klientem
- Mezi proxy a serverem už spojení není zabezpečené
- Proxy vidí zprávy
### TLS Bridging
- Vytvoření dvou TLS spojení, jedno na pro každý směr (klient->proxy, proxy->server)
- Proxy opět vidí zprávy
### TLS Passthrough
- TLS je předáváno přes proxy, je navázáno přímo jako klient->server
- Proxy nevidí zprávu, může to být však problematické pro load balancery - je nutné použít SNI rozšíření TLS (Server Name Indication)

# Autentizace
- Ověření a identifikace uživatele 
- HTTP standard specifikuje možnosti HTTP autentizace
	- Basic Access Autentication
	- Digest Access Authentication
	- Bearer tokens (pro zdroje chráněné OAuth 2.0)
		- Například [[JWT - Json Web Token]]
	- Společná autentizace, kde server zná uživatelské heslo
- Dále může být autentizace proprietární, většinou formou `Cookies`