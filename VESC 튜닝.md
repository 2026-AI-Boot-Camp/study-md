# F1TENTH VESC 초기 설정 가이드

> **이 문서의 목적**
> 
> 
> F1TENTH 차량의 VESC 펌웨어를 업데이트하고, 구동 모터 및 조향 서보모터가 정상적으로 동작하도록 기본 설정을 완료한다.
> 

VESC는 차량의 **전자식 속도 제어기이자 모터 컨트롤러**이다. Jetson에서 전달되는 명령을 받아 구동용 브러시리스 모터와 조향용 서보모터를 제어한다.

이번 설정에서는 다음 작업을 진행한다.

1. VESC Tool 설치
2. VESC 전원 및 USB 연결
3. VESC 펌웨어 업데이트
4. 조향용 Servo Output 활성화
5. Motor Configuration 적용
6. FOC 모터 파라미터 측정
7. Openloop 설정 변경
8. Speed PID Controller 튜닝
9. 최대 ERPM 설정

---

# 작업 전 안전 수칙

> **모터 설정 및 테스트 과정에서 바퀴가 갑자기 회전할 수 있다. 반드시 차량을 지면에서 띄운 상태로 작업한다.**
> 
- 차량을 전용 스탠드나 튼튼한 상자 위에 올린다.
- 네 바퀴가 모두 지면에서 떨어져 자유롭게 회전할 수 있어야 한다.
- 모터 테스트 중에는 차량이 스탠드에서 떨어지지 않도록 차량을 잡고 있는다.
- 바퀴 주변에 사람, 전선 또는 다른 물체가 없는지 확인한다.
- 모터에 충분한 전류를 공급하기 위해 완전히 충전된 LiPo 배터리를 사용한다.

## 필요한 장비

- 조립이 완료된 F1TENTH 또는 RoboRacer 차량
- 차량을 올려놓을 스탠드 또는 상자
- VESC Tool을 실행할 노트북
- 완전히 충전된 LiPo 배터리
- VESC와 노트북을 연결할 Micro USB 케이블

![image.png](VESC%20%ED%8A%9C%EB%8B%9D/image.png)

> 모터 테스트 중 차량이 움직이지 않도록 네 바퀴를 지면에서 띄운다. (더 높게 권장)
> 

---

# 1. VESC Tool 설치

차량의 구동 모터와 동력 전달 구조에 맞게 VESC를 설정하려면 먼저 **VESC Tool**을 설치해야 한다.

VESC Project 사이트에서 계정을 만든 뒤 무료 버전의 VESC Tool을 선택한다. 이메일 주소를 입력하여 절차를 완료하면 다운로드 링크가 이메일로 전송된다.

![image.png](VESC%20%ED%8A%9C%EB%8B%9D/image%201.png)

VESC Tool은 다음 운영체제를 지원한다.

- Linux
- Windows
- macOS

## 작업 방법

1. VESC Project 사이트에 접속한다.
2. 계정을 생성한다.
3. 무료 버전의 VESC Tool을 선택한다.
4. 이메일로 받은 링크를 통해 설치 파일을 다운로드한다.
5. 사용 중인 운영체제에 맞는 버전을 설치한다.

### 사진 아래 설명

> VESC 설정을 위해 노트북에 VESC Tool을 설치한다.
> 

---

# 2. VESC에 전원 연결

VESC Tool을 이용하려면 VESC에 먼저 전원이 공급되어야 한다.

차량의 배터리를 VESC에 연결한다. 이때 배터리 커넥터의 **양극과 음극이 올바르게 연결되었는지 반드시 확인한다.**

![image.png](VESC%20%ED%8A%9C%EB%8B%9D/image%202.png)

VESC를 설정할 때는 차량의 Powerboard를 별도로 켤 필요가 없다.

> **주의**
> 
> 
> 배터리 극성을 반대로 연결하면 VESC 또는 다른 전자장치가 손상될 수 있다.
> 

