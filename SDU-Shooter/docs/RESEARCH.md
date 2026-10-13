# Ресерч: как делают браузерные 3D-игры с ИИ

Дата: октябрь 2026. Много источников — блоги и вендорские статьи, поэтому цифры ниже ориентировочные.

## 1. Реальные примеры

**fly.pieter.com (Pieter Levels, февраль 2025)** — самый известный пример.
- Браузерный 3D-авиасимулятор с воздушными боями. Автор раньше не делал игр. Сделано на Cursor + Three.js, серверный код писал Grok 3. Первая версия заняла от 30 минут до 3 часов, источники расходятся.
- Пик — около 17 000 игроков одновременно. Деньги в основном с рекламы внутри игры: дирижабли и шары с логотипами. Цифры дохода в разных источниках от $5k до $87k в месяц.
- Главная оговорка: у автора около 625k подписчиков. Без аудитории так бы не взлетело. Ещё ему пришлось несколько дней чинить уязвимость, из-за которой игра рекламировала неприличный сайт.
- Вывод для нас: простая идея + мультиплеер + мгновенный запуск по ссылке работают. Но аудиторию надо заранее продумать, у нас это студенты SDU.

**Vibe Jam 2026** — геймджем, где минимум 90% кода пишет ИИ.
- Прошёл с 1 апреля по 1 мая 2026, 945 заявок. Спонсоры: Cursor, Bolt.new, Glif, Tripo AI.
- Примерно каждая седьмая мультиплеерная игра использовала Colyseus. То есть Three.js + Colyseus — обычный стек для такой игры.

**Raccoon Heist (Саймон Уиллисон)** — 3D-игра из двух концепт-картинок и одного промпта.
- Агенту дали работать самостоятельно, часто коммитить, вести файл с заметками и проверять игру через Playwright на десктопе и мобильном. Картинки генерировались отдельно и коммитились как готовые файлы.

**Студенческие игры про свой кампус**
- MacGuessr (McMaster) — GeoGuessr по фото кампуса. Больше 3000 игр, взлетел после поста на Reddit.
- Кампусы в Minecraft (Sussex, Ashoka), StaffsVerse в Fortnite, 8-битный DMUniverse.
- Вывод: «наш кампус в игре» сам по себе цепляет студентов.

## 2. Почему браузерные игры становятся популярными

По спискам популярных игр (Krunker.io, Shell Shockers, 1v1.LOL, Skribbl.io, Gartic Phone, GeoGuessr, Slither.io):
- Не надо ничего скачивать и регистрироваться. Работает на любом ноутбуке и телефоне.
- Короткие матчи, можно сыграть между парами.
- Друг заходит по ссылке, комнаты приватные.
- Простые правила, понятные с первого взгляда.
- Яркий узнаваемый стиль.
- Игра сама создаёт смешные моменты, которые хочется заснять: Among Us, Lethal Company, Gartic Phone. Стримы и клипы распространяют игру лучше рекламы.

## 3. Как работают с ИИ (лучшие практики)

1. Сначала короткий дизайн-бриф и основной цикл игры, потом код.
2. Одна сессия = одна фича. Агент коммитит часто и ведёт `NOTES.md` (или `artifacts/game-progress.md`).
3. Агент проверяет игру через Playwright: скриншоты, бот-плейтест, мобильный экран. Проверка «на глаз» в чате не считается.
4. Three.js хорошо знаком моделям: цикл рендера, коллизии, загрузка ассетов — типовые паттерны.
5. API-ключи генераторов (Tripo, Meshy, ElevenLabs, Gemini) не кладутся в клиентский код. Генерация — отдельным скриптом, результат коммитится как файл.
6. Автор VIBE_WEAVERS держал всё в одном HTML-файле, чтобы ИИ видел весь контекст. Для прототипа это нормально, но для нашей игры лучше модульная структура на Vite + TypeScript.

## 4. Стек

| Что | Выбор | Почему |
|---|---|---|
| Рендер | Three.js 0.186 | Самый популярный, ИИ его лучше всего знает, скиллы под него. |
| Сборка | Vite 8 + TypeScript | Быстрый dev-сервер, заготовка есть в скилле `threejs-gameplay-systems`. |
| Физика | Rapier (`@dimforge/rapier3d-compat` 0.21) | Используется в VIBE_WEAVERS, есть в справочнике скилла по выбору физики. |
| Мультиплеер | PartyKit (Cloudflare) или Colyseus 0.18 | PartyKit: есть готовый скилл `add-multiplayer`. Colyseus: популярен на Vibe Jam, бесплатен при своём хостинге. У Colyseus Cloud сейчас нет бесплатного тарифа. Цены PartyKit/Cloudflare проверить перед запуском. |
| Хостинг клиента | GitHub Pages или Cloudflare Pages | Бесплатно для статики. |
| Тесты | Playwright 1.64 | Скриншоты и бот-плейтест. |

