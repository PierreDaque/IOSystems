# Лабораторная работа 2

**Название:** "Разработка драйверов блочных устройств"

**Цель работы:** получить знания и навыки разработки драйверов блочных устройств для операционной системы Linux. 

## Описание функциональности драйвера

Два первичных и один расширенный разделы с размерами  10Мбайт, 20Мбайт и 20Мбайт соответственно. Расширенный  раздел должен быть разделен на два логических с размерами  по 10Мбайт каждый.

## Инструкция по сборке
Makefile:  
obj-m :=main.o  
all :  
	make -C /lib/modules/$(shell uname -r)/build M=$(shell pwd) modules  
clean :  
	make -C /lib/modules/$(shell uname -r)/build M=$(shell pwd) clean  
do:  
	sudo insmod main.ko  
rm:  
	sudo rmmod main.ko  

## Инструкция пользователя
Собрать проект командой make в соответствии с инструкцией в Makefile  
Загрузить модуль в ядро командой sudo insmod main.ko
Командой sudo fdisk /dev/mydisk -l вывести список разделов выбранного диска
Убедиться, что диск разбит на разделы верно, в соответствии с вариантом задания
Командой dmesg просмотреть вывод в кольцевой буфер ядра

Убедившись в правильном выполнении, двигаться далее

Провести форматирование разделов диска с помощью команды mkfs.vfat
sudo mkfs.vfat /dev/mydisk1
sudo mkfs.vfat /dev/mydisk2
sudo mkfs.vfat /dev/mydisk3
sudo mkfs.vfat /dev/mydisk5
sudo mkfs.vfat /dev/mydisk6

Выдать полные права для раздела mydisk1
sudo chmod 777 /dev/mydisk1

Командой dd заполнить нулями первый сектор первого раздела
sudo dd if=/dev/zero of=/dev/mydisk1 count=1

Командой cat вставить некую строку 
sudo cat > /dev/mydisk1
this is a test 

Командой xxd показать первоначальные данные, находящиеся в разделе mydisk1
sudo xxd /dev/mydisk1 | less

Убедиться в наличии строки this is a test 

Скопировать эту строку в другой раздел виртуального диска и измерить скорость передачи данных
Командой dd скопировать эту строку из mydisk1 в mydisk2 
sudo dd if=/dev/mydisk1 of=/dev/mydisk2 count=1
Скорость составила 1.2 MB/s

Командой dd скопировать эту строку из mydisk2 в mydisk5 
sudo dd if=/dev/mydisk2 of=/dev/mydisk5 count=1
Скорость составила 1.7 MB/s

Скопировать эту строку из раздела виртуального в реальный жесткий диск и измерить скорость передачи данных
Командой dd скопировать эту строку из mydisk1 в sda1 
sudo dd if=/dev/mydisk2 of=/dev/sda1 count=1
Скорость составила  1.1 MB/s

Выгрузить модуль из ядра командой sudo rmmod main
## Примеры использования

user\@user:\~/block_device$ make  
user\@user:\~/block_device$ sudo insmod main.ko  
user\@user:\~/block_device$ sudo fdisk /dev/mydisk -l  
user\@user:\~/block_device$ sudo mkfs.vfat /dev/mydisk1  
user\@user:\~/block_device$ sudo mkfs.vfat /dev/mydisk2  
user\@user:\~/block_device$ sudo mkfs.vfat /dev/mydisk3  
user\@user:\~/block_device$ sudo mkfs.vfat /dev/mydisk5  
user\@user:\~/block_device$ sudo mkfs.vfat /dev/mydisk6  
user\@user:\~/block_device$ sudo chmod 777 /dev/mydisk1   
user\@user:\~/block_device$ sudo dd if=/dev/mydisk of=main  
user\@user:\~/block_device$ sudo dd if=/dev/zero of=/dev/mydisk1 count=1  
user\@user:\~/block_device$ sudo cat > /dev/mydisk1  
this is a test  
user\@user:\~/block_device$ sudo xxd /dev/mydisk1 | less  
user\@user:\~/block_device$ sudo dd if=/dev/mydisk1 of=/dev/mydisk2 count=1  
1+0 записей получено  
1+0 записей отправлено  
512 bytes copied, 0,000424924 s, 1,2 MB/s  
user\@user:\~/block_device$ sudo xxd /dev/mydisk2 | less  
user\@user:\~/block_device$ sudo dd if=/dev/mydisk2 of=/dev/mydisk5 count=1  
1+0 записей получено  
1+0 записей отправлено  
512 bytes copied, 0,000302084 s, 1,7 MB/s  
user\@user:\~/block_device$ sudo xxd /dev/mydisk5 | less    
user\@user:\~/block_device$ sudo dd if=/dev/mydisk2 of=/dev/sda1 count=1  
1+0 записей получено  
1+0 записей отправлено   
512 bytes copied, 0,000481475 s, 1,1 MB/s  
user\@user:\~/block_device$ sudo xxd /dev/sda1 | less  

![image](https://user-images.githubusercontent.com/48588005/110636148-fc337e80-81bc-11eb-9e63-52e818f9bb8d.png)


