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

- Having one function that starts the application eg. `main()`

## [Backend] Log management

- Use the tool `npm/debug`

## [Infrastructure] Release process

- Use any tool for application deployment, e.g. `npm/release-it`

## [Tech] Modules

- ES2015 / AMD / CommonJS
- npm/webpack

## Configuration

- https://12factor.net/pl/config
  - Having one place with configuration `config.js`

## Component Events

- Communication between components `npm/super-event-emitter`

## GitHub

- Template for PR - `.github/PULL_REQUEST_TEMPLATE.md`
  - https://github.com/devspace/awesome-github-templates#rocket-templates-for-pull-requests
- Template for issues - `.github/ISSUE_TEMPLATE.md`
  - https://github.com/devspace/awesome-github-templates#bomb-templates-for-issues
- Contributing rules - `.github/CONTRIBUTING.md`

## UI

- [React](https://reactjs.org/)
- [Storybook](https://storybook.js.org/)

### Forms

- [Formik](https://formik.org/)

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

```js
'no-unsafe-optional-chaining': 'error',
'prefer-arrow-callback': 'error',
'no-param-reassign': 'error',
'no-extra-boolean-cast': 'error',
```

plugins:

- [eslint-plugin-import-helpers](https://github.com/willhoney7/eslint-plugin-import-helpers)
- [eslint-plugin-import](https://github.com/import-js/eslint-plugin-import)
  - [consistent-type-specifier-style](https://github.com/import-js/eslint-plugin-import/blob/main/docs/rules/consistent-type-specifier-style.md)
    ```js
    'import/consistent-type-specifier-style': [
      'error',
      {
        mode: 'prefer-top-level',
      },
    ],
    ```
  - [enforce-node-protocol-usage](https://github.com/import-js/eslint-plugin-import/blob/main/docs/rules/enforce-node-protocol-usage.md)
    ```js
    'import/enforce-node-protocol-usage': 'error',
    ```
  - [newline-after-import](https://github.com/import-js/eslint-plugin-import/blob/main/docs/rules/newline-after-import.md)
    ```js
    'import/newline-after-import': ["error", { "count": 1 }],
    ```
  - [no-extraneous-dependencies](https://github.com/import-js/eslint-plugin-import/blob/main/docs/rules/no-extraneous-dependencies.md)
    ```js
    'import/no-extraneous-dependencies': [
      'error',
      {
        devDependencies: false,
        optionalDependencies: false,
        peerDependencies: false,
      },
    ],
    ```
  - [no-relative-parent-imports](https://github.com/import-js/eslint-plugin-import/blob/main/docs/rules/no-relative-parent-imports.md)
    ```js
    'import/no-relative-parent-imports': 'error',
    ```
  - [no-self-import](https://github.com/import-js/eslint-plugin-import)
    ```js
    'import/no-self-import': 'error',
    ```
  - [order](https://github.com/import-js/eslint-plugin-import/blob/main/docs/rules/order.md)
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

- [@typescript-eslint/array-type](https://typescript-eslint.io/rules/array-type/)
- [@typescript-eslint/explicit-member-accessibility](https://typescript-eslint.io/rules/explicit-member-accessibility/)
- [@typescript-eslint/no-floating-promises](https://typescript-eslint.io/rules/no-floating-promises/)
- [@typescript-eslint/no-explicit-any](https://typescript-eslint.io/rules/no-explicit-any/)
- [@typescript-eslint/consistent-type-imports](https://typescript-eslint.io/rules/consistent-type-imports/)
  - https://typescript-eslint.io/blog/consistent-type-imports-and-exports-why-and-how/#benefits-of-enforcing-type-only-importsexports

## Other tools

- [Husky](https://typicode.github.io/husky/#/)
- [Lint-staged](https://github.com/okonet/lint-staged)
- [Prettier](https://prettier.io/)

## Utilities

- [Lodash](https://lodash.com/) - the best is version "lodash-es" because it supports Tree Shaking

## HTTP Request

- [react-query](https://react-query.tanstack.com/)
- [Apollo GraphQL](https://www.apollographql.com/)
- [Axios](https://axios-http.com/) ❌

## Tests

- Unit Tests
  - [Jest](https://jestjs.io/docs/testing-frameworks)
- Component Tests
  - [RTL (React Testing Library)](https://testing-library.com/docs/react-testing-library/intro/)
  - [Enzyme](https://enzymejs.github.io/enzyme/) ❌
- End to End Tests
  - [Cucumber.js](https://cucumber.io/docs/guides/overview/)
  - [Playwright](https://playwright.dev/)
  - [Cypress](https://www.cypress.io/) ❌
  - [WebDriver.io](https://webdriver.io/)

## Code Coverage

- [nyc](http://npmjs.com/package/nyc)
- [istanbul](https://istanbul.js.org/) ❌

## Pipelines

- UI Error Collector of runtime errors
- Observability
  - Logs
  - Metrics
  - Traces

## Changelog

- https://github.com/piecioshka/changelog-all-possibilities
- https://piecioshka.pl/blog/2019/03/23/husky-commitlint-git-changelog.html

## Bonus Services

- [Sourcegraph](https://sourcegraph.com/search)
- [SonarQube](https://www.sonarsource.com/products/sonarqube/)
