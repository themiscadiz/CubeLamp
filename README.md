# CubeLamp
Arduino Code for Intangible Interaction
-------

<code> #include <Adafruit_NeoPixel.h>
#ifdef __AVR__
#include <avr/power.h> 
#endif

#define NEOPIXELPIN_1 5 
#define NEOPIXELPIN_2 6 
#define NEOPIXELPIN_3 7 

#define NUMPIXELS 16 

Adafruit_NeoPixel TABLE_pixels_1(NUMPIXELS, NEOPIXELPIN_1, NEO_GRB + NEO_KHZ800);
Adafruit_NeoPixel TABLE_pixels_2(NUMPIXELS, NEOPIXELPIN_2, NEO_GRB + NEO_KHZ800);
Adafruit_NeoPixel CUBE_pixels(NUMPIXELS, NEOPIXELPIN_3, NEO_GRB + NEO_KHZ800);

#define DELAYVAL 50 // Time (in milliseconds) to pause between pixels

const int hall_Sensor_1 = A0;
const int hall_Sensor_2 = A1;
const int hall_Sensor_3 = A2;


int red = 0;
int green = 0;
int blue = 0;
int sum = 0;

int threshold = 500;


// the setup routine runs once when you press reset:
void setup() {

  #if defined(__AVR_ATtiny85__) && (F_CPU == 16000000)
    clock_prescale_set(clock_div_1);
  #endif

  pinMode(hall_Sensor_1,INPUT);    //Pin 2 is connected to the output of proximity sensor
  pinMode(hall_Sensor_2,INPUT);    //Pin 2 is connected to the output of proximity sensor
  pinMode(hall_Sensor_3,INPUT);    //Pin 2 is connected to the output of proximity sensor

  // START WITH NEOPIXELS IN TABLE (THERE ARE 2)
  TABLE_pixels_1.begin();
  TABLE_pixels_2.begin();
  CUBE_pixels.begin();
  
  // initialize serial communication at 9600 bits per second:
  Serial.begin(9600);
}

// the loop routine runs over and over again forever:
void loop() {
  TABLE_pixels_1.clear();
  TABLE_pixels_2.clear();
  CUBE_pixels.clear();

  // read the input on analog pin 0:
  int sensorValue_1 = analogRead(hall_Sensor_1);
  int sensorValue_2 = analogRead(hall_Sensor_2);
  int sensorValue_3 = analogRead(hall_Sensor_3);


   // hall sensor 1 = red
  if(sensorValue_1 < threshold){      //Check the sensor output
    
    red = red + 1;  
  }
  else{
    //red = 0;  
  }
  if(red > 255){
    red = 0;
  }
  
   // hall sensor 2 = green
  if(sensorValue_2 < threshold){      //Check the sensor output

    green = green + 1;  
  }
  else{
    //red = 0;  
  }
  if(green > 255){
    green = 0;
  }

  // hall sensor 3 = blue
  if(sensorValue_3 < threshold){      //Check the sensor output

    blue = blue + 1;  
  }
  else{
    //red = 0;  
  }
  if(blue > 255){
    blue = 0;
  }

  sum = (red+blue+green)/3;
  

  for(int i=0; i < NUMPIXELS; i++) { 
    TABLE_pixels_1.setPixelColor(i, TABLE_pixels_1.Color(sum, sum,sum));
    TABLE_pixels_2.setPixelColor(i, TABLE_pixels_2.Color(sum, sum, sum));
    CUBE_pixels.setPixelColor(i, CUBE_pixels.Color(red, green, blue));
  }

  TABLE_pixels_1.show();
  TABLE_pixels_2.show();
  CUBE_pixels.show();
  

  // print out the value you read:
  Serial.println(blue);
  delay(DELAYVAL);        // delay in between reads for stability
}</code>
