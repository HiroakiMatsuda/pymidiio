pymidiio
======================
pymidiioは[おーぷんMIDIぷろじぇくと][openmidi]で配布されている[MIDIIOライブラリ][midiio]をPythonから使用するためのラッパーモジュールです．   

[openmidi]: http://openmidiproject.sourceforge.jp/  
[midiio]: http://openmidiproject.sourceforge.jp/MIDIIOLibrary.html  

動作確認環境
------
Python:  
2.6.6  

MIDIIOライブラリ:  
1.0   

OS:  
Windows 8.1 64bit  

更新履歴:  
------

 
使い方
------
###1. MIDIIOライブラリを配置する###
まずは，MIDIIOライブラリを[おーぷんMIDIぷろじぇくとの配布ページ][midiio]よりMIDIIOライブラリ1.0(MIDIIOLib1.0.zip)をダウンロードしてください．  
ダウンロードしたMIDIOLib1.0.zipを適当な場所に解凍します．  
次に，MIDIIOLib1.0\Release\に格納されているMIDIO.dllをコピーします．  
コピーしたMIDIO.dllを本GitHubから入手したpymidiioフォルダの直下に配置してください．  
DLLファイルを配置した後の，pymidioフォルダのファイル構成は以下のようになります．  

pymidiio  
│― \_\_init\_\_.py     
│― midi\_out.py    
│― midi\_structure.py    
│― MIDIIO.dll  

###2. Python ShellからMIDIデバイスを操作してみる###
Python ShellからMIDIデバイスを操作して音声を出力しています．  

```python  
from pymidiio import midi_out

device_num = midi_out.get_device_num()
device_list = midi_out.get_device_name_list(device_num)

for num, name in enumerate(device_list):
    print("Device %d: %s" %(num, name))

midiout = midi_out.MIDIOut(device_list[0])

import time

midiout.program_change(0, 0)
    
key = 0x60
for i in range(8):
    midiout.press_key(0, 0x60 + i, 0x64)
    time.sleep(0.8)
    midiout.release_key(0, 0x60 + i, 0x00)

midiout.close_midi_device()  
```  
以上のように簡単にMIDIデバイスを制御することができます。  

###3. 関数とMIDIOutクラスの使い方###

####1. 関数
    def get_device_num()
環境中にあるMDIデバイスの数を取得し返します．  
  ・  `return` :    
    デバイス数を返します  　　
  
    def get_device_name(device_id, buffer_length = 64)
指定れた番号のデバイス名を取得し返します．    
 ・  `device_id` :  
    名前を取得するデバイス番号を指定します．  
 ・  `buffer_length` :  
    名前を格納するバッファサイズを指定します．特に必要がなければデフォルトのままで構いません．  　　  
  ・  `return` :    
    デバイス名を返します．  
    デバイス名が取得できない場合は，""が返されます．    
 
    def get_device_name_list(device_num)
指定された数のデバイス名を0番から順に取得し，リストに格納して返します．    
 ・  `device_num` :  
    名前を調べるデバイス数を指定して下さい．デバイス名は0番から順に取得されます．  
  ・  `return` :    
    デバイス名を格納したリストを返します．リストのインデックス番号がそのデバイスの番号に対応します．  
    デバイス名が取得できない場合は，""がリストに格納され返されます．    

####2. MIDIOutクラスとメソッド
    def MIDIOut(device_name)
デバイス名を指定して，MIDIOutクラスのインスタンスを作成します．  
  このとき，初期化時にデバイスのオープンが行われるので，ユーザーはopen\_deviceメソッドを使う必要はありません．  
 ・  `device_name` :  
    使用するデバイス名を指定します．  
  ・  `return` :  
    MIDIOutクラスのインスタンスを返します．  

    def press_key(channel, key, velocity)
Note Onに対応するメソッドです．キーボードを押して音を出力します．    
 ・  `channel` :    
    チャンネル番号を指定します．特に理由がなければ0を指定してください．       
 ・  `key` :    
    出力する音を指定します．    
 ・  `velocity` :    
    鍵盤を押す速度(音の大きさ)を指定します．    
 ・  `return`:  
    なし

    def release_key(channel, key, velocity)   
 Note Offに対応するメソッドです．キーボードを離して音を消音します．    
 ・  `channel` :    
    チャンネル番号を指定します．特に理由がなければ0を指定してください．       
 ・  `key` :    
    消音する音を指定します．    
 ・  `velocity` :    
    鍵盤を離す速度を指定します．    
 ・  `return`:  
    なし

    def program_change(channel, tone)
Program Changeに対応するメソッドです．音(楽器)の種類を変更します．    
 ・  `channel` :    
    チャンネル番号を指定します．特に理由がなければ0を指定してください．       
 ・  `tone` :    
    音(楽器)の種類を指定します．この番号は使用するソフトウェアシンセサイザによって異なります．    
 ・  `return`:  
    なし

    def close_midi_device()
MIDIデバイスの接続を閉じます．  
 ・  `return` :    
   なし  

ライセンス
----------
Copyright &copy; 2014 Hiroaki Matsuda  
Licensed under the [Apache License, Version 2.0][Apache]  
Distributed under the [MIT License][mit].  
Dual licensed under the [MIT license][MIT] and [GPL license][GPL].  
 
[Apache]: http://www.apache.org/licenses/LICENSE-2.0
[MIT]: http://www.opensource.org/licenses/mit-license.php
[GPL]: http://www.gnu.org/licenses/gpl.html