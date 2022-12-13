# anti_tamper_stm32i486rg_applications

Project Goal:

  • Application of monitoring anti-tamper reflective sensor  

Interface for Project:

  • C / C++

Installing Prerequisites
  
  • STM32CubeIDE – install from here
  
  •  Git : Get the latest git from here
 
 Hardware: 
 
  • STM32L486xx: Reference link from here
 
  • KazuardBoard : Custom board
   
  • Core: Arm® 32-bit Cortex®-M4 CPU with FPU1. 
    
  • Power supply: 71 V to 3.6 V
  
  • VDD: 1.71V to 1.9V 

Commponnent:
    
  • STM32L486xx - U1

  • Reflective Sensor (QRE1113)  top side – U34

  • Switch Sensor buttom side – SW2 

  • Switch Sensor top side – SW1

  • Rtc Secure-input – U15

  • Rtc Secure-output – U20

  • Red Led – LED for AT top side..

Commuincation:

  • UART protocal 

  • GPIO protocal 

  • IC2 protocal 

  • SPI protocal 
    
**Electrical diagram:**

      		![image](https://user-images.githubusercontent.com/66781442/207282876-06437a75-6c3c-406b-b0d1-2d2dc99c1788.png)
      
**BLOCK DIAGRAM:**

      		![image](https://user-images.githubusercontent.com/66781442/207283037-4d896c61-894d-4b01-9952-bd1169d6322e.png)
      
**Use cases**
**STM32L486xx memory map and peripheral register boundary addresses:**
    
      		![image](https://user-images.githubusercontent.com/66781442/207283564-97990f3e-6f13-45f1-97a4-ec68baa4f6c6.png)
      
      
**Anti-Tamper Sensor:**

  link: https://www.onsemi.com/pdf/datasheet/qre1113-d.pdf

  • The regular state for Reflective Sensor – sleep! 

  • The Reflective Sensor should constantly check (1s) the current  distance from the cover 

  **The system should have two events:**

  • system stableed 

  • tamper event

  **Reflective Sensor states:**

      • SENSOR_INITIALIZED

      • SENSOR_TAMPER_DETECTED

      • SENSOR_TAMPER_NOT_DETECTED

      • SENSOR_ERROR

  **if**  
        The Reflective Sensor dosen’t fined an tamper event it back to SENSOR_INITIALIZED 	state 
  **else if**
        The Reflective Sensor fined an tamper event:

  **Event on Reflective Sensor/Switch Sensor top side:**

  **SENSOR_TAMPER_DETECTED!**

      1. Red Led switch blink

      2. Erase the encryption keys  - Rtc Secure-input 

      3. Erase the encryption keys  -   Rtc Secure-output

      4. optenal for other location?

  **Event on Reflective Switch Sensor buttom side:**

  **SENSOR_TAMPER_DETECTED!** 

      1. Red Led switch blink

      2.  Erase encryption keys  - Rtc Secure-input 

      3. Erase encryption keys  -   Rtc Secure-output

      4. optenal for other location	
      
  **The system will try connect to the RTC (secureinput / secureoutput):**
      
      • the system try to connection for thr RTC in the power on:
      
      if it’s not connected  - state SENSOR_ERROR
      
      else  - SENSOR_TAMPER_NOT_DETECTED 
      
      
**Red Led:**

**Led states:**
  
    • LED_INITIALIZED_READY
    
    • LED_EVENT_BATTERY
    
    • LED_ERROR
	
  state for red led on (got power from main battery and coin) – 	  LED_INITIALIZED_READY
	
  state for red led off (main and cion battery off) –             	LED_EVENT_BATTERY
	   
  SENSOR_TAMPER_DETECTED!
        
  1. Erase the encryption keys  - Rtc Secure-input 
        
  2. Erase the encryption keys  -   Rtc Secure-output

**optenal for other location?**

      		![image](https://user-images.githubusercontent.com/66781442/207285369-087d4650-2bd6-4982-9536-7208ac08774a.png)

**LOG:**

  • app log print - prints all logs

  • app save log  - save logs in flash

  • app log clear - clears log
    
    		![image](https://user-images.githubusercontent.com/66781442/207285586-ac053b44-bd25-4a17-ad43-5d69a9e160e6.png)

    
**PWM:**

**SW1 / SW2 event**
   
   HAL_GPIO_EXTI_Callback(uint16_t GPIO_Pin)
   
   SW1 – GPIO PIN 2
   
   SW2 – GPIO PIN 8
   
**UART Console:**
  
  UART3 – PB10/PB11
  
  app fpr Cli Container
  
  Open UART console from Linux PC to the device:
  
  Picocom -b 115200 /dev/ttyUSB0

    		![image](https://user-images.githubusercontent.com/66781442/207285920-249fcc2b-d176-4504-a50b-b3f6dfb2ad7b.png)
    
    
**Reflective Sensor test**

  app set distance (Threshold)

  app get distance (TAMPER_DETECTED / TAMPER_NOT_DETECTED)

**LED test**

  
  gpio conf GPIOC PIN 7 out
  
  gpio set GPIOC PIN 7 < value 0 or 1> # in this case, LED is inversed logic, turn "on" on 0
  
  gpio conf GPIOC PIN 7 out gpio set GPIOC PIN 7 0 gpio set GPIOC PIN 7 1

**RTC test**

  app rtc set time 

  app rtc get time

**FLASH test:**

  app write to flash 

  app read flash data
  
  app clear flash data 


**How to use cube in order to set clock rates:**
  
  **open new project:**
    
    		![image](https://user-images.githubusercontent.com/66781442/207286356-af53610e-77eb-441e-8fc1-64cb93efd246.png)
    
    		![image](https://user-images.githubusercontent.com/66781442/207286435-27cbc6f3-7ef9-48c5-9cc3-902eee0ecba0.png)
    
  **Set internal clock 80 Mhz max :**
  
    		![image](https://user-images.githubusercontent.com/66781442/207286679-66711de9-801b-434b-b12e-5ef0cbf9ef06.png)





