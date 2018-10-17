# star.d3d
3D游戏HOOK检测，主要检测虚表中函数：IDXGISwapChain::Present、IDXGISwapChain::ResizeBuffers被HOOK情况。


先把dll注入到游戏进程并打开debugview，然后执行以下脚本：
```lua
star.setlog('trace') star.d3d('init') star.print(star.d3d('info')) star.d3d('hook') star.print(star.d3d('info')) star.d3d('unhook')
```

虚表函数共35个，序号以0开始，注意观察第8、13个函数地址，注意不要错认了自己的HOOK函数。
```none
obj: 000000008EBCFEC0 return by getObjFromCreateDeviceAndSwapChain()
00 000007FEE89B5930
01 000007FEE89B57FC
02 000007FEE89B583C
03 000007FEE89B6928
04 000007FEE89B86CC
05 000007FEE89B691C
06 000007FEE89BA048
07 000007FEE89B8240
08 000000018000D990
09 000007FEE89BAA08
10 000007FEE89C0B64
11 000007FEE89C1804
12 000007FEE89BA178
13 0000000180009E0D
14 000007FEE89C07D8
15 000007FEE89BAED8
16 000007FEE89C1BCC
17 000007FEE89C28E8
18 000007FEE89BA4B8
19 000007FEE89BA608
20 000007FEE89BA740
21 000007FEE89BA7B4
22 000007FEE89BB510
23 000007FEE89BB920
24 000007FEE89BA808
25 000007FEE89BA844
26 000007FEE89BA93C
27 000007FEE89BA9A0
28 000007FEE89BA9D0
29 000007FEE89B599C
30 000007FEE89A5ED8
31 000007FEE899C290
32 000007FEE89B3BCC
33 000007FEE89B3BE4
34 000007FEE89A5F9C

obj: 0000000005355A00 return by getObjFromPresentHooked()
00 000007FEE89B5930
01 000007FEE89B57FC
02 000007FEE89B583C
03 000007FEE89B6928
04 000007FEE89B86CC
05 000007FEE89B691C
06 000007FEE89BA048
07 000007FEE89B8240
08 000000018000D990
09 000007FEE89BAA08
10 000007FEE89C0B64
11 000007FEE89C1804
12 000007FEE89BA178
13 0000000180009E0D
14 000007FEE89C07D8
15 000007FEE89BAED8
16 000007FEE89C1BCC
17 000007FEE89C28E8
18 000007FEE89BA4B8
19 000007FEE89BA608
20 000007FEE89BA740
21 000007FEE89BA7B4
22 000007FEE89BB510
23 000007FEE89BB920
24 000007FEE89BA808
25 000007FEE89BA844
26 000007FEE89BA93C
27 000007FEE89BA9A0
28 000007FEE89BA9D0
29 000007FEE89B599C
30 000007FEE89A5ED8
31 000007FEE899C290
32 000007FEE89B3BCC
33 000007FEE89B3BE4
34 000007FEE89A5F9C
```

## 跨进程检测D3D的HOOK情况
```lua
star.d3d('check', pid, [alignment])
```
- 'check'：该字符串指令表示要进行跨进程检测D3D的HOOK情况
- pid：待扫描的进程pid
- alignment：扫描内存时加速用的，默认为8字节

示例：
```lua
pid = star.getpid('game.exe')
star.d3d('check', pid)
```

