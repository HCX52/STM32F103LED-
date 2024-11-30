# ����STM32CubdeMX��HAL����ˮ�ƺ��ⲿ�ж�ʵ��
## [һ��ʹ��HAL��ʵ����ˮ��](#1)
### [1.1 STM32CubeMX����](#1.1)
### [1.2 ��ˮ�ƵĴ���ʵ��](#1.2)
### [1.3 ʹ��keil5�߼������Ƿ���](#1.3)
## [���������ⲿ�жϿ�����ˮ��](#2)
## <a id=1></a>һ��ʹ��HAL��ʵ����ˮ��          
### <a1 id=1.1></a1>1.1   STM32CubeMX����       
GPIO����Ĭ��ģʽ����ͼ��ʾ��
![alt text](<��Ļ��ͼ 2024-12-01 005424.png>)
����ʹ���ⲿ����������ʱ��Ϊ72MHZ����ͼ��
![alt text](<��Ļ��ͼ 2024-12-01 005500.png>)
������ֻ�����ɴ��뵽keil5����
### <a2 id=1.2></a2>1.2   ��ˮ�ƵĴ���ʵ��        
�������ô��붼��STM32CubeMX�Զ�������ɣ�����ֻ������ѭ���ڱ�д��ˮ�Ʋ��ִ���
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
### <a3 id=1.3></a3>1.3 ʹ��keil5�߼������Ƿ���     
��keil5���ش���Ȼ����debug���������߼��������м�������GPIO������Ϊ��bit���۲첨�Σ���ͼ��ʾ��
![alt text](<��Ļ��ͼ 2024-12-01 015424.png>)
## <b id=2></b>���������ⲿ�жϿ�����ˮ��
��STM32CubeMX������PB12Ϊ������˫�����ж�ģʽ
![alt text](<��Ļ��ͼ 2024-12-01 005446.png>)
��NVIC��Ӧ�ж�
![alt text](<��Ļ��ͼ 2024-12-01 005454.png>)
�޸��������������£�
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
��д�жϻص���������:
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
���˿�ʵ���ⲿGPIO�ߵ͵�ƽ�ı���ˮ�ƹ���״̬