# Longan Nano で動くVGM プレーヤー (YM2151 FM + YMZ294-D PSG版)


## これはなに?
何かと話題の RISC-V マイコン Longan Nano (GD32V) を使って VGM ファイルを再生させる試みです。SD カード内のフォルダに保存した vgm 拡張子のファイルを順番に再生します。<br>
可能な限り表面実装部品を使ってコンパクトにまとめることを目標にしています。
<br><br>
![IMG_1632](https://user-images.githubusercontent.com/13434151/115113300-952b9580-9fc4-11eb-808a-bc6cd5cee498.jpg)
<br>
<br>
## コンパイルとマイコンへの書き込み
Visual Studio Code + PlatformIO IDE を使用します。具体的な使用法については以下を参照。<br>
- https://beta-notes.way-nifty.com/blog/2019/12/post-b8455c.html<br>
- http://kyoro205.blog.fc2.com/blog-entry-666.html 
<br>
<br>

## 配線図
![schematic](https://user-images.githubusercontent.com/13434151/115111944-eb490a80-9fbd-11eb-9e33-226da5a9723a.png)
<a href="https://github.com/Fujix1/Retrofire/files/6329152/LonganVGM.pdf">PDF ダウンロード</a>


## 使用部品の説明
- Longan Nano: マイコン。128KB フラッシュメモリ版（64KB 版では足りません）。IC ソケットに入るように細いピンヘッダで実装のこと。
- [AE-Si5351A](https://akizukidenshi.com/catalog/g/gK-10679/): I2C 制御の可変周波数発振 IC Si5351 を使った秋月電子で売られているモジュール。
- [YMZ294D](https://akizukidenshi.com/catalog/g/gI-12141/): AY-3-8919, YM2149 互換の PSG(SSG) 音声チップ。秋月電子で入手可能。
- YM2151: FM 音源チップ。アリエクなどで入手可能。偽物やリマーク品が増えてきたので注意。
- YM3012: DAC チップ。アリエクなどで入手可能。リマーク品あり。
- オペアンプ: 4個入り SOP14、速めのものがオススメ。超高性能なものは発振する可能性大。今回の例では [OPA1654AIDR](https://www.ti.com/store/ti/en/p/product/?p=OPA1654AIDR) と [LMC6484AIMX/NOPB](https://www.ti.com/store/ti/en/p/product/?p=LMC6484AIMX/NOPB) を試した。中華から買うと100%ニセモノが来るので TI.com から直接買ったほうがよい。
- PT2257: I2C 制御の音量調整 IC 表面実装 SOP8 版。5V 動作。アリエクなどで入手可能。
- 1000uF: 電源安定用 OS-CON。秋月で入手可能。 
- ボリューム: 10KΩ Aカーブ。[ALPSALPNE 製 RK09K/RK09D](https://tech.alpsalpine.com/prod/j/html/potentiometer/rotarypotentiometers/rk09k/rk09k_list.html) を使用。YM2151 用は[二連](https://tech.alpsalpine.com/prod/j/html/potentiometer/rotarypotentiometers/rk09k/rk09k12c0a8k.html)、PSG用は[単連](https://tech.alpsalpine.com/prod/j/html/potentiometer/rotarypotentiometers/rk09k/rk09d117000c.html)、高さ30mm。[MISUMI VONA](https://jp.misumi-ec.com/)や[MOUSER](https://www.mouser.jp/ProductDetail/Alps-Alpine/RK09D117000C?qs=3cOf6TWd2rbLWuHBV5vl9A==)で入手可能。
- その他のチップ: 表面実装 1206 サイズ。
<br>
<br>

## VGM データの保存方法など

SD カードにディレクトリを作って、その中に VGM フォーマットファイルを保存します。ファイルには「.vgm」の拡張子が必要です。ZIP 圧縮された VGM ファイルである「*.vgz」は認識しません。解凍して .vgm の拡張子をつけてください。不要なファイル、空のディレクトリは削除してください。


## ノイズについて
### SD カードのアクセスノイズ
SD カードにデータ読み込みをするときにブツブツとノイズが出ることがあります。これはアクセス時に大きく電圧降下が起こるのが原因で、SD カードの種類によってノイズが目立つものと目立たないものがあります。いろいろ試してノイズの少ないものを使うのがおすすめです。

### PC 電源のノイズ
PC から USB で電源供給を行い、さらに音声を PC に入力すると大きなグランドのループができて、ノイズが増幅されることがあります。電源は可能な限りクリーンなもの（モバイルバッテリーなど）を使ってください。
<br>
<br>

## 既知の問題点

- 一部の VGM ファイルでポーズ命令が間延びすることがあります。
- 一部の VGM ファイルでポーズ命令が正しく解釈されず、早送り状態になることがあります。
