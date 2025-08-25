# Modern Standards

Jeśli brakuje Ci odpowiedzi na poniższe pytania:

- Co wykonać przed deployem aplikacji webowej?
- Czy wszystko sprawdziłem przed pokazaniem aplikacji światu?
- Co jest ważne z punktu widzenia aplikacji komercyjnej?
- Co powinna posiadać każda aplikacja?

Myślę, że ten poradnik tyczy się również aplikacji, które już są dostępne
dla użytkowników, a którym to brakuje trochę, aby być jeszcze solidniejszą
wersją samej siebie.

---

## Table of Contents

<!-- To build TOC run: npx markdown-toc -i README.md -->

<!-- toc -->

- [Tools](#tools)
  * [Husky](#husky)
  * [Lint-staged](#lint-staged)
  * [Prettier](#prettier)
  * [TypeScript](#typescript)
  * [ESLint](#eslint)
  * [Utilities](#utilities)
  * [HTTP Request](#http-request)
  * [Changelog](#changelog)
- [Error Handling](#error-handling)
  * [Dane pochodzące od użytkownika](#dane-pochodzace-od-uzytkownika)
  * [Strona z błędem](#strona-z-bledem)
  * [Błędy HTTP: Zgodnie z naturą REST](#bledy-http-zgodnie-z-natura-rest)
  * [Błędy HTTP: Zdefiniowane przez back-end](#bledy-http-zdefiniowane-przez-back-end)
  * [Błędy HTTP: Zniekształcona odpowiedź _(en: Malformed reponses)_](#bledy-http-znieksztalcona-odpowiedz-_en-malformed-reponses_)
- [Backend](#backend)
  * [Local Resources](#local-resources)
  * [Log management (Zbieranie logów)](#log-management-zbieranie-logow)
  * [SSR (Server-Side Rendering)](#ssr-server-side-rendering)
- [Frontend (UI)](#frontend-ui)
  * [React](#react)
  * [Storybook](#storybook)
  * [Forms](#forms)
  * [Concurrent Requests](#concurrent-requests)
  * [HTTP Request Retry](#http-request-retry)
  * [Response Caching (Retention, Clearing via a Button in Settings)](#response-caching-retention-clearing-via-a-button-in-settings)
  * [Critical Path for Application Loading](#critical-path-for-application-loading)
  * [Component Optimization](#component-optimization)
  * [Loader](#loader)
  * [Test Navigation: "Back" Button in the Browser for SPA Applications](#test-navigation-back-button-in-the-browser-for-spa-applications)
- [Tests](#tests)
  * [Offline Connection Tests](#offline-connection-tests)
  * [Performance Tests](#performance-tests)
  * [Tools](#tools-1)
- [Codebase (Technical)](#codebase-technical)
  * [Bootstrap process](#bootstrap-process)
  * [Modules](#modules)
  * [One Place for All Custom Error Types](#one-place-for-all-custom-error-types)
  * [Configuration](#configuration)
  * [Component Events](#component-events)
- [Infrastructure](#infrastructure)
  * [Pipelines](#pipelines)
  * [Test application rollback deployment](#test-application-rollback-deployment)
  * [Release process](#release-process)
- [Services](#services)
  * [Sourcegraph](#sourcegraph)
  * [SonarQube](#sonarqube)
  * [GitHub](#github)

<!-- tocstop -->

---

## Tools

### Husky

- <https://typicode.github.io/husky/>

### Lint-staged

- <https://github.com/okonet/lint-staged>

### Prettier

- <https://prettier.io/>

### TypeScript

- Enable "Strict Mode" in `tsconfig.json`

  ```json
  {
    "compilerOptions": {
      "strict": true
    }
  }
  ```

### ESLint

🕹️ [Playground](https://eslint.org/play/)

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

### Utilities

- [Lodash](https://lodash.com/) - the best is version "lodash-es" because it supports Tree Shaking

### HTTP Request

- [react-query](https://react-query.tanstack.com/)
- [Apollo GraphQL](https://www.apollographql.com/)
- [Axios](https://axios-http.com/) ❌

### Changelog

- [changelog-all-possibilities](https://github.com/piecioshka/changelog-all-possibilities)
- [🇵🇱 Blogpost: husky-commitlint-git-changelog](https://piecioshka.pl/blog/2019/03/23/husky-commitlint-git-changelog.html)

---

## Error Handling

### Dane pochodzące od użytkownika

Zweryfikować:

- Czy wszystko to co wpisał użytkownik do dowolnego pola formularza jest sanityzowane?

💡 TIP:

- Wykorzystać narzędzia:
  - [sanitize-html](https://www.npmjs.com/package/sanitize-html)
  - [escape-html](https://www.npmjs.com/package/escape-html)

### Strona z błędem

Zweryfikować:

- Czy jest zbudowana specjalna strona na błędy?
- Czy przekierowujemy użytkownika na stronę z błędem kiedy nie ma dostępu
  do wyświetlanego zasobu? Przykłady:
  - nie istnieje już dana strona,
  - albo użytkownik chce przejść do strony dla zalogowanych będąc niezalogowanym

### Błędy HTTP: Zgodnie z naturą REST

Zweryfikować:

- Czy obsługujemy problem z pozyskaniem odpowiedzi z uwagi na błąd HTTP?
  - np. gdy wystąpi `HTTP Status 500 - Internal Server Error`

### Błędy HTTP: Zdefiniowane przez back-end

Zweryfikować:

- Czy obsługujemy customowe błędy zdefiniowane w części serwerowej?
  - np. w nie jest znaleziony zasób i w odpowiedzi otrzymujemy JSONa
    z kluczem `error` oraz kodem błędu

💡 TIP:

- Pozyskać wszystkie kody błędów jakie są zdefiniowane po stronie serwera
  aplikacji klienckiej, którą rozwijamy

### Błędy HTTP: Zniekształcona odpowiedź _(en: Malformed reponses)_

Zweryfikować:

- Czy odpowiedź jest w poprawnym formacie

💡 TIP:

- Wykorzystać Output Schemę lub kontrakt Swagerowy, w którym to zdefiniowany
  jest format oczekiwanej odpowiedzi
- Wykorzystać narzędzia:
  - ajv, aby zbudować schemę oczekiwanej odpowiedzi

---

## Backend

### Local Resources

- Change addresses to local ones to avoid leaving the server room.
- Switch the protocol to HTTP to avoid encrypting local requests.

### Log management (Zbieranie logów)

- Use the tool `npm/debug`

### SSR (Server-Side Rendering)

---

## Frontend (UI)

### React

- https://react.dev/

### Storybook

- https://storybook.js.org/

### Forms

- [Formik](https://formik.org/)

### Concurrent Requests

- Define the number of simultaneous requests.

### HTTP Request Retry

### Response Caching (Retention, Clearing via a Button in Settings)

### Critical Path for Application Loading

### Component Optimization

- Load only what is visible, e.g., images, lists.
  - [Tech] Intersection Observer API
- Cancel HTTP requests if they are no longer needed for the component.
  - [Tech] AbortController

### Loader

### Test Navigation: "Back" Button in the Browser for SPA Applications

---

## Tests

### Offline Connection Tests

Steps:

- Disable the internet on the machine where the application is running.

Verify:

- Are we avoiding unnecessary HTTP requests?
- Are we displaying a message about the lack of internet connection?

💡 TIP:

- You can check the internet connection using `navigator.onLine`.

### Performance Tests

Verify:

- How long does it take to handle 100k users?
- How many users can we handle per second?

💡 TIP:

- Use tools like:
  - Apache Benchmark
  - Artillery
  - Locust

### Tools

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
- Code Coverage
  - [nyc](http://npmjs.com/package/nyc)
  - [istanbul](https://istanbul.js.org/) ❌

---

## Codebase (Technical)

### Bootstrap process

- Having one function that starts the application eg. `main()`

### Modules

- ES2015 / AMD / CommonJS
- npm/webpack

### One Place for All Custom Error Types

- Define all errors in a single location to avoid scattering them across the application.
- Example: `src/errors/index.ts`

### Configuration

- https://12factor.net/pl/config
  - Having one place with configuration `config.js`

### Component Events

- Communication between components `npm/super-event-emitter`

---

## Infrastructure

### Pipelines

- UI Error Collector of runtime errors
- Observability
  - Logs
  - Metrics
  - Traces

### Test application rollback deployment

### Release process

- Use any tool for application deployment, e.g. `npm/release-it`

---

## Services

### Sourcegraph

- <https://sourcegraph.com/search>

### SonarQube

- <https://www.sonarsource.com/products/sonarqube/>

### GitHub

- Template for PR - `.github/PULL_REQUEST_TEMPLATE.md`
  - <https://github.com/devspace/awesome-github-templates#rocket-templates-for-pull-requests>
- Template for issues - `.github/ISSUE_TEMPLATE.md`
  - <https://github.com/devspace/awesome-github-templates#bomb-templates-for-issues>
- Contributing rules - `.github/CONTRIBUTING.md`
