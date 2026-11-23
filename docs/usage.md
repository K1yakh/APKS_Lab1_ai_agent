# Використання

## Підготовка репозиторію

Клонуйте репозиторій та створіть робочу гілку:

```bash
git clone https://github.com/K1yakh/APKS_Lab1_ai_agent.git
cd APKS_Lab1_ai_agent
git checkout develop
git checkout -b feature/lab1 develop
```

Вимоги:

- VS Code
- GitHub Copilot
- Node.js 22

## Запуск агента

У Copilot Chat введіть загальну команду:

```text
/lab-init
```

Команда називається `/lab-init`, а не `/init`, оскільки `/init` є вбудованою командою Copilot.

Для окремих етапів у Copilot Chat доступні такі команди:

```text
/git-init
/create-project
/create-build
/create-actions
/check
```

## Локальна перевірка

### Windows

```powershell
.\ci.bat
```

### Linux/macOS

```bash
./ci.sh
```

Окремі перевірки можна запустити командами:

```bash
npm ci
npm run build
npm test
npm start
```

Команда `npm start` виводить `Hello, World!`.

Для запуску одного тесту `BasicAddition` використовуйте:

```bash
npx vitest run tests/add.test.ts -t BasicAddition
```

## Перевірка Git

Перевірте стан репозиторію та ігноровані файли:

```bash
git status --ignored
```

Каталоги `node_modules/` та `dist/` мають бути ігнорованими.

Перевірте режим виконуваного файлу `ci.sh`:

```bash
git ls-files --stage ci.sh
```

Очікуваний режим: `100755`.

## Демонстрація зламаної перевірки

1. У файлі `tests/add.test.ts` змініть очікуване значення тесту.
2. Запустіть перевірку для Windows:

```powershell
.\ci.bat
```

Перевірка завершується з результатом `FAIL` і повертає код `1`.

3. Відновіть початковий вміст тесту:

```bash
git restore tests/add.test.ts
```