---

# 3. VESC와 노트북 연결

기존에 VESC가 Jetson과 USB로 연결되어 있다면 해당 USB 케이블을 Jetson에서 분리한다.

이후 VESC의 Micro USB 포트를 VESC Tool이 설치된 노트북에 연결한다.

차량과 노트북 사이의 거리가 짧다면 작업이 불편할 수 있으므로, 필요하면 더 긴 Micro USB 케이블을 사용한다.

![image.png](VESC%20%ED%8A%9C%EB%8B%9D/image%203.png)

> VESC에 연결된 USB 케이블을 Jetson에서 분리하고, VESC Tool을 실행할 노트북에 연결한다.
> 

---

## VESC Tool에서 장치 연결하기

1. 노트북에서 VESC Tool을 실행한다.
2. Welcome 화면 왼쪽 아래의 **AutoConnect** 버튼을 누른다.
3. 연결이 완료되면 화면 오른쪽 아래의 연결 상태가 변경되는지 확인한다.

![image.png](VESC%20%ED%8A%9C%EB%8B%9D/image%204.png)

---

# 4. VESC 펌웨어 업데이트

VESC 설정을 시작하기 전에 장치에 설치된 펌웨어를 최신 버전으로 업데이트한다.

## 작업 방법

1. VESC Tool 왼쪽 메뉴에서 **Firmware**를 선택한다.
2. **Download Latest** 버튼을 누른다.
3. 펌웨어 다운로드 및 설치가 끝날 때까지 기다린다.
4. 화면 아래의 상태 표시줄에서 진행 상황을 확인한다.
5. 업데이트가 끝나면 VESC가 재부팅되고 자동으로 다시 연결되는지 확인한다.

---

# 5. 조향용 Servo Output 활성화

F1TENTH 차량에서는 VESC의 출력 포트를 통해 조향용 서보모터를 제어한다.

따라서 펌웨어 업데이트 후 **Servo Output** 기능을 활성화해야 한다.

## 메뉴 경로

`App Settings → General → Enable Servo Output`

## 작업 방법

1. 왼쪽 메뉴에서 **App Settings**를 선택한다.
2. **General** 메뉴로 이동한다.
3. **Enable Servo Output**을 체크한다.
4. 화면 오른쪽의 **Write App Configuration** 버튼을 누른다.

Write App Configuration 버튼은 **아래쪽 화살표와 알파벳 A**가 표시된 버튼이다.

체크만 하고 저장 버튼을 누르지 않으면 설정이 실제 VESC에 기록되지 않으며 Servo Output도 활성화되지 않는다.

> 
> 
> 
> App 설정은 알파벳 **A**가 표시된 Write App Configuration 버튼으로 저장한다.
> 

![image.png](VESC%20%ED%8A%9C%EB%8B%9D/24927c7c-49a4-4ae7-98e0-b9f7aea73c68.png)

# 6. Motor Configuration XML 적용

펌웨어 업데이트가 완료되면 F1TENTH 차량용으로 제공된 **Motor Configuration XML 파일**을 불러온다.

XML 파일에는 모터를 구동하기 위한 기본 설정값이 포함되어 있다.

## 작업 방법

1. VESC Tool 상단 메뉴를 연다.
2. **Load Motor Configuration XML**을 선택한다.
3. 제공된 XML 파일을 선택한다.
4. 파일을 불러온 후 화면 오른쪽의 **Write Motor Configuration** 버튼을 누른다.

Write Motor Configuration 버튼은 **아래쪽 화살표와 알파벳 M**이 표시된 버튼이다.

앞으로 Motor Configuration의 값을 변경할 때도 반드시 이 버튼을 눌러야 변경 사항이 실제 VESC에 저장된다.

![image.png](VESC%20%ED%8A%9C%EB%8B%9D/image%205.png)

> 
> 
> 
> Motor 설정은 알파벳 **M**이 표시된 Write Motor Configuration 버튼으로 저장한다.
> 

