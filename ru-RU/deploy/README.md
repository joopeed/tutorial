# Публикация!

> **Примечание**: Эта глава может показаться сложной. Будь упорна, развертывание сайта на сервере является важной частью веб-разработки. Данная глава намеренно расположена в середине учебника для того, чтобы твой наставник смог помочь с таким мудреным процессом, как публикация сайта. Так ты сможешь самостоятельно закончить все главы, даже если время будет поджимать.

До сих пор твой сайт был доступен только с твоего компьютера. Сейчас ты узнаешь, как "развернуть" свой сайт! Развертывание (deploy) — это процесс публикации приложения в интернете, чтобы люди могли наконец увидеть твое творение. :)

Как ты уже знаешь, веб-сайт должен располагаться на сервере. В интернете есть много сервер-провайдеров, мы будем использовать [PythonAnywhere](https://www.pythonanywhere.com/). PythonAnywhere предоставляется бесплатно для небольших приложений с небольшим числом посетителей, что более чем достаточно для нас.

Другим внешним сервисом, которым мы будем пользоваться, является [GitHub](https://www.github.com) — сервис хостинга кода. Существуют и другие похожие сервисы, но практически у каждого программиста есть GitHub аккаунт, теперь будет и у тебя!

Эти три места будут важны для вас. Ваш локальный компьютер будет местом, где вы будете вести разработку и тестирование. Когда вы удовлетворены изменениями, вы разместите копию вашей программы на GitHub. Ваш веб-сайт будет на PythonAnywhere, и вы будете обновлять его, получая новые копии вашего кода с GitHub.

# Git

> **Примечание**Если вы уже проделали все шаги установки, делать это вновь нет необходимости - вы можете пропустить этот раздел и приступить к созданию Git репозитория.

{% include "/deploy/install_git.md" %}

## Создаём Git-репозиторий

Git отслеживает изменения определенного набора файлов, которая называется репозиторием (сокращенно "репо"). Давайте создадим один для нашего проекта. Откройте консоль и запустите эти команды в папке `djangogirls`:

> **Примечание**Проверьте свой текущий рабочий каталог с помощью команд `pwd` (Mac OS X/Linux) или `cd` (Windows), прежде чем инициализировать репозиторий. Ты должна находиться в директории `djangogirls`.

{% filename %}command-line{% endfilename %}

    $ git init
    Initialized empty Git repository in ~/djangogirls/.git/
    $ git config --global user.name "Your Name"
    $ git config --global user.email you@example.com
    

Инициализировать git-репозиторий необходимо только один раз за проект (и тебе больше не придется снова вводить имя пользователя и адрес электронной почты).

Git будет отслеживать изменения всех файлов и каталогов в заданной директории, однако некоторые из них мы предпочли бы игнорировать. Для этого нам нужно создать файл `.gitignore` в корневом каталоге репозитория. Открой редактор и создай новый файл со следующим содержанием:

{% filename %}.gitignore{% endfilename %}

    *.pyc
    __pycache__
    myvenv
    db.sqlite3
    .DS_Store
    

И сохрани его как `.gitignore` в корневом каталоге "djangogirls".

> **Примечание**: Точка в начале имени файла имеет важное значение! Если у тебя есть проблемы с созданием таких файлов (Mac не позволит создать файл с названием, начинающимся с точки, через Finder, например), тогда используй кнопку "Сохранить как" в меню своего редактора кода, это точно поможет. And be sure not to add `.txt`, `.py`, or any other extension to the file name -- it will only be recognized by Git if the name is just `.gitignore`.
> 
> **Примечание** Один из файлов, заданных в ваш файл `.gitignore` — `db.sqlite3`. That file is your local database, where all of your users and posts are stored. We'll follow standard web programming practice, meaning that we'll use separate databases for your local testing site and your live website on PythonAnywhere. The PythonAnywhere database could be SQLite, like your development machine, but usually you will use one called MySQL which can deal with a lot more site visitors than SQLite. Either way, by ignoring your SQLite database for the GitHub copy, it means that all of the posts and superuser you created so far are going to only be available locally, and you'll have to create new ones on production. Вы должны думать о вашей локальной базе данных как о хорошей игровой площадке, где вы можете проверить разные вещи и не бояться, что вы удалите ваши реальные сообщения из вашего блога.

Использовать команду `git status` перед `git add` или в любой другой момент, когда ты не уверен, что изменилось — хорошая идея. Это убережёт тебя от таких неприятных сюрпризов, таких как добавление или "коммит" неправильных файлов. Команда `git status` возвращает информацию обо всех ранее неотслеживаемых/изменённых/добавленных в git файлах, а также статус ветки и многое другое. Результат должен быть аналогичен следующему:

{% filename %}command-line{% endfilename %}

    $ git status
    On branch master
    
    Initial commit
    
    Untracked files:
      (use "git add <file>..." to include in what will be committed)
    
            .gitignore
            blog/
            manage.py
            mysite/
            requirements.txt
    
    nothing added to commit but untracked files present (use "git add" to track)
    

И, наконец, мы сохраним наши изменения. Переключись на консоль и набери:

{% filename %}command-line{% endfilename %}

    $ git add --all .
    $ git commit -m "My Django Girls app, first commit"
     [...]
     13 files changed, 200 insertions(+)
     create mode 100644 .gitignore
     [...]
     create mode 100644 mysite/wsgi.py
    

## Публикация твоего кода на GitHub

Перейди на сайт [GitHub.com](https://www.github.com) и зарегистрируй новый бесплатный аккаунт. (Если ты уже это сделала в рамках подготовки к воркшопу, то это отлично!)

Дальше создай новый репозиторий с именем "my-first-blog". Не ставь галочку на пункте "initialize with a README" и отметь опции .gitignore(мы создадим этот файл вручную) и License как None.

![](images/new_github_repo.png)

> **Примечание:** Имя репозитория `my-first-blog` имеет очень большое значение. Ты конечно можешь придумать другое название, но оно встречается много раз в руководстве и тебе придется каждый раз менять его на своё. It's probably easier to stick with the name `my-first-blog`.

On the next screen, you'll be shown your repo's clone URL, which you will use in some of the commands that follow:

![](images/github_get_repo_url_screenshot.png)

Теперь нам нужно связать твой локальный Git репозиторий на компьютере с репозиторием на GitHub.

Type the following into your console (replace `<your-github-username>` with the username you entered when you created your GitHub account, but without the angle-brackets -- the URL should match the clone URL you just saw):

{% filename %}command-line{% endfilename %}

    $ git remote add origin https://github.com/<your-github-username>/my-first-blog.git
    $ git push -u origin master
    

When you push to GitHub, you'll be asked for your GitHub username and password (either right there in the command-line window or in a pop-up window), and after entering credentials you should see something like this:

{% filename %}command-line{% endfilename %}

    Counting objects: 6, done.
    Writing objects: 100% (6/6), 200 bytes | 0 bytes/s, done.
    Total 3 (delta 0), reused 0 (delta 0)
    
    To https://github.com/ola/my-first-blog.git
     * [new branch]      master -> master
    Branch master set up to track remote branch master from origin.
    

<!--TODO: maybe do ssh keys installs in install party, and point ppl who dont have it to an extension -->

Теперь твой код загружен на GitHub. Зайди на сайт и проверь! Вы найдете его в компании – [Django](https://github.com/django/django), [ Django Girls туториала](https://github.com/DjangoGirls/tutorial) и многих других проектов c открытым исходным кодом на GitHub. :)

# Настройка нашего блога на PythonAnywhere

## Регистрация аккаунта на PythonAnywhere

> **Примечание** Возможно, ты уже завела учётную запись на PythonAnywhere ранее — если так, не нужно повторять это снова.

{% include "/deploy/signup_pythonanywhere.md" %}

## Настройка нашего сайта на PythonAnywhere

Вернись в главное меню [PythonAnywhere Dashboard](https://www.pythonanywhere.com/), кликнув на логотип и выбери опцию запуска консоли Bash - это версия консоли PythonAnywhere как и на твоем компьютере.

![В разделе «Новой консоли» в веб-версии PythonAnywhere, с помощью кнопки "Bash"](images/pythonanywhere_bash_console.png)

> **Примечание**: PythonAnywhere использует Linux, так что если ты используешь Windows, то терминал и команды могут немного отличаться от того, к чему ты привыкла на своем компьютере.

Развёртывание веб-приложения на PythonAnywhere включает в себя загрузку твоего кода с GitHub и настройку PythonAnywhere, чтобы распознать его и запустить в качестве веб-приложения. Существуют ручные способы, чтобы сделать это, но PythonAnywhere предоставляет вспомогательный инструмент, который сделает это за тебя. Давай сначала установим его:

{% filename %}command-line{% endfilename %}

    pip3.6 install --user pythonanywhere
    

В консоли должно напечататься что-то подобное`Collecting pythonanywhere` и в конце `Successfully installed (...) pythonanywhere- (...)`.

Теперь мы запустим помощника для автоматической настройки нашего приложения с GitHub. Type the following into the console on PythonAnywhere (don't forget to use your GitHub username in place of `<your-github-username>`, so that the URL matches the clone URL from GitHub):

{% filename %}command-line{% endfilename %}

    $ git clone https://github.com/<your-github-username>/my-first-blog.git
    

Когда ты увидишь, как это работает, то ты сможешь понять, что именно оно делает:

- Скачивает твой код с GitHub
- Создает virtualenv на PythonAnywhere, такое же, как на твоем компьютере
- Обновляет твой файл настроек с некоторыми параметрами развертывания
- Создаем базу данных на PythonAnywhere, используя команду `manage.py migrate`
- Настраивает твои статичные файлы(мы узнаем об этом позже)
- И настраивает PythonAnywhere для обслуживания твоего веб-приложения через API

On PythonAnywhere all those steps are automated, but they're the same steps you would have to go through with any other server provider.

The main thing to notice right now is that your database on PythonAnywhere is actually totally separate from your database on your own PC, so it can have different posts and admin accounts. As a result, just as we did on your own computer, we need to initialize the admin account with `createsuperuser`. PythonAnywhere has automatically activated your virtualenv for you, so all you need to do is run:

{% filename %}command-line{% endfilename %}

    (ola.pythonanywhere.com) $ python manage.py createsuperuser
    

Введи сведения для учетной записи администратора. Лучше всего использовать те же, что ты использовала на своем компьютере во избежание путаницы, если ты не хочешь сделать пароль на PythonAnywhere более безопасным.

Теперь, если ты хочешь, то ты можешь посмотреть на код на PythonAnywhere используя `ls`:

{% filename %}command-line{% endfilename %}

    (ola.pythonanywhere.com) $ ls
    blog  db.sqlite3  manage.py  mysite requirements.txt static
    (ola.pythonanywhere.com) $ ls blog/
    __init__.py  __pycache__  admin.py  forms.py  migrations  models.py  static
    templates  tests.py  urls.py  views.py
    

You can also go to the "Files" page and navigate around using PythonAnywhere's built-in file browser. (From the Console page, you can get to other PythonAnywhere pages from the menu button in the upper right corner. Once you're on one of the pages, there are links to the other ones near the top.)

## Ты в сети!

Your site should now be live on the public Internet! Click through to the PythonAnywhere "Web" page to get a link to it. You can share this with anyone you want :)

> **Примечание:** Это учебник для начинающих и при разворачивании сайта мы использовали несколько приёмов, которые не являются идеальными с точки зрения безопасности. If and when you decide to build on this project, or start a new project, you should review the [Django deployment checklist](https://docs.djangoproject.com/en/2.0/howto/deployment/checklist/) for some tips on securing your site.

## Советы по отладке

Если вы видите ошибку во время выполнения скрипта `pa_autoconfigure_django.py`, вот несколько распространенных причин:

- Не был создан PythonAnywhere API токен - ключ, для обеспечения доступа.
- Была допущена ошибка в URL GitHub репозитория
- Если ты видишь ошибку *"Could not find your settings.py"*, это вероятно потому, что не удалось добавить все твои файлы в Git и/или ты не успешно загрузила их на GitHub. Посмотри инструкции еще раз в разделе Git выше

Если видишь ошибку при попытке посетить свой сайт, для отладочной информации первым делом смотри **журнал ошибок**. You'll find a link to this on the PythonAnywhere ["Web" page](https://www.pythonanywhere.com/web_app_setup/). Посмотри, нет ли там сообщений о каких-нибудь ошибках; самые последние из них приведены ниже.

Также можешь посмотреть [общие советы по отладке на вики PythonAnywhere](http://help.pythonanywhere.com/pages/DebuggingImportError).

И помни: твой инструктор здесь, чтобы помогать!

# Проверить свой сайт!

Стандартная страница твоего сайта должна включать приветствие "Welcome to Django", точно также как было на локальном компьютере. Попробуй добавить `/admin/` к концу адреса сайта и перейдешь к панели администратора сайта. Log in with the username and password, and you'll see you can add new Posts on the server -- remember, the posts from your local test database were not sent to your live blog.

После того, как ты создала несколько постов, ты можешь вернуться к локальной настройке(не PythonAnywhere). Тут тебе следует работать для создания изменений. Это общий процесс веб-разработки - делать изменения локально, загружать эти изменения на GitHub и отправлять эти изменения на реальный веб-сервер. Это позволяет тебе работать и экспериментировать без нарушения работы твоего веб-сайта. Довольно круто, да?

Ты заслужила *огромную* похвалу! Развертывание сервера — одна из самых каверзных частей веб-разработки, и на это нередко уходит несколько дней, прежде чем заставишь всё работать. But you've got your site live, on the real Internet!