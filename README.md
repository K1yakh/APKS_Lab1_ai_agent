# APKS Lab 1 — Build Engineer

Лабораторна робота №1 — Проєктування AI-агента для автоматизації розробки та CI/CD

## Автор

- Кіях Андрій Романович
- Група: КІ-403
- Варіант 6

## Технології

TypeScript, npm scripts і Vitest.

## Короткий опис проєкту

Проєкт містить custom agent `build-engineer` для GitHub Copilot у VS Code. Агент створює та перевіряє кросплатформний TypeScript Hello World на Node.js 22, налаштовує npm scripts, юніт-тест Vitest для функції `add`, локальні CI-скрипти та GitHub Actions для Linux, Windows і macOS.

Конфігурація агента, skills і prompts розміщені в каталозі `.github`.

## Вимоги

- VS Code
- GitHub Copilot у режимі Agent
- Node.js 22
- Git

## Як запустити агента

Спочатку створіть робочу гілку:

```text
git checkout -b feature/lab1 develop
```

Команда `/init` по черзі виконує `git-init`, `create-project`, `create-build`, `create-actions` і `check`. Після кожного кроку перевіряється результат; у разі помилки виконання зупиняється. Команда `/check` лише перевіряє проєкт і нічого не змінює.

1. Відкрийте GitHub Copilot Chat у VS Code.
2. Виберіть агента `build-engineer`.
3. Уведіть `/init`.

## Як зібрати й запустити проєкт

Windows:

```text
.\ci.bat
```

Linux/macOS:

```text
./ci.sh
```

Для запуску після збірки виконайте:

```text
npm start
```

Команда виводить `Hello, World!`.

## Документація

- [Архітектура](docs/architecture.md)
- [Використання](docs/usage.md)
- [Журнал роботи AI](docs/ai-log.md)