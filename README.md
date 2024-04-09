# ARYA COLLEGE OF ENGINEERING AND IT  (FINAL YEAR RESEARCH AND INNOVATION LAB PROJECT 2020-24)
# Project Title : Automated-Drug-Dispenser-Machine


#         SUBMITTED BY:  Subhash Jangid (20EAREC073)                      Submitted to : Dr. Aditya Kumar Singh Pundir (Incharge), Prof. (ECE Dept.)
#                        Pushpendra Singh  (20EAREC052)                   
#                        Manish Varma (20EAREC033)                        Guide name:    Er. Umesh Kumar Sharma, Associate Prof. (ECE Dept.)
#                        Rahul saini (20EAREC053)                         
#                        Hemant Bhargava (20EAREC020)                     Prof. (Dr.) Rahul Srivastava  HOD (ECE Department)
#                        Sidak Preet Singh (20EAREC067)


# Project Objective:
The Automated Drug Dispenser Machine (ADDM) project aims to revolutionize medication management by provide a convenient and efficient solution for dispensing prescribed medications. 
Utilizing advanced robotics and software technology, the ADDM automates the dispensing process, ensuring accuracy and reliability while minimizing the risk of human error.
Adherence to medication schedules can be challenging for many individuals, leading to suboptimal health outcomes and increased healthcare costs. The ADDM addresses this issue by offering a user-friendly interface that allows patients to easily access their medications according to their prescribed regimen.

# FEATURES OF PROJECT 
Reducing Human errors: Manual drug dispensing prone to mistakes in dosage, medication, and patient information.
Handling Inefficiencies: Time-consuming processes leading to delays in medication administration.
Increasing Safety concerns: Risk of medication mix-ups, wrong dosages, and adverse drug reactions.
Real time Inventory management issues: Difficulty in tracking medication stock levels, leading to stock outs or excess inventory.
Healthcare resource strain: Heavy reliance on personnel for dispensing tasks diverts attention from patient care.

# WORKING
# User Verification: 
  Patient will input there registered mobile number into the machine. After that an OTP will be send to Patient’s number for authentication.

# Drug Dispensing:  
  After verification the prescribed medicines will be retrieved from machine storage with the help of motors.

# User Interface: 
  Healthcare professionals access the machine's interface to review prescriptions, monitor inventory, and track dispensing activities.

# Patient Pickup: 
  Once dispensed, medications are securely stored until patients retrieve them, ensuring timely and efficient medication distribution.









# ARDUINO CODE 
#include <SoftwareSerial.h>
#include <Wire.h> 
#include <LiquidCrystal_I2C.h>
#include <Keypad.h>
#include<Servo.h>

Servo r;
Servo b;
Servo y;

LiquidCrystal_I2C lcd(0x27,20,4);  // set the LCD address to 0x27 for a 16 chars and 2 line display


const byte ROWS = 4; //four rows
const byte COLS = 4; //four columns
//define the cymbols on the buttons of the keypads
char hexaKeys[ROWS][COLS] = {
  {'1','2','3','4'},
  {'5','6','7','8'},
  {'A','B','C','D'},
  {'9','0','#','*'}
};
byte rowPins[ROWS] = {3,2,1,0}; //connect to the row pinouts of the keypad
byte colPins[COLS] = {7,6,5,4}; //connect to the column pinouts of the keypad


Keypad customKeypad = Keypad( makeKeymap(hexaKeys), rowPins, colPins, ROWS, COLS); 

int h=0;
int d=0;
String m="";
String m1="6376526140";
String m2="9784000993";
String m3="6377337242";
String m4="8769926234";
String m5="22222";  //test purpose only

