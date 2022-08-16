Введение в Git
==============

**Git**- одна из систем контроля версий. Нужна для того, чтобы хранить различные версии проекта и командной работы.

Файлы в Git могут находиться в трех основных состояниях: **зафиксированном, модифицированном и индексированном**.

1. **Зафиксированное (committed)** состояние означает, что данные надежно сохранены в локальной базе.
2. **Модифицированное (modified)** состояние означает, что изменения уже внесены в файл, но пока не зафиксированы в базе данных. 
3. **Индексированное (staged)** состояние означает, что вы пометили текущую версию модифицированного файла как предназначенную для следующей фиксации.


Подготовка к работе
-------------------

1. Установка для Ubuntu

        apt-get install git

2. Проверяем версию git

        git --version

3. Представляемся системе контроля версий. Делается всего один раз. Храниться в глобальном конфиге.

>Свое имя (английскими буквами)
        
        git config --global user.name «Jane Doe»

>Свою почту

        git config --global user.email janedoe@example.com


Создаем репозиторий
-------------------

>Создаем и заходим в папку проекта

        mkdir project_name && cd project_name

>Инициализируем репозиторий

        git init

В результате в существующей папке появится еще одна папка с именем .git и всеми нужными вам файлами репозитория.

>Указываем файлы (file_name.ext), за которыми должна следить система - **индексировать**.

        git add file_name.ext

>Делаем фиксацию (**commit**)

        git commit -m "Наше сообщение"

Сокращенное написание этих двух команд
---------------------------------------
        
        git commit -am "Наше сообщение"

Сообщение общепринято писать на английском.

Добавления сразу несколько файлов в индекс
----------------------------------------------------------------

        git add .

Проверяем состояние файлов
--------------------------

    git status

Нет изменений в рабочем каталоге

![Статус - нет изменений в рабочем каталоге](Screenshot.png)

Изменения не в индексе

![Статус - изменения не в индексе](Screenshot2.png)

Проверяем какие ветви есть и на какой мы находимся
--------------------------------------------------

        git branch

![Работа с ветками](Screenshot3.png)

**Символ (*)** перед названием ветки указывает на то, что **это текущая ветка (мы тут)**.

Создаем новую ветку
-------------------

        git branch our_new_branch
        

Переключаемся на нужную ветку
-----------------------------

        git checkout our_new_branch

После этого необходимо проверить появилась ли наша ветвь в списке ветвей. Проверяем командой `git branch`.

Просмотр истории версий
------------------------

        git log

![История версий](Screenshot4.png)

Команда выведет несколько последних коммитов.

Форматированный вывод истории версий

        git log --oneline

![История версий - форматированный вывод](Screenshot5.png)

Команда выведет несколько последних коммитов.

Просмотр журнала ссылок
-----------------------

    git reflog

![Журнал ссылок](Screenshot6.png)

Одной из вещей, которую Git делает в фоновом режиме, является ведение журнала ссылок, в котором хранятся ссылки указателей HEAD и веток за последние несколько месяцев.

Просмотр разницы в измененных файлах
------------------------------------

        git diff e55e263 f92ee7f

![Разница в двух файлах с хэшем e55e263 и f92ee7f](Screenshot7.png)

Хэши e55e263 и f92ee7f, это хэши коммитов.

Откат в другой коммит
---------------------

        git checkout f92ee7f

![Откат изменений в файлах до версии с хешем f92ee7f](Screenshot8.png)

Переключит к нужной версии файлов (f92ee7f).

**Но, при переключении между коммитами может возникнуть ошибка HEAD detached.**

Правильно вернуться к текущей HEAD
----------------------------------

        git switch -

Или

        git checkout master

Эта команда переключит проект **в последний коммит текущей ветки (master)**.

Исправляем HEAD detached
------------------------

Указатель HEAD в Git определяет вашу текущую рабочую версию (и, следовательно, файлы, помещенные в рабочий каталог вашего проекта). Обычно при извлечении ветки Git автоматически перемещает указатель HEAD и при создании новых коммитов: вы автоматически и всегда находитесь на самом новом коммите выбранной ветки.

При проверке определенного коммита Git НЕ сделает это за вас, создается HEAD detached. Отсоединенная голова означает, что после проверки коммита все изменения, сделанные с этой точки, не принадлежат ни одной ветке, если только не будет создана новая, содержащая изменения из этой фиксации.

**Исправить эту проблему можно 4-мя командами**

        git branch temp
        git checkout temp
        git branch -f master temp
        git checkout master

