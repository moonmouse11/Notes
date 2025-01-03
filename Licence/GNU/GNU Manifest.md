# GNU Manifest
***
## General
> Copyright © 1985, 1993 Free Software Foundation, Inc.  
> перевод © 1993 Е.А. Семендяева.  
> перевод © 1999 О.С. Тихонов.

Каждому предоставляется право делать или распространять дословные копии этого документа на любом носителе при условии сохранения уведомления об авторских правах и уведомления о разрешениях и при условии предоставления распространителем разрешения получателю на повторное распространение, как в этом уведомлении.
Изменять этот документ нельзя.
***
## Манифест GNU
> Манифест GNU, приведенный ниже, был написан Ричардом Столменом в начале проекта GNU, чтобы призвать к сотрудничеству и поддержке. За первые несколько лет он был немного изменен, чтобы учесть новые разработки, но сейчас кажется лучшим оставить его неизменным, поскольку многие люди его уже видели.
> С тех пор выяснились некоторые частые случаи недопонимания, которые можно избежать, употребляя другие слова. Добавленные в 1993 году сноски помогают прояснить эти места.
> Для получения современной информации о доступных программах GNU, пожалуйста, смотрите последний выпуск Бюллетеня GNU. Этот перечень слишком длинный, чтобы приводить его здесь.
***
## Что такое GNU? GNU это не UNIX!
**GNU**, что означает `Gnu's Not Unix` - это имя для полностью Unix-совместимой программной системы, которую я пишу так, что я свободно могу передать его всем, кто сможет ею пользоваться. Несколько добровольцев помогают мне. Крайне требуется вклад времени, денег, программ и оборудования.

На данный момент у нас уже есть текстовый редактор `Emacs` с языком `Lisp` для написания команд редактора, отладчик, работающий на уровне исходного текста, YACC-совместимый генератор анализаторов, компоновщик и около 35 утилит. Оболочка (командный интерпретатор) почти завершена. Новый переносимый оптимизирующий компилятор `С` скомпилировал сам себя и может быть выпущен в этом году. Начальное ядро существует, но нужны еще многие свойства, чтобы можно было эмулировать Unix. Когда ядро и компилятор будут закончены, можно будет распространять систему GNU, пригодную для разработки программ. Мы будем использовать TeX в качестве нашего программы форматирования текста, но над nroff продолжается работа. Мы будем использовать также свободную, переносимую систему `X Window`. После этого мы добавим переносимый `Common Lisp`, игру "Империя", электронную таблицу и сотни других вещей плюс диалоговую документацию. Мы надеемся в конце концов предоставить все полезное, что обычно поступает с Unix системой, и даже еще больше.

GNU сможет запускать программы Unix, но не будет идентична Unix. Мы введем все удобные усовершенствования, основываясь на нашем опыте работы с другими операционными системами. В частности, мы планируем сделать более длинными имена файлов, номера версий файлов, защищенную от сбоев файловую систему, возможно, завершение имени файла, поддержку терминально-независимого вывода и, в конце концов, возможно, основанную на `Lisp` оконную систему, при помощи которой различные Lisp-программы и обычные программы Unix смогут совместно использовать экран. Как `С`, так и `Lisp` будут доступны в качестве системных языков программирования. Мы постараемся обеспечить поддержку UUCP, MIT Chaosnet и протоколов связи Internet.

Изначально GNU нацелена на машины класса 68000/1600 с виртуальной памятью, так как на них ее легче всего запустить. Дополнительные усилия для запуска ее на меньших машинах будут предоставлены тому, кто захочет использовать ее на них.

