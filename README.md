# Autotrack [![Build Status](https://travis-ci.org/googleanalytics/autotrack.svg?branch=master)](https://travis-ci.org/googleanalytics/autotrack)

- [Przegląd](#przegląd)
- [Pluginy](#pluginy)
- [Instalacja i używanie](#instalacja-i-używanie)
  - [Ładowanie autotracka za pomocą npm](#ładowanie-autotracka-za-pomocą-npm)
  - [Przekazywanie opcji konfiguracji](#przekazywanie-opcji-konfiguracji)
- [Zaawansowana konfiguracja](#zaawansowana-konfiguracja)
  - [Własna konfiguracja](#własna-konfiguracja)
  - [Używanie autotracka z wieloma trackerami](#używanie-autotracka-z-wieloma-trackerami)
- [Wsparcie przeglądarek](#wsparcie-przeglądarek)
- [Tłumaczenia](#tłumaczenia)

## Przegląd

Domyślny [kod śledzący JavaScript](https://developers.google.com/analytics/devguides/collection/analyticsjs/) Google Analytics działa gdy strona jest po raz pierwszy ładowana i wysyła odsłonę do Google Analytics. Jeśli chcesz wiedzieć więcej niż tylko o odsłonach strony (np. zdarzenia, interakcje socjalne), musisz napisać kod przechwytujący te informacje samodzielnie.

Skoro większość właścicieli stron troszczy się o wiele podobnych typów interakcji użytkownika, web developerzy piszą w kółko ten sam kod dla każdej strony którą tworzą.

Autotrack został stworzony by rozwiązać ten problem. Dostarcza domyślne śledzenie interakcji o które troszczy się większość osób, oraz kilka wygodnych funkcjonalności (np. śledzenie zadelarowanych zdarzeń) by jeszcze bardziej uprościć zrozumienie tego, jak ludzie korzystają z twojej witryny.

## Pluginy

Biblioteka `autotrack.js` jest mała (6K gzip), oraz zawiera wymienione pluginy. Domyślnie wszystkie pluginy są dostarczane razem, ale mogą być również dołączane i konfigurowane niezależnie. Niniejsza tabela zawiera krótki opis każdego pluginu; możesz kliknąć na nazwę pluginu by zobaczyć jego pełną dokumentację oraz instrukcję użycia.

<table>
  <tr>
    <th align="left">Plugin</th>
    <th align="left">Opis</th>
  </tr>
  <tr>
    <td><a href="/docs/plugins/clean-url-tracker.md"><code>cleanUrlTracker</code></a></td>
    <td>Zapewnia spójność w ścieżkach URL raportowanych do Google Analytics; omija problem gdzie osobne wiersze w raportach twoich stron wskazują tą samą stronę.</td>
  </tr>
  <tr>
    <td><a href="/docs/plugins/event-tracker.md"><code>eventTracker</code></a></td>
    <td>Umożliwia śledzenie zadeklarowanych zdarzeń, za pomocą atrybutów HTML.</td>
  </tr>
  <tr>
    <td><a href="/docs/plugins/impression-tracker.md"><code>impressionTracker</code></a></td>
    <td>Pozwala Tobie na śledzenie kiedy elementy są widoczne w oknie przeglądarki (viewport).</td>
  </tr>
  <tr>
    <td><a href="/docs/plugins/media-query-tracker.md"><code>mediaQueryTracker</code></a></td>
    <td>Umożliwia śledzenie pasujących media query oraz zmian media query.</td>
  </tr>
  <tr>
    <td><a href="/docs/plugins/outbound-form-tracker.md"><code>outboundFormTracker</code></a></td>
    <td>Automatycznie śledzi wysyłanie formularzy do zewnętrznych domen.</td>
  </tr>
  <tr>
    <td><a href="/docs/plugins/outbound-link-tracker.md"><code>outboundLinkTracker</code></a></td>
    <td>Automatycznie śledzi kliknięcia linków do zewnętrznych domen.</td>
  </tr>
  <tr>
    <td><a href="/docs/plugins/page-visibility-tracker.md"><code>pageVisibilityTracker</code></a></td>
    <td>Śledzi zmiany wyświetlania strony, co daje dużo bardziej dokładne informacje o sesji, czasie trwania sesji oraz dokładniejsze pomiary odsłon.</td>
  </tr>
  <tr>
    <td><a href="/docs/plugins/social-widget-tracker.md"><code>socialWidgetTracker</code></a></td>
    <td>Automatycznie śledzi interakcję użytkownika z oficjalnymi widgetami Facebooka i Twittera.</td>
  </tr>
  <tr>
    <td><a href="/docs/plugins/url-change-tracker.md"><code>urlChangeTracker</code></a></td>
    <td>Automatycznie śledzi zmiany URL dla aplikacji składających się z jednej strony (SPA).</td>
  </tr>
</table>

**Wyłączenie odpowiedzialności:** autotrack jest utrzymywany przez członków platformy developerskiej Google Analytics i jest przede wszystkim kierowany do społeczności developerów. Nie jest to oficjalny produkt Google Analytics i nie kwalifikuje się do wsparcia Google Analytics 360. Developerzy używający tej biblioteki są odpowiedzialni za zapewnienie aby ich implementacja spełniała wymogi [Warunków świadczenia usług Google Analytics](https://www.google.com/analytics/terms/us.html) oraz zobowiązania prawne ich kraju zamieszkania.

## Instalacja i używanie

Aby dodać autotrack do Twojej strony, musisz zrobić dwie rzeczy:

1. Załaduj skrypt `autotrack.js` na swojej stronie.
2. Zaktualizuj swój [kod śledzący](https://developers.google.com/analytics/devguides/collection/analyticsjs/tracking-snippet-reference) by [wymagał](https://developers.google.com/analytics/devguides/collection/analyticsjs/using-plugins) pluginy autotracka których chcesz użyć.

Jeśli twoja strona już zawiera domyślny kod śledzący JavaScript, możesz go zmodyfiować by wyglądał podobnie do tego:

```html
<script>
window.ga=window.ga||function(){(ga.q=ga.q||[]).push(arguments)};ga.l=+new Date;
ga('create', 'UA-XXXXX-Y', 'auto');

// Replace the following lines with the plugins you want to use.
ga('require', 'eventTracker');
ga('require', 'outboundLinkTracker');
ga('require', 'urlChangeTracker');
// ...

ga('send', 'pageview');
</script>
<script async src='https://www.google-analytics.com/analytics.js'></script>
<script async src='path/to/autotrack.js'></script>
```

Oczywiście będziesz musiał dokonać następujących zmian by dopasować autotrack do swoich potrzeb:

- Zamień `UA-XXXXX-Y` na twój [tracking ID](https://support.google.com/analytics/answer/1032385)
- Zamień przykładową listę pluginów w wyrażeniu `require` pluginami których chcesz użyć.
- Zamień `path/to/autotrack.js` aktualną lokalizacją pliku `autotrack.js` hostowanego na twoim serwerze.

**Nota:** [system pluginów analytics.js](https://developers.google.com/analytics/devguides/collection/analyticsjs/using-plugins) został stworzony by wspierać asynchroniczne ładowanie skryptów, więc nie ma znaczenia czy `autotrack.js` jest ładowany przed czy po `analytics.js`. Nie ma także znaczenia czy biblioteka `autotrack.js` jest ładowana samodzielnie czy też wraz z resztą Twojego kodu JavaScript.

### Ładowanie autotracka za pomocą npm

Jeśli używasz npm wraz z loaderem modułów takim jak [Browserify](http://browserify.org/), [Webpack](https://webpack.github.io/) lub [SystemJS](https://github.com/systemjs/systemjs), możesz dołączyć autotracka w swoim projekcie w taki sam sposób jak każdy inny moduł npm:

```sh
npm install autotrack
```

```js
// In your JavaScript code
require('autotrack');
```

Powyższy kod dołączy wszystkie pluginy autotracka do generowanego pliku źródłowego. Jeśli chcesz dołączyć wybrany zestaw pluginów, możesz dołączyć je indywidualnie:

```js
// In your JavaScript code
require('autotrack/lib/plugins/clean-url-tracker');
require('autotrack/lib/plugins/outbound-link-tracker');
require('autotrack/lib/plugins/url-change-tracker');
// ...
```

Powyższe przykłady pokazują jak dołączyć kod źródłowy pluginów do Twojego finalnego, wygenerowanego pliku JavaScript, co wypełnia pierwszy z dwóch kroków procesu instalacji.

Wciąż musisz zaktualizować swój kod śledzący i dołączyć pluginy których chcesz użyć:


```js
// In the analytics.js tracking snippet
ga('create', 'UA-XXXXX-Y', 'auto');

// Replace the following lines with the plugins you want to use.
ga('require', 'cleanUrlTracker');
ga('require', 'outboundLinkTracker');
ga('require', 'urlChangeTracker');
// ...

ga('send', 'pageview');
```

**Nota:** uważaj by nie pomylić wyrażenia [`require`](https://nodejs.org/api/modules.html) w module node z komendą [`require`](https://developers.google.com/analytics/devguides/collection/analyticsjs/command-queue-reference#require) ze skryptu `analytics.js`. Gdy ładujesz autotrack wraz z loaderem modułów npm, oba muszą zostać użyte.

### Przekazywanie opcji konfiguracji

Wszystkie pluginy autotracka przyjmują obiekt konfiguracyjny jako trzeci parametr komendy `require`.

Niektóre pluginy (np. `outboundLinkTracker`, `socialWidgetTracker`, `urlChangeTracker`) zachowują się w sposób domyślny wystarczający dla większości osób bez specyfiowania żadnych opcji konfiguracyjnych. Pozostałe pluginy (np. `cleanUrlTracker`, `impressionTracker`, `mediaQueryTracker`) wymagają ustawienia pewnych opcji konfiguracyjnych by mogły pracować.

Sprawdź dokumentację konkretnego pluginu by dowiedzieć się jakie opcje akceptuje dany plugin (oraz czy ma ustawioną wartość domyślną).

## Zaawansowana konfiguracja

### Własna konfiguracja

Biblioteka autotrack jest zbudowana modułowo i każdy plugin zawiera swoje własne zależności, możesz więc zbudować swoją własną wercję biblioteki używając bundlera skryptów takiego jak [Browserify](http://browserify.org/).

Następujący przykład pokazuje jak stworzyć bibliotekę która zawiera tylko pluginy `eventTracker` oraz `outboundLinkTracker`:

```sh
browserify lib/plugins/event-tracker lib/plugins/outbound-link-tracker
```

Tworząc swoją własną kompilację, upewnij się by zaktualizować swój kod śledzący aby wymagał tylko pluginy zawarte w Twojej kompilacji. Wymaganie pluginu który nie jest zawarty w Twojej kompilacji stworzy niezaspokojoną zależność, co uniemożliwi egzekucję następnych komend.

Jeśli już używasz loadera modułów jak [Browserify](http://browserify.org/), [Webpack](https://webpack.github.io/) lub [SystemJS](https://github.com/systemjs/systemjs) do budowy swojego kodu JavaScript, możesz pominąć powyższy krok i po prostu wymagać pluginów jak opisane w sekcji [ładowanie autotracka za pomocą npm](#ładowanie-autotracka-za-pomocą-npm).

### Używanie autotracka z wieloma trackerami

Wszystkie pluginy autotracka wspierają obsługę wielu trackerów na raz co wymaga nadania trackerowi nazwy w komendzie `require`. Następujący przykład pokazuje jak stworzyć dwa trackery i wymagać różnych pluginów autotracka z każdym z nich.

```js
// Creates two trackers, one named `tracker1` and one named `tracker2`.
ga('create', 'UA-XXXXX-Y', 'auto', 'tracker1');
ga('create', 'UA-XXXXX-Z', 'auto', 'tracker2');

// Requires plugins on tracker1.
ga('tracker1.require', 'eventTracker');
ga('tracker1.require', 'socialWidgetTracker');

// Requires plugins on tracker2.
ga('tracker2.require', 'eventTracker');
ga('tracker2.require', 'outboundLinkTracker');
ga('tracker2.require', 'pageVisibilityTracker');

// Sends the initial pageview for each tracker.
ga('tracker1.send', 'pageview');
ga('tracker2.send', 'pageview');
```

## Wsparcie przeglądarek

Autotrack działa bezpiecznie w każdej przeglądarce bez wyrzucania błędów, gdyż wykrywanie obsługi funkcjonalności jest zawsze używane wraz z potencjalnie nieobsługiwanym kodem. Jednakże, autotrack będzie śledził tylko używając funkcjonalności przeglądarki na której w danym momencie działa. Na przykład, jeśli użytkownik korzysta z przeglądarki Internet Explorer 8, nie będzie można śledzić użycia media query, gdyż media query nie jest wspierane przez program Internet Explorer 8.

Wszystkie pluginy autotracka są [testowane przy użyciu Sauce Labs](https://saucelabs.com/u/autotrack) w następujących przeglądarkach:

<table>
  <tr>
    <td align="center">
      <img src="https://raw.github.com/alrra/browser-logos/master/chrome/chrome_48x48.png" alt="Chrome"><br>
      ✔
    </td>
    <td align="center">
      <img src="https://raw.github.com/alrra/browser-logos/master/firefox/firefox_48x48.png" alt="Firefox"><br>
      ✔
    </td>
    <td align="center">
      <img src="https://raw.github.com/alrra/browser-logos/master/safari/safari_48x48.png" alt="Safari"><br>
      6+
    </td>
    <td align="center">
      <img src="https://raw.github.com/alrra/browser-logos/master/edge/edge_48x48.png" alt="Edge"><br>
      ✔
    </td>
    <td align="center">
      <img src="https://raw.github.com/alrra/browser-logos/master/internet-explorer/internet-explorer_48x48.png" alt="Internet Explorer"><br>
      9+
    </td>
    <td align="center">
      <img src="https://raw.github.com/alrra/browser-logos/master/opera/opera_48x48.png" alt="Opera"><br>
      ✔
    </td>
  </tr>
</table>

## Tłumaczenia

Następujące tłumaczenia zostały stworzone dzięki życzliwości społeczności. Są one nieoficjalne i mogą być niedokładne bądź przestarzałe:

* [Japoński](https://github.com/nebosuker/autotrack)
* [Chiński](https://github.com/stevezhuang/autotrack/blob/master/README.zh.md)
* [Polski](https://github.com/krisu7/autotrack)

Jeśli odkryjesz problemy z konkretnym tłumaczeniem, uprzejmie prosimy kierować je do konkretnego repozytorium. W celu stworzenia swojego własnego tłumaczenia, postępuj następująco:

1. Stwórz fork tego repozytorium.
2. Zaktualizuj ustawienia swojego forka by [umożliwić zgłaszanie problemów](http://programmers.stackexchange.com/questions/179468/forking-a-repo-on-github-but-allowing-new-issues-on-the-fork).
3. Usuń wszystkie pliki nie związane z dokumentacją.
4. Zaktualizuj pliki doumentacji swoimi tłumaczeniami.
5. Wyślij pull request do repozytorium [googleanalytics/autotrack](https://github.com/googleanalytics/autotrack) który dodaje link do twojego forka do powyższej listy.
