# Basics
***
1. **Контейнер** – это исполняемый экземпляр, который инкапсулирует требуемое программное обеспечение. Он состоит из образов. Его можно легко удалить и снова создать за короткий промежуток времени.
2. **Образ** – базовый элемент каждого контейнера. В зависимости от образа, может потребоваться некоторое время для его создания.
3. **Порт** – это порт TCP/UDP в своем первоначальном значении. Чтобы все было просто, предположим, что порты могут быть открыты во внешнем мире или подключены к контейнерам (доступны только из этих контейнеров и невидимы для внешнего мира).
4. **Том** – описывается как общая папка. Тома инициализируются при создании контейнера и предназначены для сохранения данных, независимо от жизненного цикла контейнера.
5. **Реестр** – это сервер, на котором хранятся образы. Сравним его с GitHub: вы можете вытащить образ из реестра, чтобы развернуть его локально, и так же локально можете вносить в реестр созданные образы.
6. **[DockerHub](https://hub.docker.com/explore/)** – публичный репозиторий с интерфейсом, предоставляемый Докер Inc. Он хранит множество образов. Ресурс является источником «официальных» образов, сделанных командой Докер или созданных в сотрудничестве с разработчиком ПО. Для официальных образов перечислены их потенциальные уязвимости. Эта информация открыта для любого зарегистрированного пользователя. Доступны как бесплатные, так и платные аккаунты.