# Инвентаризация скиллов и библиотек — 2026-07-03

Полный обзор всех скиллов Claude на этой машине: где лежат, откуда взялись, что кастомное.

---

## 1. Кастомные скиллы из книг и учебников ⭐

### espanol — `F:\skills for claude\espanol\`
Супер-справочник испанского для русскоговорящих, A1→B2. Собран из **4 учебников (~1700 страниц)**:
- «Полный курс» Гонсалес-Алимовой
- Дышлевая: для начинающих, для продолжающих, Gramática en uso

Структура: SKILL.md (шпаргалка) + `references/` (4 дистиллята по книгам) + `raw_text/` + `digests/` + `tessdata/` (OCR через Tesseract) + `research/`.

### obrazovatelnyy-opyt — `C:\Users\faint\.claude\skills\obrazovatelnyy-opyt\`
База знаний по книге **«Проектирование образовательного опыта» Сони Смысловой** (School of Education, 2022, ~320 стр., 7 глав). Педдизайн / LXD: таксономии Блума и Марцано, UbD, конструктивная согласованность, 4C/ID, цикл Колба, ADDIE, карта опыта. Структура: ядро в SKILL.md + `chapters/*.md` (все главы дистиллированы).

---

## 2. Кастомный пак: веб-анимационные библиотеки (25 скиллов, создан 2026-06-14)

