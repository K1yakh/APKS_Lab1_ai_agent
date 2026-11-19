---
name: create-project
description: "Створити базовий TypeScript Hello World за skill project-scaffold"
disable-model-invocation: true
---
# Create Project
Виконуй за правилами агента [build-engineer](../../agents/build-engineer.agent.md).

## Передумови

- Git-репозиторій уже перевірено command-ом [`git-init`](../git-init/SKILL.md).
- Відомі мова `TypeScript` і назва проєкту.
- Доступний skill [`project-scaffold`](../project-scaffold/SKILL.md).

## Дії

1. Прочитай і застосуй [`project-scaffold`](../project-scaffold/SKILL.md).
2. Перевір наявну структуру та не перезаписуй відповідні файли.
3. Створи `src/hello.ts`, `package.json`, `tsconfig.json`, `README.md` і `.gitignore` за вимогами skill.
4. Перевір `rootDir: "src"`, `outDir: "dist"` і команду запуску.

## Очікувані файли

- `src/hello.ts`;
- `package.json`;
- `tsconfig.json`;
- `README.md`;
- `.gitignore`.

## Команди перевірки

```text
Get-Content .github/skills/project-scaffold/SKILL.md
Test-Path src/hello.ts
Test-Path package.json
Test-Path tsconfig.json
npm run build
npm start
```

## Повідомлення про помилку

Зупинися, покажи команду, її вивід і код завершення. Вкажи відсутній або невірний файл чи параметр і виправ лише невідповідність вимогам skill.

## Правило повторного запуску

Повторний запуск нічого не руйнує і не дублює: перед створенням читай наявні файли, зберігай сумісні налаштування та змінюй лише те, що не відповідає вимогам.
