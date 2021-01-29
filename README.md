# Pygame project
## Как играть
Игра жанра _Rogulite_, поэтому смерть в игре перманентная. Чтобы победить, надо пройти 10 уровней подземелья.
Чтобы убивать врагов, нужно использовать заклинания
Играть можно как на клавиутуре, так и на джойстике от PS4.  
Управление показывается внутри игры на главном окне при нажатии на кнопку "Управление", а так же на иконках заклинаний слева

## "_Это не баг, а фича_". Все механики игры, которые кажутся _специфичными_
  1. На уровнях можно встретить комнату, сгенерированную отдельно от всех. Туда нельзя попасть, т.к. проходов там нет.
    Сделано это специально, чтобы "_потролить_". В качестве доказательства [generation_map.py](https://github.com/Nik4ant/pygame_project/blob/master/generation_map.py)
  2. Когда игрок делает рывок, то он как будто скользит по льду. Всё потому, что параметр игрока dash_force_slower
    имеет такое значение, и в результате, пока сила рывка полностью не исчезает, игрок имеет ослабленное влияние над ускорением
    что приводит к эффекту скольжения. 
(Значение dash_force_slower легко изменить, но [автор](https://github.com/Nik4ant) посчитал эффект скольжения забавным :)
  3.  У объектов с обработкой физических столкновений, есть специальная часть - колайдер (относительно него, столкновения и обрабатываются),
  но т.к. колайдер равен половине размера тайла на уровне ( => является квадратным), то могут быть явление, похожие на баг, хотя такое поведение
  были сделано намерено (см. класс _Collider_ в [_entities/base_entity.py_](https://github.com/Nik4ant/pygame_project/blob/master/entities/base_entity.py))
  4. Отсутствие угловых анимаций, т.к. когда игра ещё планировалась, было решено сделать анимации игрока только в четыре стороны. 
  (За исключением анимации с выпуском заклинания, для неё действительно понадобились угловые анимации)
  5. В файле сохранения находится представление лишь об основных физических объектах в комнате, а 
  сохранение данных о декоративных элементах отсутствует. Это сделано в угоду оптимизации файла сохранения 
  (он очень маленький, просто откройте файл save.txt в папке data и убедитесь в этом сами.)

## Установка модулей
Для установки дополнительных модулей, в корневой папке выполините команду:
```sh
$ pip3 install -r requirements.txt 
```

## Архитектура
 - Assets - папка со всеми ассетами для игры таких как: графика, аудио и шрифт.
 - Data - папка с данными для игры не являющихся ассетами, там находится файл с сохранением текущего уровня.
 - Entities – папка со всеми игровыми сущностями, а именно скриптами отвечающими за их поведение
 - UI – папка с файлом представляющим UI компоненты (сделаны вручную) и всеми игровыми меню
 - config – файл с константами для игры, содержит в себе различные игровые параметры
 - engine – файл с функциями, которые нужны в большинстве файлах, поэтому для централизации таких функций был сделан этот файл
 - game – файл с главным циклом игры.
 - main – файл в котором происходить инициализация экрана, самого pygame и pygame.mixer. Ещё осуществляются действия по смене состояний.
 Т.е. обрабатывается переход на новый
 - generation_map – файл отвечающий за процедурную генерацию уровня, привязанной к сиду.

# Классы
  - Entity – наследуется от pygame.sprite.Sprite, представляет базовую перемещамую сущность
  - WalkingMonster и ShootingMonster – два класса вида врагов (ближнего и дальнего боя), наследуемых от Entity,
  - Классы каждого врага, наследуются от его вида.
  - Класс Tile, наследуется от спрайта. Представляет тайл на уровне
  - Классы Player – от Entity и PlayerScope – от спрайта. Представляют игрока и его прицел на уровне
  - Класс Spell - от спрайта. Представляет базовую сущность заклинания
  - Классы каждого заклинания наследуются от базового Spell.я

Лицензия
----
MIT
