---
title: SkyWalker系列 | 2.0 旧项重提
date: 2024-12-4 00:00:00 +0800
categories: [SkyWalker]
tags: [硬件, STM32]
math: true
image:
   path: assets/SkyWalker%202.0/wallhaven-rr81e7.png
   alt: 封面图
---

## 旧项重提

本来一款初中时制作的小车应该很快随时间推移被忘却，结果好巧不巧赶上大一刚开学，学院科协举办四驱车比赛，来自4年前的记忆又被找回了。😅
原版SkyWalker太过陈旧而且已经被拆的体无完肤，我决定升级2.0，来应对比赛。
而事实上比赛时间很紧，初赛要求交PPT展示和答辩，这意味着我要在1周内画板并制作原型机。🫠
时间实在不够，所以上不了什么技术，接下来一起看看2.0又什么功能吧。😗

## 技术细节

1. *stm32f103c6t6a*主芯片。

![ ](assets/SkyWalker%202.0/屏幕截图%202024-12-07%20022207.png)

1. 自带*CH340N*串口转换，一键烧录程序。~~复位电路画错了，需要手捏BOOT0按键再插电源才能进入烧录状态。~~

![ ](assets/SkyWalker%202.0/屏幕截图%202024-12-07%20022236.png)

3. *L9110S*电机驱动芯片。
4. *MPU6050*陀螺仪。

![ ](assets/SkyWalker%202.0/屏幕截图%202024-12-07%20022245.png)

5. *HC-06*蓝牙串口。

![ ](assets/SkyWalker%202.0/屏幕截图%202024-12-07%20022251.png)

短时间也想不出什么创新点，就加了个陀螺仪，检测Z轴正负可以识别小车正反，再软件校正；
这样操控手就无需关注小车正反，按前进依然是前进。
为了花里胡哨，还整了两个大灯和一个*0.96OLED*显示器。

虽然结构简单没有技术含量，但是放在大一的小比赛里也足够看了，初赛我们以第一的排名成功晋级。🤩

![ ](assets/SkyWalker%202.0/屏幕截图%202024-11-02%20150959.png)

## 垃圾的结构设计

为了设计它的外壳，学习了*Fusion360*，结果没想到还挺简单的。
PLA碳纤维3D打印外壳，轮组也一样。
轮组的齿轮上本来还应该套上传动皮带，但是没到货。~~最后到货了，发现尺寸不对，也用不了🙄~~

![ ](assets/SkyWalker%202.0/小车整体外形%20.png)

我把做了一半的SkyWalker带到了学校，队员来宿舍玩，遥控SkyWalker往墙上撞。我也是真没想到居然这么不禁撞，轮组的轴部直接撞断了。
碳纤维的硬度高，但是很脆；而且这次结构设计的很垃圾，轴承太小，电机杆又很粗，导致轮组的轴部很细。😔

![ ](assets/SkyWalker%202.0/1.jpg)
![ ](assets/SkyWalker%202.0/2.jpg)

已经被拆的只剩壳的1.0和2.0的合影。🤧

![ ](assets/SkyWalker%202.0/3.jpg)

最后SkyWalker失去了两轮子，也没有办法再安装到外壳上了，我就索性拆了拿泡沫板做了个2.1版。🤢

![ ](assets/SkyWalker%202.0/4.jpg)

## 所谓的2.1版本

最后我们组就拿着这个2.1参加的比赛。
而由于是大一比赛，时间又紧，主办方允许复赛用现成买的成品车，所以我这种还坚持手搓已经赢了，事实上也就只有我还手搓。
由于两个碍眼的大减速电机，对于这个竞速赛来说，那必定是最后一名；💩
没让我想到的是，前两圈SkyWalker居然一直保持领先，原因是临时加在尾部的碳杆阻挡住了其他车的前进。
不过该来的总会来。在第二圈半的时候，SkyWalker转向没有转好，后面的车冲着SkyWalker的右前轮就撞了过去，清脆一响，又断了。
这下，也确实变成最后一名了。🤡

![ ](assets/SkyWalker%202.0/Schematic_stm32小车_2024-11-01.png)

> 代码初始化部分是CubeMX生成的，主要看蓝牙串口和*main*

