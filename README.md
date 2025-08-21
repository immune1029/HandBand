# HandBand
> **2025년 HARMAN Semicon Academy 1기** <br/> **개발기간: 2025.07.08 ~ 08.12**

**프로젝트트래커: https://github.com/users/cong2738/projects/2/views/1**

## 개발팀 소개

|박호윤                                          |임윤재                                            |임희주                                          |                                                                               
| :--------------------------------------------: | :--------------------------------------------:     | :---------------------------------------:      |
|   [@cong2738](https://github.com/cong2738)     |    [@immune1029](https://github.com/immune1029)      | [@Heeju99](https://github.com/Heeju99)|
|SystenArchitecture Design And VectorCalculater Algorithm Develop |ControlSignal Design, Virtual Stage Develop|ArmCortex Communication And System Develop|

|김민서                                          |박지수                                            |함영은                                          |                                                                               
| :--------------------------------------------: | :--------------------------------------------:     | :---------------------------------------:      |
|   [@minseo0511](https://github.com/minseo0511)     |    [@Friday930](https://github.com/Friday930)      | [@heyhoo46](https://github.com/heyhoo46)|
|Cam control module And HDMI QHD Output module Develop|Comunication Packet control module Develop|HandSignal Module improvement and Simulation|

## Introduce

- ImageSerchingAndDetect(FPGA)  
   - FPGA 기반으로 실시간 카메라 영상을 처리하고, 사용자의 마커의 궤적 좌표들을 인식한다
   - 카메라에서 입력된 원시 영상은 직접 구현한 ISP 회로를 통해 밝기 보정과 노이즈 제거 등 전처리를 거친다.
   - 좌표 추출 알고리즘: ROI(Region of Interest) 영역에서 조건에 부합하는 픽셀의 개수를 카운트하여, 깃발의 색상과 위치를 판별한다. 극한으로 단순화시킨 투표 앙상블 알고리즘이라 볼 수 있다.
   - 좌표 추출 과정은 순수 하드웨어(FPGA) 로직으로 구성되어 고속으로 처리된다.
      - FULL HD 출력이 가능한 Zybo버전의 경우 SCCB와 VDMA 설정까지는 Zynq가 사용된다(FHD 출력을 위해서는 Zybo의 DRAM에 접근해야하며 그 과정에서 VDMA모듈이 사용된다).
   - 모듈 소개
      - HandSignal     : 마커를 트래킹하는 모듈
      - GRAPHIC_Display  : 그래픽 처리장치(GPU) 영상입력을 받아 출력하기까지의 블록이다. Basys3버전의 경우 단순 그리드 출력, Zybo버전의 경우 HDMI 출력을 위해 DRAM을 드라이브 하는 VDMA모듈 등이 이에 해당된다.
      - SPI_PacketMaster: 데이터패킷통신을 제어하고 SPI를 통해 데이터를 송신하는 블록
- VectorCalculator(ARM Cortex M)
   - FPGA(GPU)로부터 받은 10개의 좌표를 통해 9개의 순간벡터를 계산한다.
   - 순간벡터 9개의 경향성을 계산하여 제어신호가 발생한다.
      - 좌표의 위치가 아닌 벡터로서 화면상 어느 위치에서든 "동일 동작 --> 동일 제어" 가능
      - 칼만 예측 적용, 잡음상황에서 신뢰성 있는 궤적 연산 가능

## Stacks
 - 핵심기술: ISP, ROI 기반 컬러 검출 마커 트레이싱, HDMI, VGA, SPI통신, 패킷컨트롤, 칼만예측
 
### Environment
![Vivado](https://img.shields.io/badge/Tool-Vivado-904cab?style=for-the-badge&logo=&logoColor=white)
![Verdi](https://img.shields.io/badge/Tool-Verdi-00c853?style=for-the-badge)
![VCS](https://img.shields.io/badge/Tool-VCS-00695c?style=for-the-badge)
![VisualstudioCode](https://img.shields.io/badge/Tool-VisualstudioCode-2196f3?style=for-the-badge)

### HW Development & Simulation
![Verilog](https://img.shields.io/badge/HDL-Verilog-ff5722?style=for-the-badge)
![SystemVerilog](https://img.shields.io/badge/HDL-SystemVerilog-ff9800?style=for-the-badge)
![UVM](https://img.shields.io/badge/SIM-UVM-3776AB?style=for-the-badge)

### SW Development
![C](https://img.shields.io/badge/LANG-C-00695c?style=for-the-badge)

### Simulation Visualizing
![Python](https://img.shields.io/badge/Lang-Python-3776AB?style=for-the-badge)

### Board
![Basys3](https://img.shields.io/badge/Board-Basys3-2196f3?style=for-the-badge)</br>

## ObjectDiagram
<img width="917" height="346" alt="image" src="https://github.com/user-attachments/assets/62fa31c4-de5a-4f4c-8e02-9d6fd7e6dad6" />

_ _ _ _ _ _

## GraphicCore(HW RTL)
#### SystemArchitecture
- Zybo Z7 Version<br/>
<img width="auto" height="346" alt="image" src="https://github.com/user-attachments/assets/712ef324-c52e-43f4-a87b-b8c471dd5be0" />

- Basys3 Version<br/>
<img width="auto" height="346" alt="image"  src="./doc/SystenArchitecture_design.png" width="auto" height="auto"/></br>

#### Cam Block: 캠 입력 처리장치
![Cam_design](./doc/CAM/CAM_design.png)</br>

#### HandSignal(MarkerDetectionModule): 마커 좌표추출 모듈(ISP)
##### Module Overview
- Marker Find: 아주 단순화 시킨 그리드 투표 앙상블 알고리즘을 사용한다</br>
<img width="auto" height="300" alt="image" src="https://github.com/user-attachments/assets/b4844c6f-188f-439b-9355-c929b4097717" /></br>

- Noise Filter<br/>
<img width="auto" height="300" alt="image" src="https://github.com/user-attachments/assets/d07d8410-fcc0-446d-b41f-1062b9ac639c" /><br/>

### SIM
- MarkerDetectionModule Simulation<br/>
   1) 노이즈가 섞인 랜덤 이미지 생성
   1) 이미지를 바이너리 파일로 변환
   1) 바이너리 파일을 Sequence 클래스에서 읽어 DUT 입력으로 사용

<img width="907" height="399" alt="image" src="https://github.com/user-attachments/assets/00e906f9-41c7-4236-8628-ec076f65d96b" /><br/>
<img width="50%" height="auto" alt="image" src="https://github.com/user-attachments/assets/b44e046a-4edb-4c81-98bd-e596bf0abe1d" />
<img width="40%" height="auto" alt="image" src="https://github.com/user-attachments/assets/33a96ddd-985c-4cb6-8e4e-92349f06521e" />
<br/>

_ _ _ _ _ _

## DataCore(CPU)
<img width="901" height="394" alt="image" src="https://github.com/user-attachments/assets/b1860e8e-99d6-4989-be82-191c4e4ce657" />

### Data Verification
<img width="901" height="auto" alt="image" src="https://github.com/user-attachments/assets/f423462d-36c5-4fa3-8c60-cb83ef9a20d8" />


_ _ _ _ _ _

## asset

<table>
   <tr>
      <td>Basys3</td>
      <td>OV7670</td>
      <td>STM32-f411</td>
   </tr>
   <tr>
      <td><img src="./doc/HW/Basys3.png" width="300" height="150"/></td>
      <td><img src="./doc/HW/OV7670.png" width="auto" height="150"/></td>
      <td><img width="auto" height="150" alt="image" src="https://github.com/user-attachments/assets/76ecfd0b-8302-4b53-8a12-daac5c1c52ce" /></td>
   </tr>
</table>

<table>
   <tr>
      <td>Zybo</td>
      <td>PCAM(OV5640)</td>
      <td>STM32-f411</td>
   </tr>
   <tr>
      <td><img width="auto" height="150" alt="image" src="https://github.com/user-attachments/assets/d938c9b5-5349-45be-b907-b56208477556" /></td>
      <td><img width="auto" height="150" alt="image" src="https://github.com/user-attachments/assets/7c36fa1f-74b3-4313-92e3-8139e1a23179" /></td>
      <td><img width="auto" height="150" alt="image" src="https://github.com/user-attachments/assets/76ecfd0b-8302-4b53-8a12-daac5c1c52ce" /></td>
   </tr>
</table>

## video  
click!--></br>
[<img src="https://github.com/user-attachments/assets/12109970-37f0-48eb-9aed-38a885886e8a" alt="Example Image">](https://youtu.be/UrpJJN5XzBg)</br>

