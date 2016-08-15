# Jak aktualizować

Niniejszy przewodnik opisuje sposób aktualizacji z wersji przed 1.0 do wersji 1.0.

## Zmiany psujące build

### Globalne zmiany

W wersji 1.0, nie możesz już ładować wszystkich pluginów komendą `ga('require', 'autotrack')`. Zmiana ta została wprowadzona by zapobiec przypadkowemu włączaniu pluginów których nie chcez.

W przyszłości, wszystkie pluginy muszą być ładowane indywidualnie, a ich opcje specyfikowane indywidualnie.

```html
<script>
window.ga=window.ga||function(){(ga.q=ga.q||[]).push(arguments)};ga.l=+new Date;
ga('create', 'UA-XXXXX-Y', 'auto');

// Plugins must be required individually.
ga('require', 'eventTracker');
ga('require', 'outboundLinkTracker');
ga('require', 'urlChangeTracker');
// ...

ga('send', 'pageview');
</script>
<script async src="https://www.google-analytics.com/analytics.js"></script>
<script async src="path/to/autotrack.js"></script>
```

We wszystkich wersjach 1.x, w konsoli będzie logowane ostrzeżenie jeśli użyjesz require `autotrack`. W wersji 2.0, ostrzeżenie to zniknie.

### Indywidualne zmiany w pluginach

#### [`mediaQueryTracker`](/docs/plugins/media-query-tracker.md)

- Opcja `mediaQueryDefinitions` zmieniła nazwę na `definitions`.
- Opcja `mediaQueryChangeTemplate` zmieniła nazwę na `changeTemplate`.
- Opcja `mediaQueryChangeTimeout` zmieniła nazwę na `changeTimeout`.

#### `socialTracker`

- Plugin `socialTracker` zmienił nazwę na [`socialWidgetTracker`](/docs/plugins/social-widget-tracker.md) i już nie wspiera deklaratywnego trackingu interakcji socjalnych (można to teraz robić w całości za pomocą pluginu [`eventTracker`](/docs/plugins/event-tracker.md)).

## Usprawnienia pluginów

### Globalne usprawnienia

- Wszystkie pluginy okceptują -All plugins that send hits accept both [`fieldsObj`](/docs/common-options.md#fieldsobj) and [`hitFilter`](/docs/common-options.md#hitfilter) options. These options can be used to set or change any valid analytics.js field prior to the hit being sent.
- All plugins that send hits as a result of user interaction with a DOM element support [setting field values declaratively](/docs/common-options.md#attributeprefix).

### Individual plugin enhancements

#### [`eventTracker`](/docs/plugins/event-tracker.md)

- Added support for declarative tracking of any DOM event, not just click events (e.g. `submit`, `contextmenu`, etc.)

#### [`outboundFormTracker`](/docs/plugins/outbound-form-tracker.md)

- Added support for tracking forms within shadow DOM subtrees.
- Added the ability to customize the selector used to identify forms.
- Added a `parseUrl` utility function to the `shouldTrackOutboundForm` method to more easily identify or exclude outbound forms.

#### [`outboundLinkTracker`](/docs/plugins/outbound-link-tracker.md)

- Added support for DOM events other than `click` (e.g. `contextmenu`, `touchend`, etc.)
- Added support for tracking links within shadow DOM subtrees.
- Added the ability to customize the selector used to identify links.
- Added a `parseUrl` utility function to the `shouldTrackOutboundLink` method to more easily identify or exclude outbound links.

## New plugins

The following new plugins have been added. See their individual documentation pages for usage details.

- [`cleanUrlTracker`](/docs/plugins/clean-url-tracker.md)
- [`impressionTracker`](/docs/plugins/impression-tracker.md)
- [`pageVisibilityTracker`](/docs/plugins/page-visibility-tracker.md)