```c
/* USER CODE BEGIN Header */
/**
  ******************************************************************************
  * @file           : main.c
  * @brief          : Main program body
  ******************************************************************************
  * @attention
  *
  * Copyright (c) 2024 STMicroelectronics.
  * All rights reserved.
  *
  * This software is licensed under terms that can be found in the LICENSE file
  * in the root directory of this software component.
  * If no LICENSE file comes with this software, it is provided AS-IS.
  *
  ******************************************************************************
  */
/* USER CODE END Header */
/* Includes ------------------------------------------------------------------*/
#include "main.h"
#include "i2c.h"
#include "usart.h"
#include "gpio.h"

/* Private includes ----------------------------------------------------------*/
/* USER CODE BEGIN Includes */

/* USER CODE END Includes */

/* Private typedef -----------------------------------------------------------*/
/* USER CODE BEGIN PTD */

/* USER CODE END PTD */

/* Private define ------------------------------------------------------------*/
/* USER CODE BEGIN PD */

/* USER CODE END PD */

/* Private macro -------------------------------------------------------------*/
/* USER CODE BEGIN PM */

/* USER CODE END PM */

/* Private variables ---------------------------------------------------------*/

/* USER CODE BEGIN PV */

/* USER CODE END PV */

/* Private function prototypes -----------------------------------------------*/
void SystemClock_Config(void);
/* USER CODE BEGIN PFP */

/* USER CODE END PFP */

/* Private user code ---------------------------------------------------------*/
/* USER CODE BEGIN 0 */
void Relax(void)
{
  HAL_GPIO_WritePin(GPIOC, GPIO_PIN_13, GPIO_PIN_SET);

  HAL_GPIO_WritePin(GPIOA, GPIO_PIN_6, GPIO_PIN_RESET);
  HAL_GPIO_WritePin(GPIOA, GPIO_PIN_7, GPIO_PIN_RESET);

  HAL_GPIO_WritePin(GPIOB, GPIO_PIN_1, GPIO_PIN_RESET);
  HAL_GPIO_WritePin(GPIOB, GPIO_PIN_2, GPIO_PIN_RESET);
}
void Go_Straight(void)
{
  HAL_GPIO_WritePin(GPIOC, GPIO_PIN_13, GPIO_PIN_RESET);

  HAL_GPIO_WritePin(GPIOA, GPIO_PIN_6, GPIO_PIN_RESET);
  HAL_GPIO_WritePin(GPIOA, GPIO_PIN_7, GPIO_PIN_SET);

  HAL_GPIO_WritePin(GPIOB, GPIO_PIN_1, GPIO_PIN_RESET);
  HAL_GPIO_WritePin(GPIOB, GPIO_PIN_2, GPIO_PIN_SET);
}
void Go_Back(void)
{
  HAL_GPIO_WritePin(GPIOC, GPIO_PIN_13, GPIO_PIN_RESET);

  HAL_GPIO_WritePin(GPIOA, GPIO_PIN_6, GPIO_PIN_SET);
  HAL_GPIO_WritePin(GPIOA, GPIO_PIN_7, GPIO_PIN_RESET);

  HAL_GPIO_WritePin(GPIOB, GPIO_PIN_1, GPIO_PIN_SET);
  HAL_GPIO_WritePin(GPIOB, GPIO_PIN_2, GPIO_PIN_RESET);
}
void Turn_Left(void)
{
  HAL_GPIO_WritePin(GPIOC, GPIO_PIN_13, GPIO_PIN_RESET);

  HAL_GPIO_WritePin(GPIOA, GPIO_PIN_6, GPIO_PIN_SET);
  HAL_GPIO_WritePin(GPIOA, GPIO_PIN_7, GPIO_PIN_RESET);

  HAL_GPIO_WritePin(GPIOB, GPIO_PIN_1, GPIO_PIN_RESET);
  HAL_GPIO_WritePin(GPIOB, GPIO_PIN_2, GPIO_PIN_SET);
}
void Turn_Right(void)
{
  HAL_GPIO_WritePin(GPIOC, GPIO_PIN_13, GPIO_PIN_RESET);

  HAL_GPIO_WritePin(GPIOA, GPIO_PIN_6, GPIO_PIN_RESET);
  HAL_GPIO_WritePin(GPIOA, GPIO_PIN_7, GPIO_PIN_SET);

  HAL_GPIO_WritePin(GPIOB, GPIO_PIN_1, GPIO_PIN_SET);
  HAL_GPIO_WritePin(GPIOB, GPIO_PIN_2, GPIO_PIN_RESET);
}



void sendToBluetooth(uint8_t *data, uint16_t size) {
    HAL_UART_Transmit(&huart2, data, size, HAL_MAX_DELAY);
}
uint8_t rxBuffer[100]; 
uint16_t rxIndex = 0;
uint8_t receivedData; 
void HAL_UART_RxCpltCallback(UART_HandleTypeDef *huart)
{
    if (huart->Instance == USART2)
    {
        rxBuffer[rxIndex++] = receivedData;
        if (rxIndex >= sizeof(rxBuffer))
            rxIndex = 0;
        if (receivedData == '1')
            Go_Straight();
        else if (receivedData == '2')
            Go_Back();
        else if (receivedData == '3')
            Turn_Left();
        else if (receivedData == '4')
            Turn_Right();
				else if (receivedData == '0')
            Relax();
        HAL_UART_Receive_IT(&huart2, &receivedData, 1);
    }
}
void receiveFromBluetooth() {
    uint8_t receivedData;
    HAL_UART_Receive_IT(&huart2, &receivedData, 1);
}

/* USER CODE END 0 */

/**
  * @brief  The application entry point.
  * @retval int
  */
int main(void)
{

  /* USER CODE BEGIN 1 */

  /* USER CODE END 1 */

  /* MCU Configuration--------------------------------------------------------*/

  /* Reset of all peripherals, Initializes the Flash interface and the Systick. */
  HAL_Init();

  /* USER CODE BEGIN Init */

  /* USER CODE END Init */

  /* Configure the system clock */
  SystemClock_Config();

  /* USER CODE BEGIN SysInit */

  /* USER CODE END SysInit */

  /* Initialize all configured peripherals */
  MX_GPIO_Init();
  MX_USART2_UART_Init();
  MX_I2C1_Init();
  MX_USART1_UART_Init();
  /* USER CODE BEGIN 2 */

  receiveFromBluetooth();

  /* USER CODE END 2 */

  /* Infinite loop */
  /* USER CODE BEGIN WHILE */
  while (1)
  {
    const char *message = "Hello, Bluetooth!\r\n";
    sendToBluetooth((uint8_t *)message, strlen(message));

    /* USER CODE END WHILE */

    /* USER CODE BEGIN 3 */
  }
  /* USER CODE END 3 */
}

/**
  * @brief System Clock Configuration
  * @retval None
  */
void SystemClock_Config(void)
{
  RCC_OscInitTypeDef RCC_OscInitStruct = {0};
  RCC_ClkInitTypeDef RCC_ClkInitStruct = {0};

  /** Initializes the RCC Oscillators according to the specified parameters
  * in the RCC_OscInitTypeDef structure.
  */
  RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_HSE;
  RCC_OscInitStruct.HSEState = RCC_HSE_ON;
  RCC_OscInitStruct.HSEPredivValue = RCC_HSE_PREDIV_DIV1;
  RCC_OscInitStruct.HSIState = RCC_HSI_ON;
  RCC_OscInitStruct.PLL.PLLState = RCC_PLL_ON;
  RCC_OscInitStruct.PLL.PLLSource = RCC_PLLSOURCE_HSE;
  RCC_OscInitStruct.PLL.PLLMUL = RCC_PLL_MUL9;
  if (HAL_RCC_OscConfig(&RCC_OscInitStruct) != HAL_OK)
  {
    Error_Handler();
  }

  /** Initializes the CPU, AHB and APB buses clocks
  */
  RCC_ClkInitStruct.ClockType = RCC_CLOCKTYPE_HCLK|RCC_CLOCKTYPE_SYSCLK
                              |RCC_CLOCKTYPE_PCLK1|RCC_CLOCKTYPE_PCLK2;
  RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_PLLCLK;
  RCC_ClkInitStruct.AHBCLKDivider = RCC_SYSCLK_DIV1;
  RCC_ClkInitStruct.APB1CLKDivider = RCC_HCLK_DIV2;
  RCC_ClkInitStruct.APB2CLKDivider = RCC_HCLK_DIV1;

  if (HAL_RCC_ClockConfig(&RCC_ClkInitStruct, FLASH_LATENCY_2) != HAL_OK)
  {
    Error_Handler();
  }
}

/* USER CODE BEGIN 4 */

/* USER CODE END 4 */

/**
  * @brief  This function is executed in case of error occurrence.
  * @retval None
  */
void Error_Handler(void)
{
  /* USER CODE BEGIN Error_Handler_Debug */
  /* User can add his own implementation to report the HAL error return state */
  __disable_irq();
  while (1)
  {
  }
  /* USER CODE END Error_Handler_Debug */
}

#ifdef  USE_FULL_ASSERT
/**
  * @brief  Reports the name of the source file and the source line number
  *         where the assert_param error has occurred.
  * @param  file: pointer to the source file name
  * @param  line: assert_param error line source number
  * @retval None
  */
void assert_failed(uint8_t *file, uint32_t line)
{
  /* USER CODE BEGIN 6 */
  /* User can add his own implementation to report the file name and line number,
     ex: printf("Wrong parameters value: file %s on line %d\r\n", file, line) */
  /* USER CODE END 6 */
}
#endif /* USE_FULL_ASSERT */
```