Чтобы избежать ужасной путаницы, пожалуйста, произносите `G` в слове `GNU`, когда оно является именем данного проекта.
***
## Почему я должен написать GNU
Я считаю, что золотое правило требует: если мне нравится программа, то я должен поделиться ею с другими, кому она тоже нравится. Продавцы программного обеспечения хотят разделить пользователей и подчинить их себе, делая так, чтобы каждый из них соглашался не делиться с другими. Я отказываюсь нарушать солидарность с другими пользователями таким образом. Я не могу с чистой совестью подписать соглашение о нераскрытии или лицензионное соглашение по программному обеспечению. Во время моей работы в Лаборатории Искусственного Интеллекта я сопротивлялся этим тенденциям и другим препонам, но в конце концов они зашли слишком далеко: я не мог оставаться в институте, где за меня делаются такие вещи против моей воли.

Чтобы я мог использовать компьютеры с чистой совестью, я решил собрать вместе достаточное количество свободных программных продуктов, с тем, чтобы я мог обходится без какого-либо несвободного программного продукта. Я ушел из Лаборатории ИИ, чтобы иметь возможность отказать MIT на любых законных основаниях помешать мне раздавать GNU.
***
## Почему GNU будет совместима с Unix
Unix не является моим идеалом системы, но он не так уж плох. Его основные черты, по-видимому, будут полезны, я надеюсь что смогу восполнить пробелы Unix без того, чтобы разрушить его основу. А система, совместимая с Unix, может быть удобна для освоения многим людям.
***
## Каким образом GNU станет доступна
GNU не является общественной собственностью. Каждому разрешено видоизменять и повторно распространять ее, но распространителю не разрешается препятствовать ее дальнейшему распространению. То есть, не разрешается присваивать модификации. Я хочу быть уверенным в том, что все версии GNU останутся свободными.

> GNU is not in the public domain. Everyone will be permitted to modify and redistribute GNU, but no distributor will be allowed to restrict its further redistribution. That is to say, proprietary modifications will not be allowed. I want to make sure that all versions of GNU remain free.
***
## Почему многие программисты хотят помочь
Я встретил много программистов, которые заинтересовались GNU и захотели помочь.

Многих программистов не устраивает коммерциализация системных программных продуктов. Она может дать им возможность заработать больше денег, но заставляет их чувствовать себя соперниками с другими программистами, а не товарищами. Основной дружеский акт среди программистов - совместное использование программам; типичные маркетинговые соглашения, используемые сегодня, по существу запрещают программистам относится друг к другу по-дружески. Покупатель программного продукта должен выбирать между дружбой и подчинением закону. Естественно, многие решат, что дружба важнее. Но те, кто верит в закон, чувствуют себя неловко, сделав какой-либо выбор. Они становятся циничными и думают, что программирование - это просто способ делать деньги.

Работая над GNU и используя ее, а не принадлежащие кому-либо программы, мы можем быть благожелательны ко всем и в то же время соблюсти закон. Кроме того, GNU служит вдохновляющим примером и знаменем, сплачивающим остальных вокруг нас для совместного использования программам. Это может дать нам ощущение гармонии, которое невозможно, если мы используем несвободный программный продукт. Для почти половины программистов, с которыми я разговаривал, это неоценимое счастье, которое деньги заменить не могут.
***
## Каким образом вы можете внести свой вклад
Я прошу производителей компьютеров о пожертвованиях в виде денег или машин. Я прошу отдельных людей о вкладе программами или работой.

Одно из следствий того, что вы внесли свой вклад машинами, это то, что на них GNU будет запущена в кратчайшие сроки. Машины должны быть укомплектованными, готовыми к использованию системами, пригодными для использования в жилом помещении и не должны нуждаться в слишком сложных системах охлаждения и питания.

Я нашел очень много программистов, готовых потратить часть своего рабочего времени для работы над GNU. Для большинства проектов такую распределенную работу с неполной занятостью очень трудно координировать; независимо написанные части, возможно, не заработают вместе. Но для частной задачи замещения Unix это проблема отсутствует. Полная система Unix содержит сотен утилит, каждая из которых документирована отдельно. Большинство описаний интерфейсов зафиксированы для совместимости с Unix. Если каждый сотрудник сможет написать совместимую замену для одной Unix-утилиты и сделает так, чтобы она работала вместо оригинала в системе Unix, тогда вместе эти утилиты будут работать надлежащим образом. Даже если позволить Мерфи создать несколько неожиданных проблем, объединение этих компонент будет осуществимой задачей. (Ядро требует более тесной взаимосвязи и будет разрабатываться небольшой компактной группой).

