# OpenVoiceOS на русском
Проект по подготовке всего необходимого для работы голосового ассистена OpenVoiceOS на русском языке с необходимым минумумом навыков:
- работа оффлайн без необходимости распознавать или генерировать текст в облаке
- информация о погоде
- прослушивание музыки через Spotify
- ответы на базовые вопросы (Википедия, DuckDuckGo, WolframAlpha или что-нибудь ещё)
- управление умным домом через Home Assistant

Установка на Raspberry Pi:
- Запишите на microSD-карту образ [Raspberry Pi OS](https://www.raspberrypi.com/software/).
  - В дополнительных настройках включите SSH и настройте Wi-Fi.
- Зайдите по SSH на Raspberry Pi и запустите скрипт: `https://gist.githubusercontent.com/putnik/26d8a8ad70cd428bb79bbe0c25811bfe/raw/install.sh | sudo sh`
- После заверешения перезагрузите устройство и немного подождите. Голосовой помощник готов* к работе.

_&ast; По крайней мере, в теории. Скрипт всё ещё дорабатывается._
