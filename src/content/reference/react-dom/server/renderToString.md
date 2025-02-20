---
title: renderToString
---

<Pitfall>

`renderToString` akışı veya veri beklemeyi desteklemez. [Alternatiflere göz atınız.](#alternatives)

</Pitfall>

<Intro>

`renderToString` bir React ağacını bir HTML string'ine dönüştürür.

```js
const html = renderToString(reactNode, options?)
```

</Intro>

<InlineToc />

---

## Başvuru dokümanı {/*reference*/}

### `renderToString(reactNode, options?)` {/*rendertostring*/}

Sunucuda, uygulamanızı HTML'e dönüştürmek için `renderToString` çağrısı yapınız.

```js
import { renderToString } from 'react-dom/server';

const html = renderToString(<App />);
```

İstemci üzerinde, sunucu tarafından oluşturulan HTML'i etkileşimli hale getirmek için [`hydrateRoot`](/reference/react-dom/client/hydrateRoot) çağrısı yapınız.

[Aşağıda daha fazla örnek görebilirsiniz.](#usage)

#### Parametreler {/*parameters*/}

* `reactNode`: HTML'e dönüştürmek istediğiniz bir React düğümü. Örneğin, `<App />` gibi bir JSX düğümü.

* **isteğe bağlı** `options`: Sunucu render'ı için bir nesne.
  * **isteğe bağlı** `identifierPrefix`: React'in [`useId`](/reference/react/useId) ile oluşturduğu ID'ler için kullandığı bir string öneki. Aynı sayfada birden fazla kök kullanıldığında çakışmaları önlemek için faydalıdır. [`hydrateRoot`](/reference/react-dom/client/hydrateRoot#parameters) fonksiyonuna geçirilen aynı önek olmalıdır.

#### Dönüş değeri {/*returns*/}

Bir HTML string'i.

#### Uyarılar {/*caveats*/}

* `renderToString` sınırlı Suspense desteğine sahiptir. Bir bileşen askıya alınırsa, `renderToString` geri dönüşünü HTML olarak hemen gönderir.

* `renderToString` tarayıcıda çalışır, ancak istemci kodunda kullanılması [tavsiye edilmez.](#removing-rendertostring-from-the-client-code)

---

## Kullanım {/*usage*/}

### Bir React ağacını HTML olarak bir string'e dönüştürme {/*rendering-a-react-tree-as-html-to-a-string*/}

Uygulamanızı sunucu yanıtınızla birlikte gönderebileceğiniz bir HTML string'ine dönüştürmek için `renderToString` öğesini çağırınız:

```js {5-6}
import { renderToString } from 'react-dom/server';

// Rota işleyici sözdizimi backend çatınıza bağlıdır
app.use('/', (request, response) => {
  const html = renderToString(<App />);
  response.send(html);
});
```

Bu, React bileşenlerinizin etkileşimli olmayan ilk HTML çıktısını üretecektir. İstemcide, sunucu tarafından oluşturulan HTML'i *hydrate* etmek ve etkileşimli hale getirmek için [`hydrateRoot`](/reference/react-dom/client/hydrateRoot) çağırmanız gerekecektir.


<Pitfall>

`renderToString` akışı veya veri beklemeyi desteklemez. [Alternatiflere göz atınız.](#alternatives)

</Pitfall>

---

## Alternatifler {/*alternatives*/}

### `renderToString`'den sunucuda bir streaming render'a geçiş yapma {/*migrating-from-rendertostring-to-a-streaming-method-on-the-server*/}

`renderToString`, hemen bir string döndürdüğü için, içerik yüklenirken streaming yapmayı desteklemez.

Mümkün olduğunda, bu tam özellikli alternatifleri kullanmanızı öneririz:

* Eğer Node.js kullanıyorsanız, [`renderToPipeableStream`](/reference/react-dom/server/renderToPipeableStream) kullanınız.
* Deno veya [Web Streams](https://developer.mozilla.org/en-US/docs/Web/API/Streams_API) ile modern bir edge çalışma zamanı kullanıyorsanız,[`renderToReadableStream`](/reference/react-dom/server/renderToReadableStream)'i kullanabilirsiniz.

Sunucu ortamınız akışları desteklemiyorsa `renderToString` kullanmaya devam edebilirsiniz.

---

### `renderToString`'den sunucuda statik önceden render'a geçiş yapma {/*migrating-from-rendertostring-to-a-static-prerender-on-the-server*/}

`renderToString`, hemen bir string döndürdüğü için, statik HTML oluşturmak için veri yüklenmesini beklemeyi desteklemez.

Bu tam özellikli alternatifleri kullanmanızı öneririz:

* Node.js kullanıyorsanız, [`prerenderToNodeStream`](/reference/react-dom/static/prerenderToNodeStream) kullanın.
* Deno veya [Web Streams](https://developer.mozilla.org/en-US/docs/Web/API/Streams_API) destekleyen modern bir edge runtime kullanıyorsanız, [`prerender`](/reference/react-dom/static/prerender) kullanın.

Statik site oluşturma ortamınız stream'leri desteklemiyorsa, `renderToString` kullanmaya devam edebilirsiniz.

---

### İstemci kodundan `renderToString`'i kaldırma {/*removing-rendertostring-from-the-client-code*/}

Bazen, `renderToString` istemcide bazı bileşenleri HTML'e dönüştürmek için kullanılır.

```js {1-2}
// 🚩 Gereksiz: istemcide renderToString kullanmak
import { renderToString } from 'react-dom/server';

const html = renderToString(<MyIcon />);
console.log(html); // Örneğin, "<svg>...</svg>"
```

**İstemci üzerinde** `react-dom/server`'ı import etmek paket boyutunuzu gereksiz yere artırır ve bundan kaçınılmalıdır. Bazı bileşenleri tarayıcıda HTML'e render etmeniz gerekiyorsa, [`createRoot`](/reference/react-dom/client/createRoot) kullanınız ve DOM'dan HTML okuyunuz:

```js
import { createRoot } from 'react-dom/client';
import { flushSync } from 'react-dom';

const div = document.createElement('div');
const root = createRoot(div);
flushSync(() => {
  root.render(<MyIcon />);
});
console.log(div.innerHTML); // Örneğin, "<svg>...</svg>"
```

DOM'un [`innerHTML`](https://developer.mozilla.org/en-US/docs/Web/API/Element/innerHTML) özelliği okunmadan önce güncellenmesi için [`flushSync`](/reference/react-dom/flushSync) çağrısı gereklidir.

---

## Sorun giderme {/*troubleshooting*/}

### Bir bileşen askıya alındığında, HTML her zaman bir fallback içerir {/*when-a-component-suspends-the-html-always-contains-a-fallback*/}

`renderToString` Suspense'i tam olarak desteklemez.

Bir bileşen askıya alınırsa (örneğin, [`lazy`](/reference/react/lazy) ile tanımlandığı veya veri getirdiği için), `renderToString` içeriğinin çözümlenmesini beklemeyecektir. Bunun yerine, `renderToString` bunun üzerindeki en yakın [`<Suspense>`](/reference/react/Suspense) sınırını bulacak ve HTML'de `fallback` prop'unu render edecektir. İstemci kodu yüklenene kadar içerik görünmeyecektir.

Bunu çözmek için, [önerilen streaming çözümlerinden](/#alternatives) birini kullanın. Sunucu tarafı render'ı için, içerik sunucuda çözülürken parça parça akıtılabilir, böylece kullanıcı istemci kodu yüklenmeden önce sayfanın kademeli olarak doldurulduğunu görür. Statik site oluşturma için, tüm içerik çözülene kadar bekleyebilir ve ardından statik HTML'i oluşturabilir.

