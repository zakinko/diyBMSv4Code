# diyBMS v4 — ESP8266 コントローラ、ビルド維持フォーク

*[English version](README.md)*

diyBMS のバージョン 4。リチウムイオン電池パックおよびセルのための、自作バッテリーマネジメントシステムです。

本リポジトリは **ESP8266 ベースのコントローラ基板** 向けで、対応モジュールは V4.00〜V4.40 です。

これは [stuartpittaway/diyBMSv4Code](https://github.com/stuartpittaway/diyBMSv4Code) のフォークで、
その世代のハードウェアを今も使っている人のためにビルドできる状態を保っています。上流は本家を
ORIGINAL/LEGACY と位置づけ、開発を ESP32 ベースの
[diyBMSv4ESP32](https://github.com/stuartpittaway/diyBMSv4ESP32) に移しました。上流の最後のコミットは
2022 年 5 月で、それ以降クリーンな clone はコンパイルが通らなくなり、CI も動かなくなっていました。
このフォークではどちらも再び動きます。

**維持していること**

* クリーンなチェックアウトからビルドが通ること。CI が push のたびに検証します
* リリースはソースから再ビルドしているので、ダウンロードできる最新の ZIP があります
* 依存関係はバージョンを固定しています。放置して流れるに任せた結果ビルドが壊れたためです。固定した
  ものは意図的に、コンパイルが通る最新のリビジョンへ進めていきます

**維持していないこと**

* **実機での動作確認はしていません。** 手元に書き込める ESP8266 コントローラがないため、ファームウェアは
  ビルドが通ることまでしか検証していません。実際に動かした方からの報告は大歓迎です。Issue を立てて
  ください
* 新機能を開発する場所ではありません。それは上に挙げた ESP32 版で進んでいて、そちらは V4.00〜V4.50 の
  モジュールに対応しています

ファームウェアの挙動そのものは何も変えていません。ここまではすべて、コードをビルドできる状態に保つための
変更です。上流の master をそのまま置いたブランチ `ci-baseline-upstream-master` があるので、比較したい
場合はそちらをご覧ください。

このプロジェクトのバージョン 3 をお探しの場合はこちら https://github.com/stuartpittaway/diyBMS


# プロジェクトへの支援

BMS が役に立ったと感じたら、作者にビールを一杯おごることを検討してみてください。詳しくは
[Patreon](https://www.patreon.com/StuartP) をご覧ください。

PayPal でビール代を送ることもできます - [paypal.me/stuart2222](https://paypal.me/stuart2222)

寄付はすべて、プロジェクトの継続的な開発と試作の費用に充てられます。

# 使い方と組み立ての動画

https://www.youtube.com/stuartpittaway

### 各デバイスへの書き込み方法の動画
https://youtu.be/wTqDMg_Ql98

### JLCPCB への発注方法の動画
https://youtu.be/E1OS0ZOmOT8


# 困ったときは

助けが必要なときは[フォーラム](https://community.openenergymonitor.org/t/diybms-v4)で質問してください。

バグを見つけた場合や機能の提案がある場合は、GitHub の Issue を立ててください。

# コードの使い方

[![PlatformIO CI](https://github.com/zakinko/diyBMSv4Code/actions/workflows/main.yml/badge.svg?branch=master)](https://github.com/zakinko/diyBMSv4Code/actions/workflows/main.yml)

このリリースでは自分でコードをコンパイルする必要はありません。代わりに GitHub Actions が自動でビルドします。

必要なファイルは [Releases](https://github.com/zakinko/diyBMSv4Code/releases) に ZIP ファイルとして置かれています。

"Compiled_Firmware_YYYY-MM-DD-HH-MM.zip" という名前の ZIP ファイルをダウンロードして*中身を展開*すると、フォルダの中に以下が見つかります。

*コントローラ用のファイル (ESP8266)*
* diybms_controller_firmware_espressif8266_esp8266_d1mini.bin
* diybms_controller_filesystemimage_espressif8266_esp8266_d1mini.bin

*モジュール用のファイル (ATTINY841)*
* module_fw_V400_attiny841_400_eF4_hD6_l62.hex
* module_fw_V410_attiny841_410_eF4_hD6_l62.hex
* module_fw_V420_attiny841_420_eF4_hD6_l62.hex
* module_fw_V420_SWAPR19R20_attiny841_420_SWAPR19R20_eF4_hD6_l62.hex
* module_fw_V421_attiny841_421_eF4_hD6_l62.hex
* module_fw_V421_LTO_attiny841_421_eF4_hD6_l62.hex
* module_fw_V440_attiny841_440_eF4_hD6_l6C.hex

esp8266 用の "filesystemimage" は無視して構いません。現在は不要です。

どのモジュール用 HEX ファイルを使うかは自分で判断する必要があります（後述の「手持ちのモジュール／基板を見分ける」を参照）。ほとんどの方は V4.00 か V4.21 の基板でしょう。

## コントローラへの書き込み

[Wemos D1 Mini](https://amzn.to/3i1gPIz) と Wemos D1 Mini Pro のどちらにも対応しています。フラッシュメモリは最低 4MB 必要です。

1. WEMOS D1 を USB ケーブルでパソコンに接続します
1. お使いの OS 向けの [esphome-flasher](https://github.com/esphome/esphome-flasher/releases) ツールをダウンロードします
1. ダウンロードしたらプログラムを起動します
1. 一覧から Wemos D1 の正しいシリアルポートを選びます
1. Browse をクリックし、ファイル "diybms_controller_firmware_espressif8266_esp8266_d1mini.bin" を選びます
1. "Flash ESP" をクリックして待ちます

以下のような出力が表示されるはずです。これは WeMos D1 Mini Pro (16MB Flash) の例です。

```
Using 'COM3' as serial port.
Connecting....
Detecting chip type... ESP8266
Connecting....

Chip Info:
 - Chip Family: ESP8266
 - Chip Model: ESP8266EX
 - Chip ID: 00123456
 - MAC Address: AA:BB:CC:DD:EE:FF
Uploading stub...
Running stub...
Stub running...
Changing baud rate to 460800
Changed.
 - Flash Size: 16MB
 - Flash Mode: dout
 - Flash Frequency: 40MHz
Erasing flash (this may take a while)...

Writing at 0x000b0000... (100 %)
Wrote 872784 bytes (728955 compressed) at 0x00000000 in 17.2 seconds...
Hash of data verified.
Leaving...
Hard Resetting...
Done! Flashing is complete!
```

## モジュールへの書き込み

モジュールのコードは ATTINY841 マイコン上で動きます。お使いの基板のバージョンに合った版のコードを書き込むことが重要です。

ATMEL AVR チップに書き込めるプログラマ（[USBASP プログラマ](https://amzn.to/2JZRp1h)など）が必要です。

### プログラマの準備

1. [USBASP プログラマ](https://amzn.to/2JZRp1h)をパソコンに接続します
1. プログラマ側で、ジャンパピン（通常 JP1 と表記）を 5V ではなく 3.3V 書き込み設定に移します
1. モジュールを電池／セルから完全に切り離します。TX/RX コネクタも外しておいてください
1. モジュール上の 6 ピン ISP コネクタでプログラマとモジュールを接続します。PIN 1 がプログラマの PIN 1 と揃っていることを十分に確認してください。PIN 1 は PCB 上に表記されています
1. AVRDUDE 6.3 以降をダウンロードします。[Windows 版](http://download.savannah.gnu.org/releases/avrdude/avrdude-6.3-mingw32.zip)、その他の版は[こちら](http://download.savannah.gnu.org/releases/avrdude/)
1. AVRDUDE の zip ファイルを展開します
1. コンソール／コマンドウィンドウを開き、AVRDUDE を展開したフォルダに移動します。Windows では以下のようになります
```
cd C:\temp\avrdude-6.3-mingw32
```
9. 標準の avrdude ツールは ATTINY841 チップに対応していません。そこで [こちら](https://raw.githubusercontent.com/SpenceKonde/ATTinyCore/master/avr/avrdude.conf) のファイルをダウンロードし、avrdude.conf を上書きしてください
1. プログラマとモジュールの接続を確認しましょう。コンソールウィンドウに戻り、下のコマンドを実行します。Linux と Mac では "usb" の代わりに別のポート（例えば /dev/tty1）を指定する必要があるかもしれません。これはパソコンによって異なります。パラメータは大文字小文字を区別する点に注意してください。
```
avrdude -C avrdude.conf -P usb -c usbasp -p t841
```
11. うまくいけば、以下のような内容が表示されます。表示されない場合は配線を確認し、正しい COM ポートを使っているか確かめてください。
```
avrdude: set SCK frequency to 187500 Hz
avrdude: AVR device initialized and ready to accept instructions
Reading | ################################################## | 100% 0.02s
avrdude: Device signature = 0x1e9315 (probably t841)
avrdude: safemode: Fuses OK (E:F4, H:D6, L:E2)
avrdude done.  Thank you.
```

### モジュールへの書き込み

モジュール 1 台の書き込みには 12 秒ほどかかります。

1. この文書の末尾にある説明を使って、手持ちのモジュール／基板がどれかを見分けます
1. 必要な ".hex" ファイルを、avrdude ツールを展開したフォルダにコピーします
1. モジュールに書き込みます。下のようなコマンドラインを実行してください。"diybms_module_firmware_400" のファイル名は該当するものに置き換えます
1. fuse の設定は重要で、ファイル名に含まれています。例えば "eF4_hD6_l62" は efuse=0xF4, hfuse=0xD6, lfuse=0x62 を意味します
```
avrdude -C avrdude.conf -P usb -c usbasp -p t841 -e -B 8 -U efuse:w:0xF4:m -U hfuse:w:0xD6:m -U lfuse:w:0x62:m -U flash:w:diybms_module_firmware_400.hex:i
```
以下のように出力されます
```
avrdude: set SCK frequency to 187500 Hz
avrdude: AVR device initialized and ready to accept instructions
avrdude: Device signature = 0x1e9315 (probably t841)
avrdude: erasing chip
avrdude: set SCK frequency to 187500 Hz
avrdude: reading input file "0xF4"
avrdude: writing efuse (1 bytes):
Writing | ################################################## | 100% 0.00s
avrdude: 1 bytes of efuse written
avrdude: verifying efuse memory against 0xF4:
avrdude: load data efuse data from input file 0xF4:
avrdude: input file 0xF4 contains 1 bytes
avrdude: reading on-chip efuse data:
Reading | ################################################## | 100% 0.00s
avrdude: verifying ...
avrdude: writing hfuse (1 bytes):
Writing | ################################################## | 100% 0.00s
avrdude: 1 bytes of hfuse written
avrdude: reading on-chip hfuse data:
Reading | ################################################## | 100% 0.00s
avrdude: verifying ...
avrdude: writing lfuse (1 bytes):
Writing | ################################################## | 100% 0.00s
avrdude: 1 bytes of lfuse written
avrdude: verifying lfuse memory against 0xE2:
avrdude: reading on-chip lfuse data:
Reading | ################################################## | 100% 0.00s
avrdude: verifying ...
avrdude: writing flash (7718 bytes):
Writing | ################################################## | 100% 6.74s
avrdude: 7718 bytes of flash written
avrdude: verifying flash memory against diybms_module_firmware_XXX.hex:
Reading | ################################################## | 100% 3.43s
avrdude: verifying ...
avrdude: 7718 bytes of flash verified
avrdude: safemode: Fuses OK (E:F4, H:D6, L:E2)
avrdude done.  Thank you.
```
1. 書き込みに失敗するもののプログラマとは通信できているようなら、"B" の値を 8 から 16 に増やして USBASP デバイスの速度を落としてみてください
1. fuse が "OK" と報告され、E:F4, H:D6, L:E2 と読めることを確認します
1. そのモジュールは USBASP プログラマから外して構いません。次のモジュールを接続し、同じ avrdude コマンドを繰り返して書き込みます



# ハードウェア

このコードに対応するハードウェアは別のリポジトリにあり、コントローラ（1 台必要）とモジュール（電池の直列セル 1 個につき 1 枚）で構成されます。

https://github.com/stuartpittaway/diyBMSv4


## 手持ちのモジュール／基板を見分ける
* V400 = 最初の基板（シルクに DIYBMS v4 と表記）。大きな抵抗が 8 個（2R20 と表記）あり、おそらく 0805 サイズの部品で手はんだされています［4.0 の基板には ATTINY841 チップの近くに TP2 があります］

* V410 = JLCPCB 製造の基板（シルクに DIYBMS v4 と表記）。大きな抵抗が 8 個（2R00 と表記）あり、0603 サイズの部品で機械実装されています［4.1 の基板には ATTINY841 チップの近くに TP2 がありません］

* V420 = JLCPCB 製造の基板（シルクに DIYBMS v4.2 と表記）。小さな抵抗が 20 個（6R20 と表記）あり、0603 サイズの部品で機械実装されています（抵抗アレイの中央に R20 があります）

* V420_SWAPR19R20 = JLCPCB 製造の基板（シルクに DIYBMS v4.2 と表記）。小さな抵抗が 20 個（6R20 と表記）あり、0603 サイズの部品で機械実装されています［サーミスタを抵抗アレイの内側へ移すために、R19 と R20 を手作業ではんだし直して位置を入れ替えたもの］

* V421 = JLCPCB 製造の基板（シルクに DIYBMS v4.21 と表記）。小さな抵抗が 20 個（6R20 と表記）あり、0603 サイズの部品で機械実装されています（抵抗アレイの中央に R19 があります）

* V440 = シルクに DIYBMS v4.4 と表記、2021 年 2 月リリース - このコードを旧版の基板に使わないでください

モジュールのコードを開き、platformio の環境 "env:attiny841_VXXX"（XXX は上記のバージョン）に移動します。USBASP プログラマをモジュールに接続し、"Upload" を選んでください。

# 警告

これは DIY の製品／ソリューションです。安全性が重要なシステムや、生命に危険が及ぶ可能性のある状況では使用しないでください。

保証はありません。期待どおりに動かないことも、まったく動かないこともあります。

このプロジェクトの利用は、すべて自己責任で行ってください。死に至りうる電圧を扱う場合があります。少しでも不安があれば、助けを求めてください。

このプロジェクトの利用は、お住まいの地域の法令に適合しない可能性があります。少しでも不安があれば、助けを求めてください。


# 自分でコードをビルドするには

このコードのビルドには [PlatformIO](https://platformio.org/) を使います。単に使いたいだけであれば、自分でコンパイルする必要はありません。上の「コードの使い方」を参照してください。

変更を加えたい、バグを直したい、中を覗いてみたいという場合は、platformio エディタで "diybms_workspace.code-workspace" という名前のワークスペースを開いてください。


# ライセンス

この作品は Creative Commons 表示 - 非営利 - 継承 2.0 UK: England & Wales ライセンスの下で提供されています。

https://creativecommons.org/licenses/by-nc-sa/2.0/uk/

あなたは以下の行為ができます。
* 共有 — どのような媒体や形式でも資料を複製・再頒布できます
* 翻案 — 資料をリミックスし、変形し、改変して作品を作れます
ライセンスの条件に従う限り、許諾者がこれらの自由を取り消すことはできません。

ただし以下の条件に従う必要があります。
* 表示 — 適切なクレジットを表示し、ライセンスへのリンクを提供し、変更があったらその旨を示さなければなりません。合理的な方法であればどのような方法でも構いませんが、許諾者があなたやあなたの利用を推奨していると示唆するような方法は除きます
* 非営利 — 営利目的で資料を利用してはなりません
* 継承 — 資料をリミックス・変形・改変した場合、あなたの貢献はオリジナルと同じライセンスの下で頒布しなければなりません
* 追加的制限の禁止 — ライセンスが許可する行為を他者が行うことを法的に制限するような法的条項や技術的手段を適用してはなりません

注意事項:
資料のうちパブリックドメインにある要素や、適用される例外・制限によってあなたの利用が許される部分については、ライセンスに従う必要はありません。

いかなる保証も提供されません。あなたの意図する利用に必要なすべての許可が、このライセンスで与えられるとは限りません。例えば、パブリシティ権、プライバシー権、著作者人格権といった他の権利が、資料の利用方法を制限する場合があります。

*上記のライセンス条項は参考のための日本語訳です。法的な効力を持つのは、上記 URL にある英語の正文です。*
