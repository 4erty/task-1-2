  ошибка при запуске сервера -> неправильный import в index.js
  
  в файле map.js исправляем var на const, т.к. все остальные переменые заданы в стиле es6
  
  пустой экран, смотрим консоль, видим, что высота карты - 0, смотрим документацию, видим, что карта растягивается по высоте родителя DOM, ставим высоту и ширину в css для div с картой
	
  Не показываются метки на карте, читаем документацию, внимательно читаем документацию и код. Видим что objectManager не добавляется к карте. Добавляем.

  Все равно не показываются, смотрим код, ищем где они рисуются, ставим breakpoint в консоли в начале формирования и загрузки точек -> все вроде работает нормально (но замечаем, что кол-во файлов меньше, чем в IDE., записываем, лишний файл popup.js) -> лезем в документацию и одновременно выводим в консоль objectManager. Смотрим внутри него objects. Координаты внутри как-будто перепутаны. Меняем их местами. -> Метки выводятся.
  
  Метки выводятся, кластеризуются, но не рисуются в виде диаграммы. Смотрим код, видим, что при создании objectManager задается clusterIconLayout, а затем objectManager.clusters.options.set('preset', 'islands#greenClusterIcons'). Удаляем втроую строку -> кластеры начинают отрисовываться правильно.

  Проверяем фильтр. Фильтр работает.

  Кликаем на метку, ничего не происходит. После чего при любом событии мыши в консоли сыпятся ошибки api карт. Ищем в коде, где формируется шаблон балуна. Ставим breakpoint. Видим, что this = undefined. В шаблоне методы заданы стрелочными функциями, которые берут this из лексического окружения, меняем на function. Балун рисуется корректно, но график пустой.

  Ищем где строится шаблон графика, читаем код и документацию Chartjs. Обнаруживаем, что ось Y рисуется с максимальным значением 0. Исправляем на max на min. График появился.
	
  Файл popup.js нигде не подключается и его экспортируемая функция нигде не используется. Для текущего функционала он не требуется.

  смотрим оформление кода. Прописываем правила для eslint. Запускаем eslint --fix.