- Platforma pro efektivní a bezpečné doručení webové aplikace
- Hodně komponent (parsování HTML, CSS, JavaScript, síťování apod.)
# Protokoly
![[Pasted image 20250529173047.png]]

# Networking
## Optimalizace
- Recyklace socketů
- Prioritizace requestů
- Omezení počtu socketů na origin (lze obejí pomocí shardingu)
## Socket management
- Stránka obsahuje frontu požadavků, které čekají na volný socket a ten vyptávají od socket manageru
- Socket manager obsahuje pooly pro spojení na origin
- Pool je identifikován kombinací $origin = (protocol, domain, port)$ 
## Zabezpečení
- Limitované počty spojení
- Formátování požadavku a zpracování odpovědi (vynucení správného formátu protokolu)
- Zabezpečení přes TLS
- Same-origin policy - omezení odkud kam mohou být ze stránky požadavky prováděny

# Přístup z browseru k externímu zdroji
- Obě tyto metody se řídí zabezpečením (předně same origin policy)
- Jedná se o následující protokoly a standardy:
	- [[XHR (XMLHttpRequest)]]
	- [[Fetch API]]
	- [[SSE (Server Sent Events)]]
	- [[WebSocket]]
# Srovnání protokolů dostupných v prohlížeči

|                             | XHR     | Fetch API | SSE          | WebSocket       |
| --------------------------- | ------- | --------- | ------------ | --------------- |
| Streamování požadavku       | Ne      | Ano       | Ne           | Ano             |
| Streamování odpovědi        | Omezeně | Ano       | Ano          | Ano             |
| Framing mechanismus         | HTTP    | HTTP      | Event stream | Binární framing |
| Binarní transfer dat        | Ano     | Ano       | Ne (base64)  | Ano             |
| Komprese                    | Ano     | Ano       | Ano          | Omezeně         |
| Protokol aplikační vrstvy   | HTTP    | HTTP      | HTTP         | WebSocket       |
| Protokol transportní vrstvy | TCP     | TCP       | TCP          | TCP             |
