3DPrint
===========

리틀몬스터
-------------
```
>build volume 340
>origin offset 170
>home direction : center
```
<br>

Start End
----------
```
### start

G28 ;Home
G1 Z15.0 F6000 ;Move the platform down 15mm
;Prime the extruder
G92 E0
G1 F200 E3
G92 E0

### end

M104 S0
M140 S0
;Retract the filament
G92 E1
G1 E-1 F300
G28 X0 Y0
G04 P120000
M84
```
<br>

윈도우 아이콘 썸네일 보기
-------------------------
`Marlin3DprinterTool `