В случае получения мною денежных пожертвований, я буду в состоянии нанять нескольких человек на полный или неполный рабочий день. Оклад будет не высоким по программистским меркам, но я ищу таких людей, для которых укрепление духа общности так же важно, как и зарабатывание денег. Я рассматриваю это как способ дать увлеченным людям возможность отдать всю свою энергию работе над GNU, оберегая их от необходимости зарабатывать себе на жизнь другим путем.
***
## Почему все пользователи компьютеров получат выгоду
Как только GNU будет написана, каждый сможет получить хороший свободный программный продукт так же свободно, как воздух.

Это означает гораздо больше, чем просто экономию каждому стоимости лицензии на использование Unix. Это означает, что будет устранена большая часть расточительного дублирования усилий по системному программированию. Эти усилия смогут пойти вместо этого на продвижение технологии.

Полные исходные коды системы будут доступны каждому. В результате, тот пользователь, которому нужны изменения в системе, всегда сможет беспрепятственно сделать их сам или нанять программистов или компанию, которые взялись бы сделать их для него. Пользователи больше не будут во власти одного программиста или компании, которые владеют исходными кодами и находятся в исключительном положении в смысле внесения изменений.

Учебные заведения смогут обеспечить более мощную образовательную среду, поощряя всех студентов изучать и улучшать системные коды. Гарвардская компьютерная лаборатория раньше проводила следующую политику: никакая программа не могла быть установлена в системе, если все ее исходные коды не были предоставлены на всеобщее обозрение; и это фактически поддерживалось отказом устанавливать определенные программы. Это меня очень вдохновило.

Наконец, будут ликвидированы расходы на рассмотрение того, кто владеет системным программным продуктом и что он вправе или не вправе делать с ним.

Соглашения о том, что люди должны платить за пользование программой, включая лицензирование копий, всегда влекут за собой громадные затраты для общества из-за громоздких механизмов, необходимых для подсчета того, сколько (то есть, за какие программы) должен платить человек. И только полицейское государство может заставить каждого подчиниться им. Рассмотрим космическую станцию, где производство воздуха должно стоить очень дорого: взимание с каждого живого существа платы за литр воздуха может быть справедливо, но ношение противогаза но счетчиком и день и ночь невыносимо, даже если каждый в состоянии заплатить по счету за воздух. А телевизионные камеры повсюду, чтобы следить, не снимаете ли вы противогаз, возмутительны. Лучше уж содержать завод по производству воздуха на средства от поголовного налога и сбросить противогазы.

Копирование всей или части программы присуще программисту как дыхание и также плодотворно. И оно должно быть столь же свободным.
***
## Некоторые легко опровергаемые возражения против целей GNU
> "Никто не будет ее использовать, если она будет бесплатной, так как это означает, что пользователь не сможет положиться ни на какое сопровождение."
> 
> "Вы должны будете назначить цену за программу, чтобы оплатить обеспечение сопровождения."

Если люди охотнее заплатят за GNU плюс обслуживание вместо получения GNU бесплатно и без обслуживания, то компания по обеспечению только обслуживания тех, кто получил GNU бесплатно, может стать прибыльной.

Мы должны различать сопровождение в виде действительной работы по программированию и простую поддержку. Первое - это нечто, в чем нельзя положиться на продавца программного продукта. Если ваши проблемы не разделяются достаточным количеством людей, то продавец скажет вам, чтобы вы убирались.

