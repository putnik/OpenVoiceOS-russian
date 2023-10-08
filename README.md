# OpenVoiceOS на русском
Проект по подготовке всего необходимого для работы голосового ассистена OpenVoiceOS на русском языке с необходимым минумумом навыков:
- работа оффлайн без необходимости распознавать или генерировать текст в облаке
- информация о погоде
- прослушивание музыки через Spotify
- ответы на базовые вопросы (Википедия, DuckDuckGo, WolframAlpha или что-нибудь ещё)
- управление умным домом через Home Assistant

## Установка
Описан процесс установки на Raspberry Pi 4B c ReSpeaker 4-Mic Array. Если вы используете другую конфигурацию, он может отличаться.

Запишите на microSD-карту образ [Raspberry Pi OS Lite (64-bit)](https://www.raspberrypi.com/software/).
- В дополнительных настройках включите SSH и настройте Wi-Fi.

Зайдите по SSH на Raspberry Pi и выполните команды ниже.

Подготовка
```shell
sudo /sbin/iwconfig wlan0 power off
sudo sed -i 's/^exit 0/\/sbin\/iwconfig wlan0 power off\n\nexit 0/g' /etc/rc.local
sudo apt-get update
sudo apt-get install git
```

Настройка ReSpeaker 4-Mic Array [см. [инструкцию]](https://wiki.seeedstudio.com/ReSpeaker_4_Mic_Array_for_Raspberry_Pi/):
```shell
git clone https://github.com/seeed-studio-projects/seeed-voicecard.git
sudo ./seeed-voicecard/install.sh
sudo reboot now
```

```shell

```

Установка Docker:
```shell
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh ./get-docker.sh
sudo usermod -aG docker $USER
newgrp docker
sudo service docker start
```

Запуск OpenVoiceOS:
```shell
git clone https://github.com/OpenVoiceOS/ovos-docker.git -b dev ~/ovos-docker
mkdir -p ~/ovos/{config,share,tmp} ~/hivemind/{config,share}
chown ${USER}:${USER} -R ~/ovos ~/hivemind
cd ~/ovos-docker/compose
cp .env-raspberrypi .env
curl https://raw.githubusercontent.com/putnik/OpenVoiceOS-russian/main/config/mycroft.conf --output ~/ovos/config/mycroft.conf
echo "espeak_phonemizer\ngit+https://github.com/OpenVoiceOS/ovos-tts-plugin-piper.git@dev" > ~/ovos/config/audio.list
docker compose --project-name ovos --file docker-compose.yml --file docker-compose.raspberrypi.yml --file docker-compose.skills.yml up --detach
```

- После завершения перезагрузите устройство и немного подождите. Голосовой помощник готов* к работе.

_&ast; По крайней мере, в теории. Скрипт всё ещё дорабатывается._
