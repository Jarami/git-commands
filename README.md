## Основные команды Git:
```bash
# Инициализация репозитория:
$ git init

# Проверить состояние репозитория:
$ git status

# Подготовить файлы к сохранению (добавить в staged):
$ git add <file>
$ git add --all # добавляет всё
$ git add . # добавляет всё

# Выполнить коммит:
$ git commit -m "Some message"
$ git commit --amend --no-edit # добавляет к предыдущему комиту
$ git commit --amend -m "New message" # добавляет к предыдущему + изменяет сообщение

# Просмотреть историю коммитов:
$ git log
$ git log --oneline # компактный вывод

# Привязать удалённый репозиторий к локальному:
$ git remote add origin https://github.com/<account-name>/<repo-name>.git 

# Информация об удаленных репозиториях:
$ git remote -v 

# Отправить изменения на удалённый репозиторий:
$ git push -u origin main # только для первого push
$ git push

# Убрать файл из staging в untracked/modified:
$ git restore --staged <file>
$ git restore --staged . # убирает всё

# Убрать файл из modified (если случайно поменяли какой-то файл):
$ git restore <file>

# Откатить коммит (осторожно, откатить откат невозможно): 
$ git reset --hard <commit hash> # в <commit hash> указывается, куда нужно откатить
```

## Статусы untracked/tracked, staged и modified
Одна из ключевых задач Git — отслеживать изменения файлов в репозитории. Для этого каждый файл помечается каким-либо статусом. Рассмотрим основные.

**untracked** (англ. «неотслеживаемый»)\
Новые файлы в Git-репозитории помечаются как untracked, то есть неотслеживаемые. Git «видит», что такой файл существует, но не следит за изменениями в нём. У untracked-файла нет предыдущих версий, зафиксированных в коммитах или через команду git add.

**staged** (англ. «подготовленный»)\
После выполнения команды ```git add``` файл попадает в staging area (от англ. stage — «сцена», «этап [процесса]» и area — «область»), то есть в список файлов, которые войдут в коммит. В этот момент файл находится в состоянии staged.\
По аналогии с фотографией, команда ```git add``` добавляет персонажей (текущее содержимое файла или нескольких файлов) на сцену (англ. stage) для общей фотографии, а ```git commit``` делает снимок всей сцены целиком.

**tracked** (англ. «отслеживаемый»)\
Состояние tracked — это противоположность untracked. Оно довольно широкое по смыслу: в него попадают файлы, которые уже были зафиксированы с помощью ```git commit```, а также файлы, которые были добавлены в staging area командой ```git add```. То есть все файлы, в которых Git так или иначе отслеживает изменения.

**modified** (англ. «изменённый»)\
Состояние modified означает, что Git сравнил содержимое файла с последней сохранённой версией и нашёл отличия. Например, файл был закоммичен и после этого изменён.

### Про staged и modified
Команда git add добавляет в staging area только текущее содержимое файла. Если вы, например, сделаете ```git add file.txt```, а затем измените file.txt, то новое содержимое файла не будет находиться в staging.
Git сообщит об этом с помощью статуса modified: файл изменён относительно той версии, которая уже в staging. Чтобы добавить в staging последнюю версию, нужно выполнить ```git add file.txt``` ещё раз.

## Типичный жизненный цикл файла в Git

![Типичный жизненный цикл файла в Git](lifecycle.png)

1. Файл только что создали. Git ещё не отслеживает содержимое этого файла. Состояние: untracked.
1. Файл добавили в staging area с помощью ```git add```. Состояние: staged (+ tracked).
   * Возможно, изменили файл ещё раз. Состояния: staged, modified (+ tracked).
   Обратите внимание: staged и modified у одного файла, но у разных его версий.
   * Ещё раз выполнили ```git add```. Состояние: staged (+ tracked).
1. Сделали коммит с помощью ```git commit```. Состояние: tracked.
1. Изменили файл. Состояние: modified (+ tracked).
1. Снова добавили в staging area с помощью ```git add```. Состояния: staged (+ tracked).
1. Сделали коммит. Состояния: tracked.
1. Повторили пункты 4−7 много-много раз.