Если ваша фирма нуждается в возможности положиться на сопровождение, то существует единственный выход - иметь все необходимые исходные коды и сервисные программы. Тогда вы можете нанять любого, кто сможет решить вашу проблему; вы не зависите ни от чьей милости. В случае Unix цена исходных кодов выводит это из рассмотрения для большинства компаний. С GNU это будет просто. Вполне возможно, что при этом не найдется достаточно компетентного человека, но вина за эту проблему не может возлагаться на соглашения по распространению. GNU не решает все мировые проблемы, а только некоторые из них.

Тем временем, пользователи, которые ничего не знают о компьютерах, нуждаются в поддержке: в выполнении для них того, что они могли бы с легкостью сделать сами, но не знают как.

Такие услуги могут предоставить компании, которые продают только поддержку и ремонтное обслуживание. Если правда, что пользователи скорее потратят деньги и получат продукт с обслуживанием, то они также будут готовы покупать обслуживание, получив продукт бесплатно. Компании по обслуживанию будут конкурировать в качестве и цене; пользователи не будут привязаны к какой-то одной из них. Тем временем, те из нас, кто не нуждается в обслуживании, смогут пользоваться программой без его оплаты.

> "Вы не сможете заинтересовать многих людей, не используя рекламу, а чтобы окупить ее, вам придется назначить цену за программу."
> 
> "Бесполезно рекламировать программу, которую люди могут получить бесплатно."

Существуют различные виды бесплатной или очень дешевой рекламы, которая может быть использована для информирования большого числа пользователей о чем-то подобном GNU. Но может быть правда и то, что можно заинтересовать большее число пользователей микрокомпьютеров с помощью рекламных объявлений. Если это действительно так, то фирма, которая рекламирует услуги по копированию и пересылке GNU за плату, должна быть достаточно процветающей, чтобы платить за рекламу и даже более того. Тогда только пользователи, которые извлекают пользу из рекламы, платят за нее.

С другой стороны, если многие получают GNU от своих друзей, и подобные фирмы не будут процветать, то это покажет, что реклама не так уж необходима для распространения GNU. Почему же сторонники свободного рынка не хотят позволить решить это ему самому?

> "Моей компания нужна собственная операционная система, чтобы получить преимущество в конкурентной борьбе."

GNU выведет программное обеспечение для операционных систем из сферы конкурентной борьбы. Вы не сможете получить преимущество в этой области, но и ваши конкуренты также не смогут получить преимущество над вами. Вы будете конкурировать с ними в других областях, совместно извлекая выгоду в этой области. Если ваш бизнес - это продажа операционных систем, то вам GNU не понравится, но это ваши трудности. Если ваш бизнес состоит в чем-нибудь еще, то GNU сможет оградить вас от втягивания в дорогостоящий бизнес по продаже операционных систем.

Я хотел бы увидеть дальнейшее развитие GNU, поддерживаемое дарами от многих производителей и пользователей, это сократит затраты для каждого.

> "Разве программисты не заслуживают вознаграждения за свое творчество?"

Если уж что-то и заслуживает вознаграждения, так это общественный вклад. Творчество может быть общественным вкладом, но только до тех пор, пока общество может свободно пользоваться его результатами. Если программисты заслуживают вознаграждения за создание прогрессивных программ, то также они заслуживают и наказания, если они ограничивают использование этих программ.

> "Может ли программист просить вознаграждение за свое творчество?"

Нет ничего плохого в том, что кто-то требует плату за работу или стремится увеличить свой доход, до тех пор, пока он не пользуется разрушительными средствами. Но сегодня привычные средства в области программного обеспечения основываются именно на разрушении.

Выжимание денег из пользователей программы, ограничивая их возможности использовать ее, --- это и есть разрушение, так как ограничения уменьшает объем и способы использования программы. Это сокращает объем прибыли, которую человечество извлекает из программы. Когда кто-то предумышленно вводит ограничения, то вредным последствием этого является преднамеренное разрушение.