SoftwareSerial mySerial(12,13);
void setup()
{
  mySerial.begin(9600);   // Setting the baud rate of GSM Module  
  Serial.begin(9600);
  delay(500);
  r.attach(9);
  b.attach(10);
  y.attach(11);
  delay(500);
  r.write(0);
  b.write(0);
  y.write(0);
  mySerial.setTimeout(1000);
  delay(500);
  mySerial.println("AT+CSQ");
  delay(500);
  while(Serial.available())
  {
  Serial.println(mySerial.readStringUntil('\n'));
  }
  delay(1000);
  lcd.init();
  Serial.println("hello");
  // initialize the lcd 
  lcd.backlight();
  lcd.begin(16, 2);
  lcd.print("Welecome!");
  delay(1000);
  
}
int flag=0;
String key="";
String k="";

void loop()
{ 
  int otp = generateOTP(6);
  if(flag==0)
  {   
     lcd.clear();
     delay(100);
     lcd.print("Welecome!"); 
     flag=1;
     delay(1000);
      lcd.clear();
     delay(100);
     Serial.println("Enter Mobile No.");
     lcd.print("Enter Mobile No."); 
     key=keypad_data_insert();
     Serial.println(key);
     lcd.setCursor(0,1);
     lcd.print(key);
      m=key;
      d=0;
      if(m==m1)
      {
      send_otp(m,otp); 
      delay(1000);
      while(mySerial.available())
      {
        Serial.println(mySerial.readStringUntil('\n'));
      }
        Serial.print("Otp has sent on your mobile Number = ");
        lcd.clear();
        delay(100);
        lcd.print("Otp has sent");
        Serial.println(m);
        delay(2000);
        Serial.println("Please  Enter OTP ");
        lcd.clear();
        delay(100);
        lcd.print("Enter OTP :-");
        k=keypad_data_insert();
        if(k.length()>3)
       {
        String x = String(otp);
        lcd.setCursor(0,1);
        lcd.print(k);
        if(k==x)
        {
          Serial.println("Person Verified ");
          Serial.println("Here You Can Collect Your Medicines");
          delay(500);
          lcd.clear();
          delay(1000);
          lcd.print("Person Verified");
          lcd.setCursor(0,1);
          lcd.print("Mr.Rahul");
          delay(2000);
          person_m1();
          lcd.clear();
          delay(500);
          lcd.print("Collect Your");
          lcd.setCursor(0,1);
          lcd.print("medicines");
          delay(5000);
          lcd.clear();
          delay(500);
          lcd.print("Thank You!");
          delay(2000);          
          flag=0; 
        }
        else
        {
          Serial.println("invalid OTP");
          Serial.println("Please Try Again");
          flag=0;
        }
       }
      }
      else if(m==m2)
      {
      send_otp(m,otp); 
      delay(1000);
      while(mySerial.available())
      {
        Serial.println(mySerial.readStringUntil('\n'));
      }
        Serial.print("Otp has sent on your mobile Number = ");
        lcd.clear();
        delay(100);
        lcd.print("Otp has sent");
        Serial.println(m);
        delay(2000);
        Serial.println("Please  Enter OTP ");
        lcd.clear();
        delay(100);
        lcd.print("Enter OTP :-");
        k=keypad_data_insert();
        if(k.length()>3)
       {
        String x = String(otp);
        lcd.setCursor(0,1);
        lcd.print(k);
        if(k==x)
        {
          Serial.println("Person Verified ");
          Serial.println("Here You Can Collect Your Medicines");
          delay(500);
          lcd.clear();
          delay(1000);
          lcd.print("Person Verified");
          lcd.setCursor(0,1);
          lcd.print("Mr.manish");
          delay(2000);
          person_m2();
          lcd.clear();
          delay(500);
          lcd.print("Collect Your");
          lcd.setCursor(0,1);
          lcd.print("medicines");
          delay(5000);
          lcd.clear();
          delay(500);
          lcd.print("Thank You!");
          delay(2000);          
          flag=0; 
        }
       }
      }
      else if(m==m3)
      {
      send_otp(m,otp); 
      delay(1000);
      while(mySerial.available())
      {
        Serial.println(mySerial.readStringUntil('\n'));
      }
        Serial.print("Otp has sent on your mobile Number = ");
        lcd.clear();
        delay(100);
        lcd.print("Otp has sent");
        Serial.println(m);
        delay(2000);
        Serial.println("Please  Enter OTP ");
        lcd.clear();
        delay(100);
        lcd.print("Enter OTP :-");
        k=keypad_data_insert();
        if(k.length()>3)
       {
        String x = String(otp);
        lcd.setCursor(0,1);
        lcd.print(k);
        if(k==x)
        {
          Serial.println("Person Verified ");
          Serial.println("Here You Can Collect Your Medicines");
          delay(500);
          lcd.clear();
          delay(1000);
          lcd.print("Person Verified");
          lcd.setCursor(0,1);
          lcd.print("Mr.Pushpit");
          delay(2000);
          person_m3();
          lcd.clear();
          delay(500);
          lcd.print("Collect Your");
          lcd.setCursor(0,1);
          lcd.print("medicines");
          delay(5000);
          lcd.clear();
          delay(500);
          lcd.print("Thank You!");
          delay(2000);          
          flag=0; 
        } 
        else
        {
          Serial.println("invalid OTP");
          Serial.println("Please Try Again");
          flag=0;
        }
       }
      }
      else if(m==m4)
      {
      send_otp(m,otp); 
      delay(1000);
      while(mySerial.available())
      {
        Serial.println(mySerial.readStringUntil('\n'));
      }
        Serial.print("Otp has sent on your mobile Number = ");
        lcd.clear();
        delay(100);
        lcd.print("Otp has sent");
        Serial.println(m);
        delay(2000);
        Serial.println("Please  Enter OTP ");
        lcd.clear();
        delay(100);
        lcd.print("Enter OTP :-");
        k=keypad_data_insert();
        if(k.length()>3)
       {
        String x = String(otp);
        lcd.setCursor(0,1);
        lcd.print(k);
        if(k==x)
        {
          Serial.println("Person Verified ");
          Serial.println("Here You Can Collect Your Medicines");
          delay(500);
          lcd.clear();
          delay(1000);
          lcd.print("Person Verified");
          lcd.setCursor(0,1);
          lcd.print("Mr.Subhash");
          delay(2000);
          person_m4();
          lcd.clear();
          delay(500);
          lcd.print("Collect Your");
          lcd.setCursor(0,1);
          lcd.print("medicines");
          delay(5000);
          lcd.clear();
          delay(500);
          lcd.print("Thank You!");
          delay(2000);          
          flag=0; 
        }
       }
      }
      else if(m==m5)
      {
      send_otp(m,otp); 
      delay(1000);
      while(mySerial.available())
      {
        Serial.println(mySerial.readStringUntil('\n'));
      }
        Serial.print("Otp has sent on your mobile Number = ");
        lcd.clear();
        delay(100);
        lcd.print("Otp has sent");
        Serial.println(m);
        delay(2000);
        Serial.println("Please  Enter OTP ");
        lcd.clear();
        delay(100);
        lcd.print("Enter OTP :-");
        k=keypad_data_insert();
        if(k.length()>3)
       {
        String x = String(otp);
        lcd.setCursor(0,1);
        lcd.print(k);
        if(k==x)
        {
          Serial.println("Person Verified ");
          Serial.println("Here You Can Collect Your Medicines");
          delay(500);
          lcd.clear();
          delay(1000);
          lcd.print("Person Verified");
          lcd.setCursor(0,1);
          lcd.print("test");
          delay(2000);
          person_m5();
          lcd.clear();
          delay(500);
          lcd.print("Collect Your");
          lcd.setCursor(0,1);
          lcd.print("medicines");
          delay(5000);
          lcd.clear();
          delay(500);
          lcd.print("Thank You!");
          delay(2000);          
          flag=0; 
        }
       }
      }
      else
      {
        Serial.println("invalid moblie number");
        lcd.clear();
        delay(100);
        lcd.print("invalid No.");
        delay(100);
        lcd.setCursor(0,1);
        lcd.print("try again");
        flag=0;
        delay(2000);
      }
    }
  }


