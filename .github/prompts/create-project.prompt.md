---
description: "Створити базовий TypeScript Hello World за skill project-scaffold"
agent: build-engineer
---

# Create Project

## Передумови

- Git-репозиторій уже перевірено prompt-ом `git-init`.
- Відомі мова `TypeScript` і назва проєкту.
- Доступний skill `.github/skills/project-scaffold/SKILL.md`.

## Дії

1. Прочитай і застосуй `.github/skills/project-scaffold/SKILL.md`.
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
