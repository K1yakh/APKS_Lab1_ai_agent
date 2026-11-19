---
name: build-and-test
description: "Налаштування TypeScript-збірки, Vitest-тесту BasicAddition і кросплатформних ci.sh та ci.bat через npm ci, build, test і запуск Hello World."
---

# Build and Test

## Коли застосовувати

Застосовуй skill, коли базовий TypeScript-проєкт уже створено і потрібно додати функцію `add`, тестування Vitest, npm scripts та локальні CI-скрипти.

## Вхідні дані

- TypeScript-проєкт із `package.json` і `src/`;
- команда або вимога до скриптів `build`, `test`, `start`;
- Node.js 22 і npm;
- вимога зберігати `package-lock.json` у Git.

## Результат

Створи або онови:

- `package.json` зі скриптами `build: "tsc"`, `test: "vitest run"` і `start: "node dist/hello.js"`;
- `src/add.ts` з функцією `add(a, b)`;
- `tests/add.test.ts` із тестом `BasicAddition`;
- `ci.sh` із `bash` і `set -euo pipefail`;
- `ci.bat`, де кожна npm-команда виконується через `call`, а помилка завершує скрипт через `exit /b 1`;
- `package-lock.json`, створений npm і закомічений у Git.

## Кроки

1. Перевір `package.json`, lockfile, структуру `src/` і наявні тести.
2. Додай Vitest як dev-залежність, якщо її ще немає, і згенеруй або онови `package-lock.json`.
3. Додай npm scripts: `build` викликає `tsc`, `test` викликає `vitest run` без watch-режиму, `start` викликає `node dist/hello.js`.
4. Створи `src/add.ts` з типізованою функцією `add(a, b)`, яка повертає суму.
5. Створи `tests/add.test.ts` із тестом, названим `BasicAddition`, що перевіряє результат додавання.
6. Створи `ci.sh` із послідовністю `npm ci`, `npm run build`, `npm test`, `node dist/hello.js` і `set -euo pipefail`.
7. Створи `ci.bat`: перед кожною npm-командою використай `call`, після кожного кроку перевіряй `errorlevel`, а помилку заверши `exit /b 1`.
8. Перевір, що `package-lock.json` не ігнорується та входить до файлів, які потрібно закомітити.

## Перевірка результату

- `npm ci` завершується успішно за наявності `package-lock.json`;
- `npm run build` завершується успішно;
- `npm test` виконує `vitest run`, не зависає у watch-режимі та проходить `BasicAddition`;
- `node dist/hello.js` виводить `Hello, World!`;
- `ci.sh` і `ci.bat` виконують усі чотири кроки та передають помилки;
- `git status` показує `package-lock.json` як tracked-файл після додавання до commit.

## Приклад входу та очікуваний результат

**Вхід:** TypeScript-проєкт із `src/hello.ts` і `package.json`, але без тестів та CI-скриптів.

**Очікуваний результат:** додано `src/add.ts`, `tests/add.test.ts`, npm scripts, `ci.sh`, `ci.bat` і `package-lock.json`; `npm test` проходить тест `BasicAddition`.

## Типовий сценарій помилки і що робити

**Помилка:** `npm test` не завершується або CI зависає.

**Що робити:** зупинись, покажи точну команду та її вивід, перевір, що script використовує саме `vitest run`, а не `vitest`, виправ script і повтори `npm ci`, build та test.
