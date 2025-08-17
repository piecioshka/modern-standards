# Modern Standards

Jeśli brakuje Ci odpowiedzi na poniższe pytania:

- Co wykonać przed deployem aplikacji webowej?
- Czy wszystko sprawdziłem przed pokazaniem aplikacji światu?
- Co jest ważne z punktu widzenia aplikacji komercyjnej?
- Co powinna posiadać każda aplikacja?

Myślę, że ten poradnik tyczy się również aplikacji, które już są dostępne
dla użytkowników, a którym to brakuje trochę, aby być jeszcze solidniejszą
wersją samej siebie.

## 1. Testy

### 1.1. Testy obsługi braku połączenia z internetem

Kroki:

- Wyłączyć internet na maszynie, gdzie jest uruchomiona aplikacja

[i] Zweryfikować:

- Czy nie robimy niepotrzebnych zapytań HTTP?
- Czy wyświetlamy komunikat o braku połączenia internetowego?

[✅] Wskazówki:

- Sprawdzenie połączenie z internetem można wykonać za pomocą `navigator.onLine`

### 1.2. Testy wydajnościowe

[i] Zweryfikować:

- W jakim czasie "obsłużymy" 100k użytkowników?
- Ile użytkowników jesteśmy w stanie obsłużyć w ciągu jednej sekundy?

[✅] Wskazówki:

- Wykorzystać narzędzia:
  - Apache Benchmark
  - Artillery
  - Locust

---

## 2. Error Handling

### 2.1. Dane pochodzące od użytkownika

[i] Zweryfikować:

- Czy wszystko to co wpisał użytkownik do dowolnego pola formularza jest sanityzowane?

[✅] Wskazówki:

- Wykorzystać narzędzia:
  - https://www.npmjs.com/package/sanitize-html
  - https://www.npmjs.com/package/escape-html

### 2.2. Strona z błędem

[i] Zweryfikować:

- Czy jest zbudowana specjalna strona na błędy?
- Czy przekierowujemy użytkownika na stronę z błędem kiedy nie ma dostępu
  do wyświetlanego zasobu? Przykłady:
  - nie istnieje już dana strona,
  - albo użytkownik chce przejść do strony dla zalogowanych będąc niezalogowanym

### 2.3. Błędy HTTP: Zgodnie z naturą REST

[i] Zweryfikować:

- Czy obsługujemy problem z pozyskaniem odpowiedzi z uwagi na błąd HTTP?
  - np. gdy wystąpi `HTTP Status 500 - Internal Server Error`

### 2.5. Błędy HTTP: Zdefiniowane przez back-end

[i] Zweryfikować:

- Czy obsługujemy customowe błędy zdefiniowane w części serwerowej?
  - np. w nie jest znaleziony zasób i w odpowiedzi otrzymujemy JSONa
    z kluczem `error` oraz kodem błędu

[✅] Wskazówki:

- Pozyskać wszystkie kody błędów jakie są zdefiniowane po stronie serwera
  aplikacji klienckiej, którą rozwijamy

### 2.6. Błędy HTTP: Zniekształcona odpowiedź _(en: Malformed reponses)_

[i] Zweryfikować:

- Czy odpowiedź jest w poprawnym formacie

[✅] Wskazówki:

- Wykorzystać Output Schemę lub kontrakt Swagerowy, w którym to zdefiniowany
  jest format oczekiwanej odpowiedzi
- Wykorzystać narzędzia:
  - ajv, aby zbudować schemę oczekiwanej odpowiedzi

### [Codebase] Jedno miejsce ze wszystkimi typami customowych błędów

- Zdefiniować wszystkie błędy w jednym miejscu, aby nie były rozproszone
  po całej aplikacji
- Przykład: `src/errors/index.ts`

---

## 3. jednoczesne requesty

- zdefiniowanie liczby jednoczesnych zapytań

## [Frontend] Ponawianie zapytania HTTP

## [Frontend] Cache responsów (retencja, czyszczenie przyciskiem w ustawieniach)

## [Frontend] Ścieżka krytyczna ładowania aplikacji

## [Backend] lokalne zasoby

