---
title: createRoot
---

<Intro>

`createRoot` React bileşenlerini bir tarayıcı DOM düğümü içinde görüntülemek için bir kök oluşturmanızı sağlar.

```js
const root = createRoot(domNode, options?)
```

</Intro>

<InlineToc />

---

## Referans {/*reference*/}

### `createRoot(domNode, options?)` {/*createroot*/}

İçeriği bir tarayıcı DOM elemanı içinde görüntülemek üzere bir React kökü oluşturmak için `createRoot` çağrısı yapın.

```js
import { createRoot } from 'react-dom/client';

const domNode = document.getElementById('root');
const root = createRoot(domNode);
```

React, `domNode` için bir kök oluşturacak ve içindeki DOM'un yönetimini üstlenecek. Bir kök oluşturduktan sonra, içinde bir React bileşeni görüntülemek için [`root.render`](#root-render) çağırmanız gerekir:

```js
root.render(<App />);
```

Tamamen React ile oluşturulmuş bir uygulama genellikle kök bileşeni için yalnızca bir `createRoot` çağrısına sahip olacaktır. Sayfanın bazı bölümleri için React "serpintileri" kullanan bir sayfa, ihtiyaç duyulan kadar çok sayıda ayrı köke sahip olabilir.

[Daha fazla örnek için aşağıya bakın.](#usage)

#### Parametreler {/*parameters*/}

* `domNode`: Bir [DOM elemanı.](https://developer.mozilla.org/en-US/docs/Web/API/Element) React bu DOM elemanı için bir kök oluşturacak ve `render` gibi render edilmiş React içeriğini görüntülemek için kök üzerinde fonksiyonlar çağırmanıza izin verecektir.

* **opsiyonel** `options`: Bu React kökü için seçenekler içeren bir nesne.

* **isteğe bağlı** `onCaughtError`: React, bir Error Boundary içinde bir hata yakaladığında çağrılan geri çağırma fonksiyonu. Error Boundary tarafından yakalanan `error` ve `componentStack` içeren bir `errorInfo` nesnesi ile çağrılır.
* **isteğe bağlı** `onUncaughtError`: Bir hata fırlatıldığında ve Error Boundary tarafından yakalanmadığında çağrılan geri çağırma fonksiyonu. Fırlatılan `error` ve `componentStack` içeren bir `errorInfo` nesnesi ile çağrılır.
* **isteğe bağlı** `onRecoverableError`: React, hatalardan otomatik olarak kurtarıldığında çağrılan geri çağırma fonksiyonu. React'in fırlattığı `error` ve `componentStack` içeren bir `errorInfo` nesnesi ile çağrılır. Bazı kurtarılabilir hatalar, orijinal hata nedenini `error.cause` olarak içerebilir.
* **isteğe bağlı** `identifierPrefix`: React'in [`useId` ](/reference/react/useId) ile oluşturduğu ID'ler için kullandığı bir string öneki. Aynı sayfada birden fazla kök kullanıldığında çakışmaları önlemek için faydalıdır.


#### Döndürülenler {/*returns*/}

`createRoot` [`render`](#root-render) ve [`unmount`](#root-unmount) olmak üzere iki yöntem içeren bir nesne döndürür.

#### Uyarılar {/*caveats*/}
* Uygulamanız sunucu tarafından oluşturulmuşsa, `createRoot()` kullanımı desteklenmez. Bunun yerine [`hydrateRoot()`](/reference/react-dom/client/hydrateRoot) kullanın.
* Uygulamanızda muhtemelen yalnızca bir `createRoot` çağrısı olacaktır. Eğer bir çatı kullanıyorsanız, bu çağrıyı sizin için yapabilir.
* Bileşeninizin alt öğesi olmayan DOM ağacının farklı bir bölümünde bir JSX parçası render etmek istediğinizde (örneğin, bir modal veya bir araç ipucu), `createRoot` yerine [`createPortal`](/reference/react-dom/createPortal) kullanın.

---

### `root.render(reactNode)` {/*root-render*/}

React root'un tarayıcı DOM düğümünde bir [JSX](/learn/writing-markup-with-jsx) parçası görüntülemek için `root.render` çağrısı yapın.

```js
root.render(<App />);
```

React, `root` içinde `<App />` gösterecek ve içindeki DOM'un yönetimini üstlenecektir.

[Daha fazla örnek için aşağıya bakın.](#usage)

#### Parametreler {/*root-render-parameters*/}

* `reactNode`: Görüntülemek istediğiniz bir *React düğümü*. Bu genellikle `<App />` gibi bir JSX parçası olacaktır, ancak [`createElement()`](/reference/react/createElement) ile oluşturulmuş bir React elemanı, bir string, bir sayı, `null` veya `undefined` da iletebilirsiniz.


#### Döndürülenler {/*root-render-returns*/}

`root.render` `undefined` değerini döndürür.

#### Uyarılar {/*root-render-caveats*/}

* İlk kez `root.render` fonksiyonunu çağırdığınız zaman React, React bileşenini render etmeden önce React kökü içindeki mevcut tüm HTML içeriğini temizleyecektir.

* Kök DOM düğümünüz sunucuda veya derleme sırasında React tarafından oluşturulan HTML içeriyorsa, bunun yerine olay işleyicilerini mevcut HTML'ye ekleyen [`hydrateRoot()`](/reference/react-dom/client/hydrateRoot) fonksiyonunu kullanın.

* Aynı kök üzerinde birden fazla kez `render` çağrısı yaparsanız, React ilettiğiniz en son JSX'i yansıtmak için DOM'u gerektiği gibi güncelleyecektir. React, DOM'un hangi bölümlerinin yeniden kullanılabileceğine ve hangilerinin yeniden oluşturulması gerektiğine daha önce oluşturulmuş ağaçla ["eşleştirerek"](/learn/preserving-and-resetting-state) daha önce oluşturulmuş ağaçla karar verecektir. Aynı kök üzerinde `render` fonksiyonunu tekrar çağırmak, kök bileşen üzerinde [`set` fonksiyonunu](/reference/react/useState#setstate) çağırmaya benzer: React gereksiz DOM güncellemelerinden kaçınır.

---

### `root.unmount()` {/*root-unmount*/}

React kökü içinde render edilmiş bir ağacı yok etmek için `root.unmount` çağırın.

```js
root.unmount();
```

Tamamen React ile oluşturulan bir uygulamada genellikle `root.unmount` çağrısı olmayacaktır.

Bu, çoğunlukla React kök DOM düğümünüzün (veya atalarından herhangi birinin) başka bir kod tarafından DOM'dan kaldırılabileceği durumlarda kullanışlıdır. Örneğin, etkin olmayan sekmeleri DOM'dan kaldıran bir jQuery sekme paneli düşünün. Bir sekme kaldırılırsa, içindeki her şey (içindeki React kökleri de dahil olmak üzere) DOM'dan da kaldırılacaktır. Bu durumda, React'e `root.unmount` çağrısı yaparak kaldırılan kökün içeriğini yönetmeyi "durdurmasını" söylemeniz gerekir. Aksi takdirde, kaldırılan kökün içindeki bileşenler, abonelikler gibi global kaynakları temizlemeyi ve boşaltmayı bilemez.

`root.unmount` çağrısı, ağaçtaki tüm olay yöneticilerini veya state'i kaldırmak da dahil olmak üzere, kökteki tüm bileşenleri DOM'dan kaldıracak ve React'i kök DOM düğümünden "ayıracaktır".


#### Parametreler {/*root-unmount-parameters*/}

`root.unmount` herhangi bir parametre kabul etmez.


#### Döndürülenler {/*root-unmount-returns*/}

`root.unmount` `undefined` döndürür.

#### Uyarılar {/*root-unmount-caveats*/}

* `root.unmount` çağrısı, ağaçtaki tüm bileşenleri DOM'dan kaldıracak ve React'i kök DOM düğümünden "ayıracaktır".

* Bir kez `root.unmount` çağrısı yaptığınızda, aynı kök üzerinde tekrar `root.render` çağrısı yapamazsınız. Bağlanmamış bir kök üzerinde `root.render` çağrılmaya çalışıldığında "Bağlanmamış bir kök güncellenemiyor" hatası verilir. Ancak, aynı DOM düğümü için önceki kökün bağlantısı kaldırıldıktan sonra yeni bir kök oluşturabilirsiniz.

---

## Kullanım {/*usage*/}

### Tamamen React ile oluşturulmuş bir uygulamayı render etmek {/*rendering-an-app-fully-built-with-react*/}

Eğer uygulamanız tamamen React ile oluşturulmuşsa, uygulamanızın tamamı için tek bir kök oluşturun.

```js [[1, 3, "document.getElementById('root')"], [2, 4, "<App />"]]
import { createRoot } from 'react-dom/client';

const root = createRoot(document.getElementById('root'));
root.render(<App />);
```

Genellikle bu kodu başlangıçta yalnızca bir kez çalıştırmanız gerekir. Bu işlem şunları yapacaktır:

1. HTML'nizde tanımlanan  <CodeStep step={1}>tarayıcı DOM</CodeStep> düğümünü bulun.
2. Uygulamanızın  <CodeStep step={2}>React bileşenini</CodeStep> içinde görüntüleyin.

<Sandpack>

```html index.html
<!DOCTYPE html>
<html>
  <head><title>Benim uygulamam</title></head>
  <body>
    <!-- Bu DOM düğümüdür -->
    <div id="root"></div>
  </body>
</html>
```

```js src/index.js active
import { createRoot } from 'react-dom/client';
import App from './App.js';
import './styles.css';

const root = createRoot(document.getElementById('root'));
root.render(<App />);
```

```js src/App.js
import { useState } from 'react';

export default function App() {
  return (
    <>
      <h1>Merhaba Dünya!</h1>
      <Counter />
    </>
  );
}

function Counter() {
  const [count, setCount] = useState(0);
  return (
    <button onClick={() => setCount(count + 1)}>
      Bana {count} kere tıkladın 
    </button>
  );
}
```

</Sandpack>

**Eğer uygulamanız tamamen React ile oluşturulmuşsa, daha fazla kök oluşturmanız veya [`root.render`](#root-render)'ı tekrar çağırmanız gerekmez.** 

Bu noktadan itibaren React tüm uygulamanızın DOM'unu yönetecektir. Daha fazla bileşen eklemek için, [bunları `App` bileşeninin içine yerleştirin.](/learn/importing-and-exporting-components) Kullanıcı arayüzünü güncellemeniz gerektiğinde, bileşenlerinizin her biri bunu [state kullanarak yapabilir.](/reference/react/useState) DOM düğümünün dışında bir modal veya araç ipucu gibi ekstra içerik görüntülemeniz gerektiğinde, [bunu bir portal ile oluşturun.](/reference/react-dom/createPortal)

<Note>

HTML'niz boş olduğunda, uygulamanın JavaScript kodu yüklenip çalışana kadar kullanıcı boş bir sayfa görür:

```html
<div id="root"></div>
```

Bu çok yavaş hissettirebilir! Bunu çözmek için, bileşenlerinizden [sunucuda veya derleme sırasında] ilk HTML'yi oluşturabilirsiniz. (/reference/react-dom/server) Ardından ziyaretçileriniz JavaScript kodunun herhangi biri yüklenmeden önce metin okuyabilir, resimleri görebilir ve bağlantılara tıklayabilir. Bu optimizasyonu otomatik olarak yapan [bir framework kullanmanızı](/learn/start-a-new-react-project#production-grade-react-frameworks) öneririz. Ne zaman çalıştığına bağlı olarak buna *sunucu taraflı render etme (SSR)* veya *statik site oluşturma (SSG)* denir.

</Note>

<Pitfall>

**Sunucu taraflı render veya statik oluşturma kullanan uygulamalar `createRoot` yerine [`hydrateRoot`](/reference/react-dom/client/hydrateRoot) çağırmalıdır.** React daha sonra DOM düğümlerini HTML'nizden yok etmek ve yeniden oluşturmak yerine *hydrate* edecektir (yeniden kullanacaktır).

</Pitfall>

---

### Kısmen React ile oluşturulan bir sayfa render etmek {/*rendering-a-page-partially-built-with-react*/}

Sayfanız [tamamen React ile oluşturulmamışsa](/learn/add-react-to-an-existing-project#using-react-for-a-part-of-your-existing-page), React tarafından yönetilen her bir üst düzey kullanıcı arayüzü parçası için bir kök oluşturmak üzere `createRoot` öğesini birden çok kez çağırabilirsiniz. Her kökte [`root.render`](#root-render) çağrısı yaparak her kökte farklı içerikler görüntüleyebilirsiniz.

Burada, index.html dosyasında tanımlanan iki farklı DOM düğümüne iki farklı React bileşeni render edilmiştir:

<Sandpack>

```html public/index.html
<!DOCTYPE html>
<html>
  <head><title>Benim uygulamam</title></head>
  <body>
    <nav id="navigation"></nav>
    <main>
      <p>Bu paragraf React tarafından render edilmez (doğrulamak için index.html dosyasını açın).</p>
      <section id="comments"></section>
    </main>
  </body>
</html>
```

```js src/index.js active
import './styles.css';
import { createRoot } from 'react-dom/client';
import { Comments, Navigation } from './Components.js';

const navDomNode = document.getElementById('navigation');
const navRoot = createRoot(navDomNode); 
navRoot.render(<Navigation />);

const commentDomNode = document.getElementById('comments');
const commentRoot = createRoot(commentDomNode); 
commentRoot.render(<Comments />);
```

```js src/Components.js
export function Navigation() {
  return (
    <ul>
      <NavLink href="/">Ana Sayfa</NavLink>
      <NavLink href="/about">Hakkında</NavLink>
    </ul>
  );
}

function NavLink({ href, children }) {
  return (
    <li>
      <a href={href}>{children}</a>
    </li>
  );
}

export function Comments() {
  return (
    <>
      <h2>Yorumlar</h2>
      <Comment text="Merhaba!" author="Alper" />
      <Comment text="Nasılsın?" author="Erdoğan" />
    </>
  );
}

function Comment({ text, author }) {
  return (
    <p>{text} — <i>{author}</i></p>
  );
}
```

```css
nav ul { padding: 0; margin: 0; }
nav ul li { display: inline-block; margin-right: 20px; }
```

</Sandpack>

Ayrıca [`document.createElement()`](https://developer.mozilla.org/en-US/docs/Web/API/Document/createElement) ile yeni bir DOM düğümü oluşturabilir ve bunu dokümana manuel olarak ekleyebilirsiniz.

```js
const domNode = document.createElement('div');
const root = createRoot(domNode);
root.render(<Comment />);
document.body.appendChild(domNode); // Dokümanın herhangi bir yerine ekleyebilirsiniz
```

React ağacını DOM düğümünden kaldırmak ve onun tarafından kullanılan tüm kaynakları temizlemek için [`root.unmount`.](#root-unmount) çağırın.

```js
root.unmount();
```

Bu, çoğunlukla React bileşenleriniz farklı bir çatıda yazılmış bir uygulamanın içindeyse kullanışlıdır.

---

### Bir kök bileşenin güncellenmesi {/*updating-a-root-component*/}

Aynı kök üzerinde `render` fonksiyonunu birden fazla kez çağırabilirsiniz. Önceki render edilen ile bileşen ağaç yapısı eşleştiği sürece, React [state'i koruyacaktır.](/learn/preserving-and-resetting-state). Bu örnekte her saniyede tekrarlanan `render` çağrılarından kaynaklanan güncellemelerin yıkıcı olmadığına dikkat edin. Örneğin girdi kutusuna yazı yazıyorsunuz:

<Sandpack>

```js src/index.js active
import { createRoot } from 'react-dom/client';
import './styles.css';
import App from './App.js';

const root = createRoot(document.getElementById('root'));

let i = 0;
setInterval(() => {
  root.render(<App counter={i} />);
  i++;
}, 1000);
```

```js src/App.js
export default function App({counter}) {
  return (
    <>
      <h1>Merhaba Dünya! {counter}</h1>
      <input placeholder="Buraya bir şeyler yazın" />
    </>
  );
}
```

</Sandpack>

Birden fazla kez `render` çağrısı yapmak nadirdir. Genellikle bileşenleriniz bunun yerine [state güncellemesi](/reference/react/useState) yapacaktır.

### Yakalanmamış hatalar için bir diyaloğu gösterme {/*show-a-dialog-for-uncaught-errors*/}

Varsayılan olarak, React tüm yakalanmamış hataları konsola kaydeder. Kendi hata raporlama sisteminizi uygulamak için isteğe bağlı `onUncaughtError` kök seçeneğini sağlayabilirsiniz:

```js [[1, 6, "onUncaughtError"], [2, 6, "error", 1], [3, 6, "errorInfo"], [4, 10, "componentStack"]]
import { createRoot } from 'react-dom/client';

const root = createRoot(
  document.getElementById('root'),
  {
    onUncaughtError: (error, errorInfo) => {
      console.error(
        'Yakalanmamış hata',
        error,
        errorInfo.componentStack
      );
    }
  }
);
root.render(<App />);
```

<CodeStep step={1}>onUncaughtError</CodeStep> seçeneği iki bağımsız değişkenle çağrılan bir fonksiyondur:

1. Fırlatılan <CodeStep step={2}>hata</CodeStep>.

2. Hatanın <CodeStep step={4}>componentStack</CodeStep>'ini içeren bir <CodeStep step={3}>errorInfo</CodeStep> nesnesi.

Hata diyalog pencerelerini görüntülemek için `onUncaughtError` kök seçeneğini kullanabilirsin:

<Sandpack>

```html index.html hidden
<!DOCTYPE html>
<html>
<head>
  <title>Benim uygulamam</title>
</head>
<body>
<!--
  Error dialog in raw HTML
  since an error in the React app may crash.
-->
<div id="error-dialog" class="hidden">
  <h1 id="error-title" class="text-red"></h1>
  <h3>
    <pre id="error-message"></pre>
  </h3>
  <p>
    <pre id="error-body"></pre>
  </p>
  <h4 class="-mb-20">Meydana gelen hata:</h4>
  <pre id="error-component-stack" class="nowrap"></pre>
  <h4 class="mb-0">Çağrı yığını:</h4>
  <pre id="error-stack" class="nowrap"></pre>
  <div id="error-cause">
    <h4 class="mb-0">Sebep olan:</h4>
    <pre id="error-cause-message"></pre>
    <pre id="error-cause-stack" class="nowrap"></pre>
  </div>
  <button
    id="error-close"
    class="mb-10"
    onclick="document.getElementById('error-dialog').classList.add('hidden')"
  >
    Close
  </button>
  <h3 id="error-not-dismissible">Bu hata göz ardı edilemez.</h3>
</div>
<!-- This is the DOM node -->
<div id="root"></div>
</body>
</html>
```

```css src/styles.css active
label, button { display: block; margin-bottom: 20px; }
html, body { min-height: 300px; }

#error-dialog {
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  background-color: white;
  padding: 15px;
  opacity: 0.9;
  text-wrap: wrap;
  overflow: scroll;
}

.text-red {
  color: red;
}

.-mb-20 {
  margin-bottom: -20px;
}

.mb-0 {
  margin-bottom: 0;
}

.mb-10 {
  margin-bottom: 10px;
}

pre {
  text-wrap: wrap;
}

pre.nowrap {
  text-wrap: nowrap;
}

.hidden {
 display: none;  
}
```

```js src/reportError.js hidden
function reportError({ title, error, componentStack, dismissable }) {
  const errorDialog = document.getElementById("error-dialog");
  const errorTitle = document.getElementById("error-title");
  const errorMessage = document.getElementById("error-message");
  const errorBody = document.getElementById("error-body");
  const errorComponentStack = document.getElementById("error-component-stack");
  const errorStack = document.getElementById("error-stack");
  const errorClose = document.getElementById("error-close");
  const errorCause = document.getElementById("error-cause");
  const errorCauseMessage = document.getElementById("error-cause-message");
  const errorCauseStack = document.getElementById("error-cause-stack");
  const errorNotDismissible = document.getElementById("error-not-dismissible");
  
  // Set the title
  errorTitle.innerText = title;
  
  // Display error message and body
  const [heading, body] = error.message.split(/\n(.*)/s);
  errorMessage.innerText = heading;
  if (body) {
    errorBody.innerText = body;
  } else {
    errorBody.innerText = '';
  }

  // Display component stack
  errorComponentStack.innerText = componentStack;

  // Display the call stack
  // Since we already displayed the message, strip it, and the first Error: line.
  errorStack.innerText = error.stack.replace(error.message, '').split(/\n(.*)/s)[1];
  
  // Display the cause, if available
  if (error.cause) {
    errorCauseMessage.innerText = error.cause.message;
    errorCauseStack.innerText = error.cause.stack;
    errorCause.classList.remove('hidden');
  } else {
    errorCause.classList.add('hidden');
  }
  // Display the close button, if dismissible
  if (dismissable) {
    errorNotDismissible.classList.add('hidden');
    errorClose.classList.remove("hidden");
  } else {
    errorNotDismissible.classList.remove('hidden');
    errorClose.classList.add("hidden");
  }
  
  // Show the dialog
  errorDialog.classList.remove("hidden");
}

export function reportCaughtError({error, cause, componentStack}) {
  reportError({ title: "Yakalanan Hata", error, componentStack,  dismissable: true});
}

export function reportUncaughtError({error, cause, componentStack}) {
  reportError({ title: "Yakalanmamış Hata", error, componentStack, dismissable: false });
}

export function reportRecoverableError({error, cause, componentStack}) {
  reportError({ title: "Kurtarılabilir Hata", error, componentStack,  dismissable: true });
}
```

```js src/index.js active
import { createRoot } from "react-dom/client";
import App from "./App.js";
import {reportUncaughtError} from "./reportError";
import "./styles.css";

const container = document.getElementById("root");
const root = createRoot(container, {
  onUncaughtError: (error, errorInfo) => {
    if (error.message !== 'Known error') {
      reportUncaughtError({
        error,
        componentStack: errorInfo.componentStack
      });
    }
  }
});
root.render(<App />);
```

```js src/App.js
import { useState } from 'react';

export default function App() {
  const [throwError, setThrowError] = useState(false);
  
  if (throwError) {
    foo.bar = 'baz';
  }
  
  return (
    <div>
      <span>Bu hata, hata diyaloğunu gösterir:</span>
      <button onClick={() => setThrowError(true)}>
        Hata fırlat
      </button>
    </div>
  );
}
```

</Sandpack>


### Hata yakalayıcı ile ilgili hataları görüntüleme {/*displaying-error-boundary-errors*/}

Varsayılan olarak, React bir Error Boundary tarafından yakalanan tüm hataları `console.error` içine kaydeder. Bu davranışı değiştirmek için, bir [Error Boundary](/reference/react/Component#catching-rendering-errors-with-an-error-boundary) tarafından yakalanan hataları işlemek için isteğe bağlı `onCaughtError` kök seçeneğini sağlayabilirsiniz:

```js [[1, 6, "onCaughtError"], [2, 6, "error", 1], [3, 6, "errorInfo"], [4, 10, "componentStack"]]
import { createRoot } from 'react-dom/client';

const root = createRoot(
  document.getElementById('root'),
  {
    onCaughtError: (error, errorInfo) => {
      console.error(
        'Yakalanan hata',
        error,
        errorInfo.componentStack
      );
    }
  }
);
root.render(<App />);
```

<CodeStep step={1}>onCaughtError</CodeStep> seçeneği iki bağımsız değişkenle çağrılan bir fonksiyondur:

1. Hata yakalayıcı tarafından yakalanan <CodeStep step={2}>hata</CodeStep>.
2. Hatanın <CodeStep step={4}>componentStack</CodeStep>'ini içeren bir <CodeStep step={3}>errorInfo</CodeStep> nesnesi.

Hata diyologlarını görüntülemek veya bilinen hataları günlükten filtrelemek için `onCaughtError` kök seçeneğini kullanabilirsin:

<Sandpack>

```html index.html hidden
<!DOCTYPE html>
<html>
<head>
  <title>Benim uygulamam</title>
</head>
<body>
<!--
  Error dialog in raw HTML
  since an error in the React app may crash.
-->
<div id="error-dialog" class="hidden">
  <h1 id="error-title" class="text-red"></h1>
  <h3>
    <pre id="error-message"></pre>
  </h3>
  <p>
    <pre id="error-body"></pre>
  </p>
  <h4 class="-mb-20">Meydana gelen hata:</h4>
  <pre id="error-component-stack" class="nowrap"></pre>
  <h4 class="mb-0">Çağrı yığını:</h4>
  <pre id="error-stack" class="nowrap"></pre>
  <div id="error-cause">
    <h4 class="mb-0">Sebep olan:</h4>
    <pre id="error-cause-message"></pre>
    <pre id="error-cause-stack" class="nowrap"></pre>
  </div>
  <button
    id="error-close"
    class="mb-10"
    onclick="document.getElementById('error-dialog').classList.add('hidden')"
  >
    Close
  </button>
  <h3 id="error-not-dismissible">Bu hata göz ardı edilemez.</h3>
</div>
<!-- This is the DOM node -->
<div id="root"></div>
</body>
</html>
```

```css src/styles.css active
label, button { display: block; margin-bottom: 20px; }
html, body { min-height: 300px; }

#error-dialog {
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  background-color: white;
  padding: 15px;
  opacity: 0.9;
  text-wrap: wrap;
  overflow: scroll;
}

.text-red {
  color: red;
}

.-mb-20 {
  margin-bottom: -20px;
}

.mb-0 {
  margin-bottom: 0;
}

.mb-10 {
  margin-bottom: 10px;
}

pre {
  text-wrap: wrap;
}

pre.nowrap {
  text-wrap: nowrap;
}

.hidden {
 display: none;  
}
```

```js src/reportError.js hidden
function reportError({ title, error, componentStack, dismissable }) {
  const errorDialog = document.getElementById("error-dialog");
  const errorTitle = document.getElementById("error-title");
  const errorMessage = document.getElementById("error-message");
  const errorBody = document.getElementById("error-body");
  const errorComponentStack = document.getElementById("error-component-stack");
  const errorStack = document.getElementById("error-stack");
  const errorClose = document.getElementById("error-close");
  const errorCause = document.getElementById("error-cause");
  const errorCauseMessage = document.getElementById("error-cause-message");
  const errorCauseStack = document.getElementById("error-cause-stack");
  const errorNotDismissible = document.getElementById("error-not-dismissible");

  // Set the title
  errorTitle.innerText = title;

  // Display error message and body
  const [heading, body] = error.message.split(/\n(.*)/s);
  errorMessage.innerText = heading;
  if (body) {
    errorBody.innerText = body;
  } else {
    errorBody.innerText = '';
  }

  // Display component stack
  errorComponentStack.innerText = componentStack;

  // Display the call stack
  // Since we already displayed the message, strip it, and the first Error: line.
  errorStack.innerText = error.stack.replace(error.message, '').split(/\n(.*)/s)[1];

  // Display the cause, if available
  if (error.cause) {
    errorCauseMessage.innerText = error.cause.message;
    errorCauseStack.innerText = error.cause.stack;
    errorCause.classList.remove('hidden');
  } else {
    errorCause.classList.add('hidden');
  }
  // Display the close button, if dismissible
  if (dismissable) {
    errorNotDismissible.classList.add('hidden');
    errorClose.classList.remove("hidden");
  } else {
    errorNotDismissible.classList.remove('hidden');
    errorClose.classList.add("hidden");
  }

  // Show the dialog
  errorDialog.classList.remove("hidden");
}

export function reportCaughtError({error, cause, componentStack}) {
  reportError({ title: "Yakalanan Hata", error, componentStack,  dismissable: true});
}

export function reportUncaughtError({error, cause, componentStack}) {
  reportError({ title: "Yakalanmamış Hata", error, componentStack, dismissable: false });
}

export function reportRecoverableError({error, cause, componentStack}) {
  reportError({ title: "Kurtarılabilir Hata", error, componentStack,  dismissable: true });
}
```

```js src/index.js active
import { createRoot } from "react-dom/client";
import App from "./App.js";
import {reportCaughtError} from "./reportError";
import "./styles.css";

const container = document.getElementById("root");
const root = createRoot(container, {
  onCaughtError: (error, errorInfo) => {
    if (error.message !== 'Known error') {
      reportCaughtError({
        error, 
        componentStack: errorInfo.componentStack,
      });
    }
  }
});
root.render(<App />);
```

```js src/App.js
import { useState } from 'react';
import { ErrorBoundary } from "react-error-boundary";

export default function App() {
  const [error, setError] = useState(null);
  
  function handleUnknown() {
    setError("unknown");
  }

  function handleKnown() {
    setError("known");
  }
  
  return (
    <>
      <ErrorBoundary
        fallbackRender={fallbackRender}
        onReset={(details) => {
          setError(null);
        }}
      >
        {error != null && <Throw error={error} />}
        <span>Bu hata, hata diyaloğunu göstermeyecektir:</span>
        <button onClick={handleKnown}>
          Bilinen hatayı fırlat
        </button>
        <span>Bu hata, hata diyoloğunu gösterecektir:</span>
        <button onClick={handleUnknown}>
          Bilinmeyen bir hata fırlat
        </button>
      </ErrorBoundary>
      
    </>
  );
}

function fallbackRender({ resetErrorBoundary }) {
  return (
    <div role="alert">
      <h3>Hata Yakalayıcı</h3>
      <p>Bir şeyler ters gitti.</p>
      <button onClick={resetErrorBoundary}>Sıfırla</button>
    </div>
  );
}

function Throw({error}) {
  if (error === "known") {
    throw new Error('Known error')
  } else {
    foo.bar = 'baz';
  }
}
```

```json package.json hidden
{
  "dependencies": {
    "react": "19.0.0-rc-3edc000d-20240926",
    "react-dom": "19.0.0-rc-3edc000d-20240926",
    "react-scripts": "^5.0.0",
    "react-error-boundary": "4.0.3"
  },
  "main": "/index.js"
}
```

</Sandpack>

### Kurtarılabilir hatalar için bir diyoloğu görüntüleme {/*displaying-a-dialog-for-recoverable-errors*/}

React, render etme sırasında atılan bir hatadan kurtulmayı denemek için bir bileşeni otomatik olarak ikinci kez render edebilir. Başarılı olursa, React geliştiriciyi bilgilendirmek için konsola kurtarılabilir bir hata günlüğü kaydeder. Bu davranışı geçersiz kılmak için, isteğe bağlı `onRecoverableError` kök seçeneğini sağlayabilirsin:

```js [[1, 6, "onRecoverableError"], [2, 6, "error", 1], [3, 10, "error.cause"], [4, 6, "errorInfo"], [5, 11, "componentStack"]]
import { createRoot } from 'react-dom/client';

const root = createRoot(
  document.getElementById('root'),
  {
    onRecoverableError: (error, errorInfo) => {
      console.error(
        'Kurtarılabilir hata',
        error,
        error.cause,
        errorInfo.componentStack,
      );
    }
  }
);
root.render(<App />);
```

<CodeStep step={1}>onRecoverableError</CodeStep> seçeneği iki bağımsız değişkenle çağrılan bir fonksiyondur:

1. React'in fırlattığı <CodeStep step={2}>hata</CodeStep>. Bazı hatalar, <CodeStep step={3}>error.cause</CodeStep> olarak orijinal nedeni içerebilir.

2. Hatanın <CodeStep step={5}>componentStack</CodeStep>'ini içeren bir <CodeStep step={4}>errorInfo</CodeStep> nesnesi.

Hata diyaloglarını görüntülemek için `onRecoverableError` kök seçeneğini kullanabilirsin:

<Sandpack>

```html index.html hidden
<!DOCTYPE html>
<html>
<head>
  <title>Benim uygulamam</title>
</head>
<body>
<!--
  Error dialog in raw HTML
  since an error in the React app may crash.
-->
<div id="error-dialog" class="hidden">
  <h1 id="error-title" class="text-red"></h1>
  <h3>
    <pre id="error-message"></pre>
  </h3>
  <p>
    <pre id="error-body"></pre>
  </p>
  <h4 class="-mb-20">Meydana gelen hata:</h4>
  <pre id="error-component-stack" class="nowrap"></pre>
  <h4 class="mb-0">Çağrı yığını:</h4>
  <pre id="error-stack" class="nowrap"></pre>
  <div id="error-cause">
    <h4 class="mb-0">Sebep olan:</h4>
    <pre id="error-cause-message"></pre>
    <pre id="error-cause-stack" class="nowrap"></pre>
  </div>
  <button
    id="error-close"
    class="mb-10"
    onclick="document.getElementById('error-dialog').classList.add('hidden')"
  >
    Close
  </button>
  <h3 id="error-not-dismissible">Bu hata göz ardı edilemez.</h3>
</div>
<!-- This is the DOM node -->
<div id="root"></div>
</body>
</html>
```

```css src/styles.css active
label, button { display: block; margin-bottom: 20px; }
html, body { min-height: 300px; }

#error-dialog {
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  background-color: white;
  padding: 15px;
  opacity: 0.9;
  text-wrap: wrap;
  overflow: scroll;
}

.text-red {
  color: red;
}

.-mb-20 {
  margin-bottom: -20px;
}

.mb-0 {
  margin-bottom: 0;
}

.mb-10 {
  margin-bottom: 10px;
}

pre {
  text-wrap: wrap;
}

pre.nowrap {
  text-wrap: nowrap;
}

.hidden {
 display: none;  
}
```

```js src/reportError.js hidden
function reportError({ title, error, componentStack, dismissable }) {
  const errorDialog = document.getElementById("error-dialog");
  const errorTitle = document.getElementById("error-title");
  const errorMessage = document.getElementById("error-message");
  const errorBody = document.getElementById("error-body");
  const errorComponentStack = document.getElementById("error-component-stack");
  const errorStack = document.getElementById("error-stack");
  const errorClose = document.getElementById("error-close");
  const errorCause = document.getElementById("error-cause");
  const errorCauseMessage = document.getElementById("error-cause-message");
  const errorCauseStack = document.getElementById("error-cause-stack");
  const errorNotDismissible = document.getElementById("error-not-dismissible");

  // Set the title
  errorTitle.innerText = title;

  // Display error message and body
  const [heading, body] = error.message.split(/\n(.*)/s);
  errorMessage.innerText = heading;
  if (body) {
    errorBody.innerText = body;
  } else {
    errorBody.innerText = '';
  }

  // Display component stack
  errorComponentStack.innerText = componentStack;

  // Display the call stack
  // Since we already displayed the message, strip it, and the first Error: line.
  errorStack.innerText = error.stack.replace(error.message, '').split(/\n(.*)/s)[1];

  // Display the cause, if available
  if (error.cause) {
    errorCauseMessage.innerText = error.cause.message;
    errorCauseStack.innerText = error.cause.stack;
    errorCause.classList.remove('hidden');
  } else {
    errorCause.classList.add('hidden');
  }
  // Display the close button, if dismissible
  if (dismissable) {
    errorNotDismissible.classList.add('hidden');
    errorClose.classList.remove("hidden");
  } else {
    errorNotDismissible.classList.remove('hidden');
    errorClose.classList.add("hidden");
  }

  // Show the dialog
  errorDialog.classList.remove("hidden");
}

export function reportCaughtError({error, cause, componentStack}) {
  reportError({ title: "Yakalanan Hata", error, componentStack,  dismissable: true});
}

export function reportUncaughtError({error, cause, componentStack}) {
  reportError({ title: "Yakalanmamış Hata", error, componentStack, dismissable: false });
}

export function reportRecoverableError({error, cause, componentStack}) {
  reportError({ title: "Kurtarılabilir Hata", error, componentStack,  dismissable: true });
}
```

```js src/index.js active
import { createRoot } from "react-dom/client";
import App from "./App.js";
import {reportRecoverableError} from "./reportError";
import "./styles.css";

const container = document.getElementById("root");
const root = createRoot(container, {
  onRecoverableError: (error, errorInfo) => {
    reportRecoverableError({
      error,
      cause: error.cause,
      componentStack: errorInfo.componentStack,
    });
  }
});
root.render(<App />);
```

```js src/App.js
import { useState } from 'react';
import { ErrorBoundary } from "react-error-boundary";

// 🚩 Bug: Never do this. This will force an error.
let errorThrown = false;
export default function App() {
  return (
    <>
      <ErrorBoundary
        fallbackRender={fallbackRender}
      >
        {!errorThrown && <Throw />}
        <p>Bu bileşen bir hata fırlattı, ancak ikinci bir render etme sırasında düzeldi.</p>
        <p>Kurtarıldığı için Hata yakalayıcı gösterilmedi, ancak bir hata diyoloğu göstermek için <code>onRecoverableError</code> kullanıldı.</p>
      </ErrorBoundary>
      
    </>
  );
}

function fallbackRender() {
  return (
    <div role="alert">
      <h3>Hata Yakalayıcı</h3>
      <p>Bir şeyler ters gitti.</p>
    </div>
  );
}

function Throw({error}) {
  // Simulate an external value changing during concurrent render.
  errorThrown = true;
  foo.bar = 'baz';
}
```

```json package.json hidden
{
  "dependencies": {
    "react": "19.0.0-rc-3edc000d-20240926",
    "react-dom": "19.0.0-rc-3edc000d-20240926",
    "react-scripts": "^5.0.0",
    "react-error-boundary": "4.0.3"
  },
  "main": "/index.js"
}
```

</Sandpack>


---
## Sorun Giderme {/*troubleshooting*/}

### Bir kök oluşturdum, fakat hiçbir şey görüntülenmiyor. {/*ive-created-a-root-but-nothing-is-displayed*/}

Uygulamanızı kök içine gerçekten render etmeyi unutmadığınızdan emin olun:

```js {5}
import { createRoot } from 'react-dom/client';
import App from './App.js';

const root = createRoot(document.getElementById('root'));
root.render(<App />);
```

Bunu yapana kadar hiçbir şey görüntülenmez.

---

### Bir hata alıyorum: "root.render'a ikinci bir argüman geçtiniz" {/*im-getting-an-error-you-passed-a-second-argument-to-root-render*/}

Sık yapılan bir hata, `createRoot` seçeneklerini `root.render(...)` öğesine aktarmaktır:

<ConsoleBlock level="error">

Uyarı: root.render(...) öğesine ikinci bir bağımsız değişken ilettiniz, ancak bu öğe yalnızca bir bağımsız değişken kabul eder.

</ConsoleBlock>

Düzeltmek için, kök seçeneklerini `root.render(...)` yerine `createRoot(...)` öğesine aktarın:
```js {2,5}
// 🚩 Wrong: root.render only takes one argument.
root.render(App, {onUncaughtError});

// ✅ Correct: pass options to createRoot.
const root = createRoot(container, {onUncaughtError}); 
root.render(<App />);
```

---

### Bir hata alıyorum: "Hedef kapsayıcı bir DOM öğesi değil" {/*im-getting-an-error-target-container-is-not-a-dom-element*/}

Bu hata, `createRoot` öğesine aktardığınız şeyin bir DOM düğümü olmadığı anlamına gelir.

Ne olduğundan emin değilseniz, yazdırmayı(log) deneyin:

```js {2}
const domNode = document.getElementById('root');
console.log(domNode); // ???
const root = createRoot(domNode);
root.render(<App />);
```

Örneğin, `domNode` `null` ise, [`getElementById`](https://developer.mozilla.org/en-US/docs/Web/API/Document/getElementById) `null` döndürmüş demektir. Bu, çağrınız sırasında dokümanda verilen kimliğe sahip bir düğüm yoksa gerçekleşir. Bunun birkaç nedeni olabilir:

1. Aradığınız ID, HTML dosyasında kullandığınız ID'den farklı olabilir. Yazım hatalarını kontrol edin!
2. Paketinizin `<script>` etiketi, HTML'de kendisinden *sonra* görünen herhangi bir DOM düğümünü "göremez".

Bu hatayı almanın bir başka yaygın yolu da `createRoot(domNode)` yerine `createRoot(<App />)` yazmaktır.

---

### Bir hata alıyorum: "Fonksiyonlar bir React alt elemanı olarak geçerli değildir." {/*im-getting-an-error-functions-are-not-valid-as-a-react-child*/}

Bu hata, `root.render`a aktardığınız şeyin bir React bileşeni olmadığı anlamına gelir.

Bu, `root.render` öğesini `<Component />` yerine `Component` ile çağırırsanız meydana gelebilir:

```js {2,5}
// 🚩 Yanlış: App bir fonksiyondur, Bileşen değildir.
root.render(App);

// ✅ Doğru: <App /> bir bileşendir.
root.render(<App />);
```

Veya `root.render`'a fonksiyonu çağırmanın sonucu yerine fonksiyonun kendisini iletirseniz:

```js {2,5}
// 🚩 Yanlış: createApp bir fonksiyondur, bileşen değildir.
root.render(createApp);

// ✅ Doğru: Bir bileşen döndürmek için createApp'i çağırın.
root.render(createApp());
```

---

### Sunucu tarafından render edilen HTML'im sıfırdan yeniden oluşturuluyor {/*my-server-rendered-html-gets-re-created-from-scratch*/}

Uygulamanız sunucu tarafından render ediliyorsa ve React tarafından oluşturulan ilk HTML'yi içeriyorsa, bir kök oluşturmanın ve `root.render` çağrısının tüm bu HTML'yi sildiğini ve ardından tüm DOM düğümlerini sıfırdan yeniden oluşturduğunu fark edebilirsiniz. Bu daha yavaş olabilir, odak ve kaydırma konumlarını sıfırlayabilir ve diğer kullanıcı girdilerini kaybedebilir.

Sunucu tarafından render edilen uygulamalar `createRoot` yerine [`hydrateRoot`](/reference/react-dom/client/hydrateRoot) kullanmalıdır:

```js {1,4-7}
import { hydrateRoot } from 'react-dom/client';
import App from './App.js';

hydrateRoot(
  document.getElementById('root'),
  <App />
);
```

API'sinin farklı olduğunu unutmayın. Özellikle, başka bir `root.render` çağrısı genellikle gerçekleşmeyecektir.