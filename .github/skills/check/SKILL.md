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
2. Перевір наявність трьох skills (`project-scaffold`, `build-and-test`, `github-actions`) і шести commands (`git-init`, `create-project`, `create-build`, `create-actions`, `check`, `lab-init`) як `.github/skills/<назва>/SKILL.md`.
3. Перевір структуру проєкту, scripts, `src/add.ts`, `tests/add.test.ts`, `BasicAddition`.
4. Запусти `npm run build` і `npm test`.
5. Перевір, що `ci.yml` містить `ubuntu-latest`, `windows-latest`, `macos-latest` і викликає лише `ci.sh` або `ci.bat`, без прямих npm-команд.
6. Перевір, що `node_modules/` і `dist/` є в `.gitignore` та не відстежуються Git.
7. Перевір, що `.gitattributes` містить правило `*.sh text eol=lf`.
8. Перевір наявність `.github/agents/build-engineer.agent.md`. У ньому мають бути розділи «Роль», «Заборонені дії» (обмеження) і «Критерії успішного завершення».
9. Для трьох skills (`project-scaffold`, `build-and-test`, `github-actions`) перевір у frontmatter поля `name` і `description`, розділи про вхідні дані, результат і перевірку результату, а також відсутність поля `disable-model-invocation`.
10. Для шести commands (`git-init`, `create-project`, `create-build`, `create-actions`, `check`, `lab-init`) перевір наявність поля `disable-model-invocation: true` у frontmatter.
11. Перевір `.github/skills/lab-init/SKILL.md`: посилання на commands мають йти в порядку `git-init` → `create-project` → `create-build` → `create-actions` → `check`.
12. Перевір, що `node_modules/`, `dist/`, `coverage/`, `.env`, `.pem`- і `.key`-файли не відстежуються Git. Також перевір файли з ключами або токенами в назві.
13. Перевір наявність `docs/ai-log.md`; файл має містити таблицю з колонками про запит, проблему і рішення та хоча б один заповнений рядок.
14. Виведи таблицю з колонками `Перевірка`, `Результат`, `Деталі` та значеннями `PASS`/`FAIL`, потім загальний результат.
15. Для кожної перевірки з пунктів 8-13 виведи окремий рядок у таблиці результатів:
   - Agent manifest;
   - Skills;
   - Commands;
   - Оркестрація lab-init;
   - Відстежувані секрети й генеровані файли;
   - Журнал AI.

## Очікувані файли

Перевіряються `package.json`, `package-lock.json`, `tsconfig.json`, `README.md`, `src/`, `tests/`, `ci.sh`, `ci.bat`, `.gitignore`, `.gitattributes`, `.github/agents/` і `.github/skills/`; нічого з переліченого не створюється чи не редагується.

## Команди перевірки

```text
git status --short
Test-Path package.json, package-lock.json, tsconfig.json, README.md, src, tests, ci.sh, ci.bat
npm run build
npm test
git ls-files node_modules dist
git check-ignore -q node_modules/
git check-ignore -q dist/
Select-String -Path .github/agents/build-engineer.agent.md -Pattern 'Роль|Заборонені дії|Критерії успішного завершення'
Get-Content .github/skills/project-scaffold/SKILL.md, .github/skills/build-and-test/SKILL.md, .github/skills/github-actions/SKILL.md | Select-String '^(name|description):|Вхід|Результат|Перевірка'
Get-Content .github/skills/git-init/SKILL.md, .github/skills/create-project/SKILL.md, .github/skills/create-build/SKILL.md, .github/skills/create-actions/SKILL.md, .github/skills/check/SKILL.md, .github/skills/lab-init/SKILL.md | Select-String 'disable-model-invocation: true'
(Get-Content .github/skills/lab-init/SKILL.md -Raw) -match '(?s)git-init.*create-project.*create-build.*create-actions.*check'
git ls-files -- node_modules dist coverage .env .env.* '*.pem' '*.key' '*key*' '*token*' '*secret*'
Select-String -Path docs/ai-log.md -Pattern '\|.*Запит.*\|.*Проблема.*\|.*Рішення.*\||\|[ \t]*[0-9]+[ \t]*\|'
```

Для YAML перевір окремо всі три runner-и, `ci.sh`/`ci.bat`, `checkout`, `setup-node`, `upload-artifact` і відсутність прямих npm-команд.

Перевірка Agent manifest має бути PASS лише за одночасної наявності всіх трьох розділів.

Для skills перевір frontmatter окремо для кожного файлу. Поле `disable-model-invocation` має бути відсутнім у всіх трьох skill-файлах.

Для commands перевір frontmatter окремо для кожного із шести файлів. Усі вони мають містити `disable-model-invocation: true`.

Перевірка оркестрації lab-init має розпізнавати послідовність посилань і етапів у одному файлі. Наявність усіх команд поза правильним порядком є FAIL.

Для Git перевірки `git ls-files -- node_modules dist coverage .env .env.* '*.pem' '*.key' '*key*' '*token*' '*secret*'` не має повернути жодного відстежуваного файлу. Також перевірте відсутність відстежуваних `.env`-файлів, ключів і токенів у інших шаблонах імен.

Перевірка AI-журналу має бути PASS лише якщо `docs/ai-log.md` існує, має markdown-таблицю з колонками запиту, проблеми і рішення та містить хоча б один рядок із непорожніми даними.

## Повідомлення про помилку

Для кожного `FAIL` покажи перевірку, команду, вивід і код завершення. Загальний результат має бути `FAIL`, якщо хоча б одна обов’язкова перевірка не пройшла.

## Правило повторного запуску

Повторний запуск нічого не руйнує і не дублює: ця команда лише читає файли та запускає перевірки, не редагує і не видаляє файли; дозволений лише повторний генерований `dist/`, який має бути в `.gitignore`.