## A 버튼과 M 버튼 구분

| 버튼 | 저장 대상 |
| --- | --- |
| Write App Configuration, `A` | App Settings, Servo Output 등의 애플리케이션 설정 |
| Write Motor Configuration, `M` | FOC, PID, ERPM 등의 모터 설정 |

---

# 7. FOC 모터 파라미터 측정

XML 파일의 기본값을 적용한 뒤 실제 연결된 모터의 FOC 파라미터를 측정한다.

FOC는 VESC가 브러시리스 모터를 구동하기 위해 사용하는 모터 제어 방식이다.

## 메뉴 경로

`Motor Settings → FOC`

## 작업 방법

1. 왼쪽의 **Motor Settings**를 선택한다.
2. **FOC** 메뉴로 이동한다.
3. 화면 아래에 있는 네 개의 측정 버튼을 확인한다.
4. 화면에 표시된 화살표 방향을 따라 버튼을 하나씩 누른다.
5. 각 단계에서 나타나는 안내 창에 따라 측정을 진행한다.

측정 중에는 모터가 소리를 내거나 갑자기 회전할 수 있다. 따라서 바퀴가 지면이나 주변 물체에 닿지 않아야 한다.

![image.png](VESC%20%ED%8A%9C%EB%8B%9D/image%206.png)

---

**측정 결과 적용**

모터 파라미터 측정이 정상적으로 완료되면 화면 아래의 측정 결과 항목이 초록색으로 표시된다.

1. 측정값이 초록색으로 표시되었는지 확인한다.
2. **Apply** 버튼을 누른다.
3. **Write Motor Configuration** 버튼을 눌러 결과를 VESC에 저장한다.

![image.png](VESC%20%ED%8A%9C%EB%8B%9D/image%207.png)

---

# 8. Openloop 설정 변경

FOC 파라미터 측정이 끝나면 화면 위쪽의 **Sensorless** 탭으로 이동한다.

다음 두 값을 모두 `0.01`로 설정한다.

| 설정 항목 | 입력값 |
| --- | --- |
| Openloop Hysteresis | 0.01 |
| Openloop Time | 0.01 |

값을 변경한 뒤 **Write Motor Configuration** 버튼을 눌러 VESC에 저장한다.

![image.png](VESC%20%ED%8A%9C%EB%8B%9D/image%208.png)

---

# 9. Speed PID Controller 튜닝

VESC가 목표 속도를 정확하게 따라가도록 Speed PID Controller를 조정한다.

먼저 모터의 실제 RPM 응답을 실시간으로 확인한다.

## 실시간 RPM 데이터 확인

### 메뉴 경로

`Data Analysis → Realtime Data`

## 작업 방법

1. 왼쪽의 **Data Analysis**를 선택한다.
2. **Realtime Data** 메뉴로 이동한다.
3. 화면 오른쪽의 **Stream Realtime Data** 버튼을 누른다.
4. 해당 버튼에는 `RT`라는 글자가 표시되어 있다.
5. 화면 위쪽의 **RPM** 탭으로 이동한다.
6. RPM 그래프가 실시간으로 표시되는지 확인한다.

![image.png](VESC%20%ED%8A%9C%EB%8B%9D/image%209.png)

---

## Step Response 측정

모터가 목표 RPM을 얼마나 빠르고 안정적으로 따라가는지 확인하기 위해 Step Response를 측정한다.

1. 화면 아래쪽의 입력창에 목표 RPM을 입력한다.
2. 원문에서는 일반적으로 `2,000~10,000 RPM` 범위의 값을 사용한다.
3. 입력창 옆의 재생 버튼을 눌러 모터를 회전시킨다.
4. 그래프에서 RPM 응답을 확인한다.
5. 테스트를 종료할 때는 **Anchor 또는 STOP 버튼**을 누른다.

