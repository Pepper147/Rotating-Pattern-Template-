//Reed Jeandell: Rotating Patterns Tempelete: January 31 2018
//******************************************************************************************
//This File will can be used as a templete to easily add your own FastLED functions and to 
//have the entire process automated for you.  
//Simple create new function at the bottom of the code and add hem into the array gPatterns.
//
//
//                    Quick Variable Reference 
//  ARRAY_SIZE        = Used to determine the array size of any array that we will use 
//  *PatternList[]()  = An array of n pointer that point to functions
//  Patterns          = An array with 'n' elements.  This is where you add the custom functions 
//  pCount            = The index value that will be within the Patterns array.  This will tell the compiler which functions to run
//  Hue               = Fast and easy hue value set to 0 (RED).  This value will be incremented over time.  
//  
//*****************************************************************************************
#include <FastLED.h>
#include <LEDMatrix.h>
#define CHIPSET        WS2812B
#define LED_PIN        7
#define COLOR_ORDER    GRB

#define MATRIX_WIDTH   24                       // Set this negative if physical led 0 is opposite to where you want logical 0
#define MATRIX_HEIGHT  20                       // Set this negative if physical led 0 is opposite to where you want logical 0
#define NUM_LEDS MATRIX_WIDTH * MATRIX_HEIGHT

#define MATRIX_TYPE    HORIZONTAL_ZIGZAG_MATRIX  // See top of LEDMatrix.h for matrix wiring types

#define BRIGHTNESS 45
#define FRAMES_PER_SECOND 120
#define ARRAY_SIZE(A) (sizeof(A) / sizeof((A)[0]))  // Fast and easy to use definition that will allow us to easily determine the size of any array.

cLEDMatrix<MATRIX_WIDTH, MATRIX_HEIGHT, MATRIX_TYPE> leds;                                                                         // Simply replace the 'A' in ARRAY_SIZE with the array name.  Ex.  ARRAY_SIZE(myArray) 


void setup() {
  delay(3000);
  FastLED.addLeds<CHIPSET, LED_PIN, COLOR_ORDER>(leds[0], leds.Size());
  //Set_max_power_in_volts_and_milliamps(5, 12000);       
  //Safety procotion, stating that i am using 5V 12A power supply (Max)
  FastLED.setBrightness(BRIGHTNESS);
  Serial.begin(9600);
  Serial.println("The program has started"); 
  FastLED.clear(true);
  delay(500);
  FastLED.showColor(CRGB::Lime);
  delay(1000);
}
//*********************************GLOBAL VARIABLES GO HERE ****************************************

  typedef void (*PatternList[])();

  PatternList Patterns = {/*ShiftOneBox, ShiftTwoBox, SinCos,*/ Lines };  //ADD YOUR FUNCTIONS HERE!!
                                                            
  uint8_t pCount = 0;           
  uint8_t Hue = 0;
  uint8_t angle;
                   
//*************************************************************************************************
//PatternList is an array made of 'n' pointers to a function that return 'void'
//"Defines PatternList as a new type of array of pointers to functions".
//PatternsList will point to Patterns.  the pCount will determine what function element in the array to activate.
//
//PatternsList Patterns = {} creates Patterns as such an array and initializes it with 'n' elements (The amount of elements that are added to gPatterns)
//
//We initialize our gCurrentPatternNumber to 0 because we want to start at the begining.
//We initialize our gHue = 0 which will start at Red and will be incremented throughout the program.
//*************************************************************************************************



void loop() {
  Patterns[pCount]();                                     // Call the current pattern function once, updating the 'leds' array
  FastLED.show(); 
  FastLED.delay(1000/FRAMES_PER_SECOND);                                  // insert a delay to keep the framerate modest


  EVERY_N_MILLISECONDS( 20 ) { Hue++; }                                   // Slowly cycle the "base color" through the rainbow.  Used only on functions that call upon "gHue"
  EVERY_N_SECONDS( 50 ) { nextPattern(); }                                // Change patterns periodically.
  EVERY_N_SECONDS( 50 ) { Serial.println("The Pattern has changed"); }    // Used for trouble shooting.  Tells us that we are switching to a new pattern
}

void nextPattern()
{
  // add one to the current pattern number, and wrap around at the end
  pCount = (pCount + 1) % ARRAY_SIZE( Patterns);  
  Serial.println(pCount);     
// Changes the current Patern to the next linear pattern on the list. 
// Adds 1 to the current Pattern Count.  This value is the moduloed with the total aray size of Patterns.
// This ensures that we will wrap around back to index 0 when we reach the end of the array indexes.
// Ex.  Patterns[5].  pCount = 3  -----> nextPattern()
// pCount = (pCount +1) %ARRAY_SIZE(Patterns) = (3+1) % 5 = 4 -----> nextPattern
//                                            = (4+1) % 5 = 0
  
}