int generateOTP(int length) {
  int otp = 1;
  for (int i = 0; i < length; i++) {
    otp = otp * 10 + random(1, 10); // Append a random digit to the OTP
  }
  int p =otp+10000;
  return abs(p);
}

void send_otp(String mobile_number,int otp)
{
   mySerial.println("AT+CMGF=1");    //Sets the GSM Module in Text Mode
     delay(1000);  // Delay of 1 second
    String t = "AT+CMGS=\"+91" + mobile_number + "\"\r";
     Serial.println(t);
     mySerial.println(t);
     delay(1000);
     mySerial.println(otp);// The SMS text you want to send
     Serial.print("OTP = ");
     Serial.println(otp);
     delay(100);
     mySerial.println((char)26);// ASCII code of CTRL+Z for saying the end of sms to  the module 
}

String keypad_data_insert()
{ 
  String v="";
  int l=0;
  while(l==0)
  {
  char customKey = customKeypad.getKey();
  if (customKey)
  {
//    Serial.print(customKey);
    if(customKey =='#')
    {
      break;
      l=1;
    }
      else if(customKey !='#')
    {
      v+=customKey;
      lcd.setCursor(0,1);
      lcd.print(v);
    }
  }
  }
return (v);  
}


void person_m1()
{
  r.write(90);
  lcd.clear();
  delay(100);
  lcd.print("R. Tablet takes");
  delay(100);
  lcd.setCursor(0,1);
  lcd.print("1-Times(morning)");
  delay(2000);
  b.write(90);
  lcd.clear();
  delay(100);
  lcd.print("G. Tablet takes");
  delay(100);
  lcd.setCursor(0,1);
  lcd.print("1-Times(noon)");
  delay(2000);
  delay(1000);
  r.write(0);
  b.write(0);
  delay(1000);  
}