Причина того, что добросовестные граждане не пользуются такими разрушительными средствами, чтобы стать богаче, состоит в том, что если каждый бы поступал так, то мы все вместе стали бы беднее от всеобщего разрушения. Это этика Канта, или Золотое правило. Так как мне не нравятся последствия, проистекающие из того, что кто-то припрятывает информацию, я вынужден считать, что тот, кто поступает так, неправ. В частности, желание быть вознагражденным за свое творчество не оправдывает лишение общества вообще всего творения или его части.

> "Не будут ли программисты голодать?"

На это я могу ответить, что никого не заставляют быть программистом. Большинство из нас не смогут ухитриться зарабатывать деньги, стоя на улице и корча гримасы. Но в результате нас же никто не заставляет провести свою жизнь, стоя на улице, корча гримасы и голодая. Мы делаем что-то другое.

Но это неправильный ответ, так как он признает подразумеваемое в вопросе предположение: что без собственности на программное обеспечение программист не сможет получить ни цента. По общему мнению - все или ничего.

Действительная же причина, по которой программисты не будут голодать, состоит в том, что по-прежнему будет существовать возможность получать деньги за программирование, просто не так много, как сейчас.

Ограничение копирования не является единственной основой бизнеса в области программного обеспечения. Это самая распространенная основа, так как она приносит больше всего денег. Если бы это было запрещено или отвергнуто покупателями, то бизнес в области программного обеспечения перешел бы на другие организационные принципы, которые в данный момент используются не так часто. Всегда существует множество способов организации любого вида бизнеса.

Вероятнее всего, программирование с новыми организационными принципами не будет таким прибыльным, как сейчас. Но это не аргумент против перемен. Не считается же несправедливым жалование, которое получают клерки по продаже сейчас. Если бы программисты получали столько же, то это также не будет несправедливым. (На практике, они будут все так же получать значительно больше, чем клерки.)

> "Разве люди не имеют права контролировать, как используется их творение?"

**"Контроль над использованием чьих-то идей"** на деле вводит контроль над жизнью других людей, и он обычно используется, чтобы сделать их жизнь более трудной.

Люди, которые внимательно изучали происхождение права на интеллектуальную собственность (например, юристы), говорят, что не существует внутренне присущего права на интеллектуальную собственность. Виды предполагаемых прав на интеллектуальную собственность, которые признаются правительством, были созданы особыми законодательными актами для специальных целей.

Например, патентная система была создана для поощрения того, чтобы изобретатели раскрывали детали своих изобретений. Это должно было помочь обществу, а не изобретателям. В то время период существования в 17 лет для патента был коротким по сравнению с темпами повышения уровня развития. Так как патенты затрагивают только предпринимателей, для которых затраты усилий и денег на лицензионное соглашение малы по сравнению с расходами на запуск производства, то патенты зачастую не наносят ощутимого вреда. Они не мешают большинству людей, которые используют запатентованные продукты.

Идея авторских прав не существовала в древности, когда авторы нередко копировали других авторов со всеми подробностями в работах, которые не относились к художественной литературе. Такая практика была полезна, и только благодаря ей многие авторские работы сохранились хотя бы частично. Система авторских прав была создана специально для того, чтобы поощрять авторство. Сфера ее действия - это книги, которые могли быть экономно размножены только при помощи печатного станка. Это приносило мало вреда и не мешало большинству людей, которые читали книги.

Все права интеллектуальной собственности - это просто лицензии, предоставленные обществом, так как считается, правильно или ошибочно, что все общество в целом будет извлекать выгоду, предоставив их. Но в каждом конкретном случае мы должны задаваться вопросом: действительно ли мы станем богаче, выдав такую лицензию? Какие именно действия человека мы лицензируем?

Сегодняшняя ситуация с программным обеспечением очень сильно отличается от положения с книгами сто лет назад. Факты, состоящие в том, что простейший способ копирования программы - это передача ее одним соседом другому, что программы имеют как исходные, так и объектные коды, различающиеся между собой, что программы используются для работы, а не для чтения и удовольствия, вместе создают ситуацию, в которой лицо, настаивающее на авторских правах, наносит ущерб обществу в целом как в материальном, так и в духовном плане, и в которой человек не должен поступать так, независимо от того, позволяет ли ему это закон или нет.