Лежат в `C:\Users\faint\.claude\skills\`, сделаны из документации библиотек. Демо и скриншоты — в `F:\skills for claude\animation-demos\` + jpeg в корне проекта.

| Скилл | Библиотека / назначение |
|---|---|
| web-animation-engines | **Роутер**: выбирает движок и нужный скилл |
| gsap-animation | GSAP + ScrollTrigger — таймлайны, DOM |
| motion-react | Motion (Framer Motion) — React, жесты |
| react-spring-animation | react-spring — физика пружин |
| lenis-smooth-scroll | Lenis — инерционный скролл |
| aos-scroll | AOS — reveal при скролле |
| scrollreveal-scroll | ScrollReveal |
| scrollmagic-scroll | ScrollMagic (legacy) |
| pixijs-animation | PixiJS — 2D WebGL, партиклы, спрайты |
| ogl-webgl | OGL — минимальный WebGL/GLSL |
| curtainsjs-webgl | curtains.js — шейдеры поверх DOM/img |
| konva-canvas | Konva — canvas-редакторы, drag/resize |
| fabricjs-canvas | Fabric.js — дизайн-тулзы, сериализация |
| paperjs-canvas | Paper.js — векторная генеративка |
| twojs-animation | two.js — renderer-agnostic 2D |
| svgjs-animation | SVG.js — программный SVG |
| vivus-svg-drawing | Vivus — «самрисующийся» SVG |
| snapsvg-animation | Snap.svg — маски, морфинг путей |
| theatrejs-editor | Theatre.js — визуальный keyframe-редактор |
| rive-animation | Rive — стейт-машины анимаций |
| lottie-web | Lottie — проигрывание AE/JSON |
| typedjs-text | Typed.js — печатающийся текст |
| splittingjs-text | Splitting.js — по-буквенные реворды |
| blotter-text | Blotter — шейдерный текст |
| text-to-lottie | Генерация Lottie из текста |

---

## 3. Прочие кастомные / индивидуально добавленные скиллы

| Скилл | Где | Дата | Что |
|---|---|---|---|
| ru-deslop | `~\.claude\skills` | 2026-06-30 | Чистка русского текста от нейрослопа и англицизмов (кастомный) |
| career-ops | `F:\skills for claude\career-ops\.claude\skills` | — | AI-центр поиска работы: офферы, CV, порталы, трекинг (кастомный) |
| improve | `F:\lms\.claude\skills` | — | Проектный скилл GrassLMS |
| miro-mcp, miro-platform, miro-code-review, miro-spec-guide, documentation-structure | `F:\practico\.claude\skills` | — | 5 проектных скиллов Practico (Miro) |
| humanizer | `~\.claude\skills` | 2026-06-30 | Установлен (community) |
| officecli | `~\.claude\skills` | 2026-06-06 | Office-документы через officecli CLI |
| bencium-controlled-ux-designer, bencium-innovative-ux-designer, design-audit | `~\.claude\skills` | 2026-05-20 | UX-дизайн пак (Bencium) |
| unity-mcp-skill | `~\.claude\skills` | 2026-05-17 | Unity MCP |
| learned | `~\.claude\skills` | 2026-04-26 | Пустая заготовка (можно удалить) |

---

## 4. Личная директория скиллов — `C:\Users\faint\.claude\skills\` — **1405 скиллов**

Две массовые установки из community-коллекций:
- **2026-03-17: 1246 скиллов**
- **2026-04-09: 125 скиллов**

Основные кластеры (примерно):
- **Языки/фреймворки**: python-pro, golang-pro, rust-pro, typescript-expert, react-*, nextjs-*, angular-*, laravel-*, django-*, fastapi-*, flutter, swiftui, kotlin, unity, unreal, godot, bevy
- **Azure SDK пак** (~100): azure-ai-*, azure-storage-*, azure-cosmos-*, azure-keyvault-* и т.д.
- **Security / pentest** (~60): sql-injection-testing, xss-html-injection, metasploit-framework, burp-suite-testing, active-directory-attacks, windows/linux-privilege-escalation, shodan, wireshark, red-team-*
- **Образование / LXD** (~100): addie-course-designer, backwards-design-unit-planner, gagne-nine-events, merrill-first-principles, mayer-multimedia-principles, worked-example-*, retrieval-practice-generator, formative-assessment-*, culturally-responsive-* и др.
- **SaaS-автоматизации** (~80): *-automation (hubspot, klaviyo, zendesk, jira, notion, slack, salesforce…)
- **SEO/маркетинг**: seo-* (~20), marketing-*, copywriting, growth-engine
- **AI/LLM инжиниринг**: rag-*, prompt-engineering, langchain, langgraph, mcp-builder, agent-*, llm-*
- **Three.js пак**: threejs-* (12: fundamentals, shaders, materials, lighting…)
- **Odoo пак** (~25): odoo-*
- **n8n пак** (7): n8n-*
- **Персоны**: warren-buffett, steve-jobs, elon-musk, bill-gates, sam-altman, karpathy, hinton, sutskever, lecun (4 варианта)
- **Здоровье**: *-analyzer (sleep, nutrition, fitness, mental-health…)
- **Юр./нишевые**: advogado-*, leiloeiro-* (порт.), junta-leiloeiros и пр.
- **Fal.ai, Hugging Face, remotion, makepad, robius паки**

Полный сырой список: см. `SKILLS-INVENTORY-FULL-LIST.txt` рядом с этим файлом.

---

## 5. Плагины Claude Code — `C:\Users\faint\.claude\plugins\cache\` (11 шт.)

| Плагин | SKILL.md файлов | Что |
|---|---|---|
| everything-claude-code | 459 | Мега-бандл: ~183 скилла + 48 агентов (ECC) |
| superpowers-dev | 14 | Воркфлоу: brainstorming, TDD, systematic-debugging, writing-plans… |
| caveman | 11 | Экономия токенов, режим caveman (активен) |
| minimalist-entrepreneur | 10 | По книге Сахила Лавингии «The Minimalist Entrepreneur» |
| reflexioai | 9 | — |
| thedotmack | 8 | claude-mem (персистентная память) |
| ui-ux-pro-max-skill | 7 | UI/UX: design-system, banner, slides, ui-styling |
| obsidian-skills | 5 | Управление Obsidian vault |
| ponytail | 4 | «Ленивый сеньор» — минимализм кода (активен) |
| effective-html | 3 | — |
| diagram-design | 1 | Архитектурные диаграммы за 60 сек |

---

## 6. Клонированные репозитории — `F:\skills for claude\` (29 папок)

Каталог с инструкциями установки: [GUIDE.md](GUIDE.md). Ключевые:
- **Токены/контекст**: caveman, squeez (сжатие вывода), graperoot (семантический граф кода)
- **Память/знания**: claude-mem, graphify (граф знаний), open-brain (16 скиллов), obsidian-skills
- **Воркфлоу**: superpowers (14), get-shit-done, ecc-tools, ClawTeam, mux, loop (агентные скиллы: music, TTS/STT, sound-effects)
- **UI/дизайн**: ui-ux-pro-max-skill, diagram-design, magic-mcp, FossFLOW, openscreen
- **MCP**: n8n-mcp, dokploy-mcp
- **Прочее**: awesome-claude-code, skills (minimalist-entrepreneur), context7
- **Кастомное**: espanol (раздел 1), career-ops, animation-demos

---

## 7. MCP-серверы и CLI-инструменты

По `F:\sources\SETUP-GUIDE.md` и папкам F:\ :
- **google-mcp** (`F:\google-mcp`) — Drive/Docs/Sheets/Gmail/Calendar; секреты в `F:\google-secrets\`
- **telegram-mcp** (`F:\telegram-mcp`) — личный аккаунт через MTProto
- **notion-mcp** (`F:\notion-mcp`) — integration token
- **upwork-mcp** (`F:\upwork-mcp`)
- **unityMCP**, **playwright** (+ docker-вариант), **filesystem** (Claude Desktop)
- CLI: **squeez** (сжатие терминального вывода), **graperoot/dgc**, **graphify**, **officecli**

---

## 8. Пайплайн дистилляции знаний — `F:\sources\`

Отдельная «фабрика» превращения источников в базу знаний (та же методика, что и скиллы из книг):
- Источники: skillbox (3845 .md), prog_tools (1917 постов TG), заметки из Telegram/LinkedIn/Instagram/фото-OCR (~730)
- Фазы: extraction → verification → ingest (Voyage embeddings + Postgres)
- Выход: `knowledge_entries` на grasslms.online/knowledge

---

## Итого

| Категория | Кол-во |
|---|---|
| Кастомные скиллы из книг | **2** (espanol — 4 учебника; obrazovatelnyy-opyt — 1 книга) |
| Кастомный пак анимационных библиотек | **25** |
| Прочие кастомные/проектные | ~10 (ru-deslop, career-ops, improve, 5×practico…) |
| Личная директория `~\.claude\skills` | **1405** |
| Плагины (11 шт.) | ~530 SKILL.md суммарно |
| Клонированные репо со скиллами | 29 папок (566+ SKILL.md) |