> **주의**
> 
> 
> 재생 버튼을 누르면 모터와 바퀴가 실제로 회전한다. 바퀴 주변에 손이나 물체를 두지 않는다.
> 
> ![image.png](VESC%20%ED%8A%9C%EB%8B%9D/image%2010.png)
> 

---

## PID 게인 조정

좋은 RPM 응답은 다음과 같은 특징을 가진다.

- 목표 RPM까지 빠르게 상승한다.
- 목표값을 크게 초과하지 않는다.
- 정상상태에서 목표값과 실제값의 차이가 작다.
- 지속적인 진동이 발생하지 않는다.

## 메뉴 경로

`Motor Settings → PID Controllers → Speed Controller`

RPM 응답을 보면서 Speed Controller의 게인을 조정한다.

응답에 진동이 많이 발생한다면 PID 게인과 함께 **Speed PID Kd Filter** 값도 확인한다.

![image.png](VESC%20%ED%8A%9C%EB%8B%9D/image%2011.png)

---

# 10. VESC 하드웨어 최대 속도 설정

기본 Motor Configuration에는 모터 보호를 위한 안전한 최대 회전속도가 설정되어 있다.

VESC 펌웨어 내부의 최대 속도 제한을 변경하려면 다음 메뉴로 이동한다.

## 메뉴 경로

`Motor Settings → General`

이곳에서 다음 값을 변경할 수 있다.

- 정방향 최대 ERPM
- 역방향 최대 ERPM

값을 변경한 후 **Write Motor Configuration** 버튼으로 저장한다.

![image.png](VESC%20%ED%8A%9C%EB%8B%9D/image%2012.png)

---

## 하드웨어 제한과 소프트웨어 제한

VESC Tool에서 설정하는 Max ERPM은 **VESC 펌웨어 내부의 하드웨어 제한**이다.

그러나 실제 차량의 최대 속도를 변경하려면 VESC 설정만 변경해서는 안 된다.

Driver Stack의 Odometry Tuning 과정에서 사용하는 설정 파일에도 모터 ERPM 제한값이 존재하므로, 소프트웨어 제한도 함께 변경해야 한다.

차량 속도가 모터 ERPM으로 변환되는 관계를 확인한 뒤 모터와 차량에 안전한 최대 ERPM을 설정해야 한다.

> **주의**
> 
> 
> Max ERPM을 단순히 큰 값으로 설정하지 않는다. 모터 사양, 기어비, 차량 속도와 ERPM 변환 관계를 확인한 뒤 설정한다.
> 

| 제한 종류 | 설정 위치 | 역할 |
| --- | --- | --- |
| 하드웨어 제한 | VESC Tool의 Motor Settings → General | VESC가 허용하는 최대 ERPM 제한 |
| 소프트웨어 제한 | F1TENTH Driver Stack 설정 파일 | ROS 명령에서 허용하는 최대 ERPM 제한 |

---

# 주의사항

## 1. 차량을 반드시 띄운다

FOC 측정과 PID 테스트 중 모터가 실제로 회전한다. 차량을 바닥에 놓고 작업하면 갑자기 출발할 수 있다.

## 2. A 버튼과 M 버튼을 구분한다

- Servo Output과 같은 App 설정은 `A` 버튼으로 저장한다.
- FOC, PID, ERPM과 같은 Motor 설정은 `M` 버튼으로 저장한다.

값만 변경하고 Write 버튼을 누르지 않으면 실제 VESC에 저장되지 않는다.

## 3. FOC 측정값은 차량마다 다시 확인한다

XML 파일은 기본 설정을 불러오는 과정이다. 이후 실제로 연결된 모터의 파라미터를 측정하고 Apply 및 저장 과정을 거쳐야 한다.

## 4. PID는 그래프를 보고 조정한다

게인을 임의로 크게 올리는 것이 아니라, 동일한 목표 RPM에서 상승 시간, 오버슈트, 정상상태 오차 및 진동을 비교한다.

