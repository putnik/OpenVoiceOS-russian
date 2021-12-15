# Mycroft на русском
Проект по подготовке всего необходимого для работы голосового ассистена Mycroft на русском языке с необходимым минумумом навыков:
- работа оффлайн без необходимости распознавать или генерировать текст в облаке
- информация о погоде
- прослушивание музыки через Spotify
- ответы на базовые вопросы (Википедия, DuckDuckGo, WolframAlpha или что-нибудь ещё)
- управление умным домом через Home Assistant

Как начать:
- смотрите [руководство](/QUICK-START.md)
- приходите в [чат](https://chat.mycroft.ai/community/channels/pycroft)

План работ:
* [x] TTS/STT:
  * [x] Выбрать оффлайновый движок [Text-to-Speech (TTS)](/TTS) — [RHVoice](/TTS#rhvoice)
  * [x] Выбрать оффлайновый движок [Speech-To-Text (STT)](/STT) — [Vosk](/STT#vosk)
* [x] Перевод ядра:
  * [x] Добавить поддержку русского языка в [lingua-franca](https://github.com/MycroftAI/lingua-franca) — [issue](https://github.com/MycroftAI/lingua-franca/issues/213), [PR](https://github.com/MycroftAI/lingua-franca/pull/214)
  * [x] Перевести [mycroft-core](https://github.com/MycroftAI/mycroft-core) — [PR](https://github.com/MycroftAI/mycroft-core/pull/3014)
* [ ] Навыки:
  * [x] Перевести основные навыки на [translate.mycroft.ai](https://translate.mycroft.ai/ru/)
  * [x] Перевести [погодный навык](https://github.com/MycroftAI/skill-weather) (translate.mycroft.ai устарел) — [PR](https://github.com/MycroftAI/skill-weather/pull/188)
    * [ ] Поправить [баг с celsius](https://github.com/MycroftAI/skill-weather/issues/189)
  * [ ] Перевести [навык Spotify](https://github.com/forslund/spotify-skill) (translate.mycroft.ai устарел) — [ветка с переводом](https://github.com/putnik/spotify-skill/tree/21.01-russian)
  * [ ] Перевести [навык Playback Control](https://github.com/MycroftAI/skill-playback-control) (нужен для Spotify) — [ветка с переводом](https://github.com/putnik/skill-playback-control/tree/21.02-russian)
  * [ ] Перевести [навык Home Assistant](https://github.com/MycroftAI/skill-homeassistant)
  * [ ] …
* [ ] Сборка образа с поддержкой русского языка:
  * [ ] Сделать инструкцию по настройке — [черновик](/QUICK-START.md)
  * [ ] Сделать [плейбук для ansible](https://opensource.com/article/21/12/mycroft-raspberry-pi-ansible)
  * [ ] Настроить автоматическую сборку образа (см. [kleo/picroft](https://github.com/kleo/picroft))

## Настройка языка
Нужно установить язык по умолчанию, а также настроить, какой движок будет использоваться для [Text-to-Speech (TTS)](/TTS) и [Speech-To-Text (STT)](/STT).

Конфиг прописывается после запуска команды `mycroft-config edit user`.

## Переводы
[translate.mycroft.ai](https://translate.mycroft.ai/ru/) — основной инструмент для перевода

Некоторые навыки его не используют:
- [skill-weather](https://github.com/MycroftAI/skill-weather) — перевод прямо в коде (`locale/ru-ru`)

## Обновление репозиториев
### Навыки
#### Вариант 1:
```bash
cd /opt/mycroft/skills/skill-name
git pull
```

#### Вариант 2.
```bash
mycroft-msm install git+https://github.com/name/path.git@branch-name
```
Обратите внимание на `git+` в начале и `@branch-name` (нужно заменить на название конкретной ветки) в конце URL'а.

Вместо `install` может быть `update`, но вероятно при том же номере версии навыка обновление не происходит. Можно сделать `mycroft-msm delete skill-name` и уже потом `install`.

### Переводы навыков
```bash
cd ~
git clone https://github.com/putnik/mycroft-update-translations.git
cd mycroft-update-translations
pip install -r requirements.txt 
./mycroft-update-translations.py -l ru-ru
```

## Полезные репозитории
* [putnik/mycroft-update-translations](https://github.com/putnik/mycroft-update-translations)
* [jmontane/mycroft-copy-translations](https://github.com/jmontane/mycroft-copy-translations)
* [jmontane/mycroft-catalan.conf](https://github.com/jmontane/mycroft-catalan.conf)
* [gras64/pootle-sync-skill](https://github.com/gras64/pootle-sync-skill)
