# Быстрый старт

1. Используйте [официальную инструкцию](https://mycroft-ai.gitbook.io/docs/using-mycroft-ai/get-mycroft/picroft) для создания образа и запуска Raspberry PI.

2. После установки подключитесь по SSH, пройдите все шаги конфигурации, настройте Wi-Fi (если нужно).

3. Загрузите последние изменения ядра с поддержкой русского языка:
```bash
cd ~/mycroft-core
sed -i 's/MycroftAI/putnik/g' .git/config
git pull
./dev_setup.sh
```

4. Обновите версию lingua-franca на кастомную с поддержкой русского языка
```bash
mycroft-pip uninstall lingua-franca -y
mycroft-pip install git+https://github.com/putnik/lingua-franca.git@issue-213
```

5. Обновите переводы скилов
```bash
cd ~
git clone https://github.com/putnik/mycroft-update-translations.git
cd mycroft-update-translations
mycroft-pip install -r requirements.txt 
./mycroft-update-translations.py -l ru-ru
```

6. Если раньше не перезагружались, то сейчас самое время это сделать:
```bash
sudo reboot now
```
Если вы до этого подлючались по кабелю, то нужно будет переподключиться к новому IP по Wi-Fi.

7. Устрановите русский язык в настройках Mycroft:
```bash
mycroft-config edit user
```
Примерное содержимое файла (с учётом выбора Yandex Cloud, замените оба значения `api_key` на свой)
```json
{
  "max_allowed_core_version": 21.2,
  "lang": "ru-ru",
  "stt": {
    "yandex": {
      "lang": "ru-RU",
      "credential": {
        "api_key": "XXXXXXXXXX"
      }
    },
    "module": "yandex"
  },
  "tts": {
    "module": "yandex",
    "yandex": {
      "lang": "ru-RU",
      "api_key": "XXXXXXXXXX",
      "voice": "filipp",
      "emotion": "good"
    }
  }
}
```

8. Отключим скиллы, которые в данный момент не поддерживают русский язык:
```bash
mycroft-msm remove fallback-duck-duck-go
mycroft-msm remove fallback-wolfram-alpha
mycroft-msm remove mycroft-stock.mycroftai
```

9. Актуализируем погодный навык:
```bash
cd /opt/mycroft/skills/mycroft-weather.mycroftai
git checkout -b 21.02 origin/21.02
```

10. Запускаем Майкрофта:
```bash
cd ~
mycroft-start debug
```
Он запустится, настроится, предложит добавить устройство в аккаунт. Какие-то скиллы могут не загрузиться из-за отсутствия переводов или изменения файлов.
