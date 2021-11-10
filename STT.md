# Движки Speech-To-Text (STT)

Подробные инструкции по настройке и ссылки на сайты можно найти в [документации Mycroft по STT](https://mycroft-ai.gitbook.io/docs/using-mycroft-ai/customizations/stt-engine).

## Таблица сравнения
| Название                          | Русский  | Лицензия        | Локально | Стоимость
| --------------------------------- | -------- | --------------- | -------- | ---------
| Google Cloud                      |          | 💔              | ☁️ нет  | 💰 [платно](https://cloud.google.com/speech-to-text/pricing): 0,4-0,9 ¢ (0,28-0,65 ₽) за 15 секунд
| GoVivace                          |          | 💔              | ?       | 💰 платно, подробностей нет
| Houndify                          |          |                 | ☁️ нет   | 
| IBM Cloud                         |          |                 | ☁️ нет   | 
| Kaldi                             |          | ✅ Apache v2    |          | ✅ бесплатно
| Mozilla DeepSpeech                | возможно | ✅ MPL v2       | ✅ да    | ✅ бесплатно
| Microsoft Azure                   |          |                 |          | 
| [Wit.ai](#witai)                 | ✅ да    | 💔              | ☁️ нет   | ✅ бесплатно
| [Yandex Cloud](#yandex-speechkit) | ✅ да    | 💔              | ☁️ нет   | 💰 [платно](https://cloud.yandex.ru/prices): 0,15 ₽ за 15 секунд
| [VK Cloud](#vk-cloud)             | ✅ да    | 💔              | ☁️ нет   | 💰 [платно](https://mcs.mail.ru/cloud-voice/#pricing): 0,12 ₽ за 15 секунд

## Wit.ai
Неплохой и достаточно быстрый в настройке способ. Требует постоянного подключения к интернету, а также регистрацию в сервисе через логин Facebook.
Не требует оплаты.

В случае использования Яндекса голоса Филиппа итоговый вариант конфига будет выглядеть примерно вот так:
```json
{
  "lang": "ru-ru",
  "stt": {
    "module": "wit",
    "wit": {
      "credential": {
        "token": "YOUR_TOKEN"
      }
    }
  }
}
```

## Yandex SpeechKit
Качественный и достаточно быстрый в настройке способ. Требует постоянного подключения к интернету, а также наличия аккаунта в облаке Яндекса.
Первый месяц бесплатно, после этого требует оплаты.

Ссылки:
- [Создание и настройка облака Яндекса](https://cloud.yandex.ru/services/speechkit)
- [Руководство по настройке](https://mycroft-ai.gitbook.io/docs/using-mycroft-ai/customizations/stt-engine#yandex-speechkit-stt)

В случае использования Яндекса голоса Филиппа итоговый вариант конфига будет выглядеть примерно вот так:
```json
{
  "lang": "ru-ru",
  "stt": {
    "module": "yandex",
    "yandex": {
      "lang": "ru-RU",
      "credential": {
        "api_key": "YOUR_API_KEY"
      }
    }
  }
}
```

См. также [Yandex SpeechKit TTS](./TTS.md#yandex-speechkit).

## VK Cloud
Неплохой и достаточно быстрый в настройке способ. Требует постоянного подключения к интернету, а также наличия аккаунта в облаке VK.
При регистрации даётся 100 рублей (можно получить до 3000 на два месяца для тестирования), дальше требует оплаты.

Установка:
`mycroft-pip install mycroft-plugin-vk-cloud`

Конфиг будет выглядеть примерно вот так:
```json
{
  "lang": "ru-ru",
  "stt": {
    "module": "vk",
    "vk": {
      "credential": {
        "service_token": "YOUR_SERVICE_TOKEN"
      }
    }
  }
}
```

См. также [VK Cloud TTS](./TTS.md#vk-cloud).
