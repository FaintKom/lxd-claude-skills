# Скиллы Claude для рабочего места — LXD

Дата: 2026-07-06. Список для согласования установки на рабочей машине.
Профиль: проектировщик образовательного опыта (LXD), b2b/b2c курсы для взрослых, включая IT-направления. Стек: Google Workspace, Adobe (Ps/Ai/Premiere), Claude Code.

Как читать: **Tier 1** — обычные скиллы: папки с текстовыми инструкциями (SKILL.md), без сети, аккаунтов и внешних сервисов — минимальный риск. **Tier 2** — плагины Claude Code (ставятся из маркетплейса, тоже локальные инструкции). **Tier 3** — интеграции: требуют внешних API/аккаунтов, согласуются с ИБ отдельно.

Отбор: все 1655 скиллов машины прогнаны через ревью по профилю задач (5 агентов + ручная редактура), см. [SKILLS-INVENTORY.md](SKILLS-INVENTORY.md).

**Структура репозитория** (всё для ревью внутри, без внешних ссылок):
- `skills/` — 128 скиллов Tier 1 (папки с SKILL.md и вложениями)
- `plugins/` — 5 плагинов Tier 2: `superpowers`, `ui-ux-pro-max`, `diagram-design`, `effective-html`, `caveman`. Шестой, `anthropic-skills`, — это официальные `docx/pptx/pdf/xlsx-official` + `skill-creator`, они в `skills/`.

