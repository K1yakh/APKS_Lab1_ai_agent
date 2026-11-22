---
name: github-actions
description: "Створення GitHub Actions CI для develop і master на Ubuntu, Windows та macOS через matrix, Node.js 22, локальні CI-скрипти й artifact dist."
---

# GitHub Actions

## Коли застосовувати

Застосовуй skill, коли проєкту потрібна кросплатформна GitHub Actions перевірка на push і pull request для гілок `develop` та `master`. Перед створенням workflow перевір готовність `ci.sh`, `ci.bat` і `package-lock.json`.

## Вхідні дані

- кореневий репозиторій із `ci.sh` і `ci.bat`;
- Node.js 22 та npm;
- зібраний каталог `dist/`;
- назви гілок `develop` і `master`;
- вимога запускати CI лише через локальні скрипти.

## Результат

Створи або онови:

- `.github/workflows/ci.yml` — workflow на `push` і `pull_request` у `develop` та `master`;
- одну job із matrix для `ubuntu-latest`, `windows-latest`, `macos-latest` і `fail-fast: false`;
- кроки checkout, setup-node 22, виклик `./ci.sh` або `ci.bat`, upload-artifact із `dist/`;
- виконуваний `ci.sh`, підтверджений через `git update-index --chmod=+x ci.sh`.
- кодування `.github/workflows/ci.yml` — UTF-8 без BOM.

## Кроки

1. Перевір, що `ci.sh` і `ci.bat` існують, а локальні команди CI вже проходять.
2. Перевір права `ci.sh`; виконай `git update-index --chmod=+x ci.sh`, якщо executable bit відсутній.
3. Створи `.github/workflows/ci.yml` із тригерами `push` і `pull_request` та фільтрами `develop` і `master`.
4. Додай одну job із `strategy.fail-fast: false` і `strategy.matrix.os: [ubuntu-latest, windows-latest, macos-latest]`.
5. Додай checkout і setup-node з версією `22` та кешуванням npm лише за коректної наявності lockfile.
6. Для Unix runner-ів викликай `./ci.sh`, для Windows задавай `shell: cmd` і викликай `ci.bat` як `run: ci.bat`. Не записуй прямі `npm`-команди в YAML.
7. Додай upload-artifact для каталогу `dist/` після успішної збірки.
8. Перевір YAML, три runner-и, `fail-fast: false`, обидва CI-виклики, тригери, відсутність прямих npm-команд і кодування UTF-8 без BOM.

## Перевірка результату

- `.github/workflows/ci.yml` існує і має валідний YAML;
- workflow запускається на `push` і `pull_request` у `develop` та `master`;
- є одна job із усіма трьома runner-ами;
- у `strategy` присутній `fail-fast: false`;
- присутні checkout, setup-node 22 і upload-artifact для `dist/`;
- Unix job викликає `./ci.sh`, Windows job використовує `shell: cmd` і викликає `ci.bat` через `run: ci.bat`;
- `.github/workflows/ci.yml` закодований у UTF-8 без BOM;
- YAML не містить прямих `npm ci`, `npm run`, `npm test` або інших команд збірки;
- `git ls-files --stage ci.sh` підтверджує executable mode `100755`.

## Приклад входу та очікуваний результат

**Вхід:** репозиторій із робочими `ci.sh`, `ci.bat`, `package-lock.json` і `dist/` після build.

**Очікуваний результат:** `.github/workflows/ci.yml` запускає одну matrix job на Ubuntu, Windows і macOS для обох подій та завантажує `dist/` як artifact.

## Типовий сценарій помилки і що робити

**Помилка:** GitHub Actions не може виконати `./ci.sh` на Ubuntu або macOS.

**Що робити:** зупинись, покажи workflow, команду перевірки та її вивід, виконай `git update-index --chmod=+x ci.sh`, перевір mode через `git ls-files --stage ci.sh` і повтори локальну перевірку. Не замінюй виклик прямими npm-командами у YAML.

**Помилка:** «ci.bat is not recognized на windows-latest» → відсутній `shell: cmd`.
