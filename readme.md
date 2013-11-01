В данном руководстве описано:

* как подготовить Eclipse Standart (впрочем, оно также подходит для версии Eclipse for Java developers, ибо тех Maven- и WindowBuilder-плагинов, что там стоят, недостаточно) для импорта данного Maven-проекта и 
* как собственно импортировать данный проект.

##I. Добавление кнопок git на панель
[Взято из предыдущего моего мануала Eclipse+GitHub](http://yadi.sk/d/Bd1JqGraBCPgK)

1. ![](http://img-fotki.yandex.ru/get/9061/165433899.0/0_e6a34_b9311bf6_orig "Window -> Customize Perspective...")
2. ![](http://img-fotki.yandex.ru/get/9065/165433899.0/0_e6a35_cc927160_orig "Command Groups Availability -> Git")

##II. Установка Maven-плагина
Взято [отсюда](http://www.apache-maven.ru/quick_start.html).  
Для установки:

1. зайдите в меню Help->Install New Software
2. Выберите Work with --All Available Sites--
3. в фильтре наберите `m2e` и отметьте плагины как показано на рисунке
![](http://img-fotki.yandex.ru/get/9480/165433899.0/0_e6a29_a835dd9_orig)
4. Отключите галочки, как написано [здесь](#diskspace), чтобы Maven не забивал Workspace лишним мусором на полгигабайта.

##III. Установка коннектора Maven SCM Handler for EGit
Взято [отсюда](http://www.machineversus.me/2012/08/if-youve-upgraded-to-eclipse-juno.html) и обновлено.  
Коннектор нужен для того чтобы [на V-м шаге в поле 'SCM URL' был пункт 'git'](#connector), иными словами, для возможности нормального импорта Maven-проектов из Git-репозиториев.  
Мы вынуждены добавлять неподписанный плагин-коннектор из стороннего репозитория, из-за того что коннектор из официального репозитория не совместим с последними версиями Eclipse (судя по [ссылке](http://www.machineversus.me/2012/08/if-youve-upgraded-to-eclipse-juno.html), эта проблема уже была начиная с Eclipse Juno).

1. Click Help
2. Install New Software
3. Uncheck the box labeled `Group items by category` (this step is important or you won't see the connector in the table)
4. Paste in this URL `http://repository.tesla.io:8081/nexus/content/sites/m2e.extras/m2eclipse-egit/0.14.0/N/0.14.0.201305250025/` - ссылка обновлена по сравнению с той что в статье...
![Скрин](http://img-fotki.yandex.ru/get/9166/165433899.0/0_e6a24_bc33a4af_orig "Снять галочку Group items by category")
5. Eclipse предупредит об установке неподписанного плагина, соглашаемся...
6. Finish the plugin install wizard and restart the workspace

##IV. Установка Window Builder(опционально)
* Плагин [WindowBuilder](http://www.eclipse.org/windowbuilder/ "Страница WindowBuilder на сайте Eclipse") добавляет WYSIWYG-редактор, с помощью которого можно быстро создавать окошки.  
![](http://img-fotki.yandex.ru/get/4912/165433899.0/0_e6a3c_e7a0a5f5_orig)
![](http://img-fotki.yandex.ru/get/9092/165433899.0/0_e6a3f_b3413d1f_orig)
* [Страница загрузки WindowBuilder](http://www.eclipse.org/windowbuilder/download.php), на которой нужно выбрать соответствующую вашей версии Eclipse ссылку. [Эта ссылка, например для Eclipse Kepler](http://download.eclipse.org/windowbuilder/WB/release/R201309271200/4.3/), как и все остальные, содержит инструкции по установке.

##V. Импорт maven-проекта из GitHub в Eclipse Workspace
Взято [отсюда](http://stackoverflow.com/questions/4869815/importing-a-maven-project-into-eclipse-from-git) и дополнено.

1. Select the "Import..." context menu from the Package Explorer view
2. Select "Check out Maven projects from SCM" option under the Maven category
3. <a name="connector"></a> ![](http://img-fotki.yandex.ru/get/9106/165433899.0/0_e6a30_bdfdd25a_orig)

Здесь есть одна особенность - а именно: Eclipse может 'задуматься' на несколько секунд(Windows) или на несколько минут(Linux) перед тем, как собственно отобразить проект в Package Explorer.  
4. Дальше обновляем проект(иногда сам Maven просит это сделать через Problems View)
![](http://img-fotki.yandex.ru/get/4912/165433899.0/0_e6a27_2152292e_orig "Выбираем Maven -> Update Project...")
![](http://img-fotki.yandex.ru/get/9109/165433899.0/0_e6a22_75102b8c_orig "Снимоем галочку Offline")  
5. Запускаем Тесты, maven скачает все нужные для сборки плагины и зависимости...
![Запускаем тесты](http://img-fotki.yandex.ru/get/9163/165433899.0/0_e6a28_6ccd25e_orig "Запускаем тесты: Run As -> Maven test")

##VI. Известные проблемы
###1. Ругается на кодировку `illegal character: \65279`
Проблема проявилась на Linux Mint amd64, но не проявилась на Windows x86.  
[Решение:](http://stackoverflow.com/questions/1068650/using-awk-to-remove-the-byte-order-mark)  
`# Removing BOM from all text files in current directory:`  
`sed -i '1 s/^\xef\xbb\xbf//' *.java`
###2. Куда подевался гигабайт дискового пространства?
1. Maven скачивает зависимости и прочие плагины в `~/.m2/repository` на Linux и в `%USERPROFILE%\.m2\repository` на Windows. Очищать эти папки во время работы над проектом нет смысла.
2. По непонятным причинам Maven забирает примерно 500МБ в `You_Workspace_path/.metadata/.plugins/org.eclipse.m2e.core/nexus`<a name="diskspace"></a>  
Это безобразие можно отключить, выставив галочки вот так: ![alt-текст](http://img-fotki.yandex.ru/get/9512/165433899.0/0_e6a23_57a3866c_orig "Галочка Offline должна быть снята!")  
(решение взято [отсюда](http://stackoverflow.com/questions/8539841/eclipse-metadata-plugins-disk-space), но нужно оставить снятой галочку `Offline`, иначе не скачаются новые файлы(плагины и зависимости), а значит мы не сможем собрать только что импортированный проект).  
Затем можно удалить папку `You_Workspace_path/.metadata/.plugins/org.eclipse.m2e.core/nexus`.

###3. pom.xml скорее всего не совместим со старым maven 2.x,
в Eclipse, судя по возникшим проблемам, используется 3.x.

###4. An error occurred while collecting items to be installed...
Или "Восстанавливаем работоспособность `Help -> Install New Software` и `Help -> Check for Updates` в Eclipse Kepler".  
Взято [отсюда](http://stackoverflow.com/questions/511367/error-when-updating-eclipse) и обновлено для Eclipse Kepler.

1. export the update site listing to bookmarks.xml file (Install/Update->Available Software Sites->Export)
2. stop eclipse
3. remove configuration/.settings/org.eclipse.equinox.p2.*.prefs files
`C:\Program Files\eclipse\p2\org.eclipse.equinox.p2.engine\.settings`  
`C:\Program Files\eclipse\p2\org.eclipse.equinox.p2.engine\profileRegistry\epp.package.standard.profile\.data\.settings`  
4. start eclipse
5. import the bookmarks.xml file (Software Updates->Manage sites->Import) that was exported in step 1