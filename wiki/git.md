# Работа с Git

## Работа с ветками

Общая канва работы с Git основана на [GitHub Flow](https://guides.github.com/introduction/flow/). Плюсом к нему добавляется "зарезервированное" название ветки `hotfix`, которая не нужна в истории постоянно, а создается как Feature Branch, если для исправления бага не существует задачи в таск-трекере.

Также чтобы не превращать историю репозитория в кромешный ад, необходимо использовать [Rebase Flow](https://habr.com/ru/company/at_consulting/blog/283326/). Коротко, правила такие:

+ Главный репозиторий форкается каждым участником команды и указывается в `git remote` как `upstream`. Форкнутый репозиторий – как `origin`
+ Feature Branch всегда отпочковываются от самой свежей версии `master`. Создание подветок от Feature Branch возбраняется, в виду проблем с последующим мёрджем подветки в `master` и возможной потери изменений.
+ После обсуждения и принятия правок в Pull / Merge Request необходимо убедиться, что текущая Feature Branch является линейным продолжением `master`. Если это не так, то необходимо произвести `rebase` на `master`.
+ Непосредственно перед тем, как смерджить Request необходимо сделать `squash` всех промежуточных коммитов в один или несколько атомарных. Создание нескольких коммитов позволяется только тогда, когда объединение в один будет лишать правки необходимого контекста.
+ Feature Branch всегда удаляется из `upstream` после мёрджа, но почти всегда сохраняется в `origin` участника команды


## Именование веток и коммитов

Название Feature Branch состоит из префикса и номера задачи из таск-трекера, если задачи нет и это критическая правка, которая не может ждать, то используется ветка `hotfix`, во всех остальных случаях создается новая задача в таск-трекере, а её префикс и номер используется для ветки.

```bash
# плохо
GA213
fixes
feature-2020

# хорошо
GOLDAPPLE-1234
hotfix
```

Название коммита состоит из:
+ префикса и номера задачи из таск-трекера
+ двоеточия и пробела 
+ описания выполненной задачи на русском языке, отвечающим на вопрос `Что сделали?` 

```bash
# плохо
213 - fixes
GOLDAPPLE-10 Auth added
GOLDAPPLE-3322-Сделали окно авторизации

# хорошо
GOLDAPPLE-1234: Сделали окно авторизации
```

## Оформление Pull / Merge Request

Заголовок реквеста состоит из:

+ флага отложенности задачи `[HOLD]` и пробела
+ флага незаконченности задачи `[WIP]` и пробела
+ префикса и номера задачи из таск-трекера
+ двоеточия и пробела
+ короткого описания выполненной задачи на русском языке, отвечающим на вопрос `Что сделали?` 

```bash
# плохо
WIP Автризация. Смерджить после задачи GA-2021
WIP Сделали окно авторизации
GOLDAPPLE-10 Auth added

# хорошо
[HOLD] GOLDAPPLE-1234: Сделали окно авторизации
[WIP] GOLDAPPLE-1234: Сделали окно авторизации
GOLDAPPLE-1234: Сделали окно авторизации
```

Текст реквеста состоит из:

+ описание выполненных задач в свободной форме и все необходимые уточнения и детали под заголовком `Описание`
+ все вопросы и скользкие моменты связанные с задачей под заголовком `Проблемы`
+ ссылка на задачу в таск-трекер*
+ пинг двух участников команды, которые обязательно должны посмотреть реквест и дать фидбек*

```md
**Описание**

Сделали окно авторизации на моках.
Формат моков произвольный, таймаут 
добавлен  для создания иллюзии загрузки. 
Если установить GET-параметр debug=1, 
то авторизация всегда будет фейлиться

**Проблемы**

Стоит ли приводить формат данных моков к виду, 
который сейчас есть на старом сайте?

[GOLDAPPLE-###](ссылка на задачу)

@dev_1 @dev_2
```

*\* можно автоматизировать скриптами*

## Код ревью

Проходит по обычной схеме: читаем код – оставляем правки. 

Обсуждение можно начать в комментариях, а можно вынести на ежедневную синхронизацию. Если комментарий был принят к сведению, то создатель реквеста должен отметить его соответствующим эмоджи и, если требуется, снабдить объяснением в комментарии ниже:
* палец вверх (:+1:) – правка принята 
* палец вниз (:-1:) – правка не принята

После того, как правки были выполнены, создатель снова пингует людей в комментарии первого уровня*.

Если ревьюера всё устраивает, то он пишет в комментариях первого уровня эмоджи OK (:ok:).

Если реквест получил два OK, то его можно мёрджить (но прежде выполнить все [необходимые действия](#работа-с-ветками)).

*\* можно автоматизировать скриптами*