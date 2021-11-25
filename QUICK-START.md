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

4. Обновите переводы навыков
```bash
cd ~
git clone https://github.com/putnik/mycroft-update-translations.git
cd mycroft-update-translations
mycroft-pip install -r requirements.txt 
./mycroft-update-translations.py -l ru-ru
```

5. Если раньше не перезагружались, то сейчас самое время это сделать:
```bash
sudo reboot now
```
Если вы до этого подлючались по кабелю, то нужно будет переподключиться к новому IP по Wi-Fi.

6. Устрановите русский язык в настройках Mycroft.

Если у вас уже имеется облако Яндекса или VK, то рекомендую сразу попробовать настроить их.
- Яндекс: [STT](./STT.md#yandex-speechkit), [TTS](./TTS.md#yandex-speechkit)
- VK: [STT](./STT.md#vk-cloud), [TTS](./TTS.md#vk-cloud)

Если нет, то настраиваем способ по умолчанию:
```bash
mycroft-config edit user
```
_TODO: заменить здесь Яндекс на что-нибудь более простое в настройке, хотя бы [Wit.ai](./STT.md#witai)_

Примерное содержимое файла (с учётом выбора Yandex Cloud для STT, замените значения ключа `api_key` на свой)
```json
{
  "max_allowed_core_version": 21.2,
  "lang": "ru-ru",
  "stt": {
    "yandex": {
      "lang": "ru-RU",
      "credential": {
        "api_key": "YOUR_API_KEY"
      }
    },
    "module": "yandex"
  },
  "tts": {
    "module": "google"
  }
}
```
После завершения процесса настройки можете посмотреть страницы настройки [STT](/STT.md) и [TTS](/TTS.md) и выбрать другие движки.

7. Отключим навыки, которые в данный момент не поддерживают русский язык:
```bash
mycroft-msm remove fallback-duck-duck-go
mycroft-msm remove fallback-wolfram-alpha
mycroft-msm remove mycroft-stock.mycroftai
```

8. Актуализируем погодный навык:
```bash
cd /opt/mycroft/skills/mycroft-weather.mycroftai
git checkout -b 21.02 origin/21.02
```

9. Запускаем Майкрофта:
```bash
cd ~
mycroft-start debug
```
Он запустится, настроится, предложит добавить устройство в аккаунт. Какие-то навыки могут не загрузиться из-за отсутствия переводов или изменения файлов.
