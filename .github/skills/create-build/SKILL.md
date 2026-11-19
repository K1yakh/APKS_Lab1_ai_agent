---
name: create-build
description: "Додати збірку, Vitest BasicAddition і локальні CI-скрипти за skill build-and-test"
disable-model-invocation: true
---
# Create Build

## Передумови

- Базовий TypeScript-проєкт створено prompt-ом `create-project`.
- Доступний skill `.github/skills/build-and-test/SKILL.md`.
- Node.js 22 і npm доступні в терміналі.

## Дії

1. Прочитай і застосуй `.github/skills/build-and-test/SKILL.md`.
2. Додай scripts `build: tsc`, `test: vitest run`, `start: node dist/hello.js`.
3. Створи `src/add.ts` і `tests/add.test.ts` із тестом `BasicAddition`.
4. Створи `ci.sh` з `set -euo pipefail` і `ci.bat` з `call` та перевіркою `errorlevel`.
5. Якщо `package-lock.json` ще немає, спочатку виконай `npm install`, щоб створити lockfile; потім у будь-якому разі виконай `npm ci`. Lockfile має бути закомічений.

## Очікувані файли

- `package.json`;
- `package-lock.json`;
- `src/add.ts`;
- `tests/add.test.ts`;
- `ci.sh`;
- `ci.bat`.

## Команди перевірки

```text
npm ci
npm run build
npm test
node dist/hello.js
bash ci.sh
git ls-files package-lock.json
```

На Windows додатково перевір `ci.bat` у `cmd.exe`.

## Повідомлення про помилку

Зупинися та покажи точну команду, повний релевантний вивід і код завершення. Якщо тест зависає, перевір, що використано `vitest run`, а не watch-режим.

## Правило повторного запуску

Повторний запуск нічого не руйнує і не дублює: використовуй наявні scripts, залежності, тести й lockfile, змінюй лише невідповідні частини та не створюй дублікати.
