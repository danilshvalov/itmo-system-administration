# ZFS

## Что такое ZFS

Zettabyte File System или ZFS — это файловая система, основанная на **Copy-On-Write** и **дереве Меркла**, поддерживающая большие объемы данных и позволяющая создавать RAID-массивы.

ZFS предназначена для работы на одном сервере с большим количеством накопителей. ZFS объединяет и управляет всеми дисками как единым целым.

## Что такое Copy-On-Write

Copy-On-Write — это стратегия управления памятью, при которой копирование данных выполняется только в том случае, если один из объектов пытается изменить данные.

## Что такое дерево Меркла

Дерево Меркла — это полное двоичное дерево, в листовые вершины которого помещены хэши от блоков данных, а внутренние вершины содержат хэши от сложения значений в дочерних вершинах. Заполнение значений в узлах дерева идет снизу вверх. Если количество блоков на каком-то уровне дерева оказывается нечетным, то один блок дублируется.

## Принцип работы ZFS

Как было сказано ранее, ZFS использует стратегию Copy-On-Write и дерево Меркла. Все указатели на блоки содержат 256-битную контрольную сумму в целевом блоке, которая проверяется, когда блок прочитан. Блоки данных никогда не перезаписываются вместе. Вместо этого, выделяется новый блок, измененные данные записываются в него, а затем — метаданные любых блоков, которые на него ссылаются.

## Устройство ZFS

На самом низком уровне несколько физических дисков объединяются виртуальную группу – VDEV (Virtual Device). На этом уровне обеспечивается избыточность. Можно выбрать различные режимы работы дисков, например RAID-1, когда один диск копирует другой, или RAID-5 —дисковый массив с чередованием блоков данных и контролем четности.

Затем, все виртуальные группы дисков объединяются в общий пул. И уже поверх всей этой структуры находится сама файловая система с пользовательскими данными.

При этом структура файловой системы ZFS позволяет динамически добавлять новые группы дисков причем, каждая группа может иметь собственную конфигурацию.

## Преимущества ZFS

- **Проверка целостности данных**. Каждый раз, когда записываются новые данные, файловая система создает для них контрольную сумму. При чтении данных происходит сравнивание чек-суммы. При наличии несоответствий файловая система отмечает ошибку и автоматически пытается ее исправить.
- **Сжатие данных**. ZFS поддерживает сжатие хранимых данных с помощью различных алгоритмов. При этом скорость сжатия может быть очень высокой — в среднем составляет 800 Мбайт/с.
- **Моментальные снимки**. ZFS может создавать копии файловой системы в заданный момент времени буквально «на лету», поскольку система сохраняет все копии данных.
- **Дедупликация**. В ZFS встроена функция дедупликации данных, которая обеспечивает эффективность хранения за счет устранения избыточных данных.

## Особенности ZFS

- **Высокий порог вхождения**. Для работы с ZFS нужно знать и понимать внутреннее устройство, сильные и слабые стороны, а также понимать, как работать с командами и утилитами ZFS.
- **Фрагментация данных**. Из-за использования Copy-On-Write постоянно появляются новые блоки, а старые не всегда пригодны к удалению. Часто возникают ситуации, когда старый блок не полностью записан, из-за чего появляются «дырки».
- **Не слишком быстрая работа на HDD**. Из-за своей структуры ZFS требует быстрого случайного доступа, чем не могут похвастаться HDD. Поэтому рекомендуется использовать ZFS преимущественно на SSD.
