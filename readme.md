![Git-logo](Git-logo.png)# Инструкция по работе с Git и удалёнными репозиториями

## Общее

Git — система контроля версий (файлов). Что-то вроде возможности сохраняться в компьютерных играх (в Git эквивалент игрового сохранения — коммит). **Важно**: добавление файлов к «сохранению» двухступенчатое: сначала добавляем файл в индекс (`git add`), потом «сохраняем» (`git commit`).

Любой файл в директории существующего репозитория может находиться или не находиться под версионным контролем (отслеживаемые и неотслеживаемые).

Отслеживаемые файлы могут быть в 3-х состояниях: неизменённые, изменённые, проиндексированные (готовые к коммиту).

### Ключ к пониманию

Ключ к пониманию концепции git — знание о «трех деревьях»:

- Рабочая директория — файловая система проекта (те файлы, с которыми вы работаете).
- Индекс — список отслеживаемых git-ом файлов и директорий, промежуточное хранилище изменений (редактирование, удаление отслеживаемых файлов).
- Директория `.git/` — все данные контроля версий этого проекта (вся история разработки: коммиты, ветки, теги и пр.).

Коммит — «сохранение» (хранит набор изменений, сделанный в рабочей директории с момента предыдущего коммита). Коммит неизменен, его нельзя отредактировать.

У всех коммитов (кроме самого первого) есть один или более родительских коммитов, поскольку коммиты хранят изменения от предыдущих состояний.

### Простейший цикл работ

- Редактирование, добавление, удаление файлов (собственно, работа).
- Индексация/добавление файлов в индекс (указание для git какие изменения нужно будет закоммитить).
- Коммит (фиксация изменений).
- Возврат к шагу 1 или отход ко сну.

### Указатели

- `HEAD` — указатель на текущий коммит или на текущую ветку (то есть, в любом случае, на коммит). Указывает на родителя коммита, который будет создан следующим.
- `ORIG_HEAD` — указатель на коммит, с которого вы только что переместили `HEAD` (командой `git reset ...`, например).
- Ветка (`master`, `develop` etc.) — указатель на коммит. При добавлении коммита, указатель ветки перемещается с родительского коммита на новый.
- Теги — простые указатели на коммиты. Не перемещаются.


## Создание локального репозитория
Создать локальный репозиторий можно с помощью команды *git init*. Для этого надо применить эту команду в той папке, где в будущем будет репозиторий.
``` bash
git init             # создать новый проект в текущей директории
git init folder-name # создать новый проект в указанной директории
```

## Добавление файлов в репозиторий
Версионность к файлу добавляется с помощью команды *git add*. Команда применяется следующим образом *git add <название файла>*
``` bash
git add .        # добавить в индекс все новые, изменённые, удалённые файлы из текущей директории и её поддиректорий
git add text.txt # добавить в индекс указанный файл (был изменён, был удалён или это новый файл)
git add -i       # запустить интерактивную оболочку для добавления в индекс только выбранных файлов
git add -p       # показать новые/изменённые файлы по очереди с указанием их изменений и вопросом об отслеживании/индексировании
```

## Создание коммита
Для того, чтобы создать новый коммит необходимо использовать команду *git commit*. Применяется она следующим образом: *git commit -m "<сообщение к коммиту>"*. Сообщения к коммиту писать ***ОБЯЗАТЕЛЬНО***!
``` bash
git commit -m "Name of commit"    # зафиксировать в коммите проиндексированные изменения (закоммитить), добавить сообщение
git commit -a -m "Name of commit" # проиндексировать отслеживаемые файлы (ТОЛЬКО отслеживаемые, но НЕ новые файлы) и закоммитить, добавить сообщение
```
## Просмотр наличия изменений
Для того, чтобы посмотреть имеются ли изменения в локальном репозитории используется команда *git status*. Достаточно её просто применить в папке с препозиторием
``` bash
git status              # показать состояние репозитория (отслеживаемые, изменённые, новые файлы и пр.)
```

## Просмотр разницы между текущей версией и последней "сохранённой"

