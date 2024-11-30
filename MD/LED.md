# 基于STM32CubdeMX的HAL库流水灯和外部中断实验
## [一、使用HAL库实现流水灯](#1)
### [1.1 STM32CubeMX配置](#1.1)
### [1.2 流水灯的代码实现](#1.2)
### [1.3 使用keil5逻辑分析仪仿真](#1.3)
## [二、加入外部中断控制流水灯](#2)
## <a id=1></a>一、使用HAL库实现流水灯          
### <a1 id=1.1></a1>1.1   STM32CubeMX配置       
GPIO是用默认模式，如图所示：
![alt text](<屏幕截图 2024-12-01 005424.png>)
配置使用外部晶振并且设置时钟为72MHZ，如图：
![alt text](<屏幕截图 2024-12-01 005500.png>)
接下来只需生成代码到keil5即可
### <a2 id=1.2></a2>1.2   流水灯的代码实现        
所有配置代码都由STM32CubeMX自动生成完成，所以只需在主循环内编写流水灯部分代码
``` c
  while (1)
  {
	HAL_GPIO_TogglePin(GPIOA,GPIO_PIN_0|GPIO_PIN_1);
	HAL_Delay(1000);
    HAL_GPIO_TogglePin(GPIOB,GPIO_PIN_0|GPIO_PIN_1);
	HAL_Delay(1000);
    HAL_GPIO_TogglePin(GPIOC,GPIO_PIN_13|GPIO_PIN_14);
	HAL_Delay(1000);
  }
```
### <a3 id=1.3></a3>1.3 使用keil5逻辑分析仪仿真     
打开keil5下载代码然后点击debug按键，在逻辑分析仪中加入三个GPIO并设置为“bit”观察波形，如图所示：
![alt text](<屏幕截图 2024-12-01 015424.png>)
## <b id=2></b>二、加入外部中断控制流水灯
在STM32CubeMX中配置PB12为上拉，双边沿中断模式
![alt text](<屏幕截图 2024-12-01 005446.png>)
打开NVIC对应中断
![alt text](<屏幕截图 2024-12-01 005454.png>)
修改主函数代码如下：
```c
int main(void)
{
  HAL_Init();
  SystemClock_Config();
  MX_GPIO_Init();
  flag=HAL_GPIO_ReadPin(GPIOB,GPIO_PIN_12);
  while (1)
  {
		while(flag)
		{
		HAL_GPIO_TogglePin(GPIOA,GPIO_PIN_0|GPIO_PIN_1);
		HAL_Delay(1000);
        HAL_GPIO_TogglePin(GPIOB,GPIO_PIN_0|GPIO_PIN_1);
		HAL_Delay(1000);
        HAL_GPIO_TogglePin(GPIOC,GPIO_PIN_13|GPIO_PIN_14);
		HAL_Delay(1000);
		}
  }
}
```
编写中断回调函数如下:
```c
void HAL_GPIO_EXTI_Callback(uint16_t GPIO_Pin)
{
if(GPIO_PIN_12==GPIO_Pin)
{
 if(RESET==HAL_GPIO_ReadPin(GPIOB,GPIO_PIN_12))
 {
	   flag=0;
 }
 else flag=1;
}
}
```
至此可实现外部GPIO高低电平改变流水灯工作状态