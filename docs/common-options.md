# Typowe ustawienia

Wiele pluginów autotracka przyjmuje opcje które są powszechne dla wielu różnych pluginów. Są one udokumentowane w tym przewodniku:

- [`fieldsObj`](#fieldsobj)
- [`attributePrefix`](#attributeprefix)
- [`hitFilter`](#hitfilter)

## `fieldsObj`

Niektóre pluginy autotracka wysyłają odsłony używając domyślnego zbioru [wartości pól biblioteki analytics.js](https://developers.google.com/analytics/devguides/collection/analyticsjs/field-reference). Pluginy te akceptują opcję `fieldsObj`, która pozwala na dostosowanie wartości dla każdego pluginu. Pozwala także na ustawienie pól które nie są ustawione domyślnie.

Opcja `fieldsObj` jest typu `Object` którego właściwości mogą przyjmować [nazwy pól biblioteki analytics.js](https://developers.google.com/analytics/devguides/collection/analyticsjs/field-reference), i którego wartości zostaną użyte jako odpowiadające wartości pól dla wszystkich odsłon wysyłanych przez plugin.

### Przykłady

#### `mediaQueryTracker`

Konfiguracja ta zapewnia że wszystkie wydarzenia wysyłane przez plugin `mediaQueryTracker` są zdarzeniami bez interakcji:

```js
ga('require', 'mediaQueryTracker', {
  definitions: [...],
  fieldsObj: {
    nonInteraction: true
  }
});
```

#### `urlChangeTracker`

Konfiguracja ta ustawia [custom dimension](https://support.google.com/analytics/answer/2709828) na indeks 1 dla wszystkich odsłon wysłanych przez `urlChangeTracker`. Pozwala to na rozróżnienie początkowych odsłon od "wirtualnych" odsłon wysyłanych zaraz po załadowaniu nowych stron poprzez AJAX:

```js
ga('require', 'urlChangeTracker', {
  fieldsObj: {
    dimension1: 'virtual'
  }
});
ga('send', 'pageview', {
  dimension1: 'pageload'
});
```

## `attributePrefix`

Wszystkie pluginy które wysyłają dane do Google Analyticsjako rezultat interakcji użytkownika z elementami DOM wspierają deklaratywne wiązania atrybutów (sprawdź plugin [`eventTracker`](/docs/plugins/event-tracker.md) by zobaczyć jak to działa). A więc każdy z tych pluginów akceptuje opcję `attributePrefix` pozwalającą na dostosowanie, jakiego prefiksu użyć wraz z atrybutem.

Domyślnie, wartość `attributePrefix` używana przez każdy plugin jest stringiem `'ga-'`, jednakże wartość ta może zostać dostosowana dla każdego pluginu z osobna.

**Nota:** używając tych samych pól w opcji `fieldsObj` a także jako atrybutów elementów DOM, wartości atrybutów nadpiszą wartości `fieldsObj`.

### Przykłady

#### `eventTracker`

```js
ga('require', 'eventTracker', {
  attributePrefix: 'data-'
});
```

```html
<button
  data-on="click"
  data-event-category="Wideo"
  data-event-action="odtwórz">
  Odtwórz wideo
</button>
```

#### `impressionTracker`

```js
ga('require', 'impressionTracker', {
  elements: ['cta'],
  attributePrefix: 'data-ga'
});
```

```html
<div
  id="cta"
  data-ga-event-category="Wywołaj"
  data-ga-event-action="widziano">
  Wywołaj
</a>
```

#### `outboundLinkTracker`

```js
ga('require', 'outboundLinkTracker', {
  attributePrefix: ''
});
```

```html
<a href="https://example.com" event-category="External Link">Kliknij</a>
```

## `hitFilter`

Opcja `hitFilter` jest użyteczna kiedy potrzebujesz dokonać bardziej zaawansowanych zmian odsłon, lub gdy chcesz porzucić odsłonę w całości. `hitFilter` jest funkcją która przyjmuje [model object](https://developers.google.com/analytics/devguides/collection/analyticsjs/model-object-reference) trackera jako pierwszy argument oraz (jeśli odsłona została zainicjowana poprzez interakcję użytkownika z elementem DOM) element DOM jako drugi argument.

W obrębie funcji `hitFilter` możesz otrzymać wartość każdego pola modelu obiektu używając metody [`get`](https://developers.google.com/analytics/devguides/collection/analyticsjs/model-object-reference#get) na argumencie `model`u. Możesz także ustawić nową wartość używając metody [`set`](https://developers.google.com/analytics/devguides/collection/analyticsjs/model-object-reference#set) na argumencie `model`u. Aby porzucić odsłonę, wyrzuć błąd.

By zmodyfikować model tylko aktualnej odsłony (bez kolejnych odsłon), ustaw trzeci argument ([`temporary`](https://developers.google.com/analytics/devguides/collection/analyticsjs/model-object-reference#set)) na `true`.

### Jak to działa

Opcja `hitFilter` działa przez nadpisywanie [`buildHitTask`](https://developers.google.com/analytics/devguides/collection/analyticsjs/tasks) trackera. Przekazana funkcja `hitFilter` jest wywoływana po tym jak wartości i pola atrybutów `fieldsObj` zostały ustawione na trackerze jednak przed wywołaniem oryginalnego `buildHitTask`. Patrz [zadania biblioteki analytics.js](https://developers.google.com/analytics/devguides/collection/analyticsjs/tasks) by dowiedzieć się więcej.

### Przykłady

#### `pageVisibilityTracker`

Konfiguracja ta ustawia custom dimension 1 na cokolwiek ustawione jest pole `eventValue` dla wszystkich zdarzeń `visibilitychange`. Używa ona `true` jako trzeci argument metody `set`, dzieki czemu zmiana ta dotyczy tylko aktualnej odsłony:

```js
ga('require', 'pageVisibilityTracker', {
  hitFilter: function(model) {
    model.set('dimension1', String(model.get('eventValue')), true);
  }
});
```

#### `impressionTracker`

Konfiguracja ta zapobiega wysyłaniu impresji na elementach z ustawioną klasą `is-invisible`.

```js
ga('require', 'impressionTracker', {
  hitFilter: function(model, element) {
    if (element.className.indexOf('is-invisible') > -1) {
      throw new Error('Aborting hit');
    }
  }
});
```