输出示例：
```none
dxgi.dll load: 00007FFC21B80000
D3D11CreateDeviceAndSwapChain Get SwapChain: 00000000005C07E0
D3D vt:
00 00007FFC21BDC018 = 00007FFC21B872C0 OK
01 00007FFC21BDC020 = 00007FFC21B875A0 OK
02 00007FFC21BDC028 = 00007FFC21B874A0 OK
03 00007FFC21BDC030 = 00007FFC21BCE660 OK
04 00007FFC21BDC038 = 00007FFC21BB1530 OK
05 00007FFC21BDC040 = 00007FFC21BCC350 OK
06 00007FFC21BDC048 = 00007FFC21BD2EA0 OK
07 00007FFC21BDC050 = 00007FFC21BCBBB0 OK
08 00007FFC21BDC058 = 00007FFC04097C80 00007FFC21B82140 c:\users\admini~1\appdata\local\temp\vcre.dll
09 00007FFC21BDC060 = 00007FFC21B97730 OK
10 00007FFC21BDC068 = 00007FFC21B9B7E0 OK
11 00007FFC21BDC070 = 00007FFC21B9BA30 OK
12 00007FFC21BDC078 = 00007FFC21B97870 OK
13 00007FFC21BDC080 = 00007FFC04097FA0 00007FFC21B947B0 c:\users\admini~1\appdata\local\temp\vcre.dll
14 00007FFC21BDC088 = 00007FFC21BD47A0 OK
15 00007FFC21BDC090 = 00007FFC21B9BB10 OK
16 00007FFC21BDC098 = 00007FFC21BD2930 OK
17 00007FFC21BDC0A0 = 00007FFC21B97A00 OK
18 00007FFC21BDC0A8 = 00007FFC21B823F0 OK
19 00007FFC21BDC0B0 = 00007FFC21BD29E0 OK
20 00007FFC21BDC0B8 = 00007FFC21BD2AB0 OK
21 00007FFC21BDC0C0 = 00007FFC21BD2700 OK
22 00007FFC21BDC0C8 = 00007FFC21B824B0 OK
23 00007FFC21BDC0D0 = 00007FFC21BD3660 OK
24 00007FFC21BDC0D8 = 00007FFC21B825B0 OK
25 00007FFC21BDC0E0 = 00007FFC21BD4EA0 OK
26 00007FFC21BDC0E8 = 00007FFC21BD2470 OK
27 00007FFC21BDC0F0 = 00007FFC21B825E0 OK
28 00007FFC21BDC0F8 = 00007FFC21BD2FE0 OK
29 00007FFC21BDC100 = 00007FFC21BD5620 OK
30 00007FFC21BDC108 = 00007FFC21BD3170 OK
31 00007FFC21BDC110 = 00007FFC21B82690 OK
32 00007FFC21BDC118 = 00007FFC21BD2C10 OK
33 00007FFC21BDC120 = 00007FFC21B82750 OK
34 00007FFC21BDC128 = 00007FFC21B82810 OK

scan D3D vt
got D3D vtable: 00007FFC21BDC018
?扗3D vt:
00 00007FFC21BDC018 = 00007FFC21B872C0 OK
01 00007FFC21BDC020 = 00007FFC21B875A0 OK
02 00007FFC21BDC028 = 00007FFC21B874A0 OK
03 00007FFC21BDC030 = 00007FFC21BCE660 OK
04 00007FFC21BDC038 = 00007FFC21BB1530 OK
05 00007FFC21BDC040 = 00007FFC21BCC350 OK
06 00007FFC21BDC048 = 00007FFC21BD2EA0 OK
07 00007FFC21BDC050 = 00007FFC21BCBBB0 OK
08 00007FFC21BDC058 = 00007FFC04097C80 00007FFC21B82140 c:\users\admini~1\appdata\local\temp\vcre.dll
09 00007FFC21BDC060 = 00007FFC21B97730 OK
10 00007FFC21BDC068 = 00007FFC21B9B7E0 OK
11 00007FFC21BDC070 = 00007FFC21B9BA30 OK
12 00007FFC21BDC078 = 00007FFC21B97870 OK
13 00007FFC21BDC080 = 00007FFC04097FA0 00007FFC21B947B0 c:\users\admini~1\appdata\local\temp\vcre.dll
14 00007FFC21BDC088 = 00007FFC21BD47A0 OK
15 00007FFC21BDC090 = 00007FFC21B9BB10 OK
16 00007FFC21BDC098 = 00007FFC21BD2930 OK
17 00007FFC21BDC0A0 = 00007FFC21B97A00 OK
18 00007FFC21BDC0A8 = 00007FFC21B823F0 OK
19 00007FFC21BDC0B0 = 00007FFC21BD29E0 OK
20 00007FFC21BDC0B8 = 00007FFC21BD2AB0 OK
21 00007FFC21BDC0C0 = 00007FFC21BD2700 OK
22 00007FFC21BDC0C8 = 00007FFC21B824B0 OK
23 00007FFC21BDC0D0 = 00007FFC21BD3660 OK
24 00007FFC21BDC0D8 = 00007FFC21B825B0 OK
25 00007FFC21BDC0E0 = 00007FFC21BD4EA0 OK
26 00007FFC21BDC0E8 = 00007FFC21BD2470 OK
27 00007FFC21BDC0F0 = 00007FFC21B825E0 OK
28 00007FFC21BDC0F8 = 00007FFC21BD2FE0 OK
29 00007FFC21BDC100 = 00007FFC21BD5620 OK
30 00007FFC21BDC108 = 00007FFC21BD3170 OK
31 00007FFC21BDC110 = 00007FFC21B82690 OK
32 00007FFC21BDC118 = 00007FFC21BD2C10 OK
33 00007FFC21BDC120 = 00007FFC21B82750 OK
34 00007FFC21BDC128 = 00007FFC21B82810 OK
```
可以看出D3D虚表函数被HOOK的情况及dll的路径。