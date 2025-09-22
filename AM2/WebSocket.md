- Specifikace pro streamování dat mezi serverem a klientem (obousměrnou komunikaci)
- Musí být podporován jak klientem tak serverem
- Je stavěn nad TCP 
- Může být zabezpečený i nezabezpečený, a má podle toho vlastní URL schéma (`ws` a `wss`)
- Navázání spojení může být skrze
	- `TLS` a `ALPN` (Application-Layer Protocol Negotiation) hlavičku
		- Neboli informace o protokolu v rámci ClientHello v TLS handshaku
	- `HTTP/1.1` connection upgrade na websocket
		- Speciální `HTTP` požadavek s následujícími hlavičkami:
			- `Connection: Upgrade`
			- `Upgrade: websocket`
			- `Sec-WebSocket-Key` - client key pro pozdější validaci
			- `Sec-WebSocket-Origin` - origin requestu
			- `Sec-WebSocket-Verion` - verze protokolu
			- `Sec-WebSocket-Protocol` - seznam podprotokolů, které klient podporuje (proprietární, nepovinné)
		- Na to server odpovídá 
			- Status: `101 Switching Protocols`
			- `Sec-WebSocket-Accept` - klíč pro potvrzení, že server přijal klientský WS handshake $key = Base64Encode(SHA1(SecWebSocketKey + magic\; number))$
			- `Sec-WebSocket-Protocol: vybraný protokol`
		
- Data jsou formátována pomocí binary framing protokolu
# Princip
- Máme frame, který obsahuje:
	- Hlavičku - FIN fragment, RSV, opcode (typ zprávy - např. text / binary, closing frame, ping nebo pong frame), maskovací bit, Délku obsahu a případně maskovací klíč
		- Maskování zabraňuje cache-poisoning
			- Jedná se o případ, kdy útočník pošle request, kdy podvrhne data v cache proxy
			- Dalším klientům potom proxy posílá útočníkem vložená data z cache
		- Maskování potom pomocí XOR s klíčem zamaskuje obsah a server je potom demaskuje
		- Maskovací klíč musí být nepredikovatelný
	- Tělo - data samotná
- Zpráva je poté reprezentována i několika framy (které se poté spojí)
	- Zprávy se neprokládají, vždy jdou framy jedné zprávy, poté druhé zprávy
	- Kvůli tomu, že velké zprávy se rozsekají na hodně framů a může nastat [[Head-of-line blocking]]

## Řešení Head-of-line blocking
- Dbát na velikost zpráv, posílat menší zprávy
- Kontrolovat frontu před odesláním zprávy (browser má API na získání poštu položek ve frontě) a odesílat zprávy, když je fronta prázdná

# Možná omezení
- Některé mezilehlé prvky mohou být problematické stran nastavení pro dlouhodobé spojení WebSocketu (vypadává) (`HAProxy` má nastavení `timeout tunnel`)
- Klientská síť může blokovat WebSocket - je potřeba mít i záložní strategii