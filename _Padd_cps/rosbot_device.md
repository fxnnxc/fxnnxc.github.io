---
layout: distill
authors: 
    - name: Bumjin Park
      affiliations:
        name: KAIST
giscus_comments: true
disqus_comments: true
date: 2022-10-16
featured: true
title: 'Rosbot Device'
description: 'instruction of rosbot control'
importance: 2
img: https://drive.google.com/uc?export=view&id=1eJtwS8059D38zAYAwOzqmu1vjsP5nrU7
---


<center>
<img src="https://drive.google.com/uc?export=view&id=1tBMBZODkl4I817B8UEwhuQ8PwqRbODgZ" style="width:48%">
<img src="https://drive.google.com/uc?export=view&id=1eJtwS8059D38zAYAwOzqmu1vjsP5nrU7" style="width:48%">
</center>

---

## 배터리 충전

배터리 충전시 세팅 
<center>
<img src="https://drive.google.com/uc?export=view&id=1yesKbXhTJQ-K1IiCfFvtbd-DhEUfF9XE" style="width:48%">
<img src="https://drive.google.com/uc?export=view&id=11VmCIESPEQJgh2eI7eq4_MMdgj8GkdjU" style="width:48%">
<img src="https://drive.google.com/uc?export=view&id=1hf5Rw1VG6usfvOSpCxiSnrM76LjQd3Oy" style="width:48%">
<img src="https://drive.google.com/uc?export=view&id=1xiDN8CLW6Bcs5lfw77ou5Poxax2LNgdp" style="width:48%">
</center>



---

## Errors


## 2023.10.17

#### 에러1 : LED 불빛 깜빡임 
  * 증상 : 불빛이 깜빡이고 화면이 안 켜지는 중. 
  * 원인 : 배터리가 없어서 / the LED1 is blinking when battery is low – please charge immediately! [link](https://husarion.com/manuals/rosbot/)
  * **해결방법 : power 를 direct 하게 꽂는다.** 

#### 에러2: Ubuntu GUI 가 뜨지 않으며, 화면 깜빡임

<center>
<img src="https://drive.google.com/uc?export=view&id=1MWU8daWZrv8rrDr-DEvByKfqnVtqozRZ" style="width:48%">
<img src="https://drive.google.com/uc?export=view&id=1ChTGdltbBDBJNyHM_5fjjTZB2aC2mELg" style="width:100%">
</center>

* **증상**
  * Error : could not get clk -517  ([link](https://forums.raspberrypi.com/viewtopic.php?t=298441#p1796235))
  * 라즈베리파이에 오류가 있는 것으로 확인된다. 
  * bcm2835 clk-517 에러는 여전히 등장 
  * ubuntu 20.04 화면에서 꽤나 오래 살아남는 중. (이전에는 계속 꺼졌다 켜짐) 이후 `husarion login:` 화면 등장
  * 여전히 안켜지는 중. 그러나 라이다 센서가 돌아가기 시작했음. 
  * 혹시 몰라서 배터리 충전중이던 것 제거 / direct electricity 
  * 스위치 껐다가 켜보는 중
  * 갑자기 로그인 커맨드 : ID/PW: `husarion` / `husarion`
  * 배터리가 없어서 화면이 깜빡이는 것은 아닌듯. (전원이 아닌 일부 충전된 배터리로 다시 켜보는 중)
  * USB serial port used for debugging the firmware on CORE2-ROS controller

> Error 517 is -EPROBE_DEFER and is a non-fatal error used during loading all the drivers. It just means that some resource that is required (in this case the clock) isn't loaded yet, so please try again later.


* **해결**: MicroSD 에 문제가 있었고, 문제가 없는 MicroSD로 바꿔 끼우니 해결됨. 
  * 혹시 필요한 경우, OS 재설치 요망. [[link](https://husarion.com/manuals/rosbot/operating-system-reinstallation/), [link](https://husarion.com/software/os/installation/) ]



---

<img src="https://husarion.com/assets/images/block_diagram_2R-41708707bbba386b14ee88ca93323c7c.png" style='width:100%'>


--- 

### 2023.10.24 

부팅 이후 twist 커맨드로 조종이 안되는 현상 

* 특정 파일을 실행해야 하는가?