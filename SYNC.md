# Синхронизация RU / EN шаблона · Template RU / EN sync

**Русская версия — источник истины.** Файлы:
`README.md`, `new-project-setup.md`, `migration.md` + `templates/` (9 файлов).

**Английская копия — `template-en/`** (зеркало той же структуры).

**Правило:** при ЛЮБОМ изменении русского шаблона в том же заходе
актуализировать соответствующий файл в `template-en/` — перевод, та же
структура, та же карта командных токенов. Расхождение RU/EN считается
устареванием (правила актуализации).

Карта токенов (RU → EN):
`-старт`→`-start`, `-коммит`→`-commit`, `-команды`→`-commands`,
`-тбд`→`-tbd`, `-план`→`-plan`, `--шаг`→`--step`, `-new`→`-new`,
`-миграция`→`-migrate`;
под-флаги `-plan`: `-s`→`-s`, `-а`→`-a`, `--м`→`--m`, `-к`→`-c`,
`--н`→`--hide-done`; под-флаги `-new`: `-т`→`-t`, `-s`→`-s`; под-флаг
`-команды`: `-к`→`-n` (создать команду); `-QA`→`-QA`.
Примечание: `-к` контекстно-зависим — под `-plan` это `-c` (concise),
под `-команды` это `-n` (new command).

---

**Russian version is the source of truth.** Files: `README.md`,
`new-project-setup.md`, `migration.md` + `templates/` (9 files).
**English copy — `template-en/`** (mirror).

**Rule:** on ANY change to the Russian template, update the matching file
under `template-en/` in the same pass — translation, same structure, same
command-token map above. An RU/EN divergence counts as staleness.

**Проверка паритета / parity check:** расхождение RU/EN по СОСТАВУ файлов
(файл есть в одной ветке и отсутствует в другой) должно ловиться проверкой
паритета списка файлов там, где это возможно (аналог правила 12 в
`actualization-rules.md` — дрейф, ловимый машинно, ловится проверкой, а не
дисциплиной). Смысловую полноту перевода машина не проверит — это остаётся
на ревью, но пропажу/лишний файл поймать можно и нужно.
