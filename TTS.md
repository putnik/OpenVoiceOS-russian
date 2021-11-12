# Движки Text-To-Speech (TTS)

Конфиг прописывается после запуска команды `mycroft-config edit user`.

Подробные инструкции по настройке и ссылки на сайты можно найти в [документации Mycroft по STT](https://mycroft-ai.gitbook.io/docs/using-mycroft-ai/customizations/stt-engine).

## Сравнение локальных движков
| Название           | Русский  | Лицензия    
| ------------------ | -------- | ------------
| [eSpeak](#espeak)  | 💔 плохо | ✅ GPL v3
| eSpeak NG          | ?        | ✅ GPL v3
| Coqui TTS          | ?        | ✅ MPL v2
| FA TTS             | ?        | ✅ LGPL v3
| MaryTTS            | нет?     | ✅ LGPL v3
| Mimic 1            | ?        | ✅ BSD-like
| Mimic 2            | ?        | ✅ Apache v2
| Mozilla TTS        | ?        | ✅ MPL v2
| RHVoice            | ✅ да    | ✅ LGPL v2.1
| Silero             | ✅ да    | ✅ AGPL v3
| SOVA               | ✅ да    | ✅ Apache v2
| [SpdSay](#spdsay)  | ✅ да    | ✅ GPL v2

## Сравнение облачных сервисов
| Название                          | Русский  | Стоимость | Пример
| --------------------------------- | -------- | --------- | ------
| Amazon Polly                      | ✅ да    | 💰 [платно](https://aws.amazon.com/polly/pricing/?nc=sn&loc=4): 0,4-1,6 ¢ (0,29—1,15 ₽) за 1000 символов
| Google TTS                        | ✅ да    | ✅ бесплатно
| IBM Watson                        | ❌ нет   | 💰 [платно](https://cloud.ibm.com/catalog/services/text-to-speech): 2,14 ¢ (1,53 ₽) за 1000 символов
| Microsoft Azure                   | ?        | ?
| Microsoft Bing                    | ?        | ?
| Responsive Voice                  | ✅ да    | ?
| [VK Cloud](#vk-cloud)             | ✅ да    | 💰 [платно](https://mcs.mail.ru/cloud-voice/#pricing): 1 ₽ за 1000 символов | [SoundCloud](https://soundcloud.com/sergey-leschina/mycroft-tts-vk-cloud)
| [Yandex Cloud](#yandex-speechkit) | ✅ да    | 💰 [платно](https://cloud.yandex.ru/prices): 0,18—1,2 ₽ за 1000 символов    | [SoundCloud](https://soundcloud.com/sergey-leschina/mycroft-tts-yandex-speechkit)

## Что выбрать?
- Самая быстрая настройка: Google TTS, голос Гугл-переводчика.
- Самый качественный голос в облаке: [Yandex Cloud](#yandex-speechkit) с голосами Филипп (`filipp`) или Алёна (`alena`).
- Самая лучшая локальная генерация: [SpdSay](#spdsay) (из проверенных)

## eSpeak
Поддержка русского формально заявлена, но на практике очень сложно понимать речь.
Даже замена файла словаря (`ru_dict`) на улучшенную версию практически не улучгает ситуацию.

## SpdSay
Высокоуровневая обёртка для генерации голоса. Для линукса использует eSpeak NG.

Проблемы:
- Не может сказать две фразы подряд (например, текущую погоду, а потом прогноз), проглатывает первую и озвучивает только вторую.
- Не умеет читать знак минуса `-`, считая его служебным и заглатывая после него ещё и число.

Установка:
```bash
sudo apt-get install speech-dispatcher
```

Конфиг будет выглядеть примерно вот так:
```json
{
  "lang": "ru-ru",
  "stt": {
    "module": "spdsay",
    "spdsay": {
      "lang": "ru",
      "voice": "child_male"
    }
  }
}
```
Возможные голоса: `male1`, `male2`, `male3`, `female1`, `female2`, `female3`, `child_male`, `child_female`. К сожалению, другие параметры, доступные в самом SpdSay (rate, pitch, pitch-range), через настройки задать невозможно.


## VK Cloud
Неплохой и достаточно быстрый в настройке способ. Требует постоянного подключения к интернету, а также наличия аккаунта в облаке VK.
При регистрации даётся 100 рублей (можно получить до 3000 на два месяца для тестирования), дальше требует оплаты.

Плюсы:
- Можно задать скорость речи (0,75-1,75), рекомендую указывать что-то в диапазоне 1-1,2 ([пример для 1.15](https://soundcloud.com/sergey-leschina/mycroft-tts-vk-cloud-tempo-115))

Проблемы:
- Только один голос (вроде бы тот же, что у Маруси)
- Достаточно большие задержки при генерации

Установка:
`mycroft-pip install mycroft-plugin-vk-cloud`

Конфиг будет выглядеть примерно вот так:
```json
{
  "lang": "ru-ru",
  "tts": {
    "module": "vk",
    "yandex": {
      "service_token": "YOUR_SERVICE_TOKEN",
      "tempo": 1.1
    }
  }
}
```

См. также [VK Cloud STT](./STT.md#vk-cloud).

### Yandex SpeechKit
Качественный и достаточно быстрый в настройке способ. Требует постоянного подключения к интернету, а также наличия аккаунта в облаке Яндекса.
Первый месяц бесплатно, после этого требует оплаты.

Есть премиальные и обычные голоса. Премиальные намного качественнее, но и дороже примерно в 10 раз. Их два:
- Филипп (`filipp`) — мужской голос ([пример](https://soundcloud.com/sergey-leschina/mycroft-tts-yandex-speechkit))
- Алёна (`alena`) — женский голос ([пример](https://soundcloud.com/sergey-leschina/yandex-alena))

Обычных голосов больше, но многие из них звучат не очень приятно. Субъективно лучшие варианты:
- Ермил (`ermil`) — мужской голос ([пример](https://soundcloud.com/sergey-leschina/yandex-ermil))
- Элис (`alyss`) — женский голос ([пример](https://soundcloud.com/sergey-leschina/yandex-alyss))
- Оксана (`oksana`) — женский голос ([пример](https://soundcloud.com/sergey-leschina/yandex-oksana))

Ссылки:
- [Создание и настройка облака Яндекса](https://cloud.yandex.ru/services/speechkit)
- [Руководство по настройке](https://mycroft-ai.gitbook.io/docs/using-mycroft-ai/customizations/tts-engine#yandex-speechkit)

В случае использования Яндекса голоса Филиппа итоговый вариант конфига будет выглядеть примерно вот так:
```json
{
  "lang": "ru-ru",
  "tts": {
    "module": "yandex",
    "yandex": {
      "lang": "ru-RU",
      "api_key": "YOUR_API_KEY",
      "voice": "filipp",
      "emotion": "good"
    }
  }
}
```

См. также [Yandex SpeechKit STT](./STT.md#yandex-speechkit).
