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

## 에러 대응  
apt update, upgrade 오류  
```
- sudo rm /var/lib/apt/lists/lock
- sudo rm /var/cache/apt/archives/lock
- sudo rm /var/lib/dpkg/lock*

sudo dpkg --configure -a  를 하시고 sudo apt update
```