//*********************************NEW PATTERN FUNCTION GO HERE ****************************************
//******************************************************************************************************(1) SimpleRainbow function check Simple_Rainbow" file for more details
void fadeall(){                         //Quick function that will fade all of the LEDS 
  for(int i = 0; i < NUM_LEDS; i++)
  {
    leds(i).nscale8(220);    
  }
}
void ShiftOneBox(){
//**************************************************************************************
//Function that will divide the Matrix ito 4 section (4 quadrants).  
//This function will start will a filled rectangle in the top left corner 
//The rectangle will then slide to the bottom left, then to the bottom right, then to the top right finaly back to the top left.
//The process will repeat and continue.   
//
//NOTE: using the nscale8 value of anything between 200 and 222 for decent results 
//**************************************************************************************
  int x = 0; 
  int y = 0;
  int gDelay = 20;          //Local variable to control the delay times 
  int hue = sin8(angle);    

  for(y = 0; y <= leds.Height() ; y++)              //Quadrant 2 to quadrant 3
  {
  leds.DrawFilledRectangle(x, y, leds.Width() / 2, leds.Height() / 2, CHSV(hue, 255, 255));  
    fadeall();
    FastLED.delay(gDelay);
   // FastLED.clear();
  }
  for(x = 0; x < leds.Width(); x++)                 //Quadrant 3 to quadrant 4
  {
    leds.DrawFilledRectangle(x , y, leds.Width() / 2, leds.Height() / 2, CHSV(hue, 255, 255));
    fadeall();
    FastLED.delay(gDelay); 
   // FastLED.clear();
  }
  for(y = leds.Height(); y > 0; y--)                //Quadrant 4 to quadrant 1
  {
    leds.DrawFilledRectangle(x, y, leds.Width() / 2, leds.Height() / 2, CHSV(hue, 255, 255));
    fadeall();
    FastLED.delay(gDelay); 
   // FastLED.clear();
  }
  for(x = leds.Width(); x > 0; x--)                 //Quadrant 1 to quadrant 2
  {
    leds.DrawFilledRectangle(x, y, leds.Width() / 2, leds.Height() / 2, CHSV(hue, 255, 255));
    fadeall();
    FastLED.delay(gDelay);
  //  FastLED.clear();
  }
  hue+=12;
  angle+=4;
}
void ShiftTwoBox(){
//**************************************************************************************
//ShiftTwoBox is an adaptation of ShiftOneBow.  With this function i am adding an additional box that 
//will adjasently mirror the first box.  
//Example: The firt box will appear in the 2nd quadrant and the second box will apear at the 4th quadrant 
//To accomlish this task i simply added a 
//**************************************************************************************
  int x = 0; 
  int y = 0;
  int gDelay = 20;          //Local variable to control the delay times 
  int hue = sin8(angle);
  
  for(y = 0; y <= leds.Height() ; y++)             
 { 
    leds.DrawFilledRectangle(x, y, leds.Width() / 2, leds.Height() / 2, CHSV(hue, 255, 255));                               //Shift box from Quadrant 2 to quadrant 3 
    leds.DrawFilledRectangle(leds.Width(), leds.Height() - y, leds.Width() / 2, leds.Height() / 2, CHSV(hue, 255, 255));    //Shift box from Quadrant 4 to quadrant 1 
    fadeall();
    FastLED.delay(gDelay);
  }
  for(x = 0; x < leds.Width(); x++)                
  {
    leds.DrawFilledRectangle(x , y, leds.Width() / 2, leds.Height() / 2, CHSV(hue, 255, 255));                                            //Shift box from Quadrant 3 to quadrant 4 
    leds.DrawFilledRectangle(leds.Width() - x, leds.Height() - leds.Height(), leds.Width() / 2, leds.Height() / 2, CHSV(hue, 255, 255));  //Shift box from Quadrant 1 to quadrant 2 
    fadeall();
    FastLED.delay(gDelay); 
  }
  hue+=12;
  angle+=4;
}
void SinCos(){
//*******************************************************************************************
//A function that will split the matrix into two sections, Even Rows and Odd Rows.
//On the Even Rows the leds will be changing the hue value based off of the sin of the "angle" and will always be on
//On the  Odd rows the leds will be will be changing hues at the same rate of the Even rows, but, their Values will be changing 
//bassed off of the cos of the "angle".  
//This will give the effet that the odd rows of the matrix are fading in and out of the matrix.  Simulates a breathing effect.
//
//NOTE: Try using different color pallets on this animation to make it stand out from some of the other tuff i have been doing.   
//*******************************************************************************************
  FastLED.clear();
  uint8_t hue = sin8(angle);
  uint8_t value = cos8(angle);
  
  for (int16_t y=leds.Height()-1; y>=0; --y)
  {
    if( y % 2 == 0) { leds.DrawLine(leds.Width() - 1, y , leds.Width() - leds.Width(), y,  CHSV(hue, 255, 255)); hue +=16;}
    else            { leds.DrawLine(leds.Width() - leds.Width(), y, leds.Width() - 1, y,   CHSV(hue, 255, value));}
  }
  angle += 4;
  FastLED.show();
  delay(20);
}
void Lines(){
//*******************************************************************************************
//Working on some line animations in combination with the Mirror function.  
//What would like to accomplish is to have a single line(starting from the left) make its way to the center 
//Using the mirror function will make it apear as though the lines are coming together and meating in the middle.   
//*******************************************************************************************
  uint8_t hue = 192;
  static int count = 0;    //The value of count will be incremented every second 
  int delayTime = 50;     //Quick variable to control the delay time in each of the loops. 
  
  if(count >= 0)      {EVERY_N_SECONDS(1) count++;}
  //Every second we will add one to the current count value
  if(count >= 1 && count <=12)                                                          
  //Begin by Sliding vertical Columns towards and away from the center
  {
    for(int x = 0; x <= (leds.Width() / 2) -1; x++){
      leds.HorizontalMirror();
      leds.DrawLine(x, leds.Height() - leds.Height(), x, leds.Height() - 1, CHSV(hue, 255, 255));
     // leds.DrawLine(x-2, leds.Height() - leds.Height(), x-2, leds.Height() - 1, CRGB::Black);
      fadeall();
      hue+=10;
      FastLED.delay(delayTime);
    }
    for(int x = (leds.Width() / 2) - 1; x >= 0; x--){
      leds.HorizontalMirror();
      leds.DrawLine(x, leds.Height() - leds.Height(), x, leds.Height() - 1, CHSV(hue,255, 255));
      fadeall();
      hue+=10;
      FastLED.delay(delayTime);
    }
  }
  if(count >= 12 && count <= 24)                                                        
  //Once our count has reached ten, we will start sliding the horizontal lines towards and away from the center. 
  {
    for(int y = 0; y <= (leds.Height() / 2) - 1; y++){
      leds.VerticalMirror();
      leds.DrawLine(leds.Width() - leds.Width(), y, leds.Width() - 1, y, CHSV(hue, 255, 255));
      fadeall();
      hue+=12;
      FastLED.delay(delayTime);
    }
    for(int y = (leds.Height() / 2) -1; y >= 0; y--){
      leds.VerticalMirror();
      leds.DrawLine(leds.Width() - leds.Width(), y, leds.Width() - 1, y, CHSV(hue, 255, 255));
      fadeall();
      hue+=12;
      FastLED.delay(delayTime);
    }
  }
  if(count >= 24 && count <= 36){
  //Transition to the Rows and columns moving away from the Center 
    for(int x = (leds.Width() / 2) - 1; x >= 0; x--){
      leds.HorizontalMirror();
      leds.DrawLine(x, leds.Height() - leds.Height(), x, leds.Height() - 1, CHSV(hue,255, 255));
      fadeall();
      hue+=10;
      FastLED.delay(delayTime);
    } 
    for(int y = (leds.Height() / 2) -1; y >= 0; y--){
      leds.VerticalMirror();
      leds.DrawLine(leds.Width() - leds.Width(), y, leds.Width() - 1, y, CHSV(hue, 255, 255));
      fadeall();
      hue+=10;
      FastLED.delay(delayTime);
      }
  }
  if(count >= 36 && count <= 48)
  //Transition to the columns sliding away from the center and the Rows moving towards the center.                                       
  {
    for(int x = (leds.Width() / 2) - 1; x >= 0; x--){
      leds.HorizontalMirror();
      leds.DrawLine(x, leds.Height() - leds.Height(), x, leds.Height() - 1, CHSV(hue,255, 255));
      fadeall();
      hue+=14;
      FastLED.delay(delayTime);
    } 
    for(int y = 0; y <= (leds.Height() / 2) - 1; y++){
      leds.VerticalMirror();
      leds.DrawLine(leds.Width() - leds.Width(), y, leds.Width() - 1, y, CHSV(hue, 255, 255));
      fadeall();
      hue+=14;
      FastLED.delay(delayTime);
    }
  }
  if(count >= 48)   {count = 0;}
  //Finish the function by reseting the count value back to 0 so that the function can begin again. 
  Serial.println(count);
  //Used to measure count
}
