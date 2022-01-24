# Стандарты и структура front-end проектов на Vue.js

## Стандарты написания кода

Принципы стандартные: "Clean Code / DRY / KISS / YAGNI". Используется единый
[пакет линтеров](https://github.com/alex-lit/lint-kit) (eslint, stylelint,
prettier и т.д.).

## Поддержка браузеров (.browserslistrc)

```ini
> 5%
last 2 versions
not Baidu > 0
not Explorer > 0
not OperaMini all
not UCAndroid > 0
not dead
```

## Предпочитаемые технологии

- [Nuxt.js](https://nuxtjs.org/) - фреймворк для универсальных приложений
- [TypeScript](http://www.typescriptlang.org/) - типизированный ЯП
- [Apollo CLient](https://apollo.vuejs.org/) - REST/GQL клиент
- [BEM](https://ru.bem.info/) - методология Yandex для построения масштабируемых
  интерфейсов
- [SCSS](https://sass-lang.com/) - CSS препроцессор
- [Jest](https://facebook.github.io/jest/) - юнит-тестирование

## Файловая структура проекта

Исходники хранятся в директории “src” и группируются по слоям.

Пример структуры:

```toml
[scripts] # npm скрипты (.sh)
[src]
    [api] # слой работы с API (Apollo)
    [assets] # ресурсы используемые при компиляции
    [components] # компоненты
    [layouts] # шаблоны страниц
    [locales] # словари локализации
    [middleware] # функции выполняемые перед переходом по страницам
    [pages] # страницы
    [plugins] # плагины Vue.js и их конфиги
    [static] # статические ресурсы (robots.txt, иконки и т.д)
    [store] # модули состояния приложения (Vuex)
    [utils] # утилиты
...конфиги
```

Нейминг папок и файлов - `kebab-case`.

## Компоненты

### Файловая структура

Файлы компонент экспортируются из индексного файла. Дочерние компоненты и ассеты
находятся в поддиректориях.

Пример:

```toml
[my-component]
    my-component.component.vue # код компонента
    my-component.spec.ts # тесты
    index.ts # экспорт
    [components]
        [my-component-partial]
            my-component-partial.component.vue # код компонента
            my-component-partial.spec.ts # тесты
            index.ts # экспорт
    [images]
        logo.svg # изображение, уникальное для текущего компонента (my-component)
```

Этот же принцип применяется и к остальным слоям - utils, plugins и т.д.

### Типизация

Для типизации компонент используется синтаксис на основе классов:
[vue-property-decorator](https://github.com/kaorun343/vue-property-decorator) +
[vuex-class](https://github.com/ktsn/vuex-class).

### Стилизация

- Инкапсуляция:
  [Локальный (scoped) CSS](https://vue-loader.vuejs.org/ru/guide/scoped-css.html)
- Препроцессор: SCSS
- Методология: БЭМ (1 компонент - 1 блок)

**Применение фреймворков типа Bootstrap, Tailwind и т.д. не допускается.**

## Работа с API

Разделение по сущностям с принципом 1 файл - 1 метод. Пример:

```toml
[user]
    get-user.query.gql
    update-user.mutation.gql
[auth]
    ...
index.ts # экспорт API-методов
```

## Хранилище данных

Используется Vuex, сегментация по модулям.

> Рекомендуется использовать стор только в случае крайней необходимости, все
> стандартные взаимодействия между компонентами решать основным способом:
> параметры вниз / события наверх, provide / inject, слоты и т.д.

## Локализация

Стандартный пакет [Vue I18n](https://kazupon.github.io/vue-i18n/). Языковые
файлы - 1 файл на 1 язык для всего приложения:

```toml
[locales]
    ru.json
    en.json
    ...
```

Структура файлов локализации `плоская`, имена ключей в `CONSTANT_CASE` с
префиксом, пример:

```json
{
  "I18N_GOODBYE": "Пока",
  "I18N_HELLO": "Привет"
}
```

## Контроль версий

Работа с ветками репозитория осуществляется по модели
[GitFlow](https://habr.com/ru/post/106912/), для коммитов используется стандарт
[Conventional Commits](https://www.conventionalcommits.org/ru/).

Перед коммитом автоматически выполняется проверка кода линтерами на наличие
ошибок. Если **ошибки будут найдены** то **коммит не пройдет**.
