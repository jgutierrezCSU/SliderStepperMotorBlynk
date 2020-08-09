
#define BLYNK_PRINT Serial

// for wifi connections

#include <ESP8266WiFi.h>
#include <BlynkSimpleEsp8266.h>

  // Your WiFi credentials
char ssid[] = "Ermahgerd, Wi-Fi!";
char pass[] = "basicspider402";
 
  
// You should get Auth Token in the Blynk App.
// Go to the Project Settings (nut icon).
char auth[] = "swxEu6TLtrgDKjJZrGAGfu5pyohF9s1k";

// Declare Pins

int Pin8 = 8;
int Pin9 = 9;


// Set measurments

int lengthOfSlider = 1050; //Travel length in mm
double traveledTime; //Used for calculations in the loop section.
double Steps = 50; //Steps required to move along the entire Rail.400 * (lengthOfSlider / (3.14 * PulleyDiameter))

double StepDelay;
int StepsTaken = 0;

// by default selected speed is 0
int selectSpeed = 0;

const int stepPin = 4; //Pin D2 -- defines the step pin
const int dirPin = 5; //Pin D1 -- defines the direction pin

bool isInInterval= false;


void setup()
{
  // Debug console
  Serial.begin(9600);
  Blynk.begin(auth, ssid, pass);


  pinMode(stepPin, OUTPUT);
  pinMode(dirPin, OUTPUT);
  Serial.println("");
  Serial.print("Steps = ");
  Serial.println(Steps);
  digitalWrite(dirPin, HIGH);  //Defines the direction of travel. LOW reverses it.

}



void MoveStepper()
{
  digitalWrite(stepPin, HIGH); //This moves the stepper by 1 step.
  //maybe delay half?
  delayMicroseconds(400); //Depends on Motor 500-1000 is good
  digitalWrite(stepPin, LOW);
  delayMicroseconds(400);
  StepsTaken++;
  // after moving stepper, check if it has reached the end
   Serial.println(StepsTaken);
   Serial.println(Steps);
  if (StepsTaken >= Steps)
  {
    // if stepper has reached end , set speed 0 and Not in interval anymore
    selectSpeed=0; 
    isInInterval == false;   
    // reset step taken
    StepsTaken = 0;
  }
}

BLYNK_WRITE(V1) {
  if(isInInterval == false) {
    switch (param.asInt())
    {
      
      case 1: // 10sec
        Serial.println("10sec");
        selectSpeed=10;
        isInInterval == true;
        break;
      case 2: // 20sec
        Serial.println("20sec");
        selectSpeed=20;
        isInInterval == true;
        break;
      case 3: // 30sec
        Serial.println("30sec");
        selectSpeed=30;
        isInInterval == true;
        break;
      case 4: // 60sec
        Serial.println("60sec");
        selectSpeed=60;
        isInInterval == true;
        break;        
      case 5: // 2 mins
        Serial.println("2 mins");
        selectSpeed=120;
        isInInterval == true;
        break;
      case 6: // 5 mins
        Serial.println("5 mins");
        selectSpeed=300;
        isInInterval == true;
        break;
      case 7: // 20 mins
        Serial.println("20 mins");
        selectSpeed=1200;
        isInInterval == true;
        break;
      case 8: // 30 mins
        Serial.println("30 mins");
        selectSpeed=1800;
        isInInterval == true;
        break;
           
      default:
        Serial.println("Unknown item selected");
    }
  } 
}



void loop()
{
  Blynk.run();
//     Serial.print("StepsTaken");
//     Serial.println(StepsTaken);
//     Serial.print("selectSpeed");
//     Serial.println(selectSpeed);
//     Serial.print("Steps");
//     Serial.println(Steps);
   if (StepsTaken < Steps && selectSpeed != 0)
    { Serial.print("Mode = ");
      Serial.println(selectSpeed);
      traveledTime = selectSpeed; //The slider will move the distance in x seconds.enter 5
      StepDelay = ((traveledTime / Steps) - 0.0008)*1000; //This calculates the delay required in ms between steps to make the journey in the desired amount of time. 0.001 Seconds are deducted because the MoveStepper Method includes 2x 500microseconds delays.
      MoveStepper();
      Serial.print("Stepdelay = ");
      Serial.println(StepDelay);
      delay (StepDelay);
      // THis is in miliseconds (ms) Not MICRO!

    }
}