Для того, чтобы посмотреть разницу между последним коммитом и текущей версией файлов, используется команда *git diff*. Достаточно её просто применить в папке с репозиторием
``` bash
git status              # показать состояние репозитория (отслеживаемые, изменённые, новые файлы и пр.)
git diff                # сравнить рабочую директорию и индекс (неотслеживаемые файлы ИГНОРИРУЮТСЯ)
git diff --color-words  # сравнить рабочую директорию и индекс, показать отличия в словах (неотслеживаемые файлы ИГНОРИРУЮТСЯ)
git diff index.html     # сравнить файл из рабочей директории и индекс
git diff HEAD           # сравнить рабочую директорию и коммит, на который указывает HEAD (неотслеживаемые файлы ИГНОРИРУЮТСЯ)
git diff --staged       # сравнить индекс и коммит с HEAD
git diff master feature # посмотреть что сделано в ветке feature по сравнению с веткой master
git diff --name-only master feature # посмотреть что сделано в ветке feature по сравнению с веткой master, показать только имена файлов
git diff master...feature # посмотреть что сделано в ветке feature с момента (коммита) расхождения с master
```


## Просмотр истории коммитов

Для просмотра истории коммитов используется команда *git log*. Для того, чтобы увидеть историю коммитов, достаточно просто применить эту команду в папке с репозиторием.
``` bash
git log             # перечисляет коммиты, сделанные в репозитории в обратном к хронологическому порядке
git log --stat      # сокращенная статистика для каждого коммита
```

## Перемещения между изменениями

Для того, чтобы переместиться на какое-то из прошлых изменений необходимо использовать команду *git checkout*. Применяется она следующим образом в папке с репозиторием: *git checkout <номер коммита>*. Для возврата к последнему коммиту ветки нужно использовать команду *git checkout master*
``` bash
git checkout b9533bb # переключиться на коммит с указанным хешем (переместить HEAD на указанный коммит, рабочую директорию вернуть к состоянию, на момент этого коммита)
git checkout master  # переключиться на коммит, на который указывает master (переместить HEAD на коммит, на который указывает master, рабочую директорию вернуть к состоянию на момент этого коммита)
```

## Работа с ветками


### Просмотр списка веток

Для того, чтобы просмотреть список имеющихся веток, необходимо использовать команду *git branch*. Для этого в папке с репозиторием пишем команду *git branch*.
``` bash
git branch                 # показать список веток
git branch -v              # показать список веток и последний коммит в каждой
```

### Создание новой ветки

Для того, чтобы создать новую ветку, необходимо использовать команду *git branch*. Используется она в папке с репозиторием следующим образом: *git branch <название новой ветки>* 
``` bash
git branch new_branch      # создать новую ветку с указанным именем на текущем коммите
git branch new_branch 5589877 # создать новую ветку с указанным именем на указанном коммите
```
## Слияние веток

Для слияния веток используется команда *git merge*. Приняется она в папке с репозиторием следующим образом: *git merge <название ветки>*. В процессе слияния может произойти ***КОНФЛИКТ*** или слияние может произойти автоматически
``` bash
git merge hotfix           # влить в ветку, в которой находимся, данные из ветки hotfix
git merge hotfix -m "Горячая правка" # влить в ветку, в которой находимся, данные из ветки hotfix (указано сообщение коммита слияния)
git merge hotfix --log     # влить в ветку, в которой находимся, данные из ветки hotfix, показать редактор описания коммита, добавить в него сообщения вливаемых коммитов
git merge hotfix --no-ff   # влить в ветку, в которой находимся, данные из ветки hotfix, запретить простой сдвиг указателя, изменения из hotfix «останутся» в ней, а в активной ветке появится только коммит слияния
```
## git clone
Команда git clone + url используется для того чтобы клонировать другие проекты на таких сайтах как github.com и т.д 
``` bash
# клонировать удаленный репозиторий в одноименную директорию
git clone https://github.com/cyberspacedk/Git-commands.git    

# клонировать удаленный репозиторий в директорию «FolderName»
git clone https://github.com/cyberspacedk/Git-commands.git FolderName 

# клонировать репозиторий в текущую директорию
git clone https://github.com:nicothin/web-design.git .           
```