## 5. Max ERPM은 속도 명령과 연결된다

VESC의 하드웨어 Max ERPM과 Driver Stack의 소프트웨어 ERPM 제한은 서로 다른 설정이다. 실제 차량의 최대 속도를 바꾸려면 두 설정을 함께 확인해야 한다.

---

# 참고 자료

RoboRacer 공식 문서

**Configuring the VESC**

이 문서는 RoboRacer 공식 VESC 설정 페이지의 절차를 바탕으로 한국어로 정리하였다.

[https://f1tenth.readthedocs.io/en/main/getting_started/firmware/firmware_vesc.html](https://f1tenth.readthedocs.io/en/main/getting_started/firmware/firmware_vesc.html)

# IMU 설정

오른쪽의 **IMU 아이콘**을 선택하면 VESC에 내장된 IMU의 설정을 변경할 수 있다.

이 화면에서는 다음 항목을 설정한다.

- **IMU Type**: 사용할 IMU 센서 선택 (설정 유지)
- **Sample Rate**: IMU 데이터를 측정하는 주기 (설정유지)
- **Accel/Gyro Filter**: 가속도와 각속도 데이터의 노이즈를 줄이기 위한 필터
- **IMU AHRS Mode**: 가속도계와 자이로스코프 데이터를 결합하여 자세를 추정하는 방식
- **Rotation Roll/Pitch/Yaw**: 실제 IMU의 장착 방향을 차량 좌표계에 맞게 보정
- **Accel/Gyro Offset**: 센서가 정지해 있을 때 발생하는 영점 오차를 보정

현재는 내부 IMU를 사용하며, 샘플링 주파수는 **200 Hz**, 자세 추정 방식은 **Madgwick AHRS**로 설정되어 있다.

> `IMU Rotation Roll/Pitch/Yaw`는 차량의 실제 자세를 직접 변경하는 값이 아니다.
> 
> 
> IMU가 차량에 기울어지거나 회전된 상태로 장착되었을 때, **센서 좌표계를 차량 좌표계에 맞추기 위한 장착 방향 보정값**이다.
> 

---

## IMU 데이터 확인

화면 하단의 그래프에서는 IMU가 측정한 값을 실시간으로 확인할 수 있다.

![Screenshot from 2026-07-11 10-16-33.png](VESC%20%ED%8A%9C%EB%8B%9D/Screenshot_from_2026-07-11_10-16-33.png)

- **RPY**: 추정된 Roll, Pitch, Yaw 자세각
- **Accel**: X, Y, Z축 방향의 가속도
- **Gyro**: X, Y, Z축을 기준으로 한 각속도

따라서 그래프는 Roll, Pitch, Yaw를 **조절하는 기능이 아니라**, 센서의 출력과 차량의 움직임이 정상적으로 측정되는지 확인하는 기능이다.

![Screenshot from 2026-07-11 10-18-21.png](VESC%20%ED%8A%9C%EB%8B%9D/Screenshot_from_2026-07-11_10-18-21.png)

위 사진에서는 **Gyro 그래프**가 선택되어 있으며, IMU를 움직이거나 회전시키면 해당 회전축의 각속도 값이 크게 변화한다. 차량이 정지해 있을 때는 각 축의 값이 대체로 0 근처에 있어야 하며, 정지 상태에서도 한쪽으로 계속 치우쳐 있다면 Gyro Offset이나 센서 바이어스를 점검해야 한다.

또한 현재처럼 Magnetometer를 사용하지 않으면 Yaw는 자이로스코프의 각속도를 적분하여 계산되기 때문에 시간이 지날수록 **Yaw drift가 발생할 수 있다.** 그러나 Magnetometer를 키게되면 주위 전선의 자기장에 의해 영향을 많이 받으므로 키지 않는 것을 권장한다.

IMU 설정까지 완료하면, VESC의 기본적인 설정 과정은 모두 마무리된다.
