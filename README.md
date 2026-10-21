**English** | [Русский](/README.ru.md)

# 🎓 SkillSwap — a Skill Exchange Platform

An SPA where users find each other to exchange knowledge

[![Tech stack](https://skillicons.dev/icons?i=ts,react,redux,vite,html,css)](https://skillicons.dev/icons?i=ts,react,redux,vite,html,css)

[![Team project](https://img.shields.io/badge/team%20project-11%20people-blueviolet)](https://img.shields.io/badge/team%20project-11%20people-blueviolet) [![Commits](https://img.shields.io/badge/my%20contribution-82%20commits-informational)](https://img.shields.io/badge/my%20contribution-82%20commits-informational) [![Role](https://img.shields.io/badge/role-frontend%20%2B%20team%20lead%20assistant-orange)](https://img.shields.io/badge/role-frontend%20%2B%20team%20lead%20assistant-orange)

[Demo](#demo) · [Figma](https://www.figma.com/design/bKwOakHJI7Z2mh2zVCBphP/SkillSwap---%D0%94%D0%BB%D1%8F-%D1%80%D0%B0%D0%B7%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D1%87%D0%B8%D0%BA%D0%BE%D0%B2?node-id=69-279&t=V9TRxM1DIym8VaxH-1) · [Quick Start](#-quick-start) · [My Contribution](#my-contribution)

---

## 📌 About the Project

SkillSwap is a team learning project (Yandex Practicum) built in 4 weeks, covering the full cycle: from the technical specification and role assignment to deployment.

Users publish skills of two types — "Teaching" and "Learning" — look for mutually matching pairs through a catalog with filters, send exchange requests and keep a list of sessions. The data is mocked (JSON + localStorage), with no backend, in accordance with the technical specification.

- a team of 11 people, a real Git flow with pull requests, code review and CI;
- the project went through a technical review by a mentor: the ["Review and Growth"](#-review-and-growth) section honestly lists what was found and the plan for improvements.

---

## 🖥️ Demo

#### Filter and search bar in action

https://github.com/user-attachments/assets/b8d4bddc-91ed-46d0-9337-6584c436cab3

#### Registration and skill card creation flow

https://github.com/user-attachments/assets/6b4cc1cd-ac2b-4b6b-b486-46061f6e3c4b

---

## 🚀 Quick Start

```
npm install
npm run dev
```

Requires Node.js 20+.

### Available Scripts

| Script                                | What it does                                |
| ------------------------------------- | ------------------------------------------- |
| `npm run dev`                         | Starts the dev server                       |
| `npm run build`                       | Type-checking (`tsc -b`) + production build |
| `npm run preview`                     | Previews the production build locally       |
| `npm run lint` / `npm run lint:fix`   | ESLint + Stylelint                          |
| `npm run format`                      | Prettier formatting                         |
| `npm run test` / `npm run test:watch` | Tests (Vitest + Testing Library)            |
| `npm run test:coverage`               | Coverage, target ≥ 70% statements           |

---

## 🙋‍♀️ My Contribution

Role: **frontend developer and team lead assistant**.

**Team management**

- As the team lead's assistant, I broke the tasks of the technical specification down to the issue level, reviewed PRs, motivated the team and ran a simple onboarding for new members.

**Redux / application state**

- Designed `authSlice` and `accountSlice` — authentication and account data are stored separately from the Redux state in localStorage; the password never ends up in the state (`Omit<UserAccount, 'password'>`).
- Implemented login with React Hook Form + Yup (`Controller`, `yupResolver`, `onChange` mode, separating validation errors from server errors).

**Catalog and search**

- Designed the catalog filter config: a discriminated union type `FilterGroupConfig[]`, the `FilterSection` component, checkboxes with an indeterminate state for nested categories (`CheckboxGroup`) and `RadioGroup`.
- Implemented the search bar in the header, with the query parameter persisted in the URL, a suggestions dropdown (`useSearchSuggestions`) and CSS-based scrolling without third-party libraries.

**Component library**

- Developed reusable UI components: `Checkbox`, `CheckboxGroup`, `RadioButton`, `RadioGroup`, `FilterSection`, `StepProgress`, `Calendar` (based on `react-datepicker`), `Header`.
- A consistent approach across the whole library: CSS Modules + `clsx`, controlled components, inline SVG without icon libraries.

**Data**

- Refined the mock skills data (`skills.json`) to fit the form constraints (title length, subcategory format, images via `picsum.photos`).

---

## 🗂️ Project Architecture

The project is built loosely following Feature-Sliced Design:

```
src/
├── api/                  # fetch functions for loading JSON mocks
├── app/
│   ├── providers/        # StoreProvider, RouterProvider
│   └── styles/           # global.css with CSS variables
├── entities/             # Domain models: Skill, User, Request
│   ├── skill/
│   ├── user/
│   └── request/
├── features/             # Features: auth, skills, favorites, requests
├── pages/                # Application pages
├── shared/
│   ├── hooks/            # useDebounce, useLocalStorage
│   ├── lib/              # constants, helpers
│   ├── types/            # shared TypeScript types
│   └── ui/               # atomic components
├── store/                # Redux store and typed hooks
└── widgets/              # Composite blocks: Header, SkillCard, FiltersBar

public/
└── db/
    ├── skills.json       # Skill mocks
    └── users.json        # User mocks
```

**Stack:** React 18 + TypeScript, Redux Toolkit, React Router v6, React Hook Form + Yup, Vite, CSS Modules, Vitest + React Testing Library, ESLint (Airbnb) + Prettier + Stylelint, GitHub Actions (CI + deployment to GitHub Pages).

---

## 🔍 Review and Growth

The project went through a technical review by a mentor. Below is the plan for improvements that I will implement gradually.

| Sprint | Focus                  | Tasks                                                                                                                                                                                                                               |
| ------ | ---------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1      | Green CI               | Fix the write race in `createSwapRequest`; enable coverage in CI; fix formatting; replace `Math.random()` with `useId()`                                                                                                            |
| 2      | Single source of truth | Remove direct `getAuthUser()` calls from component render in favor of Redux selectors; reduce `users`/`skills` loading to a single entry point; extract `localStorage` keys and event names into constants                         |
| 3      | Layer boundaries       | Move composite components from `shared` to `widgets`; dissolve the non-FSD `src/components/`; enable `eslint-plugin-boundaries` so that `shared` cannot import from `features`/`entities`                                           |

**What I took away from this review:**

- mock data can mask bugs (the dataset had only one skill per user, which is why a catalog filtering bug went unnoticed);
- a decision made by one person in a `shared` component doesn't become a team standard on its own — it takes a boundaries linter and documented agreements, not just code review.