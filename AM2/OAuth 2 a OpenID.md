- Standardy, které vznikly z potřeby sdílet data služeb třetím stranám bez poskytnutí přístupových údajů těmto stranám

# OAuth2
- Specifikuje proces, jak může uživatel autorizovat přístup ke svým datům třetí straně
- Uživatel tak= může tento přístup kdykoliv odebrat
- Uděluje *access token* na bázi *authorizacion code* flow
- Podporuje
	- Klientské aplikace (*implicit grant* - dnes už nedoporučený, doporučuje se standardní flow)
	- Serverové aplikace
	- Nativní desktopové aplikace

## Terminologie
- **Klient** - aplikace třetí strany, přistupuje k zdrojům **resource ownera**
- **Resource owner (uživatel)** - osoba vlastnící zdroje uložené na **resource serveru**
- **Authorization and Token Endpoints** - endpointy vlastněné **autorizačním serverem** skrze které **resource owner** autorizuje požadavky
- **Resource server** - aplikace která obsahuje data vlastněná **resource ownerem**
- **Authorization code** - kód, který **klient** využije k vyžádání **access tokenu**
- **Access token** - kód využívaný **klientem** pro přístup ke zdrojům

## Způsoby provedení
### Implicit grant
![[Pasted image 20250529204904.png]]
- Původně používané pro klientské aplikace v prohlížeči, dnes se již [nedoporučuje](https://oauth.net/2/grant-types/implicit/)
- Nepoužívá **authorization code**, vše probíhá na straně prohlížeče
- Kroky
	1. Klient přesměruje uživatele na autorizační endpoint
		- Metoda `GET` nebo `POST`
		- Query string parametry, nebo `application/x-www-form-urlencoded`
			- `client_id` - ID klienta. Přiděluje jej autorizační server
			- `redirect_uri` - URI na kterou autorizační server přesměruje po dokončení autorizace
			- `scope` - identifikace zdrojů nebo služeb ke kterým chceme přístup
			- `response_type` - typ odpovědi (`token` nebo `code`)
			- `state` - stav mezi požadavkem a odpovědí
	2. **Resource owner** povolí nebo zamítne požadavek
	3. **Autorizační server** poskytne **access token** cílové služby skrze request na ní
		- Po povolení nebo zamítnutí autorizační server přesměruje na `redirect_uri`
		- Klient z URI fragmentu vytáhne **access token** a **expiraci** tokenu
		- Pokud uživatel zamítnul, tak místo toho dává do fragmentu **error**
	4. **Klient** přistoupí ke zdrojům pomocí **access tokenu**
		- Zdroje jsou definované pomocí `scope`
		- Token se může předávat buď
			- Query string parametrem
			- HTTP hlavičkou `Authorization`
	5. Po expiraci tokenu klient žádá o nový (znovu se musí přihlásit)
### Authorization code 
![[Pasted image 20250529205506.png]]
- Sofistikovanější a bezpečnější schéma
	- Access token je uchovaný na serveru a neviditelný v prohlížeči
- Měla by se využívat i pro klientské aplikace
- Kroky
	1. **Klient** přesměruje uživatele na autorizační endpoint
		- Stejné jako u předchozího flow, akorát se posílá `response_type = code`
	2. **Resource owner** povolí či zamítne požadavek
	3. **Autorizační server** poskytne uživateli **authorization code** a požádá **resource server** o **access token** 
	4. **Klient** požádá **autorizační server** o **access token** a **refresh token**
		- Typicky se jedná o `POST` požadavek s URL formulářem, který obsahuje
			- `code` - hodnota kódu, který je specifikován hodnotou `grant_type`
			- `client_id` - stejné jako u předchozího flow
			- `client_secret` - hodnota uložená pouze na serveru
			- `redirect_uri` - kam potom přesměrovat
			- `grant_type` - typ `code`, pro nás `grant_type = authorization_code`
		- Toto neprobíhá přes prohlížeč, ale přímo mezi službami
	5. **Klient** využívá **access token** a přistupuje ke zdrojům
	6. Po vypršení **access tokenu** si skrze **autorizační server** vyžádá nový pomocí **refresh tokenu**
		- Získání nového access tokenu probíhá podobně, jako vyžádání access tokenu pomocí **authorization code**
		- Místo `code` se odešle hodnota `refresh_token` a `grant_type = refresh_token`

# OpenID protokol a OpenID Connect (OIDC)
- Podnětem bylo, že jeden uživatel má mnoho účtů a vznikají problémy
	- Hodně hesel
	- Konzistence dat
- Cílem proto bylo vytvořit službu třetí strany (OpenID providera), u kterého bude mít uživatel pouze jeden účet a osobní data, která bude služba poskytovat 
- Přihlášení by poté bylo možné využít ve všech aplikacích podporujících toto přihlášení
- OIDC je nadstavbu nad [[#OAuth2]] pro autentizaci, která je navíc API friendly a využívá právě OAuth 2 k autentizaci uživatele - autorizovaný prostředek jsou data uživatele

## Proces OpenID protokolu
1. Uživatel zažádá aplikaci o přihlášení pomocí providera (dostane na výběr z podporovaných providerů)
2. Aplikace se připojí na providera
3. Provider odpoví s XRDS dokumentem
4. Aplikace posílá požadavek na přihlášení a to přesměruje uživatele na OpenID stránku s přihlášením
5. Uživatel se přihlásí
	![[Pasted image 20250529224829.png]]

## Rozdíly OIDC oproti OAuth
- Místo nesmyslného access tokenu je poskytnut [[JWT - Json Web Token]], který obsahuje ostatní data o uživateli. 

## Proces OIDC
![[Pasted image 20250529214526.png]]