> "Конкуренция заставляет все делать лучше и лучше."

Наглядный пример конкуренции - это гонки: награждая победителя, мы заставляем других бежать быстрее. Когда капитализм работает именно так, он делает нужное дело. Но его защитники ошибаются, полагая, что он всегда так работает. Если бегуны забывают, за что именно предлагается награда, и стремятся выиграть любой ценой, то они могут найти другую стратегию, такую, например, как расталкивание других бегунов. Если бегуны для начала ударятся в кулачный бой, то все они придут к финишу позже.

Частный и засекреченный программный продукт, - это моральный эквивалент бегунов в кулачном бою. Досадно говорить об этом, но единственный рефери, который у нас есть, по всей видимости не возражает против драк. Он просто регулирует их (**"На каждые 10 ярдов, которые вы пробежите, вы можете сделать один удар"**). В действительности же он должен прекращать их и наказывать бегунов даже за попытку драки.

> "Не прекратят ли все программировать без денежного вознаграждения?"

На самом деле многие будут заниматься программированием совершенно без денежного вознаграждения. Программирование имеет непреодолимое очарование для некоторых людей, обычно для тех, кому это лучше всего удается. Нет недостатка в профессиональных музыкантах, которые упорно продолжают заниматься музыкой, даже несмотря на то, что они не надеются зарабатывать этим на жизнь.

В действительности этот вопрос, несмотря на то, что он часто задается, не подходит к ситуации. Оплата программирования не исчезнет, просто она станет меньше. Так что правильно ставить вопрос следующим образом: будет ли кто-нибудь программировать за уменьшенное денежное вознаграждение? Мой опыт подсказывает мне, что такие люди найдутся.

В течение более чем десяти лет многие из лучших программистов работали в Лаборатории Искусственного Интеллекта, получая гораздо меньше денег, чем они могли бы иметь где-то еще. Они получили массу неденежных вознаграждений: славу и уважение, например. Кроме того, творчество - это развлечение, само являющееся наградой.

В дальнейшем многие из них ушли, когда появился шанс делать столь же интересную работу за большие деньги.

Эти факты демонстрируют, что люди будут заниматься программированием по причинам, отличным от обогащения. Но если появится шанс, кроме того, заработать много денег, то они будут надеяться на них и требовать их. Организации с низкой оплатой слабо конкурируют с организациями, где она высокая, но дела у них не должны быть плохи, если организации с высокой оплатой будут запрещены.

> "Нам отчаянно нужны программисты. Если они потребуют, чтобы мы прекратили помогать нашим соседям, то мы должны будем подчиниться."

Вы никогда не будете в таком отчаянном положении, чтобы вас можно было подчинить требованиям такого сорта. Запомните: миллион на защиту, но ни цента на дань!

> "Программистам как-то же надо зарабатывать на жизнь."

На первый взгляд это правильно. Однако, существует много способов, которыми программист мог бы заработать на жизнь, не продавая права на использование программы. Сейчас этот путь привычен, так как он дает программистам и бизнесменам самые большие деньги, а не потому, что это единственный путь заработать на жизнь. Легко найти другие пути, если вы захотите найти их. Вот несколько примеров.

Производители, внедряя новые компьютеры, будут платить за перенос операционных систем на новое оборудование.

Программистов также можно было бы использовать при продаже услуг по обучению, сопровождению и текущему обслуживанию.

Люди с новыми идеями могли бы распространять свободное программное обеспечение, спрашивая пожертвования от удовлетворенных пользователей или продавая текущее обслуживание. Я встречал людей, которые уже успешно работают таким образом.

Пользователи со схожими потребностями могут образовывать группы пользователей и платить членские взносы. Группа заключала бы контракт с программистскими компаниями для написания программ, которыми хотели бы пользоваться члены такой группы.

