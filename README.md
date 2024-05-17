# QEI

このリポジトリは [https://os.mbed.com/users/aberk/code/QEI/](https://os.mbed.com/users/aberk/code/QEI/) をMbed OS 6に対応させたものです。
さらにそれを逆回転のＺ層に対応したものです。

## サンプルコード
### 相対角度
リセットしたところを0としてそこからの角度を取得します。
```cpp
#include "mbed.h"
#include "QEI.h"

// もしZを使わないならNCと入力
QEI rori(PA_0,PB_0,PZ_0,2048,QEI::X2_ENCODING); // A,B,Z
double angle=0.0;
int main(){
  rori.reset();
  while(true){
    angle=rori.getPulses()/2048*360;
    printf("angle: %f \t\t",angle);
    while(angle>180) angle-=360;
    while(angle<-180)angle+=360;
    printf("fixed angle: %llf\n",angle);
  }
}
```

### RPM(回転数)
1分間で何回転するかを取得します。
```cpp
#include "mbed.h"
#include "QEI.h"

// もしZを使わないならNCと入力
QEI rori(PA_0,PB_0,PZ_0,2048,QEI::X2_ENCODING); // A,B,Z
double rpm=0.0;
int main(){
  while(true){
    rori.reset();
    ThisThread::sleep_for(10ms);
    rpm=rori.getPulses()*600/1024;
    printf("rpm: %llf \n",rpm);
  }
}
```
