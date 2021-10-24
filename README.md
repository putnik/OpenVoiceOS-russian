# Mycroft на русском
Инструкции по запуску голосового помощника Mycroft с русской локалью и шаги по дальнейшему улучшению поддержки русского языка.

# Настройка языка
Нужно установить язык по умолчанию, а также настроить, какой движок будет использоваться для Text-to-Speech (TTS) и Speech-To-Text (STT).
`mycroft-config edit user`

## Yandex SpeechKit
Качественный и достаточно быстрый в настройке способ. Требует постоянного подключения к интернету, а также наличия аккаунта в облаке Яндекса. Первый месяц бесплатно, после этого требует оплаты.

Есть премиальные и обычные голоса. Премиальные намного качественнее, но и дороже примерно в 10 раз. Их два:
- Филипп (`filipp`) — мужской голос
- Алёна (`alena`) — женский голос

Обычных голосов больше, но многие из них звучат не очень приятно. Субъективно лучшие варианты:
- Ермил (`ermil`) — мужской голос
- Элис (`alyss`) — женский голос
- Оксана (`oksana`) — женский голос

Ссылки:
- [Создание и настройка облака Яндекса](https://cloud.yandex.ru/services/speechkit)
- [Настройка  Text-to-Speech](https://mycroft-ai.gitbook.io/docs/using-mycroft-ai/customizations/tts-engine#yandex-speechkit)
- [Настройка Speech-To-Text](https://mycroft-ai.gitbook.io/docs/using-mycroft-ai/customizations/stt-engine#yandex-speechkit-stt)

Ваш итоговый конфиг будет выглядеть примерно вот так:
```json
{
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

## Полезные репозитории
* [jmontane/mycroft-update-translations](https://github.com/jmontane/mycroft-update-translations)
* [jmontane/mycroft-catalan.conf](https://github.com/jmontane/mycroft-catalan.conf)
