# Архітектура build-engineer

## Призначення та проблеми, які вирішує агент

Агент `build-engineer` автоматизує початкове створення і перевірку кросплатформного TypeScript-проєкту. Він вирішує такі проблеми:

- створення узгодженої структури TypeScript Hello World;
- налаштування npm scripts для збірки, тестування і запуску;
- додавання Vitest і тесту `BasicAddition` для функції `add`;
- однакова локальна CI-послідовність для Windows, Linux і macOS;
- створення GitHub Actions із matrix для трьох операційних систем;
- перевірка manifest-файлів, workflow, `.gitignore` і Git-режиму `ci.sh`;
- безпечний повторний запуск без дублювання та руйнування наявних файлів.

## Агент

Основний custom agent — [`build-engineer`](../.github/agents/build-engineer.agent.md). Він задає роль інженера збірки, дозволені інструменти, порядок роботи, правила роботи з файлами, заборонені дії, обробку помилок і критерії завершення.

## Skills

Skills завантажуються як спеціалізовані інструкції:

- [`project-scaffold`](../.github/skills/project-scaffold/SKILL.md) — створює базовий TypeScript Hello World, `package.json`, `tsconfig.json`, README і `.gitignore`;
- [`build-and-test`](../.github/skills/build-and-test/SKILL.md) — додає npm scripts, Vitest, `add`, `BasicAddition`, `ci.sh`, `ci.bat` і `package-lock.json`;
- [`github-actions`](../.github/skills/github-actions/SKILL.md) — створює workflow для develop/master, трьох runner-ів і artifact `dist/`.

## Commands

Commands реалізовані як prompt-файли:

- [`git-init`](../.github/prompts/git-init.prompt.md) — перевіряє Git, доповнює `.gitignore`, показує status і гілку;
- [`create-project`](../.github/prompts/create-project.prompt.md) — застосовує skill `project-scaffold`;
- [`create-build`](../.github/prompts/create-build.prompt.md) — застосовує skill `build-and-test`;
- [`create-actions`](../.github/prompts/create-actions.prompt.md) — застосовує skill `github-actions`;
- [`check`](../.github/prompts/check.prompt.md) — виконує read-only перевірки та виводить таблицю PASS/FAIL;
- [`init`](../.github/prompts/init.prompt.md) — оркеструє всі етапи послідовно.

## Відповідність рекомендованій структурі

| Методичка | Цей проєкт |
|---|---|
| `CMakeLists.txt` | `package.json` + `tsconfig.json` (варіант 6: TypeScript, npm scripts, Vitest) |
| Збірка й тести | `ci.sh` / `ci.bat`, які викликає `ci.yml` |
| Commands | `.github/prompts/*.prompt.md`, запускаються в Copilot Chat як `/назва` |
| Agent manifest | `.github/agents/build-engineer.agent.md` |
| Skills | `.github/skills/<назва>/SKILL.md` (формат Agent Skills у VS Code) |

## Дані між етапами

Етапи передають один одному не окремий API-об’єкт, а стан робочого дерева і створені файли репозиторію:

1. `git-init` передає підтвердження Git-репозиторію, поточну гілку і базовий `.gitignore`.
2. `create-project` використовує цей стан і передає структуру TypeScript, `package.json`, `tsconfig.json`, `src/hello.ts` та README.
3. `create-build` використовує `package.json` і `src/`, додає `src/add.ts`, тест, npm scripts, lockfile і CI-скрипти.
4. `create-actions` використовує CI-скрипти та lockfile, додає workflow і executable mode для `ci.sh`.
5. `check` читає підсумковий стан, запускає build/test і повертає таблицю перевірок та загальний PASS/FAIL.

Кожен етап перевіряє передумови попереднього етапу та не переходить далі після помилки.

## Обробка помилок у `init`

`init` виконує prompts у такому порядку: `git-init` → `create-project` → `create-build` → `create-actions` → `check`. Після кожного кроку він перевіряє результат. Якщо команда або перевірка завершується помилкою, `init` зупиняється, називає крок і prompt, показує точну команду, релевантний вивід, код завершення та причину. Наступні етапи не виконуються, а робота не оголошується успішною.

Перед початком `init` також вимагає гілку `feature/...`; на `develop` або `master` він має зупинитися, нічого не створюючи.

## Файл → роль

| Файл | Роль |
|---|---|
| `.github/agents/build-engineer.agent.md` | Основна роль, правила та критерії агента |
| `.github/skills/project-scaffold/SKILL.md` | Створення базової структури проєкту |
| `.github/skills/build-and-test/SKILL.md` | Збірка, Vitest і локальні CI-скрипти |
| `.github/skills/github-actions/SKILL.md` | Кросплатформний GitHub Actions workflow |
| `.github/prompts/git-init.prompt.md` | Перевірка та підготовка Git |
| `.github/prompts/create-project.prompt.md` | Запуск scaffold-етапу |
| `.github/prompts/create-build.prompt.md` | Запуск build/test-етапу |
| `.github/prompts/create-actions.prompt.md` | Запуск Actions-етапу |
| `.github/prompts/check.prompt.md` | Read-only фінальна перевірка |
| `.github/prompts/init.prompt.md` | Послідовна оркестрація всіх етапів |
| `package.json` | Manifest і npm scripts проєкту |
| `tsconfig.json` | Параметри TypeScript-компіляції |
| `src/hello.ts` | Hello World |
| `src/add.ts` | Функція додавання |
| `tests/add.test.ts` | Тест `BasicAddition` |
| `ci.sh` / `ci.bat` | Кросплатформні локальні CI-команди |
| `.github/workflows/ci.yml` | GitHub Actions matrix і artifact |
| `.gitignore` | Виключення залежностей, артефактів і секретів |
| `package-lock.json` | Зафіксовані версії залежностей для `npm ci` |
| `README.md` | Опис і інструкція запуску |
| `docs/architecture.md` | Архітектура агента |
| `docs/usage.md` | Команди запуску агента і локальної перевірки |
| `docs/ai-log.md` | Шаблон журналу взаємодії з AI |
