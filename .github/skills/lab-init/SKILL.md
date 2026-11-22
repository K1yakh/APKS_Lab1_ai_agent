---
name: lab-init
description: "Повністю підготувати й перевірити TypeScript Hello World через послідовність build-engineer commands"
disable-model-invocation: true
---
# Lab-init
Виконуй за правилами агента [build-engineer](../../agents/build-engineer.agent.md).

## Передумови

- Робоча папка є коренем репозиторію.
- Доступні commands `git-init`, `create-project`, `create-build`, `create-actions`, `check`.
- Поточна гілка має бути `feature/...`; якщо це `develop` або `master`, зупинися з повідомленням і нічого не створюй.
- Користувач дозволив виконати повну підготовку, але не дозволив `git push`.

## Дії

Виконуй послідовно та після кожного кроку перевіряй результат:

1. [`git-init`](../git-init/SKILL.md) — перевір Git, `.gitignore`, status і гілку.
2. [`create-project`](../create-project/SKILL.md) — застосуй [`project-scaffold`](../project-scaffold/SKILL.md).
3. [`create-build`](../create-build/SKILL.md) — застосуй [`build-and-test`](../build-and-test/SKILL.md).
4. [`create-actions`](../create-actions/SKILL.md) — застосуй [`github-actions`](../github-actions/SKILL.md).
5. [`check`](../check/SKILL.md) — виконай повну read-only перевірку та таблицю PASS/FAIL.

Після кожного кроку зупиняйся, якщо перевірка не пройшла. Наприкінці надай підсумковий звіт із виконаними кроками, файлами, командами та результатами. Не виконуй `git push`.

## Очікувані файли

Очікуй `package.json`, `package-lock.json`, `tsconfig.json`, `README.md`, `src/hello.ts`, `src/add.ts`, `tests/add.test.ts`, `ci.sh`, `ci.bat`, `.gitignore`, `.github/workflows/ci.yml`, три skills і шість commands.

## Команди перевірки

```text
git status --short --branch
npm ci
npm run build
npm test
Windows: .\ci.bat
Linux/macOS: ./ci.sh
Якщо `ci.sh` ще не доданий у Git, спочатку виконай `git add ci.sh` і `git update-index --chmod=+x ci.sh`; очікуваний режим — `100755`.
git ls-files --stage ci.sh
```

Наприкінці застосуй усі read-only перевірки з [`check`](../check/SKILL.md) і виведи загальну таблицю.

## Повідомлення про помилку

Якщо крок помилився, негайно зупинись і напиши назву кроку, prompt, точну команду, вивід, код завершення та причину. Не переходь до наступного кроку й не оголошуй роботу успішною.

## Правило повторного запуску

Повторний запуск нічого не руйнує і не дублює: кожен підprompt спочатку перевіряє наявні файли, зберігає сумісні налаштування й змінює лише невідповідне; `check` не вносить змін.
