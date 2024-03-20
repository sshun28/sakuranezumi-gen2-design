# 回路設計

## 車両回路設計

### 回路ブロック設計

回路ブロック図を以下に示す。

![車両回路ブロック図](assets/img/circuit_vehicle_block.png)

### 電源系統の設計

電源系統図を以下に示す。

![車両電源系統図](assets/img/circuit_vehicle_power.png)

### 光センサ回路の選定

#### TIA の設計

##### アンプの選定

AnalogDevices 社が公開している[フォトダイオード回路設計ウィザード](https://tools.analog.com/jp/photodiode/)を使って選定する。

> この方法だと AnalogDevices の OPAMP に限定されてしまうが、OPAMP は普通に選定を進めてもたいてい AnalogDevices の IC に落ち着くのでこれを使ってみる。
> (マイクロマウス大会にスポンサーしていただいている事もあるし・・・)

■ 1 フォトダイオードのパラメータを設定

フォトダイオードには[VEMD2023X01](https://www.digikey.jp/ja/products/detail/vishay-semiconductor-opto-division/VEMD2023X01/4075873)を使用する。

下記のように設定した。

![条件の設定](assets/img/circuit-pd_select1.png)

これらの値は、データシートの下記部分から算出した。

![VEMD2023X01 datasheet](assets/img/VEMD2023X01_DS.png)

- ピーク入力光強度は1[mW/cm^2^]に設定
- 逆電圧は、きりの良い値として1[V]を設定(根拠なし)
- 静電容量は逆電圧1[V]のときの値をデータシートFig.4から拾ってきた値に設定
- シャント抵抗はFig.1から25度のとき暗電流=1[nA],V~R~=10[V]なので、R~SH~=10[V]/1[nA]=10[GΩ]
- ピーク電流は1[mW/cm^2^]のときの値を採用し、少し余裕を見て12[uAとした]

■ 2 OPAMPの選定

- ピーク電圧は2[V]とした
  - 単電源回路なので、逆電圧1[V]は入力コモン電圧となる。それに対し、電源電圧が3.3[V]であるため、残り2.3[V]の中から適当な値を選択した。
- 速度はパルス幅を5[uS]とした
  - MCUのA/D変換器が受け付けられ(A/Dの最小周期は0.2[uS])、多少余裕がある中で狭めのパルス幅。
  - 狭めのパルス幅とするのは
    - 発光の消費電力を下げるため
    - 積分により分解能を向上させるため
- ピークスライダーはパルス応答画面を見ながら適当に調整した
- オペアンプをAD8605に選定
  - パッケージサイズでソートして選定した

![OPAMPの選定画面3](assets/img/OPAMP_device_select.png)

![OPAMPの選定画面](assets/img/OPAMP_Select.png)

![OPAMPの選定画面2](assets/img/OPAMP_waveform.png)
