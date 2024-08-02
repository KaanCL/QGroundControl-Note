# Dosya yapısı

# src/: Ana kodlarının bulunduğu klasördür.

- **API/**: API ile ilgili dosyalar.
- **AutoPilotPlugins/**: Farklı autopilot sistemleri için eklentiler.
- **Camera/**: Kamera kontrol ve yönetim dosyaları.
- **Comm/**: Haberleşme ile ilgili dosyalar.
- **FirmwarePlugin/**: Farklı firmware'ler için eklentiler.
- **FlightDisplay/**: Uçuş ekranı ile ilgili dosyalar.
- **FlightMap/**: Uçuş haritası ile ilgili dosyalar.
- **Joystick/**: Joystick kontrol dosyaları.
- **MissionManager/**: Görev yönetimi dosyaları.
- **QmlControls/**: QML kontrolleri.
- **Settings/**: Ayarlar ile ilgili dosyalar.
- **UI/**: Kullanıcı arayüzü dosyaları.
- **Vehicle/**: Araçlarla ilgili dosyalar.

# libs/:Harici bağımlılıkların bulunduğu klasördür.

# resources/:Görseller, simgeler, stil dosyaları ve diğer yardımcı medya dosyalarını içerir.

### QGroundControl'de Resource Dosyası Yapısı

QGroundControl'ün kaynak dosyaları, `qgroundcontrol.qrc` dosyası içinde tanımlanır. Bu dosya, projenin ana kaynak dosyasıdır ve çeşitli alt kaynak dosyalarını içerir. Kaynak dosyaları `.qrc` uzantısına sahiptir ve XML formatında tanımlanır.

### Resource Dosyasının Yapısı

1. **RCC**: Ana XML kök elemanıdır ve tüm kaynakları içerir.
2. **qresource**: Her bir `qresource` elemanı, bir kaynak koleksiyonunu temsil eder. `prefix` özniteliği, kaynak dosyalarının uygulamada hangi yol ile erişileceğini belirtir.
3. **file**: Her bir `file` elemanı, projeye dahil edilecek olan bir dosyayı belirtir.

### Kaynak Dosyalarının Kullanımı

Kaynak dosyaları, Qt Resource System sayesinde uygulama içinde kolayca kullanılabilir. Örneğin, bir QML dosyasını `qrc:/` öneki ile yükleyebilirsiniz:

### 

### QGroundControl'de Kaynak Dosyalarının Yönetimi

QGroundControl'de kaynak dosyaları genellikle aşağıdaki klasörlerde bulunur:

- **images/**: Uygulama ikonları ve diğer görseller.
- **qml/**: QML dosyaları ve arayüz bileşenleri.
- **fonts/**: Yazı tipleri.
- **styles/**: CSS ve stil dosyaları.

# .pro

`.pro` dosyası, Qt projelerinde kullanılan bir yapılandırma dosyasıdır. Bu dosya, proje ayarlarını ve derleme sürecinde gerekli olan bilgileri içerir. `.pro` dosyası, qmake tarafından kullanılır ve proje dosyalarının nasıl derleneceğini belirler.

- **Proje Ayarları:**
Proje hakkında genel bilgileri tanımlar. Örneğin, proje adı ve versiyonu gibi bilgiler burada yer alır.
- **Kaynak Dosyaları:**
Projede derlenecek olan kaynak kod dosyalarının listesini içerir. Bu genellikle `SOURCES` anahtar kelimesi ile belirtilir.
- **Header Dosyaları:**
Projede kullanılan header dosyalarını içerir. Bu genellikle `HEADERS` anahtar kelimesi ile belirtilir.
- **QML Dosyaları:**
Projede kullanılan QML dosyalarını içerir. Bu genellikle `QML_FILES` anahtar kelimesi ile belirtilir.
- **Form Dosyaları:**
Qt Designer tarafından oluşturulan UI dosyalarını içerir. Bu genellikle `FORMS` anahtar kelimesi ile belirtilir.
- **Kaynak Dosyaları (Resimler, İkonlar vb.):**
Projede kullanılan kaynak dosyalarını içerir. Bu genellikle `RESOURCES` anahtar kelimesi ile belirtilir.
- **Bağımlılıklar:**
Projede kullanılacak olan ek kütüphaneler ve bağımlılıkları tanımlar. Bu genellikle `LIBS` ve `INCLUDEPATH` anahtar kelimeleri ile belirtilir.

# Mimarisi

QGroundControl MVC (Model-View-Controller) benzeri bir mimari kullanır .

### 1. Model

Model, verilerin depolandığı ve yönetildiği yerdir. QGC'de model, uçuş bilgileri, araç durumu, görev planları ve diğer veri yapılarını içerir. Model bileşeni genellikle Qt'nin `QObject` sınıfından türetilir ve `Q_PROPERTY` makrosu ile özellikler tanımlanır.

### 2. View

View, kullanıcı arayüzünü temsil eder ve genellikle QML kullanılarak tanımlanır. View bileşeni, modeli görüntüler ve kullanıcıdan gelen etkileşimleri kontrolcüye iletir. QGC'de View, çeşitli QML bileşenleri ve sayfaları içerir.

### 3. Controller

Controller, model ve view arasındaki etkileşimi yönetir. Kullanıcı girişlerini işler, modeldeki verileri günceller ve view bileşenlerini tetikler. QGC'de Controller genellikle C++ ile yazılır ve QML bileşenleri ile etkileşime girer.

### Plugin Mimarisi

QGC, firmware'e özgü kodu tüm firmware için genel olan koddan izole etmek için bir plugin mimarisi kullanır. Bu işlevi yerine getiren üç ana plugin vardır:

1. **FirmwarePlugin**
2. **AutoPilotPlugin**
3. **QGCCorePlugin**

Aşağıdaki şema, QGC mimarisini göstermektedir. QGC'nin ön ucu için dört ana görünümü vardır:

1. **Planview**
2. **Flyview**
3. **Settings View**
4. **Setup View**

<img src="https://media.licdn.com/dms/image/C5612AQHVnPg8Kf2FqA/article-inline_image-shrink_1000_1488/0/1641558978546?e=1727913600&v=beta&t=apzFP0CqO6jkoFrfE0CrXyaOwjx_-qmc38cFRQ9xuCQ">

### 

### QGCApplication ve QGCToolBox

**QGCApplication**, QGC'nin ana uygulama gövdesidir. Tüm ana işlevler ve yönetim yapıları bu sınıf üzerinden başlatılır ve kontrol edilir. QGCApplication, QGCToolBox ile yakından ilişkilidir.

**QGCToolBox**, QGCApplication ile ilişkili olan ve çeşitli ana işlevleri içeren bir yapılandırma ve yardımcı araç sınıfıdır. QGCToolBox, QGC'nin temel işlevlerini ve hizmetlerini sağlar ve aşağıdaki ana bileşenleri içerir:

- **MAVLinkProtocol**: MAVLink mesajlaşma protokolünü yönetir.
- **LinkManager**: Bağlantıları ve veri akışını yönetir.
- **MissionCommandTree**: Görev komutlarını ve yapısını yönetir.

### MAVLinkProtocol

**MAVLinkProtocol**, MAVLink mesajlarını yönetir ve işler. MAVLink, dronlar ve kontrol cihazları arasında iletişim kurmak için kullanılan bir protokoldür. Bu protokol, uçuş kontrol komutlarını, telemetri verilerini ve diğer önemli bilgileri iletmek için kullanılır.

### LinkManager İşleyişi

**LinkManager**, dron veya araç ile iletişim kurmak için bağlantıları yönetir. İşleyişi şu şekildedir:

1. **Bağlantı Açma**: LinkManager her zaman bir drone veya vehicle heartbeat (kalp atışı) mesajını bekleyen bir UDP bağlantısı açar.
2. **Yeni Cihaz Algılama**: LinkManager, bilgisayara bağlı yeni bir bilinen cihazı (örneğin, Pixhawk, PX4 Flow) algılar ve cihaza bağlı yeni bir SerialLink oluşturur.
3. **Veri Akışı**: Bağlantıdan gelen baytlar MAVLinkProtocol'e gönderilir.
4. **HEARTBEAT Mesajı**: Mesaj bir HEARTBEAT ise, MultiVehicleManager bilgilendirilir.
5. **Araç Oluşturma**: MultiVehicleManager, HEARTBEAT mesajındaki bilgilere dayanarak yeni bir Vehicle nesnesi oluşturur. Vehicle, araç tipine uygun pluginleri başlatır.

### Görev Öğelerinin Eklenmesi ve JSON Olarak Kaydedilmesi

Görev öğelerinin eklenmesi, yüklenmesi ve JSON olarak kaydedilmesi şu şekilde gerçekleşir:

1. **Görev Bilgisi Toplama**: PlanMasterController, görev bilgilerini toplar.
2. **Kontrolcü Bilgisi Kullanma**: MissionController, GeoFenceController ve RallyPointController bilgileri kullanır.
3. **Görev Öğesi Ekleme**: Görev öğeleri eklendikten sonra, bu öğeler JSON formatında kaydedilir.

# Signal / Slot

Qt framework'ü event based (olay tabanlı) programlamayı destekler , signal ve slot mekanizması ile nesneler arasındaki iletişimi sağlar. QGroundControl (QGC) gibi Qt tabanlı uygulamalarda bu mekanizma yoğun bir şekilde kullanılır. Signal ve slot, bir nesnede bir olay (signal) meydana geldiğinde, başka bir nesnede bu olaya yanıt (slot) vermeyi sağlar.

### Signal ve Slot Mekanizması

### Signal

Signal, bir olay meydana geldiğinde gönderilen bir mesajdır. Signal, herhangi bir işlemi gerçekleştirmez; sadece meydana gelen bir olayı bildirir. Örneğin, bir butona tıklandığında bir signal gönderilir.

### Slot

Slot, bir signal alındığında yürütülen bir işlevdir. Slot, bir signal'e yanıt olarak belirli bir işlemi gerçekleştirir.

### QGroundControl'de Signal ve Slot Kullanımı

QGC, Qt'nin signal ve slot mekanizmasını yoğun bir şekilde kullanır. Örneğin, bir drone'un konum verilerini güncellemek, kullanıcı arayüzündeki bileşenleri güncellemek veya kullanıcı etkileşimlerine yanıt vermek için signal ve slot kullanılır.

# QGC den Pluginlere Veri Aktarımı

### QGroundControl ve MAVLink Veri Akışı

1. **Veri Toplama**: QGC, drone'dan gelen MAVLink mesajlarını toplar. Bu mesajlar, telemetri verileri, sensör verileri, görev bilgileri ve durum güncellemeleri gibi çeşitli bilgileri içerir.
2. **Veri İşleme**: Alınan MAVLink mesajları, QGC'nin çekirdek bileşenleri tarafından işlenir. Bu işleme süreci, mesajların ayrıştırılmasını, doğrulanmasını ve ilgili bileşenlere yönlendirilmesini içerir.
3. **Veri Dağıtımı**: İşlenen veriler, QGC'nin çeşitli modüllerine ve eklentilerine yönlendirilir. Bu modüller ve eklentiler, belirli işlevleri yerine getirmek için bu verileri kullanır.

### MAVLink Mesaj Türleri ve Eklentilere İletilen Veriler

### Görev İlgili Mesajlar

- **MISSION_COUNT**: Görev planının toplam öğe sayısını belirtir.
- **MISSION_ITEM**: Tek bir görev öğesinin (mission item) ayrıntılarını içerir. Bu mesaj, waypoint'ler, takeoff, land, vs. gibi çeşitli görev öğelerini tanımlar.
- **MISSION_REQUEST**: Araç, belirli bir görev öğesini istemek için bu mesajı gönderir.
- **MISSION_ACK**: Görev planının başarıyla alındığını onaylar.
- **MISSION_CLEAR_ALL**: Tüm görev öğelerini temizlemek için kullanılır.

### Telemetri ve Durum Mesajları

- **HEARTBEAT**: Araç ve yer istasyonu arasında bağlantının canlı olduğunu belirtir. Bu mesaj, sistem durumu, uçuş modu ve sistem tipi gibi bilgileri içerir.
- **ATTITUDE**: Aracın yuvarlanma (roll), yunuslama (pitch) ve sapma (yaw) açılarının yanı sıra açısal hızlarını bildirir.
- **GLOBAL_POSITION_INT**: Aracın küresel konumunu (enlem, boylam, yükseklik) ve hızını (kuzey, doğu, aşağı) bildirir.
- **LOCAL_POSITION_NED**: Aracın yerel koordinat sistemindeki konumunu ve hızını (kuzey, doğu, aşağı) bildirir.
- **SYS_STATUS**: Batarya durumu, iletişim hataları ve sensör durumları gibi sistem durum bilgilerini içerir
- **GPS_RAW_INT**: GPS verilerini içerir (konum, hız, doğruluk, uydu sayısı vb.).

### Komut ve Kontrol Mesajları

- **COMMAND_LONG**: Araca belirli bir komut gönderir (örneğin, motorları başlat, kalibrasyon yap, vb.).
- **SET_MODE**: Uçuş modunu değiştirmek için kullanılır.
- **MANUAL_CONTROL**: Manuel kontrol verilerini (joystick girişleri gibi) araca gönderir.
- **RC_CHANNELS_OVERRIDE**: RC kanallarının yerini almak için kullanılır (manuel RC sinyali yerine).

### Sensör Verileri ve Diğer Mesajlar

- **RAW_IMU**: Ham IMU verilerini (ivmeölçer, jiroskop, manyetometre) içerir.
- **SCALED_PRESSURE**: Barometrik basınç ve sıcaklık verilerini içerir.
- **VFR_HUD**: Hız, yüksek irtifa, başlık, tırmanış hızı gibi uçuş göstergesi verilerini içerir.
- **BATTERY_STATUS**: Batarya gerilimi, akımı ve pil durumu gibi batarya bilgilerini içerir.

### Görev Bilgilerinin MAVLink Mesajları ile Gönderilmesi

MAVLink, JSON formatını doğrudan desteklemez. Bu nedenle, QGC görev bilgilerini MAVLink mesajlarına dönüştürür ve bu mesajları drone'a gönderir. Görev bilgilerini iletmek için kullanılan temel MAVLink mesajları şunlardır:

1. **MISSION_COUNT**: Drone'a kaç adet görev öğesi gönderileceğini belirtir.
2. **MISSION_ITEM** veya **MISSION_ITEM_INT**: Her bir görev öğesinin ayrıntılarını içerir.
3. **MISSION_ACK**: Görev öğelerinin alındığını onaylar.

### JSON'dan MAVLink Mesajlarına Dönüşüm

QGC, görev dosyasını yüklediğinde JSON formatındaki görev öğelerini okuyup bunları MAVLink mesajlarına dönüştürür. Bu süreçte, her bir görev öğesi, `MISSION_ITEM` veya `MISSION_ITEM_INT` mesajına çevrilir.

### MISSION_ITEM Mesajı İçeriği

- **target_system**: Hedef sistem (drone) kimliği.
- **target_component**: Hedef bileşen kimliği.
- **seq**: Görev öğesi sıra numarası.
- **frame**: Koordinat çerçevesi (örneğin, MAV_FRAME_GLOBAL_RELATIVE_ALT).
- **command**: MAVLink komutu (örneğin, MAV_CMD_NAV_WAYPOINT).
- **current**: Bu öğenin şu anki görev öğesi olup olmadığını belirler.
- **autocontinue**: Bu öğeden sonra otomatik olarak bir sonraki öğeye geçilip geçilmeyeceğini belirler.
- **param1 - param7**: Görev öğesi için parametreler (örneğin, konum, yükseklik).

<img src="https://prod-files-secure.s3.us-west-2.amazonaws.com/05f2bc3a-34c5-4bc8-b18b-6fda5890294c/9dfb1db6-ac56-4c27-a9da-c03e365df363/Untitled.png">

### Diagram Üzerindeki Akış

1. **GeoFenceManager.cpp** ve **RallyPointManager.cpp**:
    - Bu iki yöneticiden gelen `MissionItem` verileri PlanMasterController'a iletilir.
2. **PlanMasterController**:
    - Görev öğelerini alır ve tüm görev planını yönetir.
    - Görev öğelerini MAVLink mesajlarına dönüştürür.
3. **MAVLink**:
    - Dönüştürülen MAVLink mesajları drone'a gönderilir.

,

QGroundControl (QGC), firmware'e özgü kodu tüm firmware'ler için genel olan koddan ayırmak için bir plugin mimarisi kullanır. İki ana plugin bu işlevi yerine getirir: **FirmwarePlugin** ve **AutoPilotPlugin**.

Bu plugin mimarisi, standart QGC'nin sağlayabileceğinden daha fazla özelleştirme yapmak isteyen özel yapılar tarafından da kullanılır.

- **FirmwarePlugin**: MAVLink'in genellikle standartlaştırılmamış kısımlarına standart bir arayüz sağlar.
- **AutoPilotPlugin**: Araç Kurulumu için kullanıcı arayüzünü sağlar.
- **QGCCorePlugin**: QGC uygulamasının araçlarla ilgili olmayan özelliklerini standart bir arayüz aracılığıyla açığa çıkarır. Bu, özel yapılar tarafından QGC özellik setini ihtiyaçlarına göre ayarlamak için kullanılır.


