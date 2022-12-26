# NeuralNetworksTimeSeries

## Применение нейронных сетей для прогнозирования временных рядов с использованием GRU

## Описание:

Нейронная сеть — это громадный распределенный параллельный процессор, состоящий из элементарных единиц обработки 
информации, накапливающих экспериментальные знания и предоставляющих их для последующей обработки.
Нейронные сети обычно используют для классификации и регрессии. Также они используются для прогнозирования временных
рядов. 
Временной ряд представляет собой последовательность наблюдений за изменениями во времени значений параметров некоторого 
объекта или процесса. Строго говоря, каждый процесс непрерывен во времени, то есть некоторые значения параметров этого 
процесса существуют в любой момент времени. Но для задач анализа не нужно знать значения параметров объектов в любой 
момент времени. Интерес представляют временные отсчеты – значения, зафиксированные в некоторые, обычно равноотстоящие
 моменты времени. Отсчеты могут браться через различные промежутки: через минуту, час, день, неделю, месяц или год – 
в зависимости от того, насколько детально должен быть проанализирован процесс. В задачах анализа временных рядов 
мы имеем дело с дискретным временем, когда каждое наблюдение за параметром образуют временной отсчет.
Глубокие нейронные сети в настоящее время становятся одним из самых популярных методов машинного обучения. 
Они показывают лучшие результаты по сравнению с альтернативными методами в таких областях, как распознавание речи, 
обработка естественного языка, компьютерное зрение, медицинская информатика и др.
В рекуррентных же сетях связи между нейронами могут идти не только от нижнего слоя к верхнему, но и от нейрона к «самому себе», 
точнее, к предыдущему значению самого этого нейрона или других нейронов того же слоя. 
Именно это позволяет отразить зависимость переменной от своих значений в разные моменты времени: нейрон обучается 
использовать не только текущий вход и то, что с ним сделали нейроны предыдущих уровней, но и то, что происходило с ним 
самим и, возможно, другими нейронами на предыдущих входах.
GRU используются фильтры обновления и сброса, которые определяют, какую информацию следует передавать на выход. 
Особенность такой архитектуры заключается в том, что можно научить сеть хранить только ту информацию, 
причем достаточно долго, которая имеет отношения к прогнозу.
Так как на выходе мы должны получить 1 результат, то данная задана сводится к задаче регрессии.
Для задач регрессии чаще всего применяются: 
MSE - среднее квадратичное отклонение (mean square error – MSE);
MAE - среднее абсолютное отклонение (mean absolute error – MAE).
Нейронной сети, чтобы обучаться, также нужна какая-то мера. Она должна
уметь сравнить свой ответ с верным ответом из учебного ответа, оценить,
насколько ее ответ близок к истине или далек от нее, а затем с помощью
специального алгоритма обратного распространения ошибки обновить все свои веса. 
Эта функция должна быть дифференцируемой, чтобы в дальнейшем нейронная сеть смогла посчитать производные по этой функции 
и определить градиент данной функции. Так как данная функция определяет, насколько нейронная сеть ошиблась, то ее принято 
называть функцией потерь (loss function). Для данной задачи  метрик функциями потерь могут послужить MAE, MSE.
За сам процесс распространения посчитанной ошибки обратно на все веса модели нейронной сети отвечают так называемые
 оптимизаторы. Это функции, которые распространяют ошибку, используя различные стратегии: 
- SGD – стохастический градиентный спуск; 
- RMSprop – модификация градиентного спуска; 
- Adam – модификация градиентного спуска.
Также процесс обучения нейронной сети способен настраивать лишь веса модели и смещения нейронов. 
Смещения и веса нейронов – это параметры нейронной сети. Но существует ряд параметров, которые нельзя эффективно
 настроить в процессе обучения нейронной сети. Они задаются еще до начала обучения и в процессе обучения не изменяются. 
Такие параметры называются гиперпараметрами нейронной сети. К ним относятся: количество слоев нейронной сети; количество 
нейронов в слое нейронной сети;  количество эпох обучения;  скорость обучения;  размер обучающей выборки; функции активации; 
 функция ошибок.

В данной программе мы реализуем прогнозирование временного ряда с использованием нейронной сети.


## Применение:

Вначале при выборе данных необходимо иметь ввиду, чтобы они относились к решению задач временного ряда,  были помечены, 
не было текстовых данных или пропущенных, так как данная программа не обрабатывает вышеописанные ситуации. 
Также данные должны быть формата '.csv' и разделены на тренировочные и тестовые.
В качестве исследуемых данных используется "«Отрасль экономики США -> Банки »" за период 2012.07.11 - 2016.07.11.  

Производим считывание данных из файла, делим данные на тренировочные и тестовые.
После преобразовываем поле 'data' во временную метку.

В функции create_plot(history) строим график временного ряда для последующего тестирования на предмет того, как модель научилась
прогнозировать данные.
В функции build_model(numberNeurons1,numberNeurons2, optimizer, activation,size) производим создание нейронной сети,
добавляем к модели полносвязные слои с нейронами, также создаем слой GRU и производим компиляцию нашей модели. 
Перед этим вначале строим анонимный слой для изменения формата данных, то есть далем 3-х мерный массив, чтобы его принял слой GRU.
После мы обучаем мjдель с помощью метода fit, в котором в качестве параметров указываем:
тренировочные данные;
epochs - количество эпох, на протяжении которых будет учиться нейронная сеть. Одна эпоха – это полный проход
 всех учебных данных через нейронную сеть. То есть к концу первой эпохи нейронная сеть уже увидит все 
обучающие данные и в последующих эпохах будет видеть их же, но в другом сочетании внутри одного batch.
batch_size - количество объектов из тренировочных данных, которые делают проход по нейронной сети за один раз и 
после которых выполняется обновление весов нейронной сети.
validation_slpit - берем часть данных из обучающей выборки для тестирования модели по эпохам.
verbose -  отвечает за выведение информации о процессе обучения.
После производим оценку качества модели на тренировочных и тестовых данных с помощью evaluate, делаем предсказание 
модели на тестовых данных и строим график временного ряда, на котором сравниваем изначальные реальные данные и спрогнозированные.

## Настройка проекта:

Проект написан на python.
Для его запуска необходимо установить вначале Anaconda – дистрибутив языков программирования Python и R, включающий набор популярных свободных библиотек, объединённых проблематиками науки о данных и машинного обучения.
После необходимо использовать терминал для запуска jupyter notebook. Его лучше запускать из папки, в которой будет находиться ваш проект. Перейти в соответствующую папку можно с помощью команды cd. После чего пишем в командной строке jupyter notebook.
Если все шаги выполнены верно, то в браузере откроется jupyter notebook, вы увидите список файлов из вашей рабочей папки (если она не
пуста) и сможете создать новый notebook.
Также можно запускать его в Google Colab. Так как при работе с нейронными сетями необходимо иметь высокопроизводительные
GRU.
Google Colab — это сервис который позволяет запускать Jupyter Notebook-ки, причём если нажать на вкладку 
Runtime -> Change runtime type -> GPU, то вы получите доступ к одному GPU (NVidia Tesla K80) бесплатно.
Использовать видеокарту можно 12 часов в сутки, поэтому для более сложных задач, стоит рассмотреть другие варианты.
