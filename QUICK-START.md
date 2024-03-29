# Быстрый старт

Есть неплохая [официальная инструкция](https://mycroft-ai.gitbook.io/docs/using-mycroft-ai/get-mycroft/picroft#getting-started-with-picroft) для Picroft, к которой можно обратиться при возникновении проблем.

## Подготовка к запуску
1. Загрузите самую [свежий weekly-образ](https://sourceforge.net/projects/picroft/files/weekly/) Picroft из [kleo/picroft](https://github.com/kleo/picroft). Это Debian с предустановленным Майкрофтом. Не берите arm64-версию, если не знаете, зачем она вам. Так как это SourceForge, то скачиваться будет долго, лучше сделайте это заранее.
2. Запишите образ на MicroSD через [Raspberry Pi Imager](https://www.raspberrypi.com/software/)
3. Создайте в корне MicroSD пустой файл `ssh.txt` для включения доступа по SSH.
4. Создайте в корне MicroSD файл `wpa_supplicant.conf` с таким содержимым для подключения к Wi-Fi (замените имя сети и пароль на свои):
```ini
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
country=RU
update_config=1

network={
 ssid="YOUR_SSID"
 psk="YOUR_PASSWORD"
}
```
5. Вставляете MicroSD в Raspberry Pi, втыкаете что-нибудь в джек для воспроизведения звука и включаете.

## Запуск
1. После первого запуска будут подгружаться обновления, так что не ожидайте быстрой реакции.
2. Через какое-то время Майкрофт предложит создать аккаунт, а потом продиктует код для сопряжения с сервером. Он нужен для того, чтобы часть настроек навыков можно было задавать через веб-интерфейс. Так что идём на [account.mycroft.ai](https://account.mycroft.ai/) и регистрируемся, а потом в [/devices](https://account.mycroft.ai/devices) и создаём новое устройство с кодом, который будет продиктован. Если вы его не услышали, то подождите повтора.
3. Если код не воспроизводится, то подключитесь по SSH (`ssh pi@IP` с паролем `mycroft`). Возможно, что только в этот момент начнётся инициализация. После завершения установки код появится в консоли.
4. После сопряжения продолжится настройка навыков, и Майкрофт сообщит об окончании установки. Но в целом с этого момента можно использовать Майкрофта на анлийском.

## Русский язык
1. Настройте русскую локаль (см. также [интрукцию Debian](https://wiki.debian.org/Locale#Standard), для выхода в консоль можно воспользоваться Ctrl+C):
```bash
sudo dpkg-reconfigure locales # везде выбрать ru_RU.UTF-8
echo ': "${LC_ALL:=ru_RU.UTF-8}"; export LC_ALL' | sudo tee -a /etc/profile
```
2. Установите dev-версию lingua-franca с поддержкой русского языка _(после следующего релиза можно будет убрать этот пункт)_
```bash
mycroft-pip uninstall lingua-franca -y
mycroft-pip install git+https://github.com/MycroftAI/lingua-franca.git
```
3. Установите русский язык и STT/TTS с его поддержкой в настройках Mycroft. Выберите более подходящие вам варианты на страницах настройки [STT](/STT.md) и [TTS](/TTS.md). Если не знаете, что выбрать, то выбирайте [Vosk](./STT.md#vosk) для STT и [RHVoice](./TTS.md#rhvoice) для TTS. Их нужно будет установить. Редактируем конфиг (`mycroft-config edit user`) и прописываем то, что выбрали. Будет примерно так:
```json
{
  "max_allowed_core_version": 21.2,
  "lang": "ru-ru",
  "stt": {
    "module": "ovos-stt-plugin-vosk",
    "ovos-stt-plugin-vosk": {
      "model": "https://alphacephei.com/vosk/models/vosk-model-small-ru-0.22.zip"
    }
  },
  "tts": {
    "module": "rhvoice"
  }
}
```
4. Обновите переводы навыков
```bash
git clone https://github.com/putnik/mycroft-update-translations.git ~/mycroft-update-translations
cd ~/mycroft-update-translations
mycroft-pip install -r requirements.txt
./mycroft-update-translations.py -l ru-ru
```
6. Перезапускаем Майкрофта:
```bash
cd ~/mycroft-core
./stop-mycroft.sh
./start-mycroft.sh all
./start-mycroft.sh cli
```

Поздравляю, ваш Майкрофт готов к работе. Дальше устанавливайте и настраивайте навыки.
