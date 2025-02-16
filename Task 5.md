# Colorimeter Overview

## Overview

This document provides an overview of a simple colorimeter based on the CH32V003F4U6 microcontroller. The colorimeter is used to measure the absorbance of a liquid sample by detecting the intensity of transmitted light through the sample. The device uses an LED with a wavelength of 615 nm as the light source. The led, potentiometer and the switch is connected seperately to get high intensity, however they can also be connected directly to RICS-V.

## Features

Uses a 615 nm LED as the light source.

Includes a potentiometer to adjust the LED intensity.

Light intensity is measured using a BH1750FVI light sensor.

Data is displayed on a 20x4 I2C LCD display.

Uses a push button to record intensity values.

Supports serial communication for real-time data logging.

## Pin Layout

**Function      --->       CH32V003F4U6 (RISC-V) Pin**

LED Control         --->     TO WIPER OF  POTENTIOMETER

Potentiometer           --->  TO SWITCH AND THE NEGETIVE TERMINAL OF BATTERY AND THE WIPER TO THE LED

Light Sensor (SDA)      ---> PC1

Light Sensor (SCL)     ---> PC2

Button Input           ---> PA1

LCD SDA                ---> PC1

LCD SCL                   --->PC2

## Code
```
#include "ch32v003fun.h"
#include "debug.h"
#include <Wire.h>
#include <BH1750FVI.h>
#include <LiquidCrystal_I2C.h>

// Measurement variables initialization
float I0 = 0.0;
float I = 0.0;
float A = 0.0;

// Create the LightSensor instance
BH1750FVI LightSensor(BH1750FVI::k_DevModeContLowRes);
LiquidCrystal_I2C lcd(0x27, 20, 4);




void setup()
{
    SystemInit();
    Delay_Init();
    Serial.begin(115200);
    LightSensor.begin();  
    lcd.init();
    lcd.backlight();

    // Set up GPIO for LEDs and button
    GPIO_InitTypeDef GPIO_InitStructure = {0};
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOB, ENABLE);

    GPIO_InitStructure.GPIO_Pin = RED_LED;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
    GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
    GPIO_Init(GPIOB, &GPIO_InitStructure);

    GPIO_ResetBits(GPIOB, RED_LED);

    // Initialize button pin
    RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA, ENABLE);
    GPIO_InitStructure.GPIO_Pin = BUTTON_PIN;
    GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IPU;
    GPIO_Init(GPIOA, &GPIO_InitStructure);
    
    delay(1000);
    lcd.print("Press for I0");
    
    while (GPIO_ReadInputDataBit(GPIOA, BUTTON_PIN) == 1) {
        delay(200);
    }
    
    I0 = LightSensor.GetLightIntensity();
    lcd.clear();
    lcd.print("I0: ");
    lcd.print(I0);
    delay(2000);
}

void loop()
{
    lcd.clear();
    delay(1000);
    I = LightSensor.GetLightIntensity();
    
    lcd.print("I: ");
    lcd.print(I);
    delay(1000);
    lcd.setCursor(1,1);
    lcd.print("Showing Abs");
    delay(1000);
    lcd.clear();

    if (I > 0) {
        A = log10(I0 / I);
        Serial.print(I0);
        Serial.print(",");
        Serial.print(I);
        Serial.print(",");
        Serial.println(A);

        lcd.print("Absorbance...");
        lcd.setCursor(1,1);
        lcd.print("Abs: ");
        lcd.print(A);
        delay(1000);
        lcd.clear();
        
        delay(5000);
        
    } else {
        lcd.clear();
        lcd.setCursor(0, 0);
        lcd.print("Error: I is 0");
        delay(1000);
    }
}
```
