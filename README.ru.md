[English](/README.md) | **Русский**

# 🎓 SkillSwap — платформа обмена навыками

SPA-сервис, где пользователи находят друг друга для обмена знаниями

[![Tech stack](https://skillicons.dev/icons?i=ts,react,redux,vite,html,css)](https://skillicons.dev/icons?i=ts,react,redux,vite,html,css)

[![Team project](https://img.shields.io/badge/team%20project-11%20people-blueviolet)](https://img.shields.io/badge/team%20project-11%20people-blueviolet) [![Commits](https://img.shields.io/badge/my%20contribution-82%20commits-informational)](https://img.shields.io/badge/my%20contribution-82%20commits-informational) [![Role](https://img.shields.io/badge/role-frontend%20%2B%20team%20lead%20assistant-orange)](https://img.shields.io/badge/role-frontend%20%2B%20team%20lead%20assistant-orange)

[Demo](#demo) · [Figma](https://www.figma.com/design/bKwOakHJI7Z2mh2zVCBphP/SkillSwap---%D0%94%D0%BB%D1%8F-%D1%80%D0%B0%D0%B7%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D1%87%D0%B8%D0%BA%D0%BE%D0%B2?node-id=69-279&t=V9TRxM1DIym8VaxH-1) · [Quick Start](#-quick-start) · [My Contribution](#my-contribution)

---

## 📌 О проекте

SkillSwap — командный учебный проект (Яндекс Практикум), реализованный за 4 недели по полному циклу: от технического задания и распределения ролей до деплоя.

Пользователи публикуют навыки двух типов — «Учу» и «Учусь», ищут взаимно подходящие пары через каталог с фильтрами, отправляют заявки на обмен и ведут список сессий. Данные — моковые (JSON + localStorage), без бэкенда с соответствии с ТЗ.

- команда из 11 человек, реальный Git-флоу с Pull Request'ами, код-ревью и CI;
- проект прошёл техническое ревью наставника:в разделе [«Ревью и рост»](#-ревью-и-рост) честно перечислено, что было найдено и план доработок.

---

<a id="demo"></a>
## 🖥️ Демо
#### Работа фильтра и поисковой строки

https://github.com/user-attachments/assets/b8d4bddc-91ed-46d0-9337-6584c436cab3

#### Процесс регистрации и создания карточки навыка

https://github.com/user-attachments/assets/6b4cc1cd-ac2b-4b6b-b486-46061f6e3c4b

<!-- Продакшен-сборка деплоится автоматически на GitHub Pages при пуше в `main` (см. `.github/workflows/cd.yml`). -->

---

## 🚀 Быстрый старт

```bash
npm install
npm run dev
```

Требуется Node.js 20+.

### Доступные скрипты

| Скрипт | Что делает |
|---|---|
| `npm run dev` | Запуск дев-сервера |
| `npm run build` | Типизация (`tsc -b`) + продакшен-сборка |
| `npm run preview` | Просмотр продакшен-сборки локально |
| `npm run lint` / `npm run lint:fix` | ESLint + Stylelint |
| `npm run format` | Форматирование Prettier |
| `npm run test` / `npm run test:watch` | Тесты (Vitest + Testing Library) |
| `npm run test:coverage` | Покрытие, цель ≥ 70% statements |

---

<a id="my-contribution"></a>
## 🙋‍♀️ Мой вклад в проект

Роль: **frontend-разработчик и помощник тимлида**.

**Управление командой**
- Как помощник тимлида декомпозировала задачи технического задания на issue-уровень, проводила code review PR, мотивировала команду и проводила простой онбординг новых участников.

**Redux / состояние приложения**
- Спроектировала `authSlice` и `accountSlice` — авторизация и данные аккаунта хранятся раздельно от Redux-стейта в localStorage; пароль никогда не попадает в стейт (`Omit<UserAccount, 'password'>`).
- Реализовала логин через React Hook Form + Yup (`Controller`, `yupResolver`, режим `onChange`, разделение ошибок валидации и серверных ошибок).

**Каталог и поиск**
- Спроектировала конфиг фильтров каталога: дискриминированный union-тип `FilterGroupConfig[]`, компонент `FilterSection`, чекбоксы с indeterminate-состоянием для вложенных категорий (`CheckboxGroup`) и `RadioGroup`.
- Реализовала поисковую строку в хедере с сохранением query-параметра в URL, дропдауном подсказок (`useSearchSuggestions`) и CSS-скроллом без сторонних библиотек.

**Компонентная библиотека**
- Разработала переиспользуемые UI-компоненты: `Checkbox`, `CheckboxGroup`, `RadioButton`, `RadioGroup`, `FilterSection`, `StepProgress`, `Calendar` (на базе `react-datepicker`), `Header`.
- Единый подход по всей библиотеке: CSS Modules + `clsx`, контролируемые компоненты, инлайн-SVG без иконочных библиотек.

**Данные**
- Дорабатывала моковые данные навыков (`skills.json`) с учётом ограничений формы (длина заголовков, формат подкатегорий, изображения через `picsum.photos`).

---

## 🗂️ Архитектура проекта

Проект построен по мотивам Feature-Sliced Design:

```
src/
├── api/                  # fetch-функции для загрузки JSON-моков
├── app/
│   ├── providers/        # StoreProvider, RouterProvider
│   └── styles/           # global.css с CSS-переменными
├── entities/             # Доменные модели: Skill, User, Request
│   ├── skill/
│   ├── user/
│   └── request/
├── features/             # Фичи: auth, skills, favorites, requests
├── pages/                # Страницы приложения
├── shared/
│   ├── hooks/            # useDebounce, useLocalStorage
│   ├── lib/              # constants, helpers
│   ├── types/            # общие TypeScript-типы
│   └── ui/               # атомарные компоненты
├── store/                # Redux store и типизированные хуки
└── widgets/              # Составные блоки: Header, SkillCard, FiltersBar

public/
└── db/
    ├── skills.json       # Моки навыков
    └── users.json        # Моки пользователей
```

**Стек:** React 18 + TypeScript, Redux Toolkit, React Router v6, React Hook Form + Yup, Vite, CSS Modules, Vitest + React Testing Library, ESLint (Airbnb) + Prettier + Stylelint, GitHub Actions (CI + деплой на GitHub Pages).

---

## 🔍 Ревью и рост

Проект прошёл техническое ревью наставника. Ниже представлен план доработок, которые буду внедрять постепенно.

| Спринт | Фокус | Задачи |
|---|---|---|
| 1 | Зелёный CI | Починить гонку записи в `createSwapRequest`; включить coverage в CI; выправить форматирование; заменить `Math.random()` на `useId()` |
| 2 | Единый источник правды | Убрать прямые вызовы `getAuthUser()` из рендера компонентов в пользу Redux-селекторов; свести загрузку `users`/`skills` к одной точке входа; вынести ключи `localStorage` и имена событий в константы |
| 3 | Границы слоёв | Перенести составные компоненты из `shared` в `widgets`; расформировать внефсдэшный `src/components/`; включить `eslint-plugin-boundaries`, чтобы `shared` не мог тянуть `features`/`entities` |

**Что я вынесла из этого ревью:**
- моковые данные могут маскировать баги (в датасете было по одному навыку на пользователя — из-за этого ошибка фильтрации в каталоге оставалась незамеченной);
- решение, принятое одним человеком в `shared`-компоненте, не становится стандартом команды само по себе — нужны линтер границ и зафиксированные договорённости, а не только код-ревью.
