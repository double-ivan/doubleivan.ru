# doubleivan.ru

Сайт подкаста «Два Ивана» на Hugo + тема PaperMod (через git submodule в `themes/PaperMod`). Деплой через `rclone` на reg.ru. Аудио и обложки лежат на Mave CDN, сайт — это только индекс выпусков и статичные страницы.

## Структура

- `content/podcasts/YYYY_MM_DD_NNNN_<slug>.md` — все выпуски, по одному файлу. Frontmatter содержит `mp3`, `length`, `duration`, `image` (всё с Mave CDN), `season`, `episode`, `timecodes`. Тело — короткий разговорный абзац-описание.
- `content/about.md`, `content/archive.md` — статические страницы.
- `layouts/podcasts/`, `layouts/_default/`, `layouts/partials/` — переопределения шаблонов поверх PaperMod.
- `static/` — обложки, фавиконки и прочая статика.
- `config.yaml` — конфиг Hugo (baseURL, меню, socialIcons).
- `themes/PaperMod` — submodule, не редактируй напрямую.
- `public/` — output `hugo build`, в гите игнорируется.

## Команды (через mise)

```bash
mise run build    # rm -rf public && hugo build
mise run deploy   # build + rclone sync public regru:/www/doubleivan.ru/
```

Для локального превью используй `hugo server` напрямую.

## Сезоны

Каждый календарный год = новый сезон. 2025 → 3, 2026 → 4. При создании выпуска сверяйся с последним файлом в нужном году.

## Создание нового выпуска

Для подготовки описания нового выпуска есть скилл `/podcast-episode` (`.claude/skills/podcast-episode/SKILL.md`). Он покрывает весь флоу: чтение последних выпусков для сверки формата, уточнение имени гостя и темы, создание ветки, заполнение frontmatter (включая `length` через `curl -sI` и `duration` через `ffprobe`), укрупнение Riverside-таймкодов, синтез интро-абзаца, и подготовка plain-text описания для Mave dashboard.

Скилл триггерится сам на любой запрос про новый выпуск — не нужно звать его явно.

## Что НЕ делать

- Не делай `git add`/`commit`/`push` для выпусков — пользователь сам делает ревью и мерж.
- Не редактируй `themes/PaperMod` (submodule на upstream).
- Не выдумывай URL обложки/mp3 — если ссылки нет, оставляй поля пустыми.
