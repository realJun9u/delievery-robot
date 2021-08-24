# delievery-robot

## 사용 기술

### 시간별 진행 상황  
6.25  
우분투 설치, ros noetic 설치, rplidar 연결 완료  
라즈베리파이4 우분투 설치: https://blog.naver.com/roboholic84/221701573539  + SD카드 용량이 64GB이상이면 ext4로 포맷되어 FAT32로 포맷한 후 이미지를 설치해야한다.  
우분투 서버 20.04 LTS GUI 설치: https://velog.io/@kyoung99u/Ubuntu-GUI-%EC%84%A4%EC%B9%98   
```
sudo apt-get install ubuntu-desktop
sudo apt-get install indicator-appmenu-tools (hud service not connected 오류 해결)
startx
```
라즈베리파이4 우분투 와이파이 설정: https://vanilet.tistory.com/16  
와이파이 설정, 시간대 변경  
```
sudo vim /etc/netplan/50-cloud-init.yaml  
sudo netplan generate  
sudo netplan apply  
sudo dpkg-reconfigure tzdata 에서 Asia - Seoul 선택  
``` 
ros noetic 설치: http://wiki.ros.org/noetic/Installation/Ubuntu  
rplidar_ros 클론, 빌드: http://wiki.ros.org/rplidar  
```
git clone https://github.com/tu-darmstadt-ros-pkg/hector_slam.git
ls -l /dev |grep ttyUSB
sudo chmod 666 /dev/ttyUSB0
Rviz에서 보기
roslaunch rplidar_ros view_rplidar.launch
콘솔에서 보기
roslaunch rplidar_ros rplidar.launch
rosrun rplidar_ros rplidarNodeClient
```
![image](https://user-images.githubusercontent.com/78460105/123499509-56742480-d672-11eb-9abe-ffae6076edfd.png)  

6.26  
hector slam 적용: https://doongdoongeee.tistory.com/104  
``` 
git clone https://github.com/tu-darmstadt-ros-pkg/hector_slam.git
gedit catkin_ws/src/hector_slam/hector_mapping/launch/mapping_default.launch 파일 수정
gedit catkin_ws/src/hector_slam/hector_slam_launch/launch/tutorial.launch 파일 수정
catkin_make
source /devel/setup.bash
roslaunch rpldiar_ros rplidar.launch
roslaunch hector_slam_launch tutorial.launch
```
![image](https://user-images.githubusercontent.com/78460105/123499975-87098d80-d675-11eb-9006-c118b6d62d38.png)  

6.28  
Cartographer 적용: https://medium.com/robotics-weekends/2d-mapping-using-google-cartographer-and-rplidar-with-raspberry-pi-a94ce11e44c5  
```
git clone https://github.com/Andrew-rw/gbot_core.git
catkin_make
source ./devel/setup.bash
roslaunch gbot_core gbot.launch
roslaunch gbot_core visualization.launch
```
![image](https://user-images.githubusercontent.com/78460105/124073770-74c88e80-da7d-11eb-8fd8-9c4b37ee92fc.png)  

7.1  
resserial: http://wiki.ros.org/rosserial  
rosserial_arduino: http://wiki.ros.org/rosserial_arduino/Tutorials/Arduino%20IDE%20Setup  
arduino IDE 설치, wiringPi 라이브러리 설치  
rosserial-arduino, rosserial-python 설치
```
git clone https://github.com/WiringPi/WiringPi.git
./build
아두이노에서 MPU9250_raw.ino, MPU9250_DMP6.ino 업로드하면 IMU값 출력

sudo apt install ros-noetic-rosserial
sudo apt install ros-noetic-rosserial-arduino
sudo apt install ros-noetic-rosserial-python
rm -rf ros_lib
cd ~/Arduino/libraries -> ~/sketchbook/libraries에 하는 거 아님.
rosrun rosserial_arduino make_libraries.py.

/usr/bin/env: ‘python’: No such file or directory
sudo apt install python-is-python3
```  

7.4  
rosserial-arduino 튜토리얼 학습  
Arduino IDE 에서 rosserial 사용하려면 #include <ros.h> #include <std_msgs / String.h> 필요  

7.12  
터틀봇3 사용 https://emanual.robotis.com/docs/en/platform/turtlebot3/quick-start/

7.14
rosserial로 topic publish하여 로봇 구동. 방향키 입력으로 할 수 있게 해야함.  
아두이노 방향 LOW 일떄 안돌아감.  

7.15  
teleop_twist_keyboard 패키지가 UIOJKLM<>로 geometry_msgs/Twist 메시지 퍼블리시 한다.

7.16  
코드상은 문제가 없어 보이지만 출력이 제대로 발생하지 않는 경우가 있다.  
배터리의 성능(전류)에 따라 모터의 성능이 차이가 많이나고 11.1V(3.7x3), 2200mAh 배터리는 구동이 안되었다.  
모터 드라이버의 발열이 심함. 배터리에서 드라이버로 들어가는 도선이 전류를 감당하지 못하는 것 같음.

7.17  
모터의 안정적인 구동을 위해 PID 제어가 필요하다. PID 라이브러리: https://playground.arduino.cc/Code/PIDLibrary/  
P, I, K 파라미터에 따라 안정도가 달라짐 - 자동제어에서 배움  
엔코더 값을 받으려면 회전수, 방향을 알아야 해서 각 두개의 인터럽트 핀이 필요. Encoder 라이브러리: https://www.arduino.cc/reference/en/libraries/encoder/
엔코더 라이브러리에서 둘 중 하나만 인터럽트여도 괜찮게 동작하게 한다고 함.

7.19  
Arduino Mega 2560 은 2, 3, 18, 19, 20, 21이 Interrupt 핀. 20, 21은 SDA, SCL로 MPU9250 연결 되어야 하므로 2, 3, 18, 19를 Encoder의 첫번째 핀으로 사용해야한다.  
Cytron FD04A 4채널 드라이버에서 안정적인 구동을 위해 L298N 두 개로 교체. 방향 제어에 각 2개, PWM 1개 씩 해서 모터 하나당 총 4개의 핀 필요.
드라이버 교체 완료. 강압회로, 스위치 구성 완료. 배터리 여전히 문제  

7.21  
회로기판을 바꾸고 구성을 깔끔하게 해서 합선 위험을 피하는 설계. 배터리 선에서 점퍼케이블을 꼬아 구동하니 점퍼케이블이 녹았음. -> 모터 드라이버에 들어가는 12V 전선은 점퍼를 쓰지 않고 납땜하여 높은 허용전류를 갖는 도선 사용.  
메카넘 휠의 제자리 회전, 대각 이동 완벽히 됨.  
서스펜션 장치가 없어 회전하거나 주행 중 떨림 발생.  
3D 프린터로 로봇 팔 파츠 뽑기 시작.  
Cartographer, gbot 설치.  

7.22  
기판 최종 변경 완료. 전방향 동작 구현 완료.  

7.23  
MsTimer2 라이브러리 사용하려면 Mega 2560에서 PWM 9, 10 핀 사용 불가하여 13, 12로 교체함.  
모터를 해체하고 옆면을 보강했는데 여러 문제 발생. 현재 고침. 3번 모터가 느린 경향.- PID 제어로 해결 예정.  

7.29  
모터 속도가 차이나서 잘 되던 전방향 이동 동작이 제대로 되지 않음.  
모터 자체가 고장인지, 여러 전원 공급 요소에 문제가 있는지 파악이 어려움.  
배터리를 리튬이온배터리팩을 3S2P로 보호 회로부터 충전 회로까지 설계하고 자제 주문 완료.  
적재, 하차부분은 컨베이어 벨트와 서보 모터를 이용한 푸시 방식으로 최대 3개 적재되게 함.  
이번 주는 대체로 설계, 자제 탐구에 시간을 많이 썼고, 모터 이상으로 odometry를 SLAM 파트에 적용해 볼 수 없었다.  

8.21
오래동안 자재가 오지 않아 진행이 어려웠다.  
ttyUSB 포트 고정을 위해 심볼릭 링크 생성  
```bash
dmesg | grep ttyUSB
lsusb
udevadm info -a /dev/ttyUSBx | grep serial
sudo vim /etc/udev/rule.d/99-sub-serial.rules
```
```vim
SUBSYSTEM=="tty", ATTRS{idVendor}=="10c4", ATTRS{idProduct}=="ea60", SYMLINK+="rplidar"
SUBSYSTEM=="tty", ATTRS{idVendor}=="1a86", ATTRS{idProduct}=="7523", SYMLINK+="arduino"
```
```
sudo udevadm trigger
```  
이제까지 모터 속도차이로 생각했던 문제들이 단순 바퀴 방향 문제였다.. 차체 옆면 보강 후부터 이런 현상이 생겼는데 바퀴를 다시 조립하는 과정에서 생긴 문제인가 보다..  

8.24  
로봇 오픈소스인 linorobot을 참고하였다. https://github.com/linorobot/linorobot/wiki/1.-Getting-Started  
```bash
git clone https://github.com/linorobot/lino_install
cd lino_install
sudo apt-get install dphys-swapfile
./install mecanum rplidar

```
install 파일을 수정해야 noetic에서 사용가능.  
```vim
...
python-dev 지우고 python-dev-is-python3
python-gudev 지우고 gir1.2-gudev-1.0 \
python-is-python3

sudo easy_install pip 지우고 sudo apt install python3-pip
sudo python2.7 -m pip install -U platformio 지우고 sudo pip3 install -U platformio
...
rplidar 설치 부분을 모두 지워야한다.
```
rplidar는 ros-noetic-rplidar가 apt로 설치 불가하여 깃 클론으로 설치했다. https://github.com/robopeak/rplidar_ros  
아래는 노트북 ubuntu에 설치한다.  
```vim
cs
git clone https://github.com/linorobot/lino_pid.git
git clone https://github.com/linorobot/lino_msgs.git
git clone https://github.com/linorobot/lino_visualize.git
sudo apt-get install ros-$(rosversion -d)-teleop-twist-keyboard
cm
```  
다음에는 teensy를 연결하여 설정을 마무리해야한다.  
Robot's computer  
export ROS_MASTER_URI=http://robot-ip:11311  
export ROS_HOSTNAME=robot-ip  

Development computer  
export ROS_MASTER_URI=http://robot-up:11311  
export ROS_HOSTNAME=devcom-ip  

노트북 환경인 wsl2의 외부 접속이 필요할 수 있을 것 같아서 9929번으로 뚫어놨다.    
https://blog.dalso.org/linux/wsl2/11430  
https://blog.dalso.org/it/11432  
![image](https://user-images.githubusercontent.com/78460105/130570487-23fd00ce-4e96-4bc8-adae-ac05e3db03fa.png)

## 에러 대응  
apt update, upgrade 오류  
```
- sudo rm /var/lib/apt/lists/lock
- sudo rm /var/cache/apt/archives/lock
- sudo rm /var/lib/dpkg/lock*

sudo dpkg --configure -a  를 하시고 sudo apt update
```
