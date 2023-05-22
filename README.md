# Dansari
Alex Róbertsson, Dagur Hafsteinsson &amp; Eva Rut Tryggvadóttir

Í verkefninu á að byggja vélmenni og kóða hreyfingu fyrir það. Þemað er Halloween. Okkur datt í hug að stríða aðeins vini okkar, sem er í öðrum hóp, með vélmenninu.  Vélmennið á sem sagt að byrja á því að dansa þegar þú kemur inn á ákveðin radíus sem og kveikja á LED perum í augunum. Dansinn er í raun bara búkurinn að hreyfast aðeins til hliðana og hendurnar dragast með. Einnig byrjar hljóð að spilast sem hvetur einstaklinginn til að koma nær.  Þegar persónan er komin í færi er skynjari sem kveikir á seinni kóðanum. Þá byrjar vélmennið að snúast hratt í hringi sem gerir það að verkum að hendurnar skjótast út og slær fórnarlambið.  
Loka niðurstaðan var hins vegar ekki sú sem við vonuðumst eftir. Við lentum í mörgum hnökrum en loka hnökrinn sem varð verkefninu að falli var víraflækja í höfuðkúpunni. 

<img src="https://github.com/TskoliV5/V23_S2_Dansari/blob/main/dansari.jpeg
" width="50%" height="50%">

---

### Kóði fyrir búk

```C++
const int HRADI = 5;
const int STEFNA_A = 2;
const int STEFNA_B = 4;
const int ECHO = 8; const int TRIG = 3; int distance = 0;
int fjarlaegd(); // fall sem sér um fjarlægðamælinguna, skilar fjarlægð í cm.

void setup() {

Serial.begin(9600); 
pinMode(TRIG,OUTPUT);
pinMode(ECHO,INPUT);
pinMode(HRADI, OUTPUT);
pinMode(STEFNA_A, OUTPUT);
pinMode(STEFNA_B, OUTPUT);
}

void loop () {

distance = fjarlaegd();           // fáum fjarlægðamælingu í cm.
Serial.print("Fjarlaegd: ");     // til að sjá mælingu í skjánum
Serial.println(fjarlaegd());     // til að sjá mælingu í skjánum
// // fjarlægð getur ekki verið mínustala // if(distance < 0 { // distance = 0; // ;}

// // ef fjarlægð í hlut er minna en 50 cm, má ekki vera neikvæð tala. // if(distance < 50 && distance != 0) {
// // setja sýninguna af stað // }

if(distance > 10 && distance < 50) { haegri(255) ;}

else { haegri(150); delay(2000); vinstri(150); delay(2000); } } }

void stoppa() { digitalWrite(STEFNA_A, LOW); digitalWrite(STEFNA_B, LOW); analogWrite(HRADI, 0); }

void haegri(int hradi) { digitalWrite(STEFNA_A, HIGH); digitalWrite(STEFNA_B, LOW); analogWrite(HRADI, hradi); }

void vinstri(int hradi) { digitalWrite(STEFNA_A, LOW); digitalWrite(STEFNA_B, HIGH); analogWrite(HRADI, hradi); }

int fjarlaegd() {

// sendir út púlsa
digitalWrite(TRIG,LOW);
delayMicroseconds(2); // of stutt delay til að skipta máli
digitalWrite(TRIG,HIGH);
delayMicroseconds(10); // of stutt delay til að skipta máli
digitalWrite(TRIG,LOW);

// mælt hvað púlsarnir voru lengi að berast til baka
return (int)pulseIn(ECHO,HIGH)/58.0; // deilt með 58 til að breyta í cm
}
```

### Kóði fyrir haus

```C++
#include "tdelay.h" #include <Servo.h> // Sambærilegt og import í python #include "Arduino.h" #include "SoftwareSerial.h" #include "DFRobotDFPlayerMini.h"

SoftwareSerial mySoftwareSerial(10, 11); // RX, TX
DFRobotDFPlayerMini myDFPlayer; void playSound(); // útfært neðar TDelay spilun(40000);

Servo motor; // bý til tilvik af Servo klasanum int motor_pinni = 9; // pinninn sem ég nota til að stjórna mótornum int led = 12;

// svipað og listi í python, geymir stefnurnar sem mótorinn á að fara í og í hvaða röð int motor_stefnur[] = {90, 45, 0, 45}; int motor_stefnu_fjoldi = 4; // breytan geymir hversu margar stefnur eru í listanum int motor_stefnu_teljari = 0; // breytan heldur utan um í hvaða stefnu mótorinn á að benda

TDelay motor_bid(500);

const int ECHO = 2; const int TRIG = 3; int distance = 0;
// int fjarlaegd(); // fall sem sér um fjarlægðamælinguna, skilar fjarlægð í cm.

void setup() {

motor.attach(9);  // attaches the servo on pin 9 to the servo object
Serial.begin(9600); 
pinMode(TRIG,OUTPUT);
pinMode(ECHO,INPUT);
motor.attach(motor_pinni); // segi servo tilvikinu hvaða pinna á að nota
motor.write(motor_stefnur[motor_stefnu_teljari]);
 mySoftwareSerial.begin(9600);  
 digitalWrite(led, HIGH);   
 delay(1000);               
 digitalWrite(led, LOW);    
 delay(1000);     
  pinMode(led, OUTPUT);              
// Use softwareSerial to communicate with mp3.

if (!myDFPlayer.begin(mySoftwareSerial)) {
while(true); } myDFPlayer.volume(30);

pinMode(led, OUTPUT); }

void loop () {

digitalWrite(led, HIGH);
// distance = fjarlaegd(); // fáum fjarlægðamælingu í cm. // Serial.print("Fjarlaegd: "); // til að sjá mælingu í skjánum // Serial.println(fjarlaegd()); // til að sjá mælingu í skjánum

// fjarlægð getur ekki verið mínustala
// if(distance < 0) { // distance = 0; // } // if(distance > 70 && distance < 150) { // if(motor_bid.timiLidinn()) { // uppfæra stefnu_teljara breytuna, modulus notað til að talan verði // aldrei hærri en fjöldi stefnanna sem eru í listanum motor_stefnu_teljari = (motor_stefnu_teljari + 1) % motor_stefnu_fjoldi; // veljum svo rétta stefnu úr listanum motor.write(motor_stefnur[motor_stefnu_teljari]);

//} playSound(); }

// if(distance > 160 || distance < 60) { // myDFPlayer.pause(); // motor.write(90); // delay(100);

// }

void playSound() { if (spilun.timiLidinn() == true) { myDFPlayer.play(1); // Play mp3 } }

int fjarlaegd() {

// sendir út púlsa
digitalWrite(TRIG,LOW);
delayMicroseconds(2); // of stutt delay til að skipta máli
digitalWrite(TRIG,HIGH);
delayMicroseconds(10); // of stutt delay til að skipta máli
digitalWrite(TRIG,LOW);

// mælt hvað púlsarnir voru lengi að berast til baka
return (int)pulseIn(ECHO,HIGH)/58.0; // deilt með 58 til að breyta í cm
}

Footer

```

