#小车循迹功能



#### 实现功能：

- **使小车能够沿着黑色轨道的方向行进**



#### 原理：

- **小车前安装有左右两个红外光电对管，当对管遇到黑线时，光线被全部吸收，没有返回的光线，此时对管没有接收到返回的信息，输出高电平。反之，当对管遇到白线时，输出低电平。**

- **两个光电对管输出可以组合成四种状态（00,01,10,11），根据不同的状态，给小车下达不同的指令**



| 00 | 01 |10 | 11 |
|--------|--------|--------|--------|
|前进|右转|左转|刹车|




#### 循迹逻辑代码如下：



```cpp

	void loop()

	{ 

  	SL = digitalRead(SensorLeft);      //数字读出 LOW 表明此传感器在白色区域，传感器指示灯亮；数字读出 HIGH 表明此传感器在黑色区域，传感器指示灭

	  SR = digitalRead(SensorRight);     //数字读出 LOW 表明此传感器在白色区域，传感器指示灯亮；数字读出 HIGH 表明此传感器在黑色区域，传感器指示灭  

 	 if (SL == LOW&&SR==LOW)

  	  goStraight();                    //调用前进函数

 	 else if (SL == HIGH & SR == LOW)   // 左循迹红外传感器,检测到信号，车子向右偏离轨道，向左转 

 	   turnLeft();

  	else if (SR == HIGH & SL == LOW)   // 右循迹红外传感器,检测到信号，车子向左偏离轨道，向右转  

 	   turnRight();

 	 else                               // 都是黑色, 停止

 	 brake();

  

	}

```



#### 电机控制



- **写出四个电机控制函数控制前进，左转，右转和刹车。**

- **电机输出可用`analogWrite`函数实现。**



**例如小车前进的代码控制如下，其它行进方式可类比写出**



```cpp

void goStraight()

{

  // 右电机前进 

  analogWrite(Right_motor_go,60);   //PWM比例0~255调速，左右轮差异略增减

  analogWrite(Right_motor_back,0);

  // 左电机前进

  analogWrite(Left_motor_go,60);    //PWM比例0~255调速，左右轮差异略增减

  analogWrite(Left_motor_back,0);

}

```

####引脚配置



- **红外光电对管的信号线引脚可连接到arduino主控板的任意数字输出引脚。**

- **由于电机要用PWM波控制，所以要用能够输出PWM波的引脚（D3，D5，D6，D9，D10，D11）。**



####其它

**完整代码`tracking.ino`可到项目中查看并下载**



