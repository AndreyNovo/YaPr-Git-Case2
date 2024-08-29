# Шпаргалка по Git

## Основные понятия
- `Ветка` - параллельная версия репозитория
- `Коммит` - фиксация изменений в локальный репозиторий
- `Рабочее дерево` - иерархия папок и файлов репозитория, относящихся к проекту
- `Репозиторий` - папка с файлами проекта, изменения в которой записываются и отслеживаются Git

## Основные команды
- `git init` - создать репозиторий
```bash
cd ~/dev/first-project # перешли в нужную папку
git init # создали репозиторий
```
- `rm -rf .git` - «разгитить» папку
```bash
cd ~/dev/first-project # перешли в нужную папку
rm -rf .git # удалили скрытую подпапку .git
```
- `git status` - проверить состояние репозитория
    + `staged` - "Changes to be committed: ..." в выводе;
    + `modified` - "Changes not staged for commit: ..." в выводе;
    + `untracked` - "Untracked files: ..." в выводе.

См. также `Статусы файлов в репозитории`
- `git add` - добавить изменённые файлы к коммиту
```bash
git add readme.txt # подготовить файл readme.txt к сохранению в репозитории
git add . # подготовить все файлы текущей папки к сохранению в репозитории
git add --all # подготовить все файлы рабочего дерева к сохранению в репозитории
```
- `git commit` - сохранить подготовленные изменения в репозитории
```bash
git commit -m "Комментарий к коммиту"
```
- `git commit --amend` - добавить изменения к последнему коммиту
```bash
git commit --amend --no-edit # не менять сообщение
git commit --amend -m "Комментарий к коммиту" # заменить сообщение к коммиту
```
- `git diff` - сравнить 2 коммита
```bash
git diff a9928ab 11bada1 # вывести разницу между двумя коммитами
git diff --staged # вывести изменения, которые добавлены в staged-файлах
```
- `git restore` - откатить последние изменения
```bash
git restore hello.txt # вернуть изменённый файл к последней версии, которая была сохранена через git commit или git add
git restore --staged hello.txt # перевести файл из состояния staged обратно в untracked или modified
```
- `git reset --hard` - откатить к указанному коммиту, с потерей изменений в последующих
```bash
git reset --hard b576d89 # удалить все незакоммиченные изменения из staging и «рабочей зоны» вплоть до коммита с указанным хэшем
```
- `git log ` - посмотреть историю коммитов. Запись лога содержит в себе:
    + строку из цифр и латинских букв после слова commit — это хеш коммита;
    + Author — имя автора и его электронная почта;
    + Date — дату и время создания коммита;
    + сообщение коммита (комментарий к коммиту).

Нажать `Q` для выхода.
- `git log --oneline` - посмотреть сокращённую историю коммитов  
Каждый коммит - одной строкой.  
Хэш не полный, а только первые несколько символов. Сокращённые хэши уникальны.  
Умещается максимум 72 первых символа сообщения.
- `git config --global user.name "[имя]"` - установить имя автора для выполняемых коммитов
- `git config --global user.email "email address"` - установить адрес электронной почты автора для выполняемых коммитов

## Коммиты
- снимки состояния, снэпшоты
- идентификатор коммита - хэш SHA-1, уникален в пределах репозитория
- HEAD - ссылка (указатель) на последний (самый новый) коммит; служебный файл в подпапке .git

### Сообщения (комментарии) к коммитам  
Сообщение должно быть:
- относительно коротким (см. `git log --oneline`);
- информативным: что сделано, где, уточнение контекста;
- единообразными (в одном стиле);
- содержать номер задачи (например, в Jira), которую решает исправление в коммите;
- начинаться с инфинитива (рус.) / императива (англ.) / глагола прошедшего времени / сущ. в именит. падеже.  
Примеры: Исправить / Fix / Исправлено / Исправление.

Стили оформления сообщений:
- Стандарт Conventional Commits  
Для репозиториев с исходным кодом программ.  
Отличается качественной документацией и подробной проработкой.  
[Веб сайт Conventional Commits](https://www.conventionalcommits.org/ru/)  
- GitHub-стиль  
Если коммит решает какую-то задачу из списка задач в GitHub, указывать ссылку на неё: #<номер задачи> в любом месте сообщения. GitHub свяжет коммит и задачу.

## Статусы файлов в репозитории
- `untracked` - ещё не добавленный через `git add`
- `tracked` - добавленный через `git add`, на любом из последующих этапов
- `staged` (aka `indexed`, `cached`) - добавленный к коммиту через `git add`, но ещё не сохранённый через `git commit` (готов к коммиту)
- `modified` - изменённый после последнего выполнения `git add` или `git commit`, но ещё не в переведённый статус `staged` через `git add`

### Жизненный цикл файла в репозитории
![Типичный жизненный цикл файла в репозитории Git](https://pictures.s3.yandex.net/resources/M2_T5_1686651284.png)

```mermaid
graph LR;
  A(untracked) -- "git add" --> B(staged + tracked);
  B -- "изменения" --> C(modified + tracked);
  C --"git add" --> B;
  B -- "git commit" --> D("tracked (+ comitted)");
  D -- "изменения" --> C;
```

## Работа с удалённым репозиторием на GitHub
### Первичное связывание локального и удалённого репозиториев
1. Связать репозитории
```bash
cd ~/dev/first-project
git remote add origin git@github.com:%ИМЯ_АККАУНТА%/first-project.git
```
`origin` (англ. «источник») — стандартный псевдоним, с помощью которого можно обращаться к главному удалённому репозиторию.

`git remote -v` - проверить, что репозитории успешно связались.

2. Переименовать основную ветку в локальном репозитории
```bash
git branch -M main
```

3. Связать ветки
```bash
git push -u origin main # Если команда приведёт к ошибке, попробуйте заменить main на master
```

### Синхронизация изменений между локальным на удалённым репозиториями
- `git push` - синхронизировать изменения (выгрузить коммиты) из локального репозитория на удалённый
```bash
git add .
git commit -m 'COMMENT'
git push # краткая форма, поразумевается текущий удалённый репозиторий и текущая ветка
git push origin main # полная комманда, с указанием удалённого репозитория и ветки
```
- `git pull` - синхронизировать изменения (загрузить коммиты) из удалённого репозитория на локальный

См. также:
- [Шпаргалка на Яндекс.Практикум. Начало работы с Git](https://practicum.yandex.ru/trainer/git-basics/lesson/b1ecee27-bb78-46a0-8d13-0364c7803f55/)
