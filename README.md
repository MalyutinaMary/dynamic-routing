# Динамическая маршрутизация

## Эмулятор
* GNS3

## Оборудование
* Маршрутизатор Cisco 3745
* Коммутатор (GNS3 built-in switch)

## OSPF
![image](https://github.com/user-attachments/assets/01ae052a-f036-47fe-b29a-cae18e2f72ce)
![image](https://github.com/user-attachments/assets/2a5fa14e-0f19-4b5f-a3ce-93f024b8055a)

## HSRP
HSRP (Hot Standby Router Protocol) — проприетарный протокол Cisco, предназначенный для обеспечения отказоустойчивости шлюза по умолчанию в локальной сети. Относится к семейству FHRP (First-hop Redundancy Protocols).

![image](https://github.com/user-attachments/assets/15ed3450-865f-46ce-a965-992023892b49)

В качестве шлюза по умолчанию для PC1 укажем IP-адрес `10.10.10.10/24`.

Объединим маршрутизаторы R1 и R2 в HSRP-группу `1` с единым виртуальным IP-адресом `10.10.10.10/24`:
> R1# (config-if) `standby 1 ip 10.10.10.10`
> 
> R2# (config-if) `standby 1 ip 10.10.10.10`

После выполнения команды `standby` для HSRP-группы будет сгенерирован виртуальный MAC-адрес `0000.0C07.AC01`, где `01` указывает на номер группы.

Поскольку R1 был сконфигурирован первым, скорее всего, он получит статус активного маршрутизатора (active). R2 получит статус резервного маршрутизатора (standby). Теперь R1 использует виртуальный IP-адрес для обработки трафика и отвечает на ARP-запросы с виртуальным MAC-адресом, а R2 находится в режиме ожидания.

![image](https://github.com/user-attachments/assets/0fd50594-2e16-4828-9d11-fdd9ed02da3a)
![image](https://github.com/user-attachments/assets/326a92e3-c897-4503-8034-5803bfed991e)

R1 регулярно отправляет R2 сообщения `Hello`. Если R1 выходит из строя и R2 перестает получать сообщения `Hello`, R2 отправляет Gratuitous ARP, чтобы коммутатор обновил таблицу MAC-адресов. При этом таблица маршрутизации PC1 не изменится.

![image](https://github.com/user-attachments/assets/9f6b7e7c-f621-4a3c-a702-558a5d554a03)
