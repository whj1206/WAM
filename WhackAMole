#include <LiquidCrystal.h>
#include <avr/wdt.h>

// 初始化對應 Arduino 的腳位
LiquidCrystal lcd(8, 9, 4, 5, 6, 7);

int score = 0;
int roundCount = 1;
int y[16] = {};

void setup() {
  pinMode(11, INPUT); // 初始化3按鈕 左1
  pinMode(12, INPUT); // 初始化6按鈕 左2
  pinMode(2, INPUT); // 初始化9按鈕 右2
  pinMode(3, INPUT); // 初始化12按鈕 右1

  lcd.begin(16, 2);  // 設定 LCD 為 16×2 格式

  lcd.clear(); // 清空螢幕
  lcd.print("Hello, Arduino!");  // 顯示文字
  delay(1000);
  lcd.clear();// 清理螢幕

  for(int i=0;i<16;i++)
  {
    y[i] = 0;
  }

  y[3] = 11;
  y[6] = 12;
  y[9] = 2;
  y[12] = 3;

  intro();
}

void loop() {

  bool l[16] = {};
  bool b[16] = {};
  bool hit[16] = {};

  unsigned long startTime = millis(); // 記錄開始時間

  // 遊戲結束 
  if (roundCount == 0)
  {
    lcd.clear();
    lcd.print("Game Overrr!");
    lcd.setCursor(0, 1);
    lcd.print("score:");
    lcd.print(score);
    delay(500);
    // 等玩家先放開
    while (digitalRead(y[3]) == LOW || digitalRead(y[6]) == LOW || digitalRead(y[9]) == LOW || digitalRead(y[12]) == LOW) 
    {
      delay(10);
    }
    // 按任一按鈕重啟
    while (digitalRead(y[3]) == HIGH && digitalRead(y[6]) == HIGH && digitalRead(y[9]) == HIGH && (digitalRead(y[12]) == HIGH)) 
    {
    delay(10); 
    }
    wdt_enable(WDTO_15MS);
  }

  // 等玩家先放開
  while (digitalRead(y[3]) == LOW || digitalRead(y[6]) == LOW || digitalRead(y[9]) == LOW || digitalRead(y[12]) == LOW) 
  {
    delay(10);
  }
  
  // 初始化第二行
  lcd.setCursor(0, 1);
  lcd.print("                ");

  // 地鼠
  for(int x=3;x<13;x= x+3)
  {
    int mole = random(0, 2);
    if(mole == 0)
    {
      lcd.setCursor(x, 1);
      lcd.print("O");
      l[x] = true;
    }
  }

  // 炸彈
  int bomb = random(1, 5); 
  int z = random(0,16);
  if(y[z] != 0)
  {
    if(l[z] == true)
    {

    }
    else
    {
      lcd.setCursor(z, 1);
      lcd.print("X");
      b[z] = true;
    }
  }
  

  // 開始打地鼠
  while (millis() - startTime < random(1500, 3000))
  {
    // 打對了
    for(int x=3;x<13;x = x+3)
    {
      if ((l[x] == true) && (digitalRead(y[x]) == LOW) && !hit[x]) 
      {
        score = score + 1;
        lcd.setCursor(0, 0);
        lcd.print(score);
        lcd.setCursor(x, 1);
        lcd.print(" ");
        hit[x] = true;
        l[x] = false;
      }
    }

    // 按鈕按錯
    for(int x=3;x<13;x= x+3)
    {
      if (l[x]== false && !hit[x] && (digitalRead(y[x]) == LOW)) 
        {
          lcd.setCursor(x, 1);
          lcd.print(" ");
          roundCount = 0;
          break;
        }
    }        

    // 打到炸彈
    for(int x=3;x<13;x = x+3)
    {
      if ((b[x] == true) && (digitalRead(y[x]) == LOW) && !hit[x]) 
      {
        score = score - 20;
        lcd.setCursor(0, 0);
        lcd.print(score);
        lcd.setCursor(x, 1);
        lcd.print(" ");
        hit[x] = true;
        b[x] = false;

        if(score < 0)
        {
          roundCount = 0;
          break;
        }
      }
    }

    // 地鼠都被打完了
    if(l[3] == false && l[6] == false && l[9] == false && l[12] == false)
    {
      delay(300);
      break;
    }

  }

  // 按鈕沒按
  for(int x=3;x<13;x= x+3)
  {
    if (l[x] == true) 
    {
      lcd.setCursor(x, 1);
      lcd.print(" ");
      roundCount = 0;
    }
  }

  lcd.setCursor(0, 0);
  lcd.print(score);

  // 初始化第二行
  lcd.setCursor(0, 1);
  lcd.print("                ");
  
}

void intro()
{
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Wellcome to ...");
  delay(1500);

  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Whack A Mole");
  delay(1500);

  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Do you need");
  lcd.setCursor(0, 1);
  lcd.print("an intro?");
  delay(1500);

  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("yes: left 1"); // 3
  lcd.setCursor(0, 1);
  lcd.print("no: right 1"); // 12

  while (true)
  {
    if(digitalRead(y[12]) == LOW)
  {
    lcd.clear();
    break;
  }

    if(digitalRead(y[3]) == LOW)
  {
    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print("O is a mole.");
    delay(1500);

    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print("if you see:");
    lcd.setCursor(3, 1);
    lcd.print("O");
    delay(1500);
    lcd.setCursor(0, 0);
    lcd.print("                ");
    lcd.setCursor(0, 0);
    lcd.print("please hit 1");
    delay(1500);

    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print("if you see:");
    lcd.setCursor(6, 1);
    lcd.print("O");
    delay(1500);
    lcd.setCursor(0, 0);
    lcd.print("                ");
    lcd.setCursor(0, 0);
    lcd.print("please hit 2");
    delay(1500);

    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print("if you see:");
    lcd.setCursor(9, 1);
    lcd.print("O");
    delay(1500);
    lcd.setCursor(0, 0);
    lcd.print("                ");
    lcd.setCursor(0, 0);
    lcd.print("please hit 3");
    delay(1500);

    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print("if you see:");
    lcd.setCursor(12, 1);
    lcd.print("O");
    delay(1500);
    lcd.setCursor(0, 0);
    lcd.print("                ");
    lcd.setCursor(0, 0);
    lcd.print("please hit 4");
    delay(1500);

    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print("one hit equals");
    lcd.setCursor(0, 1);
    lcd.print("one point!");
    delay(2000);

    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print("if you miss");
    lcd.setCursor(0, 1);
    lcd.print("game will over!");
    delay(2000);

    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print("X is a bomb.");
    delay(1500);

    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print("if you hit");
    lcd.setCursor(0, 1);
    lcd.print("will cut 10 points");
    delay(2000);

    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print("let's start!");
    delay(1500);

    lcd.clear();

    break;
  }
  }

}
