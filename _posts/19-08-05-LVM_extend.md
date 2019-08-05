---
title : "LVM Extend"
categories:
  - Linux
---
기존 LVM에 추가하기

## 1. 명령어

~~~
pvcreate [파티션명]
vgextend [볼륩그룹명] [물리볼륨경로]
lvextend [논리볼륨경로] -l +100%FREE

resize2fs /dev/[볼륨그룹명]/[논리볼륨명]
~~~

## 2. 예시

### 2-0. disk format

~~~
disk format 이후..
-> fdisk /dev/vdh n p 1 [enter] [enter] t 8e w
~~~

### 2-1. pvcreate

~~~
bcode@server:/> pvcreate /dev/vdh1
  Physical volume "/dev/vdh1" successfully created
~~~

### 2-2. vgdisplay로 볼륨그룹의 용량 확인

~~~
bcode@server:/> vgdisplay
  --- Volume group ---
  VG Name               vg
  ...
  VG Size               5.86 TiB
  PE Size               4.00 MiB
  Total PE              1535994
  Alloc PE / Size       1535994 / 5.86 TiB
  Free  PE / Size       0 / 0   
  ...
~~~

### 2-3. vgextend

~~~
bcode@server:/> vgextend  vg /dev/vdh1
  Volume group "vg" successfully extended
~~~

### 2-4. vgdisplay로 볼륨그룹의 용량 확인

~~~
bcode@server:/> vgdisplay
  --- Volume group ---
  VG Name               vg
  ...
  VG Size               6.84 TiB
  PE Size               4.00 MiB
  Total PE              1791993
  Alloc PE / Size       1535994 / 5.86 TiB
  Free  PE / Size       255999 / 1000.00 GiB
  ...
~~~

### 2-5. lvdisplay로 논리볼륨 용량 확인

~~~
bcode@server:/> lvdisplay
  --- Logical volume ---
  LV Path                /dev/vg/lv
  LV Name                lv
  VG Name                vg
  ...
  LV Size                5.86 TiB
  Current LE             1535994
  ...
~~~

### 2-6. lvextend

~~~
bcode@server:/> lvextend /dev/vg/lv -l +100%FREE
  Size of logical volume vg/lv changed from 5.86 TiB (1535994 extents) to 6.84 TiB (1791993 extents).
  Logical volume lv successfully resized
~~~

### 2-7. lvdisplay로 논리볼륨 용량 확인

~~~
bcode@server:/> lvdisplay
  --- Logical volume ---
  LV Path                /dev/vg/lv
  LV Name                lv
  VG Name                vg
  ...
  LV Size                6.84 TiB
  Current LE             1791993
  ...
~~~

### 2-8. resize

~~~
bcode@server:/> resize2fs /dev/vg/lv
~~~
