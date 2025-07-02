# Modern Standards

---
published: false
layout: regular-page
title: "Solidna Aplikacja Webowa. Poradnik"
image: "/assets/guides/solid-web-apps/poradnik-solidnej-aplikacji-webowej-1920x1080.png"
permalink: /poradnik-solidnej-aplikacji-webowej/
---

# {{ page.title }}

Jeśli brakuje Ci odpowiedzi na poniższe pytania:

* Co wykonać przed deployem aplikacji webowej?
* Czy wszystko sprawdziłem przed pokazaniem aplikacji światu?
* Co jest ważne z punktu widzenia aplikacji komercyjnej?
* Co powinna posiadać każda aplikacja?

To w tym poradniku postaram się odpowiedzieć na wyżej wymienione pytania.

<figure>
    <img src="{{ page.image }}" itemprop="image" alt="{{ page.title }}"/>
</figure>

Myślę, że ten poradnik tyczy się również aplikacji, które już są dostępne
dla użytkowników, a którym to brakuje trochę, aby być jeszcze solidniejszą
wersją samej siebie.

Zapraszam do zapoznania się listy punktów, którymi realizuje
**Solidna Aplikacja Webowa**!

## 1. Testy

### 1.1. Testy obsługi braku połączenia z internetem

Kroki:

* Wyłączyć internet na maszynie, gdzie jest uruchomiona aplikacja

<i class="icon-average"></i> Zweryfikować:

* Czy nie robimy niepotrzebnych zapytań HTTP?
* Czy wyświetlamy komunikat o braku połączenia internetowego?

<i class="icon-good"></i> Wskazówki:

* Sprawdzenie połączenie z internetem można wykonać za pomocą `navigator.onLine`

### 1.2. Testy wydajnościowe

<i class="icon-average"></i> Zweryfikować:

* W jakim czasie "obsłużymy" 100k użytkowników?
* Ile użytkowników jesteśmy w stanie obsłużyć w ciągu jednej sekundy?

<i class="icon-good"></i> Wskazówki:

* Wykorzystać narzędzia:
    + Apache Benchmark
    + Artillery
    + Locust

---

## 2. Error Handling

### 2.1. Dane pochodzące od użytkownika

<i class="icon-average"></i> Zweryfikować:

* Czy wszystko to co wpisał użytkownik do dowolnego pola formularza jest sanityzowane?

<i class="icon-good"></i> Wskazówki:

* Wykorzystać narzędzia:
    + https://www.npmjs.com/package/sanitize-html
    + https://www.npmjs.com/package/escape-html

### 2.2. Strona z błędem

<i class="icon-average"></i> Zweryfikować:

* Czy jest zbudowana specjalna strona na błędy?
* Czy przekierowujemy użytkownika na stronę z błędem kiedy nie ma dostępu
    do wyświetlanego zasobu? Przykłady:
    + nie istnieje już dana strona,
    + albo użytkownik chce przejść do strony dla zalogowanych będąc niezalogowanym

### 2.3. Błędy HTTP: Zgodnie z naturą REST

<i class="icon-average"></i> Zweryfikować:

* Czy obsługujemy problem z pozyskaniem odpowiedzi z uwagi na błąd HTTP?
    + np. gdy wystąpi `HTTP Status 500 - Internal Server Error`

### 2.5. Błędy HTTP: Zdefiniowane przez back-end

<i class="icon-average"></i> Zweryfikować:

* Czy obsługujemy customowe błędy zdefiniowane w części serwerowej?
    + np. w nie jest znaleziony zasób i w odpowiedzi otrzymujemy JSONa
        z kluczem `error` oraz kodem błędu

<i class="icon-good"></i> Wskazówki:

* Pozyskać wszystkie kody błędów jakie są zdefiniowane po stronie serwera
    aplikacji klienckiej, którą rozwijamy

### 2.6. Błędy HTTP: Zniekształcona odpowiedź _(en: Malformed reponses)_

<i class="icon-average"></i> Zweryfikować:

* Czy odpowiedź jest w poprawnym formacie

<i class="icon-good"></i> Wskazówki:

* Wykorzystać Output Schemę lub kontrakt Swagerowy, w którym to zdefiniowany
    jest format oczekiwanej odpowiedzi
* Wykorzystać narzędzia:
    + ajv, aby zbudować schemę oczekiwanej odpowiedzi

---

## 3. jednoczesne requesty

+ zdefiniowanie liczby jednoczesnych zapytań

## [ ] ponawianie zapytania HTTP

## [ ] cache responsów (retencja, czyszczenie przyciskiem w ustawieniach)

## [ ] Ścieżka krytyczna ładowania aplikacji

## [ ] lokalne zasoby

+ zmienić adresy na lokalne, aby nie wychodziły po serwerownie
+ zmienić protokół na HTTP, aby nie szyfrować lokalnych requestów

## [ ] optymalizacja komponentów

+ nie ładowanie wszystkiego, tylko to, co jest widoczne
    - obrazki
    - listy
+ przerywać zapytania HTTP jeśli już nie są potrzebne dla komponentu

## [ ] loader

## [ ] przetestować nawigację: przycisk "Wstecz" w przeglądarce dla aplikacji SPA

## [ ] zbieranie logów

## [ ] SSR

## [ ] przetestować cofanie deployu aplikacji - tzw. rollback

- Bootstrap process
  - main()
- Log management
  - npm/debug
- Release process
  - npm/release-it
- Modules
  - ES2015 / AMD / CommonJS
  - npm/webpack
- Configuration
  - config.js
- Component Events
  - npm/super-event-emitter
- Tests
  - units / integrations / system
  - end-to-end

---


- GitHub
  - .github/PULL_REQUEST_TEMPLATE.md
- UI
  - React
- Forms
  - Formik
- Storybook
  - [Talk] How to work on components with Storybook? Tips & Tricks
- Types
  - TypeScript
    - Enable "Strict Mode" in tsconfig.json
      ```json
      {
        "compilerOptions": {
          "strict": true
        }
      }
      ```
  - Flow ❌
    - Enable "Strict Mode" in .flowconfig
      ```ini
      [options]
      strict_mode=*
      ```
- ESLint, plugins:
  - @typescript-eslint/consistent-type-imports
    - https://typescript-eslint.io/blog/consistent-type-imports-and-exports-why-and-how/#benefits-of-enforcing-type-only-importsexports
  - rule: import/no-relative-parent-imports
  - rule: consistent-type-imports
  - [notes](../___SECRET___/yt/videos/???-elint-plugin-todo-with-label.md)
  - [notes](../@dev/ideas/???-my-productive-eslint-plugins.md)
- Husky
- Lint-staged
- Prettier
- Utilities
  - Lodash - the best is version "lodash-es" because it supports Tree Shaking
- HTTP Request
  - react-query
  - Apollo GraphQL
  - axios ❌
- Tests
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
- Code Coverage
  - nyc
  - istanbul ❌

Pipelines

- UI Error Collector of runtime errors
- Observability on BE
  - Logs
  - Metrics
  - Traces

Bonus Services

- Sourcegraph
- SonarQube - https://sonarqube.dev.box.net/profiles
