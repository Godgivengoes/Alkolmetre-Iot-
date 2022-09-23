# Alkolmetre-Iot-
docker npm kullanarak yaptıgım ardunio ide kullanarak gerçeklerştiripdiğim IoT projesi
Özet
Esp8266 kartını kullanarak MQ-3 sensor yardımı ile OLED ekrandan ve Grafana, InfluxDB ve Node-red yardımıyla gerekli hesaplamalarla alkol miktarı tespiti ve veri tabanına kaydedilmesi.
Giriş
Dünyada her dönem de olduğu gibi içinde bulunduğumuz dönemde dahil olmak üzere içki tüketimi hiçbir zaman insanların sosyal hayatlarından ayrı tutulmamıştır. Kimi zaman arkadaş buluşmalarında kimi zaman bir akşam yemeğinde insanların sofrasında yerini almıştır içkiler. Bununla birlikte ortaya çıkan insanların sarhoşluk durumları günlük hayatını etkileyebildiği gibi başka bireylerin de hayatını doğrudan veya dolaylı bir şekilde etkilemektedir. Bu sebebten alkol içme derecesinin ölçülmesi çoğu durumda gerekli bir durum halini almıştır. Özellikle , trafik polislerinin kullandığı alkolmetre cihazları büyük bir iş yapmaktadır. Bu bilgilerden yola çıkıp , alkolmetre kullanmanın insanları daha bilinçli bir sürücü yapacağını düşündüğüm için alkolmetre yapma fikrine karar verdim. Aslında yaptığım proje , sürücülerin üflediği  cihazlarda benzer mantıkta çalışıyor. Materyal ve yöntem kısmında daha açıklayıcı şekilde ilerlediğim adımlar açıklanmıştır.
Materyal yöntem
1-)ESP8266
Hızla gelişen IOT (Nesnelerin İnterneti) alanında fenomen olma yolunda ilerleyen bu minik modül ucuz olması ve geniş doküman kaynaklarıyla halen popülerliğini korumaktadır.
ESP8266, TTL (Seri Haberleşme) ile kablosuz internet ağına bağlanabilen bir modüldür. Ucuz ve kolay kullanıma sahip olmasından dolayı nesnelerin interneti projelerinde yaygın olarak kullanılmaktadır.
ESP8266 modülü 3.3V ile çalışmaktadır.
Bu modül ortamda bulunan kablosuz ağlara bağlanabileceği gibi, kendi internet ağına yayarak diğer cihazların bu ağa bağlanabilmesine de imkan sağlamaktadır. Bir çok farklı işlev olarak kullanabiliriz.Server, AccessPoint vb. farklı sekillerde kullanılabilir.

2-)MQ-3 Sensör
Alkolmetremizin kalbindeki cihaz MQ-3 alkol sensörü bu sensor havadaki ethanol moleküllerini tespit ediyor. Ethenol aslında içkilere konulan bir alkol türü sensörün alkolu tam anlamıyla tespit edebilmesi için 40°C ye kadar ısınması gerekiyor. İçinde kendi ısıtıcıları var. Bu ısıtıcılar optimum sıcaklıga gelmesine bir kaç dakika içinde yardımcı oluyor ancak bu sıcaklık seviyesinde sensorun için de kimyasal tepkimeler gerçeklesiyor. Ethanol sıcak yüzeye düşünce asetik asite dönüşüyor ve asetik asit iletken görevi görüyor ve geçen elektrik akımını artırıyor. Dolayısıla elektrik akımının fazlalığını alkol moleküllerinin kabaca miktarını veriyor. PCB ye oturtulmuş olmaları cok daha kullanışlı.
Sensör de 4 tane bacak bulunuyor bunlar başlıca şunlardır :
GND : Toprak
DOUT : Çıkış pini
AOUT : Analog out pini anlamına geliyor bunu ESP8266 da A0 pinine bağlıyoruz.
VCC: 3.3 V u bağladğımız bacak

					 
					Şekil.1-MQ-3 sensor
3-)OLED I2C 0.96 inch Display
64x128 piksel olan adafriut industries tarafından üretilen oldukça kullanışlı bir ekran
Ve I2C haberleşmesi sayesinde 4 adet pin kullanılıyor bunlar şöyle sıralanabilir:
VCC: 3.3V a bağladımız bacak
GND:Toprak
SCL:D2 pinine bağlıyoruz
SDA :D1 pininde bağlıyoruz


					 
					Şekil.2-I2C 0.96 inch OLED ekran




Öncelikle gerekli bağlantıları yaptıktan sonra alkol ölçüm biçimlerinden bahsetmek istiyorum.

			 
			Şekil.3-Kandaki alkol miktarı

