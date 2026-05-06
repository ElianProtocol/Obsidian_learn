В экосистеме Linux ключевые дистрибутивы формируют стандарты администрирования и инструменты, с которыми ежедневно работает инженер. Для производственных сред особенно важны три семейства: Debian, Red Hat (RHEL и совместимые AlmaLinux/Rocky) и Ubuntu. Они различаются моделью релизов, системой пакетов, политиками безопасности и экосистемой поддержки, что влияет на выбор базового образа, подход к обновлениям и совместимость с ПО.  

## Модели релизов и поддержка

- Debian ориентирован на стабильность. Ветви: stable (консервативная, редкие обновления), testing (более свежие пакеты), unstable (для разработчиков). LTS-поддержка расширена сообществом; релизы выходят не по жесткому календарю.
- Ubuntu базируется на Debian, имеет регулярный цикл: LTS-версии каждые 2 года с поддержкой 5 лет (расширяемо в коммерческих редакциях). Широко представлен в облаках и контейнерах, быстро получает новые ядра и драйверы.
- Red Hat Enterprise Linux — коммерческая поддержка до 10 лет, консервативные обновления, предсказуемость API/ABI. Бесплатные бинарно-совместимые форки (AlmaLinux, Rocky) повторяют модель жизненного цикла. CentOS Stream — «препрод» ветвь между Fedora и RHEL.

## Сравнение ключевых свойств

|Критерий|Debian|Ubuntu|Red Hat (RHEL/Alma/Rocky)|
|---|---|---|---|
|Формат пакетов|.deb|.deb|.rpm|
|Менеджер пакетов|apt, dpkg|apt, dpkg|dnf/yum, rpm|
|Init/Service|systemd|systemd|systemd|
|Брандмауэр по умолчанию|iptables/nftables (ручная настройка)|ufw поверх nftables|firewalld (nftables)|
|Безопасность|AppArmor доступен, по умолчанию не всегда активен|AppArmor по умолчанию|SELinux Enforcing по умолчанию|
|Релизная стратегия|стабильность важнее новизны|регулярные LTS и промежуточные|длительная поддержка, консервативность|
|Целевое применение|минималистичные серверы, embedded|облака, десктоп, серверы|корпоративные серверы, сертификации|
|Репозитории|main, contrib, non-free|main, universe, multiverse|BaseOS, AppStream, CRB/EPEL|

## Пакеты и репозитории: базовые операции

Обновление индексов и установка пакетов:  

```bash

# Debian/Ubuntu
sudo apt update
sudo apt install nginx
dpkg -l | grep nginx

# RHEL-семейство
sudo dnf makecache
sudo dnf install nginx
rpm -qi nginx
```

Подключение сторонних репозиториев:  

```bash

# Ubuntu: PPA или отдельный источник
sudo add-apt-repository ppa:ondrej/php
# либо
echo "deb http://mirror.example/repo stable main" | sudo tee /etc/apt/sources.list.d/custom.list
sudo apt update

# RHEL: EPEL и внешние репозитории
sudo dnf install epel-release
sudo dnf config-manager --add-repo https://repo.example/custom.repo
sudo dnf makecache
```

Управление службами единообразно благодаря systemd:  

```bash

sudo systemctl enable --now nginx
sudo systemctl status nginx
sudo journalctl -u nginx -e
```

## Политики безопасности и конфигурация сети

- SELinux (RHEL) обеспечивает строгую модель мандатного контроля; требует понимания контекстов и логов audit. Подходит для сред с жесткими требованиями комплаенса.
- AppArmor (Ubuntu) использует профили на уровне приложений, проще в начальной настройке.
- Межсетевой экран: ufw в Ubuntu упрощает базовые правила, firewalld в RHEL предоставляет зоны и богатую модель управления, Debian чаще настраивают напрямую через nftables.

Сетевая конфигурация отличается инструментами по умолчанию:  

- Ubuntu Server использует Netplan (рендерит в systemd-networkd или NetworkManager).
- RHEL-семейство использует NetworkManager и nmcli/nmtui.
- Debian допускает как systemd-networkd, так и ifupdown; выбор зависит от профиля установки.

## Рекомендации выбора

- Требуется длительная поддержка, сертификации, SELinux, предсказуемые ABI — RHEL/Alma/Rocky.
- Нужны быстрый старт в облаке, широкая экосистема образов и драйверов — Ubuntu LTS.
- Нужна максимальная предсказуемость без лишних надстроек и минимализм — Debian stable.


[[Linux администрирование]]