Все виды развития могут финансироваться с помощью налога на программный продукт:

> Предположим, что каждый, купивший компьютер, должен заплатить x процентов от его цены в качестве налога на программное обеспечение. Правительство передает эти деньги агентству, подобному Национальному научному фонду, чтобы оно потратило их на развитие программного обеспечения.
> 
> Но если покупатель компьютера сделает пожертвование на развитие программного обеспечения сам, то он может получить кредит в уплату налога. Он может сделать денежное пожертвование на проект по своему выбору, причем выбор проекта часто объясняется тем, что он надеется воспользоваться его результатами. Он может получить кредит на любую сумму пожертвования вплоть до общей суммы налога, которую он должен заплатить.
> 
> Полная ставка налога может определяться голосованием налогоплательщиков, при этом налог определяется пропорционально сумме, с которой он берется.
> 
> Следствия:
> 
> - Общество, использующее компьютеры, поддерживает развитие программного обеспечения.
> - Общество решает, какой уровень поддержки требуется.
> - Пользователи, которых волнует, на какой проект пойдет их доля, могут сами выбрать его.

В конце концов, создание свободных программ - это шаг по направлению к постдефицитному миру, где никто не должен будет работать очень напряженно, чтобы просто прожить. Люди смогут посвятить себя тому виду деятельности, который доставляет им удовольствие, например программированию, отработав требуемые десять часов в неделю на необходимых заданиях, таких как законотворчество, семейные советы, починка роботов и астероидные исследования. Не надо будет зарабатывать на жизнь программированием.

Мы уже имеем значительное сокращение объема работ, которые общество в целом должно делать для обеспечения реальной производительности, но только малая часть этого преобразуется в досуг для работников, так как требуется много деятельности в непроизводственной сфере для сопровождения производственной деятельности. Основная причина этого - бюрократия и соответственные усилия на борьбу с конкуренцией. Свободное программное обеспечение значительно сократит эти расходы в области производства программных продуктов. Мы должны сделать это, чтобы повышение производительности преобразовывалось в сокращение работы для нас.
***
## Сноски
1. Некоторые слова здесь были употреблены небрежно. Намерение состояло в том, чтобы никто не должен был платить за _разрешение_ пользоваться системой GNU. Но эти слова не проясняют это, и люди часто интерпретируют их, как будто они говорят, что копии GNU всегда должны распространяться бесплатно или за малую плату. Это никогда не было целью; далее манифест упоминает о возможности компаний предоставлять услуги по распространению за плату. Впоследствии я понял, **что нужно точно различать "free" в смысле свободы и "free" в смысле цены.** Свободное программное обеспечение - это программное обеспечение, которое пользователи вольны распространять и изменять. Некоторые пользователи могут получить копии бесплатно, тогда как другие платят за получение копий, и если эти деньги помогают улучшать эти программы, то чем больше, тем лучше. Важно то, что каждый, кто имеет копию, имеет свободу сотрудничать с другими при ее использовании.
2. Это еще одно место, где я не выявил тщательно разницу между двумя различными значениями слова **"free"**. Само по себе это предложение не является неправдой - вы можете получить копии программ GNU бесплатно от ваших друзей или по сети. Но оно предполагает ложную идею.
3. Сейчас существуют несколько таких компаний.
4. Фонд Свободного Программного Обеспечения получает большую часть средств за услуги по распространению, хотя он является организацией, существующей на пожертвования, а не коммерческой компанией. Если _никто_ не выберет в качестве способа получить копии заказ от Фонда, он будет неспособен выполнять свои работу. Но это не означает, что собственнические ограничения оправданы, чтобы заставить каждого пользователя платить. Если малая часть всех пользователей заказывает копии в Фонде, этого достаточно, чтобы Фонд остался на плаву. Поэтому мы просим пользователей поддержать нас таким образом. Сделали ли вы свою часть?
5. Группа компьютерных компаний недавно выдала средства на поддержку сопровождения компилятора GNU C Compiler.
***
