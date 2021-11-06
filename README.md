# Mycroft на русском
Инструкции по запуску голосового помощника Mycroft с русской локалью и шаги по дальнейшему улучшению поддержки русского языка.

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