Yukardaki tabloya göre kişilerin yaş cinsiyet değişimiyle beraber farklılıkları göz önüne alınarak ,kandaki alkol miktarı tahmini gösterilmiştir. Ek olarak , yasal sınırda belirtilmistir.
Çoğu Avrupa ülkesi başta olmak üzere Kanada ve Amerika Birleşik Devletleri vatandaşarına  bu tabloya göre yasal alkol sürüş limiti hakkı tanımıştır.

Serial olarak takip edebilir olan BAC değerinin hesaplanması aşağıdaki gibidir.

 
Ana formul yukardaki gibidir.


burada F1, x1 ve F0, x0, arsa üzerindeki çizgiden iki noktadır. Yalnızca yukarıdaki grafikteki alkol çizgisini ele alırsak, iki noktanın konumunu yaklaşık olarak tahmin edebiliriz:

 
Noktaları yukardaki ana formule eklersek:

 
RO(temiz hava) / RS (son hava) arasındaki ilişkiyi gösteren formul
 
Denklem çözülünce :
 
Miligramdan grama dönüşğüm yapıldıktan sonra son hali:
 
Kurumlular
Rasperry Pi işletim sisteminde düzgün çalışmayacağı için ubuntunun son sürümünü kuruyoruz. Ardından IOTstack kurulumuna geçiyoruz.IOTstack kurduktan sonra kullanacağımız yapıları işaretleyip kurduktan sonra final olarak bu görüntüyü almamız gerek
 
			Şekil.4 IOTstack kurulum final ekran görüntüsü

Kurulum bittikten sonra bağlanmış olduğumuz IP nin onun port numaraları eklenerek özelleştirme sayfalarına gidilir.Node-Red 1880 , Grafana 3000, Portainer 9000, olarak erişebiliriz.İlk olarak es
 





























Bulgular

Temiz havayı R0 olarak tanıplayıp değeri yaklasık 130000 olarak alındı ve ekler kısmındaki kodlara döküldü ve bu BAC oranı serial olarak izlenebilmekte.
 
			Şekil.4 – Serial olarak görünen BAC değeri
Serial de gözüken bana request geldi kısımlarını alkololc fonksyonunda ne zaman girdiğini görebilmek için kullanıldı.
MQ-3 sensörüne alkol tutulduğunda serialde görülen değişimler aşağıdaki gibidir. Hem BAC değerleri ve Alkol miktarlarına göre değişim gözükmektedir.                         
Şekil.5 Serialde sensorun alkole bağlı görünen değişimi

Serial porta görünen alkol ölçüm ekran görüntüleri ayrıca ekler kısmında gösterilmiştir
Projenin diğer bi amacı olan internet (WIFI) haberlesmesi ise ekler kısmındaki kodlardan gözükebilmektedir. Öncelikle ESP8266 nın bağlanacağı WIFI baglantısının ssid ve sifresini girdikten sonra sketch atıldıktan hemen sonra WIFI ağına bağlanıyor ve bize bi IP veriyor (172.20.10.2) verilen IPyi tarayıcıya 172.20.10.2/alkol olarak girdiğimizde ise tarayıcıd.Seri bağlantı dışında asıl amaç olan WIFI ile tarayıcıdan veri okuyabilmekti. Gerekli farklı ölçüm değerleri tarayıcıdan görüntülenebilmekte gerekli ekran görüntüleri ekler kısmında bulunmaktadır.

Değerleri ayrıca devreye bağladığımız OLED ekrandan dahi görünteleyebilmekteyiz. ESP8266 bir WIFI ağına bağlandıktan sonra ekran calısmaya başlıyor.Gerekli resimler ekler kısmında mevcuttur.



Sonuç
Bu projede yaptığımız ölçüm sonuçlarına baktığımızda gerçeğe tam anlamıyla yakın saptayamadığını farkettim fakat MQ-3 sensorunun yetenekleri sınırlı olduğundan gerçeğini göz önünde bulundurmak gerekir. Projede biraz daha maliyet açısından yukarlara çıkılabilseydi MQ303A sensörüyle genellikçe tam ve yerinde ölçümler yapılabilirdi. Ek olarak , Polislerin kullandığı alkol ölçüm cihazlarıyla eşdeğer olabilirdi. Ayrıca , projenin oluşturduğu farkındalık yadsınamaz bir gerçek alkolmetre kullanımı insanların daha sorumlu bir sürücü olmasını destekler nitelikte olabilir bu kullanıcıları alkolik yapmaz sorumluluk sahibi bir sürücü olmasını teşvik eder. Projenin gelişim ve kullanım alanları olarak da arabalara entegre edilen bu modül ile birlikte belli bir BAC (0.03) üzerinde olunca motoru çalıstırmayan bir düzeneklere insanların hayatına etki edip sürücü hatalı kazaların önüne geçebilir.






