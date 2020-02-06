# Tiny-Tim
Line following robot created for IGEN 230, September-November 2019
Created in C++ for an Arduino Uno controller

const int aninPin1=A1;
const int aninPin2=A2;
const int aninPin3=A3;
const int aninPin4=A4;
const int aninPin5=A5;

int sensorVal=0;
int outputVal=0;
float read1;
float read2;
float read3;
float read4;
float read5;
const int threshold=0.12;
int LastSeen;
int a;

int in1=6;
int in2=5;
int in3=2;
int in4=3;
int ConP=8;
int ConS=9;

int high=1;
int low=0;
int Portslow=255; //Motor A is port side
int Portfast=255;
int Starslow=155; //Motor B is starboard side
int Starfast=155;

int SensArray[5];


void setup(){
  pinMode(in1, OUTPUT);
  pinMode(in2, OUTPUT);
  pinMode(in3, OUTPUT);
  pinMode(in4, OUTPUT);
  pinMode(ConP, OUTPUT);
  pinMode(ConS, OUTPUT);

  pinMode(aninPin1, INPUT);
  pinMode(aninPin2, INPUT);
  pinMode(aninPin3, INPUT);
  pinMode(aninPin4, INPUT);
  pinMode(aninPin5, INPUT);

  Serial.begin(9600);
}

//things the motors can do
void PortForwardFAST(){
  digitalWrite(in1, high);
  digitalWrite(in2, low);
  analogWrite(ConP, Portfast);
}

void PortForwardSLOW(){
  digitalWrite(in1, high);
  digitalWrite(in2, low);
  analogWrite(ConP, Portslow);
}

void PortBackwardFAST(){
  digitalWrite(in1, low);
  digitalWrite(in2, high);
  analogWrite(ConP, Portfast);
}

void PortBackwardSLOW(){
  digitalWrite(in1, low);
  digitalWrite(in2, high);
  analogWrite(ConP, Portslow);
}

void PortOFF(){
  digitalWrite(in1, low);
  digitalWrite(in2, low);
}

void StarForwardFAST(){
  digitalWrite(in3, high);
  digitalWrite(in4, low);
  analogWrite(ConS, Starfast);
}

void StarForwardSLOW(){
  digitalWrite(in3, high);
  digitalWrite(in4, low);
  analogWrite(ConS, Starslow);
}

void StarBackwardFAST(){
  digitalWrite(in3, low);
  digitalWrite(in4, high);
  analogWrite(ConS, Starfast);
}

void StarBackwardSLOW(){
  digitalWrite(in3, low);
  digitalWrite(in4, high);
  analogWrite(ConS, Starslow);
}

void StarOFF(){
  digitalWrite(in3, low);
  digitalWrite(in4, low);
}

//ways the robot can move
void CurveStarboard(){
  PortForwardFAST();
  StarForwardSLOW();
}

void SharpStarboard(){
  PortForwardSLOW();
  StarOFF();
}

void CurvePort(){
  StarForwardFAST();
  PortForwardSLOW();
}

void SharpPort(){
  StarForwardSLOW();
  PortOFF();
}

void Forward(){
  PortForwardSLOW();
  StarForwardSLOW();
}

void Backward(){
  PortBackwardSLOW();
  StarBackwardSLOW();
}

void BackwardtoPort(){
  PortBackwardFAST();
  StarOFF();
}

void BackwardtoStarboard(){
  StarBackwardFAST();
  PortOFF();
}

void Stop(){
  PortOFF();
  StarOFF();
}

void loop(){

  //get proximity from each sensor
  SensArray[0]=analogRead(aninPin1);
//  float proximity1V=(float)proximity1*5/1023.0;
  SensArray[1]=analogRead(aninPin2);
//  float proximity2V=(float)proximity2*5/1023.0;
  SensArray[2]=analogRead(aninPin3);
//  float proximity3V=(float)proximity3*5/1023.0;
  SensArray[3]=analogRead(aninPin4);
//  float proximity4V=(float)proximity4*5/1023.0;
  SensArray[4]=analogRead(aninPin5);
//  float proximity5V=(float)proximity5*5/1023.0;


//determine what the last thing the sensors read was
if(SensArray[1]>20){
  LastSeen=1;
}

if(SensArray[3]>20){
  LastSeen=3;
}

if(SensArray[2]>800){
  LastSeen=2;
//  SensArray[2]=40;
}
if(SensArray[2]<800){
  SensArray[2]=0;
}

if(SensArray[0]>20){
  LastSeen=0;
}

if(SensArray[4]>20){
  LastSeen=4;
}

  SensArray[LastSeen]=40;

//  Serial.println(SensArray[3]);
  //CONSIDER not letting anything else read until we get back to middle sensor high, test first to see if it works without

//  for(int i = 0; i < 5; i++){
//  Serial.println(SensArray[i]);
//  }                             //For testing purposes



     //based on the last thing the sensors saw, make the motors go
  switch(LastSeen){
    case 0:
    // delay(10);
      SharpStarboard();
      break;
    
    case 1:
    //  delay(10);
      SharpStarboard();
      break;

    case 2:
    //  delay(10);
      Forward();
      break;

    case 3:
    //  delay(10);
      SharpPort();
      break;

    case 4:
    //  delay(10);
      SharpPort();
      break;

    default:
  //    delay(10);
      Stop();
    }

}
