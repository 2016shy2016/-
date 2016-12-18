#include <Adafruit_NeoPixel.h> //引用此文件
#define PIN 4   //led灯控制引脚
#define PIN_NUM 1  //允许接的led灯的个数
#define mic_pin A2 //麦克风（接收声音）
#define val_max 255
#define val_min 0

Adafruit_NeoPixel strip = Adafruit_NeoPixel(PIN_NUM, PIN, NEO_GRB + NEO_KHZ800);  
//该函数第一个参数控制串联灯的个数，第二个是控制用哪个pin脚输出，第三个显示颜色和变化闪烁频率
int voice_data;

void setup()
{
  Serial.begin(115200);
  pinMode(mic_pin, INPUT);//pinMode()函数用来设置引脚是输入模式还是输出模式
  strip.begin(); //准备对灯珠进行数据发送
  strip.show(); //初始化LED为关的状态
  voice_data=analogRead(mic_pin);
  }

void loop()
{

  if (voice_data < 50)
  {
  rainbowCycle( 255, 0, 0, 20); //红色呼吸很慢
  }

  else if (voice_data < 70)
  {
  rainbowCycle( 0, 0, 255, 12); //蓝色呼吸较慢
  }

  else if (voice_data < 90)
  {
  rainbowCycle( 0, 255, 0, 6); //绿色呼吸较
  }
  else if (voice_data < 110)
  {
  rainbowCycle( 255, 0, 0, 3); //红色呼吸
  rainbowCycle( 0, 0, 255, 3); //蓝色呼吸快
  }
  else if (voice_data < 130)
  {
  rainbowCycle( 255, 0, 0, 3); //红色呼吸
  rainbowCycle( 0, 255, 0, 3); //绿色呼吸快
  }


  else (voice_data >=150);
 {
  rainbowCycle( 255, 0, 0, 1); //红色呼吸
  rainbowCycle( 0, 255, 0, 1); //绿色呼吸
  rainbowCycle( 0, 0, 255, 1); //蓝色呼吸敲快
  }
 // */
}
void colorSet(uint32_t c) 
{
  for (uint16_t i = 0; i < strip.numPixels(); i++) 
  //从0自增到LED灯个数减1
{
strip.setPixelColor(i, c); //此函数表示将第i个LED点亮
}
  strip.show(); //LED灯显示
}

void rainbowCycle( int r, int g, int b, uint8_t wait) {
  for (int val = 0; val < 255; val++) 
  //val由0自增到254不断循环
  {
colorSet(strip.Color(map(val, val_min, val_max, 0, r), map(val, val_min, val_max, 0, g), map(val, val_min, val_max, 0, b)));
//红绿蓝LED灯依次从暗到亮
/*“map(val,x,y,m,n)”函数为映射函数，可将某个区间的值（x-y）变幻成（m-n），val则是你需要用来映射的数据*/
    delay(wait); //延时
  }
  for (int val = 255; val >= 0; val--)  //val从255自减到0不断循环
  {
colorSet(strip.Color(map(val, val_min, val_max, 0, r), map(val, val_min, val_max, 0, g), map(val, val_min, val_max, 0, b)));
//红绿蓝LED灯依次由亮到暗
    delay(wait); //延时
  }
}