Ekler
Kodlar

#include <ESP8266WiFi.h>
#include <PubSubClient.h>
#include <SPI.h>
#include <Wire.h>
#include <ESP8266WiFi.h>
#include <ESP8266WebServer.h> ///kalsın
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>
#include <ESP8266HTTPClient.h>




#define SCREEN_WIDTH 128 // OLED display width, in pixels
#define SCREEN_HEIGHT 64
#define OLED_RESET -1
#define NUMFLAKES 10
#define XPOS 0
#define YPOS 1
#define DELTAY 2
#define LOGO16_GLCD_HEIGHT 16
#define LOGO16_GLCD_WIDTH  16
// Update these with values suitable for your network.



const char* ssid     = "Godgiven";
const char* password = "tanriverdi";
const char* mqtt_server = "172.20.10.3";

WiFiClient espClient;
PubSubClient client(espClient);
unsigned long lastMsg = 0;
#define MSG_BUFFER_SIZE  (50)
char msg[MSG_BUFFER_SIZE];


Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);

static const unsigned char PROGMEM logo16_glcd_bmp[] =
{ B00000000, B11000000,
  B00000001, B11000000,
  B00000001, B11000000,
  B00000011, B11100000,
  B11110011, B11100000,
  B11111110, B11111000,
  B01111110, B11111111,
  B00110011, B10011111,
  B00011111, B11111100,
  B00001101, B01110000,
  B00011011, B10100000,
  B00111111, B11100000,
  B00111111, B11110000,
  B01111100, B11110000,
  B01110000, B01110000,
  B00000000, B00110000
};

#if (SSD1306_LCDHEIGHT != 64)

#endif

const int AOUTpin = A0; //sensor baglandıgı pin
const int ledPin = 13;

int limit;
int deger;
float sensor_volt;
float RS_gas;
float R0;
float ratio;
float BAC;
int R2 = 2000;
String mesaj;


void alkololc() {
  Serial.println("bana request geldi.");
  
  deger = analogRead(AOUTpin);
 
  Serial.println("b1");
  if (deger < 300)
  {
    client.publish("alkol_oran", "Alkol Tespit Edilmedi!(250 den kucuk)");
    //server.send(200,"text/Plain","Alkol Tespit Edilmedi!(250 den kucuk)");
    Serial.println("bana request geldivfdvffvvfedfv.");
  }
  if (deger >= 300 && deger < 350)
  {
    client.publish("alkol_oran", "Alkol Tespit Edildi!(200-250)");
    //server.send(200,"text/plain","Alkol Tespit Edildi!(200-250)");
  }
  if (deger >= 350 && deger < 400)
  {
    client.publish("alkol_oran", "Sinirdan fazla alkol tespit edildi! Dikkat!(250-300)");     
    //server.send(200,"text/plain","Sinirdan fazla alkol tespit edildi! Dikkat!(250-300)");
  }
  if (deger >= 400 && deger < 450)
  {
     client.publish("alkol_oran", "Kardesim gel opucem!(300-400)");
    //server.send(200, "text/plain", "Kardesim gel opucem!(300-400)");
  }
  if (deger > 450)
  {
    client.publish("alkol_oran", "Sarhossun!(400 den buyuk)");
    //server.send(200, "text/plain", "Sarhossun!(400 den buyuk");
  }

}



void setup_wifi() {

  delay(10);
  // We start by connecting to a WiFi network
  Serial.println();
  Serial.print("Connecting to ");
  Serial.println(ssid);

  WiFi.mode(WIFI_STA);
  WiFi.begin(ssid, password);

  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  randomSeed(micros());

  Serial.println("");
  Serial.println("WiFi connected");
  Serial.println("IP address: ");
  Serial.println(WiFi.localIP());
}

void callback(char* topic, byte* payload, unsigned int length) {
  Serial.print("Message arrived [");
  Serial.print(topic);
  Serial.print("] ");
  for (int i = 0; i < length; i++) {
    Serial.print((char)payload[i]);
  }
  Serial.println();
  int hangisi = 0;
 /* if ((String)topic == "espLed")
  {
    if ((char)payload[0] == 'r')
      hangisi = kirmizi;
    else if ((char)payload[0] == 'g')
      hangisi = yesil;
    else if ((char)payload[0] == 'b')
      hangisi = mavi;
    Serial.println(payload[1]);

    if ((char)payload[1] == '1')
      digitalWrite(hangisi, HIGH);
    else
      digitalWrite(hangisi, LOW);
  }
*/



}

