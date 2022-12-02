# Рекомендации по архитектуре front-end проектов на Vue.js

## Стандарты написания кода

### Принципы проектирования

Использовать общепринятые принципы проектирования кода:

- [Clean Code](https://github.com/devSchacht/clean-code-javascript)
- [DRY](https://web-creator.ru/articles/dry)
- [KISS](https://web-creator.ru/articles/kiss)
- [YAGNI](https://web-creator.ru/articles/yagni)
- [SOLID](https://web-creator.ru/articles/solid)

### Статический анализ и форматирование

Для предотвращения ошибок в коде использовать линтеры:

- [CommitLint](https://commitlint.js.org/)
  ([пример конфигурации](https://github.com/alex-lit/config-commitlint))
- [ESLint](https://eslint.org/)
  ([пример конфигурации](https://github.com/alex-lit/config-eslint))
- [HTMLLint](https://github.com/linthtml/linthtml)
  ([пример конфигурации](https://github.com/alex-lit/config-htmllint))
- [MarkdownLint](https://github.com/DavidAnson/markdownlint)
  ([пример конфигурации](https://github.com/alex-lit/config-markdownlint))
- [NPMLint](https://github.com/tclindner/npm-package-json-lint)
  ([пример конфигурации](https://github.com/alex-lit/config-npmlint))
- [Stylelint](https://stylelint.io/)
  ([пример конфигурации](https://github.com/alex-lit/config-stylelint))

Стандартизировать код-стайл:

- [Prettier](https://prettier.io/)
  ([пример конфигурации](https://github.com/alex-lit/config-prettier))

Пример пакета, объединяющего в себе все вышеперечисленные линтеры:
[lint-kit](https://github.com/alex-lit/lint-kit).

## Поддержка браузеров

Для явного указания поддерживаемых браузеров использовать
[Browserslist](https://github.com/browserslist/browserslist).

Пример конфигурации `.browserslistrc`:

```ini
> 5%

last 3 years

not Baidu > 0
not Explorer > 0
not KaiOS > 0
not OperaMini all
not UCAndroid > 0
not dead
```

## Предпочитаемые технологии

### Общее

- [TypeScript](http://www.typescriptlang.org/) - типизированный ЯП
- [Nuxt.js](https://nuxtjs.org/) - фреймворк для разработки универсальных
  приложений

  Полезные расширения для nuxt:

  - [apollo](https://github.com/nuxt-community/apollo-module)
  - [auth](https://auth.nuxtjs.org/)
  - [axios](https://axios.nuxtjs.org/)
  - [content](https://content.nuxtjs.org/installation/)
  - [i18n](https://i18n.nuxtjs.org/basic-usage/)
  - [image](https://image.nuxtjs.org/api/$img/)
  - [sitemap](https://sitemap.nuxtjs.org/)

  Официальный список расширений:

  - [modules.nuxtjs.org](https://modules.nuxtjs.org/)

### Работа с API

- [Apollo Client](https://apollo.vuejs.org/) - REST/GQL клиент
  - [apollo-link-rest](https://www.apollographql.com/docs/react/api/link/apollo-link-rest/) -
    расширение Apollo для работы с REST API

либо

- [Vue Query](https://vue-query.vercel.app/) - библиотека для работы с
  асинхронными данными во Vue

### Тестирование

- [Vitest](https://vitest.dev/) - юнит-тестирование

## Файловая архитектура проекта

Исходники хранятся в директории `src` и группируются по слоям.

Пример структуры:

```
[scripts] # npm скрипты (.sh)
[src]
    [api] # слой работы с API
    [assets] # ресурсы используемые при компиляции
    [components] # компоненты
    [layouts] # шаблоны страниц
    [locales] # словари локализации
    [middleware] # функции выполняемые перед переходом по страницам
    [pages] # страницы
    [plugins] # плагины Vue.js и их конфиги
    [router] # маршрутизатор
    [static] # статические ресурсы (robots.txt, иконки и т.д)
    [store] # модули состояния приложения
    [utils] # утилиты
...конфиги
```

Нейминг папок и файлов - `kebab-case`.

## Компоненты

### Файловая архитектура

Файлы компонент экспортируются из индексного файла. Дочерние компоненты и ассеты
находятся в поддиректориях (`фрактальная архитектура`, крайне желательно
придерживаться максимально допустимого уровня вложенности - 2).

Пример:

```
[components]
    [my-component]
        my-component.component.vue # код компонента
        my-component.interfaces.ts # интерфейсы
        my-component.spec.ts # тесты
        my-component.store.ts # локальный стор
        ...
        index.ts # экспорт компонента, констант, интерфейсов и т.д.
        [components]
            [my-component-partial] # дочерний компонент
                my-component-partial.component.vue # код компонента
                ...
                index.ts # экспорт
        [images]
            logo.svg # изображение, уникальное для текущего компонента (my-component)
            ...
    [my-another-component]
    ...
```

Этот же принцип применяется и к остальным слоям - utils, plugins и т.д.

### Синтаксис

Применять `script setup`.

### Стилизация

- Инкапсуляция:
  [Scoped CSS](https://vuejs.org/api/sfc-css-features.html#scoped-css) либо [CSS Modules](https://vuejs.org/api/sfc-css-features.html#css-modules)
- [BEM](https://ru.bem.info/) - методология Yandex для построения масштабируемых
  интерфейсов (1 компонент - 1 блок)
- [SCSS](https://sass-lang.com/) - CSS препроцессор
- При разработке кастомного UI-кита использовать
  [Open Props](https://open-props.style/)

**Применение фреймворков типа `Bootstrap`, `Tailwind` и т.д. крайне
нежелательно.**

## API

Разделение по сущностям с принципом 1 файл - 1 метод. Пример:

```
[api]
    [user]
        get-user.query.gql
        update-user.mutation.gql
    [auth]
        check-auth.query.gql
        ...
    index.ts # экспорт API-методов
```

## Хранилище данных

Использовать [Pinia](https://pinia.vuejs.org/).

> Рекомендуется использовать стор только в случае крайней необходимости, все
> стандартные взаимодействия между компонентами решать основным способом:
> параметры вниз / события наверх, provide / inject, слоты и т.д.

## Локализация

Стандартный пакет [Vue I18n](https://kazupon.github.io/vue-i18n/).

Структура файлов локализации `плоская`, имена ключей в `CONSTANT_CASE` с
префиксом, пример:

```json
{
  "I18N_GOODBYE": "Пока",
  "I18N_HELLO": "Привет"
}
```

## Контроль версий

Работу с ветками репозитория осуществлять по модели
[GitFlow](https://habr.com/ru/post/106912/), для коммитов использовать стандарт
[Conventional Commits](https://www.conventionalcommits.org/ru/).

Перед коммитом автоматически выполнять проверку кода линтерами на наличие
ошибок.
