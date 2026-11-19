---
name: check
description: "Безпечна повна перевірка manifest, skills, commands, TypeScript-проєкту та CI без змін"
disable-model-invocation: true
---
# Check
Виконуй за правилами агента [build-engineer](../../agents/build-engineer.agent.md).

## Передумови

- Репозиторій доступний для читання.
- Встановлені Node.js 22, npm і Git.
- Перевірка не повинна змінювати джерела, конфігурацію, lockfile або Git index; `npm run build` може створити лише ігнорований `dist/`.

## Дії

1. Перевір наявність manifest `package.json`, `package-lock.json`, `tsconfig.json`, `README.md`, `src/`, `tests/`, `ci.sh`, `ci.bat`.
2. Перевір наявність трьох skills (`project-scaffold`, `build-and-test`, `github-actions`) і шести commands (`git-init`, `create-project`, `create-build`, `create-actions`, `check`, `init`) як `.github/skills/<назва>/SKILL.md` із `disable-model-invocation: true`.
3. Перевір структуру проєкту, scripts, `src/add.ts`, `tests/add.test.ts`, `BasicAddition`.
4. Запусти `npm run build` і `npm test`.
5. Перевір, що `ci.yml` містить `ubuntu-latest`, `windows-latest`, `macos-latest` і викликає лише `ci.sh` або `ci.bat`, без прямих npm-команд.
6. Перевір, що `node_modules/` і `dist/` є в `.gitignore` та не відстежуються Git.
7. Виведи таблицю з колонками `Перевірка`, `Результат`, `Деталі` та значеннями `PASS`/`FAIL`, потім загальний результат.

## Очікувані файли

Перевіряються `package.json`, `package-lock.json`, `tsconfig.json`, `README.md`, `src/`, `tests/`, `ci.sh`, `ci.bat`, `.gitignore`, `.github/agents/` і `.github/skills/`; нічого з переліченого не створюється чи не редагується.

## Команди перевірки

```text
git status --short
Test-Path package.json, package-lock.json, tsconfig.json, README.md, src, tests, ci.sh, ci.bat
npm run build
npm test
git ls-files node_modules dist
git check-ignore -q node_modules/
git check-ignore -q dist/
```

Для YAML перевір окремо всі три runner-и, `ci.sh`/`ci.bat`, `checkout`, `setup-node`, `upload-artifact` і відсутність прямих npm-команд.

## Повідомлення про помилку

Для кожного `FAIL` покажи перевірку, команду, вивід і код завершення. Загальний результат має бути `FAIL`, якщо хоча б одна обов’язкова перевірка не пройшла.

## Правило повторного запуску

Повторний запуск нічого не руйнує і не дублює: цей prompt лише читає файли та запускає перевірки, не редагує і не видаляє файли; дозволений лише повторний генерований `dist/`, який має бути в `.gitignore`.
