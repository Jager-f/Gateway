# Zigbee + BLE шлюз

Этот продукт  предназначен для работы с распространенными устройствами ZigBee, BLE.  В основе шлюза лежит контроллер [ESP32 от Espressif ](https://www.espressif.com/sites/default/files/documentation/esp32-wrover_datasheet_en.pdf). В качестве связущего звена протокола Zigbee  выступает тандем чипов от Texas Instruments [ZIgbee CC2538](https://www.ti.com/product/CC2538?utm_source=google&utm_medium=cpc&utm_campaign=epd-null-null-GPN_EN-cpc-pf-google-wwe&utm_content=CC2538&ds_k=%7b_dssearchterm%7d&DCM=yes&gclid=CjwKCAiA35rxBRAWEiwADqB37x__0Gm1rR2TUfCBETyuqrLjOtof6TuYSD3ZHzINYdNAbrXqfDxrwRoCpToQAvD_BwE&gclsrc=aw.ds) и  усилителя  [сс2592](https://www.ti.com/product/CC2592?utm_source=google&utm_medium=cpc&utm_campaign=epd-null-null-GPN_EN-cpc-pf-google-wwe&utm_content=CC2592&ds_k=%7b_dssearchterm%7d&DCM=yes&gclid=CjwKCAiA35rxBRAWEiwADqB3776CVlMD1GHdk-unOn9R0YeMtlwAnjUv-CIPuWvjhNqZRbiq6zy-ExoCxjYQAvD_BwE&gclsrc=aw.ds), либо готовый чип от [NXP JN5168](https://www.nxp.com/products/wireless/zigbee/zigbee-and-ieee802.15.4-wireless-microcontroller-with-256-kb-flash-32-kb-ram:JN5168). Для связи с устройствами по протоколу BLE используются встроенные возможности ESP32.

# Общие сведения
Шлюз выполняет роль координатора Zigbee и позволяет:
1) Использовать большинство доступного Zigbee оборудования. Список поддерживаемого и протестированного обрудования доступен по [ссылке](/devices/devices.md). Новое оборудование может быть добавлено после обсуждения с нами.
2) Отказаться от необходимости использования облаков производителей устройств. В качестве альтернативы, предлагается использовать облачный сервис [Smart Logic System](https://cloud.slsys.io), либо нативные приложения для Android и Apple iPhone (в разработке). 
3) Использовать распространенные  локальные системы автоматизации, такие как [MajorDomo](https://mjdm.ru/), [ioBroker Smarthome](https://www.iobroker.net), [HomeAssisiant](https://www.home-assistant.io), [Node-Red](https://nodered.org) и др. Для интеграции с этими системами используется протокол MQTT. Структура топиков протокола MQTT идентична  проекту  [zigbee2mqtt](https://www.zigbee2mqtt.io), это позволяет использовать шлюз без какого-либо написания кода.

# Дополнительные возможности шлюза через Web интерфейс
1) Управление и просмотр сведений  устройств через Web интерфейс шлюза по адресу http://ipadress (80 порт). Возможность отображения источника питания, уровня заряда батареи, доступных [EndPoint устройств](https://community.nxp.com/thread/332332)  в web-интерфейсе.
2) Создание локальных автоматизаций внутри шлюза [Simplelink](/simplelink.md)
3) Возможность написания сценариев на языке [Lua](https://ru.wikipedia.org/wiki/Lua) [Книга по Lua на русском языке](https://www.htbook.ru/kompjutery_i_seti/programmirovanie/programmirovanie-na-yazyke-lua)
4.	Возможность создания групп для управления несколькими устройствами одновременно (в разработке).
5.	Возможность задавать имя устройству. Если вы планируете использовать  шлюз с локальными системами автоматизации, рекомендуется установить галочку отправки адреса вместо устройств.
5.	Возможность удаления устройства. 
6.	Возможность отображения маршрутов в web-интерфейсе (в разработке).
8.	Возможность установить прямые связи [Bind](/bind.md) между устройствами ZigBee без участия координатора для управления конечными устройствами.
9.	Возможность управлять аппаратными светодиодами (адресными или RGB). 
```
Необходимо отправить в JSON значение в топик ZigBeeGW/led следующего содержания:
{"mode":"manual","hex":"#FFFFFF"}

mode - устанавливает режим, допустимы значения off, manual и auto
hex - значение цвета в RGB Hex формате
```
10.	Возможность управлять звуком (при наличии распаянного усилителя) (в разработке)
11.	Возможность изменить PanId и номер канала.
12.	Возможность задать имя шлюза в сети.
13.	Возможность перехода шлюза в режим АР при нажатии аппаратной кнопки в течение 2-5 секунд после подачи питания.
14.	Список поддерживаемых устройств постоянно обновляется (информация находится в файле converters.txt в архиве с прошивкой)
 
# Аппаратная часть
Устройство можно [собрать самостоятельно](https://modkam.ru/?p=1342), или приобрести на сайте [Smart Logic System](slsys.io)

# Прошивка устройства
[Постоянная ссылка на прошивку устроуства](https://github.com/slsys/Gateway/raw/master/rom/zigbee_gw_full.7z)


[История изменений прошивки](/rom/history_ru.md)





# Первый запуск.
После прошивки и перезагрузки устройства, шлюз создает точку доступа без пароля в формате zgwABCD. После подключения к ней, автоматически открывается страница настроек (если не открылась, можно зайти по адресу 192.168.1.1) и прописать настройки для подключения к домашней точке доступа и к MQTT серверу (но его можно указать и позже), нажимаем перезагрузку и шлюз подключится к точке доступа и начнет слать сообщения в MQTT.

# Web-интерфейс.
 
1.	Zigbee – настройки Zigbee.
2.	WiFi Setup – настройки WiFi.
3.	Link Setup – настройки сервера MQTT
4.	HW Setup – настройки аппаратной части шлюза.
5.	Log – отображение лога шлюза.
6.	Update – обновление прошивки шлюза по ОТА.
7.	Reset – сброс настроек к первоначальному состоянию.
8.	Reboot – перезагрузка шлюза.



# 1.	Zigbee
 
nwkAddr – Адрес устройства в сети. Координатор всегда имеет адрес 0x0000.
FriendlyName - Дружественное имя устройства в сети.
ieeeAddr – Физический адрес устройства в сети.
ManufName – Наименование производителя.
ModelId – Условное обозначение модели устройства.
LinkQuality – Оценки качества связи.
Interview – Процесс получения исходных данных от устройства при первой привязке устройства к шлюзу.
LastSeen – Время, которое прошло с момента последнего сообщения от устройства.
Routes – Адрес устройства, являющегося маршрутизатором.
PS – Заряд батареи в %, при условии, что устройство питается от батареи. Если устройство питается от сети, то отображается значок «≈»
Actions: управление устройствами. Удалить устройство, задать имя

Back – вернуться на предыдущую страницу.
Refresh – Обновить текущую страницу.
Groups – создание, редактирование, удаление групп. (На стадии разработки)
 
Setup – настройка номера канала и PanId.  Возможность выбора формата передаваемых данных MQTT.
 
Start Join – запуск режима спаривания устройств в течение 255 секунд.

#2.	WiFi Setup
 








#3.	Link Setup
 

 










#4.	HW Setup
 
 В данном меню можно выбрать тип модуля Zigbee TI или NXP, а также указать номера GPIO модуля ESP32: 
1.	RX
2.	TX
3.	Аппаратной кнопки. (если кнопка притянута к земле, а при нажатии на нее на вход ESP32 подается 3.3В, то обязательно нужно ставить галку PullUp.
4.	Красный светодиод. (зажигается, когда приходит сообщение от конечного zigbee устройства)
5.	Зеленый светодиод. (зажигается, когда активен режим Join)
6.	Синий светодиод.

 


#5.	Log 
 
#6.	Update 
 
#7.	Reset 
Очистка памяти модуля Zigbee, удаление привязанных устройств.
 
Координатор + шлюз для модернизации оригинального шлюза Xiaomi.
(круглая плата)
Для данной платы используется та же прошивка, соответственно на нее распространяются те же правила, что и для простого шлюза SLS. Описано в FAQ.
Однако есть несколько особенностей, которые относятся только к этой плате.
Так как она собрана по схеме уважаемого @Jager, следует иметь ввиду следующее:
1.	При первом запуске, после настройки параметров WiFi, обязательно нужно зайти в настройки HW Setup и выставить настройки GPIO следующим образом:
 
PullUp – галка нужна обязательно, дабы исключить постоянные зависания шлюза при старте, которые устраняются коротким нажатием кнопки SW1.
2.	
 
# [FAQ](/faq.md).
