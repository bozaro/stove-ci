## Марат Радченко [2020-03-04]

1. Про докер: виртуалки лучше, если инфраструктура позволяет. У докера меньше гибкости, хз насколько продакшен-ready с виндой внутри и вообще кажется никак с макосью.
2. Проект должен мочь объявлять джобы (причём более одного) тупо коммитя новые файлы в репу. в женькинсе это сделать никак, например.
3. Подсмотрено в github-actions: возможность писать и подключать реюзабельные step'ы внутрь пайплайна силами самого проекта - это очень крутая фича. причём step является репозиторием -> реюзается между проектами, с версионированием и шлюпками. у женькинса файл пайплайна даже из того же самого репозитория импорт файла сделать не может.
4. Про слак: у слака есть апи "найди юзера по емейлу". емейл есть в том-кто-пушнул-коммит.
5. Про выразительность пайплайна: возможность задать направленный граф зависимостей, выполняемых потенциально на разных типах агентов - мастхев. дженкинс в граф не умеет.
6. Возможно пекарь тоже должен сидеть в сандбоксе/докере. зависит от того насколько сильную кастомщину можно писать в рецепте. в gh-actions можно писать _очень_ кастомщину. это еще один провал женькинса. с одной стороны, их сандбокс ублюдочно запрещает совершенно невинные вещи. с другой, у них постоянно находят секьюрити-уязвимости. ну и груви - так себе язычок  прямо скажем.

## Сергей Загурский [2020-03-05]

1. Нужно проработать модель билдов/билд-конфигураций. Особенно важно, чтобы она хорошо ложилась на многобранчевые репки. И в TeamCity, и в Jenkins мультибранчовые репки поддержаны на троечку с минусом. Они проектировались во времена Subversion, когда новый бранч был событием.
2. Нужно проработать модель "билд-артефактов", чтобы можно было и кастомные репорты прикрепить, и метрики. Должна быть возможность публиковать логи, метрики и результаты тестов. Причём логов у нас куча разных. И тестовых результатов тоже. А ещё, некоторые штуки хочется в реальном времени публиковать, а не тогда, когда билд закончился. Возможно, это тоже какая-то разновидность артефактов.
3. В TeamCity была хорошая фича, что билд можно было покрасить красным в любой момент его исполнения. На это можно было прикручивать ранние оповещения и раньше приступать к починке.
4. В Jenkins есть фича, которой можно сохранить билд от автоматического удаления. Полезно, когда ссылка на билд подшивается к какой-нибудь таске.
5. Возможно, билды со своими артефактами должны храниться в каком-то артефакт-сторадже. Туда можно делегировать политики чистки и всё такое. В БД хранить только самую базовую метаинфу.
6. Нужна extensibility model. Я бы смотрел в сторону современных подходов, где "плагины" разговаривают с мастером через gRPC. Чтобы поднять такой "плагин" мастеру достаточно знать его docker image. Такие "плагины" понадобится использовать в куче мест. Например, для формирования кастомного билдрепорта. Например, для кастомного гуя запуска билда.
7. Из совсем очевидного, нужно поднимать Печку в кубенях. Иначе придётся городить колхоз с абстракциями поверх поднятия агентов и прочего.
8. Нужно API. Нормальное.
9. Очень хочется, чтобы не тормозил гуй (горизонтальное масштабирование? реплики только на чтение?)
10. Executor'ы могут отгнивать. Билды от этого не должны краснеть.