void person_m2()
{
  y.write(90);
  lcd.clear();
  delay(100);
  lcd.print("Y. Tablet takes");
  delay(100);
  lcd.setCursor(0,1);
  lcd.print("1-Times(night)");
  delay(2000);
  b.write(90);
  lcd.clear();
  delay(100);
  lcd.print("G. Tablet takes");
  delay(100);
  lcd.setCursor(0,1);
  lcd.print("1-Times(noon)");
  delay(2000);
  delay(1000);
  y.write(0);
  b.write(0);
  delay(1000);  
}
void person_m3()
{
  r.write(90);
  lcd.clear();
  delay(100);
  lcd.print("R. Tablet takes");
  delay(100);
  lcd.setCursor(0,1);
  lcd.print("1-Times(morning)");
  delay(2000);
  y.write(90);
  lcd.clear();
  delay(100);
  lcd.print("Y. Tablet takes");
  delay(100);
  lcd.setCursor(0,1);
  lcd.print("1-Times(night)");
  delay(2000);
  delay(1000);
  r.write(0);
  y.write(0);
  delay(1000);  
}
void person_m4()
{
  r.write(90);
  lcd.clear();
  delay(100);
  lcd.print("R. Tablet takes");
  delay(100);
  lcd.setCursor(0,1);
  lcd.print("2-Times in a day");
  delay(2000);
  delay(1000);
  r.write(0);
  delay(1000);
  r.write(90);
  delay(1000);
  r.write(0);
  delay(1000);  
}
void person_m5()
{
  r.write(90);
  lcd.clear();
  delay(100);
  lcd.print("R. Tablet takes");
  delay(100);
  lcd.setCursor(0,1);
  lcd.print("1-Times(morning)");
  delay(2000);
  b.write(90);
    lcd.clear();
  delay(100);
  lcd.print("G. Tablet takes");
  delay(100);
  lcd.setCursor(0,1);
  lcd.print("1-Times(noon)");
  delay(2000);
  y.write(90);
  lcd.clear();
  delay(100);
  lcd.print("Y. Tablet takes");
  delay(100);
  lcd.setCursor(0,1);
  lcd.print("1-Times(night)");
  delay(3000);
  r.write(0);
  b.write(0);
  y.write(0);
  delay(1000);  
}
