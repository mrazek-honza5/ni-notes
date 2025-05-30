- Modernější přístup pro komunikaci s API
- Nativně využívá `Promise` místo callbacku
- Má 3 rozhraní
	- Request - objekt představující request
	- Response - objekt vrácený z volání `fetch` funkce
	- Headers - pro požadavky i odpovědi
- Dále také můžeme přistoupit k datovému streamu, takže můžu zastavit přenos dříve
	- Je to pomocí `ReadableStream` v `Response` body
	- Funguje to díky [[Streams API]]
	- Stream potom mohu číst díky `ReadableStream.getReader()`
	- 
# Ukázka
```js
async function getData() {
  const url = "https://example.org/products.json";
  try {
    // Alternativelry, a Request interface could be passed to fetch
    const response = await fetch(url); // Returns Response interface
    if (!response.ok) {
      throw new Error(`Response status: ${response.status}`);
    }

    const json = await response.json();
    console.log(json);
  } catch (error) {
    console.error(error.message);
  }
}
```