**Источники (легенда колонки «Источник»):**
- **edu** — [GarethManning/education-agent-skills](https://github.com/GarethManning/education-agent-skills), 165 education-скиллов
- **anthropic** — [anthropics/skills](https://github.com/anthropics/skills), официальные скиллы Anthropic
- **ecc** — [affaan-m/everything-claude-code](https://github.com/affaan-m/everything-claude-code)
- **skills.sh** — community-скилл без точной атрибуции; локальная копия рабочая, ищется по имени на [skills.sh](https://skills.sh)
- **локальный** — кастомный, публичной ссылки нет, переносить папкой с личной машины

---

## Tier 1 — ядро (~70 скиллов, без внешних зависимостей)

### 1. Методология проектирования курсов (~35)

**Фреймворки и цикл разработки:**
| Скилл | Про что | Источник |
|---|---|---|
| `obrazovatelnyy-opyt` ⭐ | База LXD на русском (по книге Смысловой): таксономии, UbD, согласованность, ADDIE, Колб | локальный |
| `action-mapping-designer` | Кэти Мур: от бизнес-цели к поведению — главный b2b-фреймворк | edu |
| `training-needs-analyzer` | Анализ потребностей в обучении до старта проекта | edu |
| `kirkpatrick-evaluation-planner` | Оценка эффективности по 4 уровням — отчётность заказчику | edu |
| `addie-course-designer`, `sam-iterative-course-prototyper` | Классический и итеративный циклы разработки | edu |
| `backwards-design-unit-planner` | От результатов через оценивание к активностям (UbD) | edu |
| `gagne-nine-events-lesson-designer`, `merrill-first-principles-lesson-designer` | Структуры уроков | edu |
| `competency-unpacker`, `competency-framework-translator` | Распаковка компетенций в цели и критерии | edu |
| `learning-progression-builder`, `scope-and-sequence-designer` | Прогрессия и последовательность программы | edu |
| `learner-persona-builder`, `learner-journey-mapper` | Персоны и путь обучающегося | edu |
| `course-onboarding-designer` | Первые 15 минут курса | edu |
| `curriculum-knowledge-architecture-designer`, `kud-knowledge-type-mapper` | Архитектура знаний, Know/Understand/Do | edu |

**Когнитивная наука и мультимедиа:**
| Скилл | Про что | Источник |
|---|---|---|
| `mayer-multimedia-principles-designer` | 12 принципов Мейера — аудит видео и слайдов | edu |
| `cognitive-load-analyser`, `dual-coding-designer` | Когнитивная нагрузка, визуальное + вербальное | edu |
| `retrieval-practice-generator`, `spaced-practice-scheduler`, `interleaving-unit-planner` | Извлечение, интервалы, чередование | edu |
| `worked-example-fading-designer`, `worked-example-to-problem-solving-transition-designer` | Примеры с затуханием поддержки | edu |
| `practice-problem-sequence-designer`, `erroneous-example-designer` | Последовательности задач, обучение на ошибках | edu |
| `elaborative-interrogation-generator`, `self-explanation-prompt-designer` | Глубокая переработка материала | edu |
| `productive-failure-desirable-difficulty-designer` | Продуктивная неудача, желательные трудности | edu |

**Оценивание и обратная связь:**
| Скилл | Про что | Источник |
|---|---|---|
| `criterion-referenced-rubric-generator`, `assessment-validity-checker` | Рубрики и валидность | edu |
| `formative-assessment-loop-designer`, `formative-assessment-technique-selector`, `hinge-question-designer` | Формирующее оценивание | edu |
| `feedback-quality-analyser`, `ai-feedback-design-principles` | Качество обратной связи, в т.ч. автоматической | edu |
| `learning-analytics-interpretation-guide` | Интерпретация учебной аналитики | edu |

**Формат и вовлечение:**
| Скилл | Про что | Источник |
|---|---|---|
| `microlearning-architect`, `mobile-first-learning-designer` | Микрообучение, mobile-first | edu |
| `tutorial-engineer` | Технические туториалы — IT-курсы | skills.sh |
| `engagement-mechanics-designer`, `motivation-diagnostic-task-redesign` | Вовлечение, мотивация (SDT) | edu |
| `intelligent-tutoring-dialogue-designer`, `adaptive-hint-sequence-designer`, `socratic-questioning-sequence-generator` | Диалоговые сценарии, подсказки — AI-тьюторы | edu |
| `scaffolded-task-modifier`, `differentiation-adapter`, `explicit-instruction-sequence-builder` | Скаффолдинг и дифференциация | edu |
| `accessible-learning-designer`, `cross-cultural-task-validity-checker`, `text-complexity-analyser` | Доступность, кросс-культурность, сложность текста | edu |
| `professional-development-session-designer`, `experiential-learning-cycle-designer` | Сессии для взрослых, цикл Колба | edu |

### 2. Производство контента (~22)

**Слайды и документы:**
| Скилл | Про что | Источник |
|---|---|---|
| `frontend-slides` | Веб-презентации reveal-типа с анимацией | skills.sh |
| `pptx-official`, `docx-official`, `pdf-official`, `xlsx-official` | PowerPoint, Word, PDF, Excel | anthropic |
| `theme-factory`, `canvas-design` | Темы оформления, визуальный дизайн | anthropic |
| `brand-guidelines` | Оформление материалов под брендбук заказчика | anthropic |
| `data-storytelling` | Данные → понятная история: отчёты по Киркпатрику, аналитика для заказчика | skills.sh |

**Видео:**
| Скилл | Про что | Источник |
|---|---|---|
| `learning-video-producer` | Видеоурок: сценарий → съёмка → монтаж | skills.sh |
| `remotion`, `remotion-best-practices` | Программное видео и моушн-графика (React) | skills.sh / ecc |
| `manim-video` | Анимации схем и математики | ecc |
| `video-editing` | Монтаж и постобработка | ecc |
| `seek-and-analyze-video`, `audio-transcriber` | Анализ видео, транскрипция | skills.sh |
| `think-aloud-script-generator` | Сценарии «мышления вслух» для скринкастов | edu |

**Веб-интерактивы (кастомный анимационный пак):**
| Скилл | Про что | Источник |
|---|---|---|
| `web-animation-engines` | Роутер: подбирает движок под задачу | локальный |
| `gsap-animation`, `motion-react`, `lottie-web`, `rive-animation` | Анимации, стейт-машины | локальный |
| `konva-canvas`, `fabricjs-canvas` | Canvas-тренажёры с drag-and-drop | локальный |
| `typedjs-text`, `splittingjs-text`, `vivus-svg-drawing` | Текстовые и SVG-эффекты | локальный |
| `text-to-lottie` | Текст → Lottie-анимация | [diffusionstudio/lottie](https://github.com/diffusionstudio/lottie) |
| `web-artifacts-builder`, `frontend-design` | Самодостаточные HTML-интерактивы | anthropic |
| `tailwind-design-system` | Дизайн-система на Tailwind | skills.sh |
| `mermaid-expert` | Диаграммы и схемы | skills.sh |

### 3. Тексты RU/EN (~8)
| Скилл | Про что | Источник |
|---|---|---|
| `ru-deslop` ⭐ | Чистка русского текста от нейрослопа | локальный |
| `humanizer` | Убирает признаки AI-текста | [blader/humanizer](https://github.com/blader/humanizer) |
| `avoid-ai-writing` | Правила письма без AI-штампов | skills.sh |
| `copy-editing`, `professional-proofreader` | Редактура и корректура | skills.sh |
| `beautiful-prose` | Стиль и ритм прозы | skills.sh |
| `article-writing` | Структура и написание статей | ecc |
| `learning-narrative-designer` | Нарратив и сторителлинг в обучении | edu |

### 4. Исследования и база знаний (~7)
| Скилл | Про что | Источник |
|---|---|---|
| `deep-research` | Глубокое многошаговое исследование темы | skills.sh |
| `citation-management` | Оформление и управление цитированием | skills.sh |
| `source-credibility-evaluation-protocol` | Оценка достоверности источников | edu |
| `media-literacy-deconstruction-protocol` | Разбор медиа-сообщений | edu |
| `youtube-summarizer` | Конспекты видео с YouTube | skills.sh |
| `wiki-researcher` | Сбор материала в базу знаний | skills.sh |
| `skill-creator` | Дистилляция книг и методик в новые скиллы | anthropic |

---

## Tier 1 — приложение: расширенная методология (по желанию, ~25)

Тот же нулевой риск, более нишевые (частично школьная педагогика — пригодятся для отдельных форматов). Источник всех — **edu**:
`academic-language-sentence-frame-generator`, `checking-for-understanding-protocol-designer`, `coherent-rubric-logic-builder`, `critical-thinking-task-designer`, `culturally-responsive-teaching-designer`, `dialogic-teaching-move-generator`, `digital-worked-example-sequence`, `discussion-protocol-selector`, `error-analysis-protocol`, `flow-state-condition-designer`, `gap-analysis-from-student-work`, `goal-setting-protocol-designer`, `implementation-intention-designer`, `instructional-coaching-conversation-guide`, `interdisciplinary-real-world-connection-mapper`, `language-demand-analyser`, `lesson-observation-protocol-designer`, `lesson-opening-designer`, `lesson-study-cycle-designer`, `pedagogical-content-knowledge-developer`, `reflective-practice-prompt-generator`, `self-efficacy-builder-sequence`, `self-regulation-scaffold-generator`, `study-strategy-selector`, `three-part-lesson-designer`, `vocabulary-tiering-tool`

---

## Tier 2 — плагины Claude Code

| Плагин | Про что | Ссылка |
|---|---|---|
| `anthropic-skills` | Официальные docx/pptx/pdf/xlsx + skill-creator | маркетплейс [claude-plugins-official](https://github.com/anthropics/claude-plugins-official) |
| `superpowers` | Рабочие процессы: brainstorming, планирование, верификация | [obra/superpowers](https://github.com/obra/superpowers) |
| `diagram-design` | Архитектурные и концептуальные диаграммы | [cathrynlavery/diagram-design](https://github.com/cathrynlavery/diagram-design) |
| `ui-ux-pro-max` | Дизайн-система, стайлинг веб-деков и лендингов | [nextlevelbuilder/ui-ux-pro-max-skill](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill) |
| `effective-html` | Самодостаточные HTML-файлы (интерактивы, планы) | [plannotator/effective-html](https://github.com/plannotator/effective-html) |
| `caveman` | Экономия токенов в ответах | [JuliusBrussee/caveman](https://github.com/JuliusBrussee/caveman) |

---

## Tier 3 — интеграции (внешние сервисы, отдельное согласование с ИБ)

| Позиция | Что требует | Зачем |
|---|---|---|
| `google-docs/slides/sheets/drive/gmail-automation` | Рабочий Google-аккаунт, MCP (Rube/Composio) или Google Workspace MCP | Автоматизация рабочего стека |
| `notebooklm` | Google-аккаунт | Источники → конспекты/подкасты |
| `image-studio` / `nanobanana-ppt-skills` | API генерации изображений | Иллюстрации к курсам и слайдам |
| Playwright MCP | Локальный браузер | Скриншоты, тест веб-интерактивов |
| context7 MCP | Внешний API документации | Свежие доки библиотек для IT-курсов |

---

## Сознательно НЕ включено
Личные интеграции (Telegram, Notion, Upwork), security/pentest-пак, SaaS-автоматизации (CRM/маркетинг), Azure/облака, геймдев, персоны, health-аналитика, `espanol` (личное обучение).

## Установка
- Tier 1: публичные — из репозиториев по ссылкам; локальные и skills.sh-копии — перенести папки в `~/.claude/skills/` (или `.claude/skills/` проекта) с личной машины, см. [SKILLS-INVENTORY.md](SKILLS-INVENTORY.md)
- Tier 2: `claude plugin install <name>` из маркетплейса
- Tier 3: конфиг MCP в `~/.claude.json` после согласования доступов

Список программ и библиотек для рабочей машины вынесен в отдельное сообщение (Adobe CC, Claude Code, Node.js, Python, ffmpeg, SCORM-авторинг, AI-видео/озвучка).
