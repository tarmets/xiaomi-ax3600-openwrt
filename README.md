Установка OpenWrt на роутер Xiaomi AIoT AX3600
Этот репозиторий содержит пошаговую инструкцию по установке прошивки OpenWrt версии 24.10.2 на роутер Xiaomi AIoT AX3600. OpenWrt предоставляет расширенные возможности настройки сети, VPN, QoS и других функций по сравнению со стоковой прошивкой.

Предупреждение:

Установка альтернативной прошивки аннулирует гарантию.
Неправильные действия могут "закирпичить" роутер. Следуйте инструкции внимательно.
Сохраните резервную копию заводской прошивки перед началом.


Содержание

Требования
Подготовка
Получение SSH-доступа
Установка initramfs прошивки
Установка squashfs прошивки
Настройка OpenWrt
Решение проблем
Ресурсы
Лицензия

Требования

Роутер: Xiaomi AIoT AX3600 (кодовое имя R3600).
Компьютер: С установленными PuTTY (Windows) или терминалом (Linux/Mac).
Сетевой кабель: Для подключения роутера к компьютеру.
Прошивки:
openwrt-24.10.2-qualcommax-ipq807x-xiaomi_ax3600-initramfs-factory.ubi
openwrt-24.10.2-qualcommax-ipq807x-xiaomi_ax3600-squashfs-sysupgrade.bin
Скачать с OpenWrt Firmware Selector.


XMiR-Patcher: Для упрощения получения SSH (GitHub).
Python: Для генерации пароля SSH/Telnet.
Стоковая прошивка: Последняя версия (например, 1.1.25) с miwifi.com для восстановления.

Подготовка

Сброс роутера:
Нажмите и удерживайте кнопку Reset на роутере 10 секунд, пока индикатор не начнет мигать.


Подключение:
Подключите роутер к компьютеру через сетевой кабель.
Убедитесь, что компьютер получил IP в диапазоне 192.168.31.x (IP роутера: 192.168.31.1).


Проверка прошивки:
Зайдите в веб-интерфейс роутера (http://192.168.31.1).
Проверьте версию прошивки. Если не 1.1.25, обновите через официальный интерфейс.



Получение SSH-доступа
Xiaomi ограничивает доступ к SSH/Telnet. Используйте XMiR-Patcher для активации.

Скачивание XMiR-Patcher:
Загрузите с GitHub и распакуйте в папку (например, AX3600).


Генерация пароля:
Найдите серийный номер роутера (на наклейке или в веб-интерфейсе).
Запустите скрипт password.py:python scripts/password.py

Введите серийный номер, чтобы получить пароль.


Подключение:
Откройте PuTTY, выберите Telnet (или SSH, если активирован), введите 192.168.31.1.
Логин: root, пароль: сгенерированный.




Если Telnet/SSH не работает, используйте OpenWRTInvasion.

Установка initramfs прошивки

Копирование прошивки:
Переместите файл openwrt-24.10.2-qualcommax-ipq807x-xiaomi_ax3600-initramfs-factory.ubi:scp openwrt-24.10.2-qualcommax-ipq807x-xiaomi_ax3600-initramfs-factory.ubi root@192.168.31.1:/tmp




Прошивка:
В PuTTY выполните:ubiformat /dev/mtd13 -y -f /tmp/openwrt-24.10.2-qualcommax-ipq807x-xiaomi_ax3600-initramfs-factory.ubi -s 2048 -O 2048
nvram set flag_boot_rootfs=1
nvram set flag_last_success=1
nvram commit
reboot





Установка squashfs прошивки

Доступ к OpenWrt:
После перезагрузки зайдите в LuCI: http://192.168.1.1.
Установите пароль для root-пользователя.


Копирование прошивки:
Переместите файл openwrt-24.10.2-qualcommax-ipq807x-xiaomi_ax3600-squashfs-sysupgrade.bin:scp openwrt-24.10.2-qualcommax-ipq807x-xiaomi_ax3600-squashfs-sysupgrade.bin root@192.168.1.1:/tmp




Обновление:
Выполните:sysupgrade -n /tmp/openwrt-24.10.2-qualcommax-ipq807x-xiaomi_ax3600-squashfs-sysupgrade.bin





Настройка OpenWrt

Веб-интерфейс:
Зайдите в LuCI (http://192.168.1.1).
Настройте Wi-Fi (Network > Wireless, включите радиомодули) и интернет.


Проверка:
Убедитесь, что интернет и Wi-Fi работают.
Установите пакеты через LuCI или opkg при необходимости.



Решение проблем

Роутер не загружается:
Используйте стоковую прошивку и Xiaomi Recovery Tool.
Создайте резервную копию заводской прошивки:dd if=/dev/mtd0 of=/tmp/backup.bin
scp /tmp/backup.bin user@your_pc:/path/to/backup




Wi-Fi не работает:
Включите радиомодули в LuCI (Network > Wireless).


SSH/Telnet проблемы:
Проверьте версию прошивки и используйте OpenWRTInvasion.


Чип памяти:
Проверьте тип чипа:dmesg | grep nand


Убедитесь, что прошивка соответствует чипу (Foresee, Winbond, ESMT).



Ресурсы

OpenWrt Wiki
XMiR-Patcher
4PDA
Firmware Selector

Лицензия
Этот проект распространяется под лицензией MIT. См. файл LICENSE.
