# Движки Text-To-Speech (TTS)

Конфиг прописывается после запуска команды `mycroft-config edit user`.

Подробные инструкции по настройке и ссылки на сайты можно найти в [документации Mycroft по TTS](https://mycroft-ai.gitbook.io/docs/using-mycroft-ai/customizations/tts-engine).

## Сравнение локальных движков
| Название            | Русский  | Лицензия     | Пример
| ------------------- | -------- | ------------ | ------ 
| [eSpeak](#espeak)   | 💔 плохо | ✅ GPL v3    | 
| eSpeak NG           | ?        | ✅ GPL v3    | 
| Coqui TTS           | ?        | ✅ MPL v2    | 
| FA TTS              | ?        | ✅ LGPL v3   | 
| MaryTTS             | нет?     | ✅ LGPL v3   | 
| Mimic 1             | ?        | ✅ BSD-like  | 
| Mimic 2             | ?        | ✅ Apache v2 | 
| Mozilla TTS         | ?        | ✅ MPL v2    | 
| [RHVoice](#rhvoice) | ✅ да    | ✅ LGPL v2.1 | [SoundCloud](https://soundcloud.com/putnik-tech/sets/mycroft-tts-rhvoice)
| [Silero](#silero)   | ✅ да    | ✅ AGPL v3   | [SoundCloud](https://soundcloud.com/putnik-tech/mycroft-tts-silero-ruslan-v2)
| SOVA                | ✅ да    | ✅ Apache v2 | 
| [SpdSay](#spdsay)   | ✅ да    | ✅ GPL v2    | 

## Сравнение облачных сервисов
| Название                          | Русский  | Стоимость | Пример
| --------------------------------- | -------- | --------- | ------
| Amazon Polly                      | ✅ да    | 💰 [платно](https://aws.amazon.com/polly/pricing/?nc=sn&loc=4): 0,4-1,6 ¢ (0,29—1,15 ₽) за 1000 символов
| Google TTS                        | ✅ да    | ✅ бесплатно
| IBM Watson                        | ❌ нет   | 💰 [платно](https://cloud.ibm.com/catalog/services/text-to-speech): 2,14 ¢ (1,53 ₽) за 1000 символов
| Microsoft Azure                   | ?        | ?
| Microsoft Bing                    | ?        | ?
| Responsive Voice                  | ✅ да    | ?
| [VK Cloud](#vk-cloud)             | ✅ да    | 💰 [платно](https://mcs.mail.ru/cloud-voice/#pricing): 1 ₽ за 1000 символов | [SoundCloud](https://soundcloud.com/putnik-tech/sets/mycroft-tts-silero)
| [Yandex Cloud](#yandex-speechkit) | ✅ да    | 💰 [платно](https://cloud.yandex.ru/prices): 0,18—1,2 ₽ за 1000 символов    | [SoundCloud](https://soundcloud.com/putnik-tech/sets/mycroft-tts-yandex-speechkit)

## Что выбрать?
- Самая быстрая настройка: Google TTS, голос Гугл-переводчика.
- Самый качественный голос в облаке: [Yandex Cloud](#yandex-speechkit) с голосами Филипп (`filipp`) или Алёна (`alena`).
- Самая лучшая локальная генерация: [RHVoice](#rhvoice)

## eSpeak
Поддержка русского формально заявлена, но на практике очень сложно понимать речь.
Даже замена файла словаря (`ru_dict`) на улучшенную версию практически не улучгает ситуацию.

## RHVoice
Плюсы:
- Локальная работа
- Быстрый ответ
- Нет проблем с установкой, независимо от платформы
- Неплохое качество голосов

Минусы:
- Большой репозиторий с кодом и долгая установка из исходников
- Озвучка числительных только как количественных, но не как порядковых

Установка:
- см. также [инструкцию в репозитории RHVoice](https://github.com/RHVoice/RHVoice/blob/master/doc/ru/Compiling-on-Linux.md)

```bash
sudo apt-get install scons libspeechd-dev
git clone --recurse-submodules https://github.com/RHVoice/RHVoice.git ~/RHVoice
cd ~/RHVoice
scons
sudo scons install
sudo ldconfig
mycroft-pip install mycroft-plugin-rhvoice
```

Базовый конфиг будет выглядеть так, подробный см. в [репозитории плагина](https://github.com/putnik/mycroft-plugin-rhvoice):
```json
{
  "lang": "ru-ru",
  "tts": {
    "module": "rhvoice"
  }
}
```

Модели для русского языка:
- `aleksandr` — мужской голос ([пример](https://soundcloud.com/putnik-tech/mycroft-tts-rhvoice-aleksandr))
- `aleksandr-hq` — мужской голос ([пример](https://soundcloud.com/putnik-tech/mycroft-tts-rhvoice-aleksandr-hq))
- `anna` — женский голос ([пример](https://soundcloud.com/putnik-tech/mycroft-tts-rhvoice-anna))
- `elena` — женский голос ([пример](https://soundcloud.com/putnik-tech/mycroft-tts-rhvoice-elena))
- `irina` — женский голос ([пример](https://soundcloud.com/putnik-tech/mycroft-tts-rhvoice-irina))

Актуальный список см. на [сайте RHVoice](https://rhvoice.org/ru-voices/).


## Silero
Модели на Pytorch для генерации голоса.

Проблемы:
- Нужен Pytorch 1.10+, его сложно собрать под многие платформы (для RPi 4 есть [инструкция](https://qengineering.eu/install-pytorch-on-raspberry-pi-4.html)).
- Долгая генерация, на RPi 4 выходит 10-15 секунд для одной фразы для v2, возможно в v3 всё значительно лучше.
- Не умеет читать знак минуса `-` и числа, нужна дополнительная конвертация в текст ([пример](https://soundcloud.com/putnik-tech/mycroft-tts-silero-ruslan-v2-missing-numbers)).

Установка:
- идёт работа над плагином

Конфиг будет выглядеть примерно вот так:
```json
{
  "lang": "ru-ru",
  "tts": {
    "module": "silero",
    "silero": {
      "lang": "ru",
      "model": "v3_1_ru",
      "voice": "eugene",
      "rate": 8000
    }
  }
}
```

Голоса для русского языка:
- `aidar` — мужской голос ([пример](https://soundcloud.com/alexander-veysov/aidar))
- `baya` — женский голос ([пример](https://soundcloud.com/alexander-veysov/baya))
- `kseniya` — женский голос ([пример](https://soundcloud.com/alexander-veysov/kseniya))
- `xenia` — женский голос ([пример](https://soundcloud.com/alexander-veysov/xenia))
- `eugene` — мужской голос ([пример](https://soundcloud.com/alexander-veysov/eugene))

Актуальный список см. в [репозитории Silero](https://github.com/snakers4/silero-models#models-and-speakers).


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
- Можно задать скорость речи (0,75-1,75), рекомендую указывать что-то в диапазоне 1-1,2 ([пример для 1.15](https://soundcloud.com/putnik-tech/mycroft-tts-vk-cloud-tempo-115))

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
- Филипп (`filipp`) — мужской голос ([пример](https://soundcloud.com/putnik-tech/mycroft-tts-yandex-speechkit))
- Алёна (`alena`) — женский голос ([пример](https://soundcloud.com/putnik-tech/yandex-alena))

Обычных голосов больше, но многие из них звучат не очень приятно. Субъективно лучшие варианты:
- Ермил (`ermil`) — мужской голос ([пример](https://soundcloud.com/putnik-tech/yandex-ermil))
- Элис (`alyss`) — женский голос ([пример](https://soundcloud.com/putnik-tech/yandex-alyss))
- Оксана (`oksana`) — женский голос ([пример](https://soundcloud.com/putnik-tech/yandex-oksana))

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
