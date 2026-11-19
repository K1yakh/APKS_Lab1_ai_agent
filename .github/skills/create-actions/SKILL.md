---
name: create-actions
description: "Створити кросплатформний GitHub Actions workflow за skill github-actions"
disable-model-invocation: true
---
# Create Actions
Виконуй за правилами агента [build-engineer](../../agents/build-engineer.agent.md).

## Передумови

- `ci.sh`, `ci.bat` і `package-lock.json` існують.
- Локальна збірка та тести проходять.
- Доступний skill [`github-actions`](../github-actions/SKILL.md).

## Дії

1. Прочитай і застосуй [`github-actions`](../github-actions/SKILL.md).
2. Створи `.github/workflows/ci.yml` для `push` і `pull_request` у `develop` та `master`.
3. Додай одну job із matrix `ubuntu-latest`, `windows-latest`, `macos-latest`.
4. Додай checkout, setup-node 22, виклик відповідного CI-скрипта та upload-artifact для `dist/`.
5. Виконай `git update-index --chmod=+x ci.sh`.
6. Перевір, що YAML не містить прямих npm-команд.

## Очікувані файли

- `.github/workflows/ci.yml`;
- executable mode `100755` для `ci.sh` у Git index.

## Команди перевірки

```text
git update-index --chmod=+x ci.sh
git ls-files --stage ci.sh
Select-String -Path .github/workflows/ci.yml -Pattern 'ubuntu-latest|windows-latest|macos-latest'
Select-String -Path .github/workflows/ci.yml -Pattern 'ci.sh|ci.bat|upload-artifact'
```

Перевір також відсутність у YAML `npm ci`, `npm run` і `npm test`.

## Повідомлення про помилку

Зупинися, покажи команду, вивід і код завершення. Вкажи, чи проблема у YAML, runner-ах, виклику скрипта або правах `ci.sh`; не приховуй помилки прямими командами в workflow.

## Правило повторного запуску

Повторний запуск нічого не руйнує і не дублює: зберігай наявні job-и та triggers, оновлюй лише невідповідні частини, не додавай дублікати workflow.
