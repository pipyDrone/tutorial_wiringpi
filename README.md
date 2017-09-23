# USAGE
## 大まかな手順
1. wiringPiSetup**関数でピン番号のナンバリングスキーマを設定する
1. pinMode関数でピンに対してモードを指定する
1. pwmSetMode関数でPWMのモードを指定する
1. pwmWrite関数でピンに値を流す 

# 関数の説明
## [Setup](http://wiringpi.com/reference/setup/)
ピン番号についてどのナンバリングスキーマを使用するか設定する
コードの最初に以下のいずれかの関数を呼び出す必要がある
### wiringPiSetup()
- Broadcom GPIO pin番号にマッピングである、[wiringPiピン番号](https://projects.drogon.net/raspberry-pi/wiringpi/pins/)を使用する
### wiringPiSetupGpio()
- Broadcom GPIO pin番号を使用する
### wiringPiSetupPhys()
- 物理ピン番号を使用する
### wiringPiSetupSys()
- Broadcom GPIO pin番号を使用する
- ハードウェアに直接アクセスするのではなく、/sys/class/gpioインタフェースを使用する
- GPIOピンがgpioプログラム(gpioプログラムがなにかはしらない)を使用して事前にエクスポートされていれば、root以外のユーザとして呼び出すことができる

## [Core Functions](http://wiringpi.com/reference/core-functions/)
### pinMode(int pin, int mode)
- ピンのモードを設定する
#### モードの種類（INPUT,OUTPUT,PWM_OUTPUT,GPIO_CLOCK)
##### INPUT
- wiringpi.GPIO.INPUT
##### OUTPUT
- wiringpi.GPIO.OUTPUT
##### PWM_OUTPUT
- wiringpi.GPIO.PWM_OUTPUT
##### GPIO_CLOCK
- wiringpi.GPIO.GPIO_CLOCK

### pinWrite(int pin, int value)
- PWMレジスタに値を書き込む
- ラズパイのレンジは0 - 1024なので、その間の数値を与える必要がある

## [Raspberry Pi Specifics](http://wiringpi.com/reference/raspberry-pi-specifics/)
### pwmSetMode(int mode)
- PWMジェネレーターのモードを設定設定する
#### モードの種類
##### balanced
- デフォルト
- 制御しづらいそうなので、こちらはあまり使われない
- wiringpi.GPIO.PWM_MODE_BAL
##### mark:space
- wiringpi.GPIO.PWM_MODE_MS

### pwmSetRange(unsigned int range)
- PWMジェネレータのレンジを設定する
- デフォルトは1024

### pwmSetClock(int divisor)
- PWMクロックの除数を設定する
#### 除数を求める計算式
- 除数＝<ラズパイのPWMのベースロック数>/<PWMジェネレータのレンジ>/<ピンに設定したいHz数>

# 用語
## PWM制御
- 電圧を変化させる代わりに加える前提は一定にし、ONにする時間を変化させることで、モータなどの回転数を制御する
## PWM周期
- ON/OFFの一定の繰り返し時間
## デューティ比
- デューティ比をあたえることによってみかけの電圧を変化させる
- デューティ比＝デューティ/PWM周期
- 0~5VまでPWM制御により電圧を変化させる場合、デューティ0%で0V、50%で2．5V、100%で5Vと対比させることができる
## PWM周波数
- PWM周波数＝1/PWM周期
- ラズパイのPWMが持つベースクロックの周波数は19.2MHzである
- 操作対象の機器のクロック周波数に、ラズパイのPWMの周波数を合わせてやる必要がある
## レンジ
- 100%を数値で表す
- デフォルトは1024


# 参考リンク
- [wiringpi.com](http://wiringpi.com/)
- [PWMってな〜んだ？](http://www.oidenansho.com/elekijack/indoor_plane/shigyo_siki/pwm/pwm.htm)
