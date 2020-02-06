# Tiny-Tim
Line following robot created for IGEN 230, September-November 2019
Created in C++ for an Arduino Uno controller


 //based on the last thing the sensors saw, make the motors go
  switch(LastSeen){
    case 0:
    //  delay(10);
      SharpStarboard();
      break;