- zmienić adresy na lokalne, aby nie wychodziły poza serwerownie
- zmienić protokół na HTTP, aby nie szyfrować lokalnych requestów

## [Frontend] Optymalizacja komponentów

- nie ładowanie wszystkiego, tylko to, co jest widoczne np. obrazki, listy
  - [Tech] Intersection Observer API
- przerywać zapytania HTTP jeśli już nie są potrzebne dla komponentu
  - [Tech] AbortController

## [Frontend] Loader

## [Frontend] Przetestować nawigację: przycisk "Wstecz" w przeglądarce dla aplikacji SPA

## [Frontend] Zbieranie logów

## [Frontend] SSR

## [Infrastructure] Przetestować cofanie deployu aplikacji - tzw. rollback

## [Tech] Bootstrap process

- posiadanie jednej funkcji, która startuje aplikację:
- [Tech] `main()`

## [Backend] Log management

- Wykorzystać paczkę `npm/debug`

## [Infrastructure] Release process

- Wykorzystać dowolne narzędzie do publikacji aplikacji np. `npm/release-it`

## [Tech] Modules

- ES2015 / AMD / CommonJS
- npm/webpack

## Configuration

- Posiadanie jednego miejsca z konfiguracją `config.js`

## Component Events

- Komunikacja między komponentami `npm/super-event-emitter`

## GitHub

- .github/PULL_REQUEST_TEMPLATE.md

## UI

- React

### Forms

- Formik

## Storybook

- [Talk] How to work on components with Storybook? Tips & Tricks

## TypeScript

- Enable "Strict Mode" in `tsconfig.json`

  ```json
  {
    "compilerOptions": {
      "strict": true
    }
  }
  ```

## ESLint ([playground](https://eslint.org/play/))

- import/no-relative-parent-imports
- `no-unsafe-optional-chaining: 'error'`
- `prefer-arrow-callback: 'error'`
- import/no-extraneous-dependencies

plugins:

- [eslint-plugin-import-helpers](https://github.com/willhoney7/eslint-plugin-import-helpers)
- [eslint-plugin-import](https://github.com/import-js/eslint-plugin-import)
  ```js
  'import/order': [
    'error',
    {
      groups: [
        'builtin',
        'external',
        'internal',
        'unknown',
        'parent',
        'sibling',
        'index',
        'object',
        'type',
      ],
      'newlines-between': 'always',
    },
  ],
  ```
- [eslint-plugin-no-relative-import-paths](https://github.com/MelvinVermeer/eslint-plugin-no-relative-import-paths)
- [eslint-plugin-react](https://github.com/jsx-eslint/eslint-plugin-react)
- [eslint-plugin-regexp](https://github.com/ota-meshi/eslint-plugin-regexp)
- [eslint-plugin-smells](https://github.com/elijahmanor/eslint-plugin-smells)
- [eslint-plugin-unicorn](https://github.com/sindresorhus/eslint-plugin-unicorn)
- [eslint-plugin-unused-imports](https://github.com/sweepline/eslint-plugin-unused-imports)

TypeScript plugins:

- [@typescript-eslint/no-floating-promises](https://typescript-eslint.io/rules/no-floating-promises/)
- [@typescript-eslint/no-explicit-any](https://typescript-eslint.io/rules/no-explicit-any/)
- [@typescript-eslint/consistent-type-imports](https://typescript-eslint.io/rules/consistent-type-imports/)
  - https://typescript-eslint.io/blog/consistent-type-imports-and-exports-why-and-how/#benefits-of-enforcing-type-only-importsexports

## Other tools

- Husky
- Lint-staged
- Prettier

## Utilities

- Lodash - the best is version "lodash-es" because it supports Tree Shaking

## HTTP Request

- react-query
- Apollo GraphQL
- axios ❌

## Tests

- Unit Tests
  - jest
- Component Tests
  - RTL (React Testing Library)
  - Enzyme ❌
- End to End Tests
  - Cucumber.js
  - Playwright
  - Cypress ❌
  - WebDriver.io

## Code Coverage

- nyc
- istanbul ❌

## Pipelines

- UI Error Collector of runtime errors
- Observability
  - Logs
  - Metrics
  - Traces

## Bonus Services

- Sourcegraph
- SonarQube