void reconnect() {
  // Loop until we're reconnected
  while (!client.connected()) {
    Serial.print("Attempting MQTT connection...");
    // Create a random client ID
    String clientId = "ESP8266Client-";
    clientId += String(random(0xffff), HEX);
    // Attempt to connect
    if (client.connect(clientId.c_str())) {
      Serial.println("connected");
      // Once connected, publish an announcement...
      client.publish("outTopic", "hello world");
      // ... and resubscribe
      client.subscribe("inTopic");
      client.subscribe("espLed");
    } else {
      Serial.print("failed, rc=");
      Serial.print(client.state());
      Serial.println(" try again in 5 seconds");
      // Wait 5 seconds before retrying
      delay(5000);
    }
  }
}

void setup() {
   
  Serial.begin(115200);
  setup_wifi();
  client.setServer(mqtt_server, 1883);
  client.setCallback(callback);


  display.begin(SSD1306_SWITCHCAPVCC, 0x3C);
  display.display();
  delay(2000);
  display.clearDisplay();

}

void loop() {

  if (!client.connected()) {
    reconnect();
  }
  client.loop();

  int sensorValue = analogRead(A0);
  sensor_volt = (float)sensorValue / 1024 * 5.0;
  RS_gas = ((5.0 * R2) / sensor_volt) - R2;
  R0 = 130000; // temiiz hava derecesi
  ratio = RS_gas / R0; // ratio = RS/R0
  double x = 0.4 * ratio;
  BAC = pow(x, -1.431); //BAC in mg/L (blood alchocol content / kandaki alkol miktarı)
  Serial.print("BAC = ");
  Serial.print(BAC * 0.0001); //cevrim to g/dL
  Serial.print(" g/DL\n\n");
  delay(1000);

  deger = analogRead(AOUTpin); //sensorun okunduğu AOUT pini
  Serial.println(deger);
  // lcd metin görüntüleme
  display.clearDisplay();
  display.setCursor(0, 0);
  display.setTextSize(2);
  display.setTextColor(WHITE);
  display.println(deger);

 alkololc();
  if (deger < 300)
  {
    Serial.println("Alkol tespit edilmedi.");
    display.println("Alkol Yok.");

  }
  if (deger >= 300 && deger < 350)
  {
    Serial.println("Alkol tespit edildi");
    display.println("Alkol Tespit Edildi.");

  }

  if (deger >= 350 && deger < 400)
  {
    Serial.println("Sinirdan fazla alkol tespit edildi! Dikkat! ");
    display.println("Cok icki! Dikkat");

  }
  if (deger >= 400 && deger < 450)
  {
    Serial.println("Kardesim gel opucem! :)");
    display.println("Icelim guzelleselim!");

  }
  if (deger > 450)
  {
    Serial.println("Sarhossun!");
    display.println("Sarhossun!");

  }
  display.display();
  delay (500);

 
  } 
Şekil.6 WIFI  bağlanma ve verdiği IP adresini görüntüleme

Yukardaki ekran görüntüsünde gözüken IP değerlerini tarayıcıya girince tarayıcıdan da değerleri görüntüleyebilmekteyiz.
 
Şekil.7 Alkolmetrenin hazır görüntüsü

  
Şekil.8 Tarayıcıda gözüken ölçüm değerleri
  Şekil.9 Tarayıcıda gözüken ölçüm değerleri


  
Şekil.10 OLED ekranında gözüken ölçüm değerleri

  
Şekil.11 OLED ekranında gözüken ölçüm değerleri


  
Şekil.12 Serial porta gözlemlenen ölçüm değerleri (BAC dahil)

<img width="191" alt="image" src="https://user-images.githubusercontent.com/76569487/191966038-1755f04c-05b9-43c2-ad4a-3213ceb609b0.png">

  
Şekil.13 Serial porta gözlemlenen ölçüm değerleri (BAC dahil)



Kaynakça

https://github.com/miguel5612/MQSensorsLib/blob/master/examples/MQ-3/MQ-3.ino

https://gelecegiyazanlar.turkcell.com.tr/konu/arduino#courses_list

https://en.wikipedia.org/wiki/Breathalyzer

https://www.mcgill.ca/oss/article/did-you-know/did-you-know-breathalyzers-dont-directly-measure-your-blood-alcohol-concentration

https://circuits4you.com/2016/12/16/esp8266-web-server-html/

https://electronics.howstuffworks.com/gadgets/automotive/breathalyzer.html

https://www.bactrack.com/blogs/expert-center/35457349-how-does-a-breathalyzer-work

https://theiotprojects.com/esp8266-data-logger-upload-data-on-webserver/

https://www.youtube.com/watch?v=VNgFbQAVboA&list=LL&index=3&t=1s

https://github.com/esp8266/Arduino/tree/master/libraries/ESP8266WebServer
