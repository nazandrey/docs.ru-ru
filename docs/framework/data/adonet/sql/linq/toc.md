# [LINQ to SQL](index.md)
## [Начало работы](getting-started.md)
### [Что можно сделать с помощью LINQ to SQL](what-you-can-do-with-linq-to-sql.md)
### [Типичные действия по использованию LINQ to SQL](typical-steps-for-using-linq-to-sql.md)
### [Загрузка примеров баз данных](downloading-sample-databases.md)
### [Обучение с помощью пошаговых руководств](learning-by-walkthroughs.md)
#### [Пошаговое руководство: Простая модель объектов и запрос (Visual Basic)](walkthrough-simple-object-model-and-query-visual-basic.md)
#### [Пошаговое руководство: Запросы по связям (Visual Basic)](walkthrough-querying-across-relationships-visual-basic.md)
#### [Пошаговое руководство: Обработка данных (Visual Basic)](walkthrough-manipulating-data-visual-basic.md)
#### [Пошаговое руководство: Применение только хранимых процедур (Visual Basic)](walkthrough-using-only-stored-procedures-visual-basic.md)
#### [Пошаговое руководство: Простая модель объектов и запросов (C#)](walkthrough-simple-object-model-and-query-csharp.md)
#### [Пошаговое руководство: Запросы по связям (C#)](walkthrough-querying-across-relationships-csharp.md)
#### [Пошаговое руководство: Обработка данных (C#)](walkthrough-manipulating-data-csharp.md)
#### [Пошаговое руководство: Применение только хранимых процедур (C#)](walkthrough-using-only-stored-procedures-csharp.md)
## [Руководство по программированию](programming-guide.md)
### [Создание модели объектов](creating-the-object-model.md)
#### [Как: Создание модели объектов в Visual Basic или C#](how-to-generate-the-object-model-in-visual-basic-or-csharp.md)
#### [Как: создать модель объектов в виде внешнего файла](how-to-generate-the-object-model-as-an-external-file.md)
#### [Как: Создание настраиваемого кода за счет изменения файла DBML](how-to-generate-customized-code-by-modifying-a-dbml-file.md)
#### [Как: проверка внешние файлы сопоставлений и DBML-](how-to-validate-dbml-and-external-mapping-files.md)
#### [Как: обеспечения возможности сериализации сущностей](how-to-make-entities-serializable.md)
#### [Как: Настройка классов сущностей с помощью редактора кода](how-to-customize-entity-classes-by-using-the-code-editor.md)
##### [Как: Указание имен баз данных](how-to-specify-database-names.md)
##### [Как: представление таблиц как классов](how-to-represent-tables-as-classes.md)
##### [Как: представление столбцов как членов класса](how-to-represent-columns-as-class-members.md)
##### [Как: представление первичных ключей](how-to-represent-primary-keys.md)
##### [Как: сопоставление связей базы данных](how-to-map-database-relationships.md)
##### [Как: представление столбцов как созданные базой данных](how-to-represent-columns-as-database-generated.md)
##### [Как: представление столбцов как меток времени или столбцов версии](how-to-represent-columns-as-timestamp-or-version-columns.md)
##### [Как: укажите типы данных базы данных](how-to-specify-database-data-types.md)
##### [Как: представление вычисляемых столбцов](how-to-represent-computed-columns.md)
##### [Как: задание частных полей хранения](how-to-specify-private-storage-fields.md)
##### [Как: представлять столбцы, допускающие значения Null](how-to-represent-columns-as-allowing-null-values.md)
##### [Как: сопоставление иерархий наследования](how-to-map-inheritance-hierarchies.md)
##### [Как: задания проверки конфликт параллелизма](how-to-specify-concurrency-conflict-checking.md)
### [Взаимодействие с базой данных](communicating-with-the-database.md)
#### [Как: подключение к базе данных](how-to-connect-to-a-database.md)
#### [Как: непосредственное выполнение команд SQL](how-to-directly-execute-sql-commands.md)
#### [Как: повторного использования подключения между командой ADO.NET и контекстом DataContext](how-to-reuse-a-connection-between-an-ado-net-command-and-a-datacontext.md)
### [Запросы к базе данных](querying-the-database.md)
#### [Как: запрашивать информацию](how-to-query-for-information.md)
#### [Как: получение сведений как доступных только для чтения](how-to-retrieve-information-as-read-only.md)
#### [Как: управления извлекаемых объема связанных данных](how-to-control-how-much-related-data-is-retrieved.md)
#### [Как: фильтрации взаимосвязанных данных](how-to-filter-related-data.md)
#### [Как: отключить отложенную загрузку](how-to-turn-off-deferred-loading.md)
#### [Как: непосредственное выполнение запросов SQL](how-to-directly-execute-sql-queries.md)
#### [Как: хранение и повторное использование запросов](how-to-store-and-reuse-queries.md)
#### [Как: обработка составных ключей в запросах](how-to-handle-composite-keys-in-queries.md)
#### [Как: получить несколько объектов одновременно](how-to-retrieve-many-objects-at-once.md)
#### [Как: фильтра на уровне DataContext](how-to-filter-at-the-datacontext-level.md)
#### [Примеры запросов](query-examples.md)
##### [Статистические запросы](aggregate-queries.md)
###### [Возврат среднего значения из числовой последовательности](return-the-average-value-from-a-numeric-sequence.md)
###### [Число элементов последовательности](count-the-number-of-elements-in-a-sequence.md)
###### [Нахождение максимального значения в числовой последовательности](find-the-maximum-value-in-a-numeric-sequence.md)
###### [Найти минимальное значение в числовой последовательности](find-the-minimum-value-in-a-numeric-sequence.md)
###### [Вычисление суммы значений в числовой последовательности](compute-the-sum-of-values-in-a-numeric-sequence.md)
##### [Возвращает первый элемент в последовательности](return-the-first-element-in-a-sequence.md)
##### [Возврат или пропуск элементов последовательности](return-or-skip-elements-in-a-sequence.md)
##### [Сортировка элементов последовательности](sort-elements-in-a-sequence.md)
##### [Группировка элементов в последовательности](group-elements-in-a-sequence.md)
##### [Удаление дубликатов элементов из последовательности](eliminate-duplicate-elements-from-a-sequence.md)
##### [Определить, если некоторые или все элементы в последовательности удовлетворяют условию](determine-if-any-or-all-elements-in-a-sequence-satisfy-a-condition.md)
##### [Объединение двух последовательностей](concatenate-two-sequences.md)
##### [Возврат разности наборов между двумя последовательностями](return-the-set-difference-between-two-sequences.md)
##### [Возврат пересечения наборов двух последовательностей.](return-the-set-intersection-of-two-sequences.md)
##### [Возврат объединения наборов двух последовательностей.](return-the-set-union-of-two-sequences.md)
##### [Преобразование последовательности в массив](convert-a-sequence-to-an-array.md)
##### [Преобразование последовательности в универсальный список](convert-a-sequence-to-a-generic-list.md)
##### [Преобразование типа в универсальный интерфейс IEnumerable](convert-a-type-to-a-generic-ienumerable.md)
##### [Сформулировать соединения и запросы перекрестного произведения](formulate-joins-and-cross-product-queries.md)
##### [Формулировка проекций](formulate-projections.md)
### [Внесение и отправка изменений данных](making-and-submitting-data-changes.md)
#### [Как: вставка строк в базе данных](how-to-insert-rows-into-the-database.md)
#### [Как: обновление строк в базе данных](how-to-update-rows-in-the-database.md)
#### [Как: удаление строк из базы данных](how-to-delete-rows-from-the-database.md)
#### [Как: Отправка изменений в базу данных](how-to-submit-changes-to-the-database.md)
#### [Как: группировка отправок данных с помощью транзакций](how-to-bracket-data-submissions-by-using-transactions.md)
#### [Как: динамическое создание базы данных](how-to-dynamically-create-a-database.md)
#### [Как: управление конфликтами изменений](how-to-manage-change-conflicts.md)
##### [Как: обнаруживать и разрешать конфликты отправки](how-to-detect-and-resolve-conflicting-submissions.md)
##### [Как: Укажите, при которых создаются исключения параллельности](how-to-specify-when-concurrency-exceptions-are-thrown.md)
##### [Как: Укажите, какие элементы проверяются на наличие конфликтов параллелизма](how-to-specify-which-members-are-tested-for-concurrency-conflicts.md)
##### [Как: получение сведений о конфликте сущностей](how-to-retrieve-entity-conflict-information.md)
##### [Как: получение сведений о конфликте членов](how-to-retrieve-member-conflict-information.md)
##### [Как: разрешение конфликтов за счет сохранения значений базы данных](how-to-resolve-conflicts-by-retaining-database-values.md)
##### [Как: разрешение конфликтов за счет перезаписи значений базы данных](how-to-resolve-conflicts-by-overwriting-database-values.md)
##### [Как: разрешать конфликты путем объединения значений базы данных](how-to-resolve-conflicts-by-merging-with-database-values.md)
### [Поддержка отладки](debugging-support.md)
#### [Как: отображение сформированного кода SQL](how-to-display-generated-sql.md)
#### [Как: отображение набора изменений](how-to-display-a-changeset.md)
#### [Как: отображение LINQ to SQL-команды](how-to-display-linq-to-sql-commands.md)
#### [Устранение неполадок](troubleshooting.md)
### [Общие сведения](background-information.md)
#### [ADO.NET и LINQ to SQL](ado-net-and-linq-to-sql.md)
#### [Анализ кода LINQ to SQL источника](analyzing-linq-to-sql-source-code.md)
#### [Настройка вставки, обновления и удаления](customizing-insert-update-and-delete-operations.md)
##### [Настройка операций: Общие сведения](customizing-operations-overview.md)
##### [Операций вставки, обновления и удаления](insert-update-and-delete-operations.md)
##### [Обязанности разработчика при переопределении поведения по умолчанию](responsibilities-of-the-developer-in-overriding-default-behavior.md)
##### [Добавление бизнес-логики с помощью разделяемых методов](adding-business-logic-by-using-partial-methods.md)
#### [Привязка данных](data-binding.md)
#### [Поддержка наследования](inheritance-support.md)
#### [Локальные вызовы методов](local-method-calls.md)
#### [N-уровневые и удаленные приложения и LINQ to SQL](n-tier-and-remote-applications-with-linq-to-sql.md)
##### [LINQ to SQL N-уровневые с ASP.NET](linq-to-sql-n-tier-with-aspnet.md)
##### [LINQ to SQL N-уровневые веб-службы](linq-to-sql-n-tier-with-web-services.md)
##### [LINQ to SQL и тесно связанные клиентские / серверные приложения](linq-to-sql-with-tightly-coupled-client-server-applications.md)
##### [Реализация N-уровневой бизнес-логики](implementing-business-logic-linq-to-sql.md)
##### [Извлечение данных и операции CUD в N-уровневых приложениях (LINQ to SQL)](data-retrieval-and-cud-operations-in-n-tier-applications.md)
#### [Идентификация объектов](object-identity.md)
#### [LINQ to SQL модель объектов](the-linq-to-sql-object-model.md)
#### [Состояния объектов и отслеживание изменений](object-states-and-change-tracking.md)
#### [Оптимистический параллелизм: Обзор](optimistic-concurrency-overview.md)
#### [Основные принципы запросов](query-concepts.md)
##### [Запросы LINQ to SQL](linq-to-sql-queries.md)
##### [Запросы по связям](querying-across-relationships.md)
##### [Сравнение удаленного и Локальное выполнение](remote-vs-local-execution.md)
##### [Отложенная и немедленная загрузка](deferred-versus-immediate-loading.md)
#### [Получение объектов из кэша идентификаторов](retrieving-objects-from-the-identity-cache.md)
#### [Безопасность в LINQ to SQL](security-in-linq-to-sql.md)
#### [Сериализация](serialization.md)
#### [Хранимые процедуры](stored-procedures.md)
##### [Как: возврат наборов строк](how-to-return-rowsets.md)
##### [Как: использовать хранимые процедуры, которые принимают параметры](how-to-use-stored-procedures-that-take-parameters.md)
##### [Как: использование хранимых процедур, сопоставленных для нескольких форм результатов](how-to-use-stored-procedures-mapped-for-multiple-result-shapes.md)
##### [Как: использование хранимых процедур, сопоставленных для последовательных форм результатов](how-to-use-stored-procedures-mapped-for-sequential-result-shapes.md)
##### [Настройка операций с помощью хранимых процедур](customizing-operations-by-using-stored-procedures.md)
##### [Настройка операций с использованием только хранимых процедур](customizing-operations-by-using-stored-procedures-exclusively.md)
#### [Поддержка транзакций](transaction-support.md)
#### [Несоответствия типов SQL-CLR](sql-clr-type-mismatches.md)
#### [Сопоставления пользовательских типов SQL-CLR](sql-clr-custom-type-mappings.md)
#### [Определяемые пользователем функции](user-defined-functions.md)
##### [Как: используйте скалярные определяемые пользователем функции](how-to-use-scalar-valued-user-defined-functions.md)
##### [Как: использование возвращающих табличные значения определяемых пользователем функций](how-to-use-table-valued-user-defined-functions.md)
##### [Как: встроенный вызов пользовательских функций](how-to-call-user-defined-functions-inline.md)
## [Ссылки](reference.md)
### [Типы данных и функции](data-types-and-functions.md)
#### [Сопоставление типов SQL-CLR](sql-clr-type-mapping.md)
#### [Базовые типы данных](basic-data-types.md)
#### [Логические типы данных](boolean-data-types.md)
#### [Null-семантика](null-semantics.md)
#### [Числовые операторы и операторы сравнения](numeric-and-comparison-operators.md)
#### [Операторы последовательности](sequence-operators.md)
#### [Методы System.Convert](system-convert-methods.md)
#### [Методы System.DateTime](system-datetime-methods.md)
#### [Методы System.Math](system-math-methods.md)
#### [Методы System.Object](system-object-methods.md)
#### [Методы System.String](system-string-methods.md)
#### [Методы System.TimeSpan](system-timespan-methods.md)
#### [Неподдерживаемые функции](unsupported-functionality.md)
#### [Методы System.DateTimeOffset](system-datetimeoffset-methods.md)
### [Сопоставление на основе атрибутов](attribute-based-mapping.md)
### [Создание кода в LINQ to SQL](code-generation-in-linq-to-sql.md)
### [Внешнее сопоставление](external-mapping.md)
### [Часто задаваемые вопросы](frequently-asked-questions.md)
### [SQL Server Compact и LINQ to SQL](sql-server-compact-and-linq-to-sql.md)
### [Трансляция стандартных операторов запросов](standard-query-operator-translation.md)
## [Примеры](samples.md)