И, опционально,

        git branch -d temp

Работа с удаленными репозиториями
----------------------------------

**Подключиться к удаленному репозиторию** 

        git remote add [сокращение] [url]

Пример:

        git remote add origin https://github.com/github_username/someproject.git

Если имя origin уже занято, его можно заменить на любое другое, например gb

        git remote add gb https://github.com/github_username/someproject.git

или **скопировать**

        git clone https://github.com/github_username/someproject.git

По умолчанию команда `git clone` автоматически настраивает вашу **локальную ветку** `main` на отслеживание **удалённой ветки** `main` на сервере, с которого клонировали (подразумевается, что на удалённом сервере есть ветка main). 

Список всех удаленных репозиториев
----------------------------------

        git remote

Узнать какому URL соответствует сокращённое имя origin в Git
------------------------------------------------------------

        git remote -v show origin

Получения данных из удалённых проектов
--------------------------------------

### Команда Fetch
        
        git fetch [remote-name] (у меня origin или gb)

        git fetch origin 

Данная команда связывается с указанным удалённым проектом и забирает все те данные проекта, которых у вас ещё нет. После того как вы выполнили команду, у вас должны появиться ссылки на все ветки из этого удалённого проекта. Теперь эти ветки в любой момент могут быть просмотрены или слиты. 

Когда вы клонируете репозиторий, команда `clone` автоматически добавляет этот удалённый репозиторий под именем `origin`. Таким образом `git fetch origin` извлекает все наработки, отправленные (`push`) на этот сервер после того, как вы склонировали его (или получили изменения с помощью `fetch`). 

Важно: **команда fetch забирает данные** в ваш локальный репозиторий, но **не сливает их** с какими-либо вашими наработками, и **не модифицирует то**, над чем вы работаете в данный момент. Вам **необходимо вручную слить эти данные с вашими**, когда вы будете готовы.

### Команда Pull

Если наша ветка настроенна на отслеживание удалённой ветки(автоматически-если использовали `git clone`), то используем команду

        git pull
        
Она автоматичеси извлекает и затем сливает данные из удалённой ветки в нашу текущую ветку.Выполнение `git pull` как правило извлекает (`fetch`) данные с сервера, с которого вы изначально склонировали, и автоматически пытается слить (`merge`) их с кодом, над которым вы в данный момент работаете. 

### Команда Push
Поделиться наработками, для этого необходимо отправить (`push`) их в главный репозиторий. 
        
        git push [удал. сервер] [ветка]. 

Чтобы отправить нашу ветку `main` на сервер `origin` (клонирование настраивает оба этих имени автоматически).

        git push origin main 

Эта команда срабатывает только в случае, если мы клонировали с сервера, на котором у нас есть права на запись, и если никто другой с тех пор не выполнял команду `push`. Если мы и кто-то ещё одновременно клонируете, затем он выполняет команду push, а затем команду push выполняете вы, то ваш push точно будет отклонён. Вам придётся сначала вытянуть (`pull`) их изменения и объединить с вашими. Только после этого вам будет позволено выполнить push.

Удаление удалённых репозиториев
-------------------------------
        git remote rm origin

Заливка своего существующего репозитория на удаленный сервис
----------------------------------------------------------------

1. Создать аккаунт на любом сервисе контроля версий. У меня GitHub
2. Связать локальный репозиторий с удаленным

        git remote add origin https://github.com/github_username/someproject.git

3. Указать основную ветвь

        git branch -M main

4. Авторизоваться при первом **push**, сгенерировать ssh ключи. Скопировать публичный ключ и добавить его в разделе Security на GitHub.

        git push origin main

Изменили файл в локальном репозитории -> Commit -am "Something" -> git push.

Pull Request
------------
1. Делаем fork (ответвление) репозитория

       git fork https://github.com/github_username2/someproject2.git

2. Делаем `git clone` СВОЕЙ версии репозитория

        git clone https://github.com/github_your_username/someproject2.git

3. Создаем новую ветку и переходим на НЕЁ. В НЕЙ вносим свои изменения

        git checkout -b my_branch_changes

4. Фиксируем изменения (делаем коммиты)

        git commit -am "Some changes"

5. Отправляем свою версию в свой аккаунт GitHub
        
        git push origin main

6. На сайте GitHub нажимаем кнопку pull request. 
**Проверить кому , в какую ветку, я из какой ветки сделаю pull request.**


