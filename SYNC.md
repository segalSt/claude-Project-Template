# Синхронизация RU / EN шаблона · Template RU / EN sync

**Русская версия — источник истины.** Файлы:
`README.md`, `new-project-setup.md`, `migration.md` + `templates/` (10 файлов, вкл. `token-economy.md`, `qa-findings.md`).

**Английская копия — `template-en/`** (зеркало той же структуры).

**Правило:** при ЛЮБОМ изменении русского шаблона в том же заходе
актуализировать соответствующий файл в `template-en/` — перевод, та же
структура, та же карта командных токенов. Расхождение RU/EN считается
устареванием (правила актуализации).

Карта токенов: токены команд теперь ОДИНАКОВЫ в RU и EN (латиница) —
`-start`, `-commands`, `-plan`, `-QA`, `-new`, `-tbd`, `-commit`, `-drift`,
`-park`. Аргументы — с двойным дефисом (`--s`, `--a`, `--c`, `--m`, `--t`,
`--fix`, `--step`, `--day`, `--full`, `--hide-done`); команды — с одним.
Токены команд полностью совпадают в RU и EN (латиница).

---

**Russian version is the source of truth.** Files: `README.md`,
`new-project-setup.md`, `migration.md` + `templates/` (10 files, incl. `token-economy.md`, `qa-findings.md`).
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
