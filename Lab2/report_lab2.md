# Языка программирования для задач искусственного интеллекта
Доренская Елизавета Артёмовна, НПИмд-01-24

## Лабораторная работа №2

#### Задание:
- Установить ROS2 Humble
- Изучить планировщик Pyperplan [Pyperplan Github](https://github.com/aibasel/pyperplan)
- Построить модель среды с tb3 (4) с манипулятором, либо любой другой колесный робот с манипулятором [E-manual for tb3](https://emanual.robotis.com/docs/en/platform/turtlebot3/overview/)
- Создать ROS узел с планировщиком.

### Немного теории из интернета:
- *ROS2 (Robot Operating System 2)* — это открытая платформа для разработки программного обеспечения в области робототехники, которая предоставляет набор инструментов и библиотек для создания сложных роботизированных систем. ROS2 улучшает возможности своего предшественника, ROS, добавляя поддержку реального времени, улучшенную межпроцессорную связь и возможность работы на различных аппаратных платформах. Он основан на архитектуре узлов, где каждый узел может обмениваться сообщениями через топики, что позволяет разработчикам легко интегрировать различные компоненты и модули в своих проектах. ROS2 широко используется в научных исследованиях, промышленности и образовательных учреждениях для создания и управления роботами.

### Подготовительная работа:
- Для выполнения лабораторной работы мной было приняно решение работать Ubuntu (версия 22.04 с графическим интерфейстом). Для этого нужно было скачать iso-образ с официального сайта Ubuntu, а также скачать Oracle VM VirlualBox, чтобы запустить виртуальную машину.
  
- Выбор среды был связан с тем, что ROS2 (Robot Operating System 2) официально поддерживается на Ubuntu, что обеспечивает совместимость и стабильность. Многие инструменты и библиотеки для робототехники, включая Pyperplan, часто разрабатываются с учетом работы на Ubuntu. Также для установки ROS2 можно легко ввести команды в терминале, что очень упрощает процесс.

Результат выполнения:<br /> 
![](pics/1.png)

### Установка ROS2 Humble:
- Проверка локали и установка необходимых пакетов:
```bash
locale  # check for UTF-8
sudo apt update && sudo apt install locales
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8
locale  # verify settings
```

Проверка текущие настройки локали системы до изменений:<br />
![](pics/2.jpg)

Проверка текущие настройки локали системы после изменений (по факту ничего не поменялось, все было так, как нужно):<br />
![](pics/3.jpg)<br />
Ремарка: сначала меняла настройки через администратора root, потому что от моего имя вылетала ошибка доступа, но потом добавила своего пользователя и назначила ему права администратора и все стало хорошо.

- Установка пакета для управления репозиториями:
```bash
sudo apt install software-properties-common
```

- Добавление репозитория ROS2:
```bash
sudo add-apt-repository universe
```

- Добавление ключа для пакетов ROS2:
```bash
sudo apt update && sudo apt install curl -y
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
```

- Добавление источника пакетов ROS2:
```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
```
 
- Обновление списка пакетов и установка ROS2:
```bash
sudo apt update
sudo apt upgrade
```

- Установка полного рабочего стола ROS2:
```bash
sudo apt install ros-humble-desktop-full
```
 
- Установка базовых компонентов ROS2:
```bash
sudo apt install ros-humble-ros-base
sudo apt install ros-dev-tools
```

- Настройка окружения:
```bash
source /opt/ros/humble/setup.bash
```

- Тестирование Talker и Listener: <br/>

В одном терминале и запуск Talker:
```bash
source /opt/ros/humble/setup.bash
ros2 run demo_nodes_cpp talker
```

В другом терминале запуск Listener:
```bash
source /opt/ros/humble/setup.bash
ros2 run demo_nodes_py listener
```

Результат выполнения:<br /> 
![](pics/4.png)

После завершения установки мы запускаем два узла: Talker и Listener. Talker отправляет сообщения в определённый топик, а Listener подписывается на этот топик и принимает сообщения, что позволяет продемонстрировать механизм обмена данными между узлами в ROS2. Этот процесс иллюстрирует основные принципы работы с системой ROS2, включая публикацию и подписку на сообщения, что является важным аспектом разработки распределённых приложений в робототехнике.


### Изучение планировщика Pyperplan [Pyperplan Github](https://github.com/aibasel/pyperplan):
- В документации прописано, что установить Pyperplan можно либо клонировав репозиторий, либо использовав команду ```pip install pyperplan```. Я решила воспользоваться 2ым способом, предварительно скачаа пакеты python

- Посмотреть подробную информацию о планировщике (как его запустить) можно выполнив команду:
```bash
pyperplan --help
```
или проще 
```bash
pyperplan -h
```

- Далее, чтобы убедиться, что pyperplan работает, можно запустить домен и задачу. <br />
 
Результат выполнения:<br /> 
![](pics/5.png)

### Построение модели среды с tb3 (4) с манипулятором

### Немного теории из интернета:<br /> 
-*TurtleBot3* — это небольшой, доступный и программируемый мобильный робот, основанный на Robot Operating System (ROS), предназначенный для использования в образовании, исследованиях, хобби и прототипировании продуктов. Разработанный компанией ROBOTIS, TurtleBot3 предлагает модульную конструкцию и возможность настройки, что позволяет пользователям адаптировать его под различные задачи. Он оснащён датчиками LiDAR и 3D, что позволяет ему автономно перемещаться и выполнять задачи, такие как одновременная локализация и картографирование (SLAM). TurtleBot3 доступен в нескольких моделях, включая "Burger" и "Waffle", каждая из которых имеет свои особенности и возможности для расширения.

- Я установила TurtleBot3 с его зависимостями и симуляциями из исходного кода, чтобы убедиться, что не будет никаких проблем. После завершения всех шагов в документации я запустила TurtleBot3 с моделью MODEL=waffle внутри Gazebo в пустом мире, чтобы убедиться, что после этого шага смогу просто запустить его с манипуляторной рукой. <br />
![](pics/6.png) <br />
![](pics/7.png) <br />

- Команды для установки
```bash
sudo apt install ros-humble-gazebo-*
sudo apt install ros-humble-cartographer
sudo apt install ros-humble-cartographer-ros
sudo apt install ros-humble-navigation2
sudo apt install ros-humble-nav2-bringup

mkdir -p ~/turtlebot3_ws/src
cd ~/turtlebot3_ws/src/
git clone -b humble-devel https://github.com/ROBOTIS-GIT/DynamixelSDK.git
git clone -b humble-devel https://github.com/ROBOTIS-GIT/turtlebot3_msgs.git
git clone -b humble-devel https://github.com/ROBOTIS-GIT/turtlebot3.git
cd ~/turtlebot3_ws
colcon build --symlink-install
echo 'source ~/turtlebot3_ws/install/setup.bash' >> ~/.bashrc
source ~/.bashrc

echo 'export ROS_DOMAIN_ID=30 #TURTLEBOT3' >> ~/.bashrc
source ~/.bashrc

cd ~/turtlebot3_ws/src/
git clone -b humble-devel https://github.com/ROBOTIS-GIT/turtlebot3_simulations.git
cd ~/turtlebot3_ws && colcon build --symlink-install

- launch turtlebot3
source /usr/share/gazebo/setup.sh
export TURTLEBOT3_MODEL=waffle
ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py
```
- Затем я установила зависимости для манипуляторной руки на TB3 Waffle и запустил её в среде машинного обучения.
Скринжот получился не совсем информативный, поскольку виртуальной машине не хватает оперативки для работы в программе.
![](pics/8.png) <br />


### 4- Создать ROS узел с планировщиком.
- Прочитав несколько документаций в интернете, я создал базовый узел ROS2, который просто использует pyperplan без инструкций, чтобы показать, что pyperplan запускается, затем узел вращается и корректно завершает работу. <br>

Результат выполнения: <br />
![](pics/9a.png) <br />
![](pics/9b.png) <br />
![](pics/9.png) <br />
![](pics/10.png) <br />

