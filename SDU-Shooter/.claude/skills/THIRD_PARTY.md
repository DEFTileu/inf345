# Сторонние скиллы

Скиллы в этой папке скопированы из открытых репозиториев. Перед установкой их проверили: скрипты обращаются только к официальным API (Tripo, ElevenLabs, Gemini), ключи берутся из переменных окружения.

| Скиллы | Источник | Коммит | Лицензия |
|---|---|---|---|
| `threejs-game-director`, `threejs-gameplay-systems`, `threejs-aaa-graphics-builder`, `threejs-game-ui-designer`, `threejs-debug-profiler`, `threejs-qa-release`, `threejs-3d-generator`, `threejs-image-generator`, `threejs-audio-generator` | [majidmanzarpour/threejs-game-skills](https://github.com/majidmanzarpour/threejs-game-skills) | `8286774` (2026-09-27) | MIT, © 2026 Majid Manzarpour |
| `add-multiplayer`, `game-audio`, `threejs-perf` | [OpusGameLabs/game-creator](https://github.com/OpusGameLabs/game-creator) | `4e64b83` (2026-05-25) | MIT (по README репозитория) |

Чтобы обновить скиллы, склонируйте репозиторий заново, сравните изменения и замените папки.

Важно: `game-audio` опционально предлагает `@strudel/web`. Он под AGPL-3.0, поэтому подключайте его, только если готовы к этой лицензии.

## MIT License (threejs-game-skills)

Copyright (c) 2026 Majid Manzarpour

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