Альтернативы: Babylon.js (встроенная физика и редактор), PlayCanvas (браузерный редактор), Godot (web-экспорт только на GDScript). Для работы с ИИ Three.js проще.

## 5. Ассеты (графика, модели, звук)

- **Бесплатные CC0-паки** low-poly моделей: Kenney, Quaternius, Poly Pizza. Быстрый старт без ключей.
- **ИИ-генерация 3D:** Tripo (скилл `threejs-3d-generator`) или Meshy. Текст или картинка → `.glb`. Платные тарифы Tripo примерно от $19.90/мес, проверить на сайте.
- **Картинки и текстуры:** Gemini image API (скилл `threejs-image-generator`).
- **Звук:** ElevenLabs (скилл `threejs-audio-generator`) или процедурный звук на Web Audio без ключей (скилл `game-audio`).
- **Здания SDU:** ИИ не знает планировку. Нужны свои фото, планы этажей, видео-проходы по корпусу. Геометрию корпуса строим в коде или в Blender по этим референсам.

## 6. Контекст SDU

- SDU (Suleyman Demirel University), кампус в Каскелене, около 20 км от Алматы.
- Около 8500 студентов, 4 факультета: Education & Humanities, Engineering & Natural Sciences, Law & Social Sciences, Business School.
- Мессенджеры в Казахстане: WhatsApp и Telegram почти наравне, Telegram популярнее у молодёжи. Telegram Mini Apps поддерживают HTML5/WebGL игры — это отдельный канал распространения на будущее.

## Источники

- [404 Media: fly.pieter.com](https://404media.co/this-game-created-by-ai-vibe-coding-makes-50-000-a-month-yours-probably-wont)
- [VGTimes: fly.pieter.com $87k](https://vgtimes.com/tech-and-hardware/122582-dutch-startup-founder-creates-game-using-ai-in-180-minutes-and-earns-87000-in-one-month.html)
- [Niche Site Growth: аудитория Levels](https://nichesitegrowth.beehiiv.com/p/67k-mo-from-an-ai-coded-game)
- [Vibe Jam 2026](https://jam.pieter.com/2026/), [Colyseus о Vibe Jam 2026](https://colyseus.io/blog/vibe-jam-2026/), [пресс-страница](https://vibej.am/2026/press)
- [Raccoon Heist](https://letsdatascience.com/news/claude-fable-5-turns-a-2022-raccoon-heist-concept-into-a-pla-81aee8ad)
- [VIBE_WEAVERS](https://discourse.threejs.org/t/vibe-weavers-a-vibecode-open-playground-p2p-colaborative-game-project/85381)
- [MacGuessr (McMaster)](https://news.mcmaster.ca/where-in-the-world-is-kylie-gun/)
- [StaffsVerse](https://www.staffs.ac.uk/london/news/university-launches-game-changing-virtual-campus-in-fortnite)
- [gg.deals: лучшие браузерные игры 2026](https://gg.deals/blog/the-best-browser-games/), [PCGamesN](https://www.pcgamesn.com/best-browser-games), [GamingStation: игры с приватными комнатами](https://gamingstation.org/multiplayer-browser-games/)
- [Почему популярные игры живут недолго](https://arcadiaquill.com/26064/opinion/why-popular-games-are-short-lived/), [Gartic Phone](https://knowyourmeme.com/editorials/guides/what-is-gartic-phone-and-why-do-streamers-love-it-the-multiplayer-drawing-game-explained), [Among Us](https://qz.com/1911718/how-among-us-became-a-global-video-game-phenomenon), [The best games kinda suck](https://world.hey.com/thome/the-best-games-kinda-suck-68982918)
- [Сравнение web-движков 2026](https://app.cinevva.com/blog/2026-06-09-web-game-engines-2026-comparison)
- [Гайд по мультиплееру в браузере](https://app.cinevva.com/guides/multiplayer-browser-game), [Colyseus JS SDK](https://docs.colyseus.io/getting-started/javascript), [Colyseus vs managed](https://crux.supercraft.host/blog/colyseus-vs-managed-backend/)
- [threejs-game-skills](https://github.com/majidmanzarpour/threejs-game-skills), [game-creator](https://github.com/OpusGameLabs/game-creator)
- [Tripo text-to-model](https://comfy.org/workflows/api_tripo_text_to_model-4540c3f34805/), [Tripo в браузере](https://sorceress.games/blog/tripo-ai-image-to-3d-model-in-your-browser)
- [SDU в Wikipedia](https://en.wikipedia.org/wiki/Suleyman_Demirel_University)
- [Мессенджеры в Центральной Азии](https://ca-barometer.org/ru/publications/which-messaging-apps-are-popular-in-uzbekistan-kyrgyzstan-and-kazakhstan)
- [Telegram Mini Apps гайд](https://merge.rocks/blog/how-to-build-a-telegram-mini-app-your-telegram-mini-apps-guide)
