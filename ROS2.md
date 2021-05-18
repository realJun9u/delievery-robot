### ROS2 
- ROS2 용어 정리  
Node : 최소 단위의 실행 가능한 프로세스. 하나의 실행 가능한 프로그램  
Package : 하나 이상의 노드, 노드 실행을 위한 정보 등을 묶어 놓은 것. 패키지의 묶음을 메타패키지.  
Message : 노드간의 데이터를 주고 받는 방식은 메시지 통신이다. 메시지는 integer, floating point, boolean, string 같은 변수형태. 메시지안에 메시지가 있는 데이터 구조, 메시지들의 배열 같은 구조도 사용가능.  
Topic : publish-subscribe 개념 방식. 단방향, 연속적 통신. 보내는 쪽이 퍼블리셔 노드, 받는 쪽이 서브스크라이버 노드가 된다. 1:N, N:1, N:N 통신 가능. 센서와 같은 일방적으로 데이터를 보낼 때 주로 사용. ROS에서 가장 많이 사용한다. 노드가 퍼블리셔인 동시에 서브스크라이버가 될 수 있다.   
Service : 서버-클라이언트 개념의 양방향, 일회성 통신. 클라이언트가 서버에 요청하고, 서버가 클라이언트에 응답. 토픽과 달리 일회성 통신.  
Action : 서버-클라이언트 개념인데, 서비스와 다른 점은 중간에 피드백이 있다. 액션 목표 전달 - 액션 피드백 전달 - 액션 결과 전달. 사용 빈도는 낮다.  
메시지 통신 개념 : 마스터가 노드들의 정보를 가지고 노드 간의 매칭을 하여 통신 시작.  
Name(네임) : 노드, 메시지가 가지는 고유의 식별자. ROS는 graph라는 abstract data type 지원.
좌표 변환(TF) : 각 조인트들의 상대 좌표 변환  
클라이언트 라이브러리 : 다양한 프로그래밍 언어 지원  
#### DDS
ROS2에서 사용하는 표준 통신 미들웨어. 실시간 데이터 전송 보장, 임베디드 시스템에 사용. ROS Master가 없어도 DDS 프로그램(노드)간 통신 가능. 다양하게 강화된 프로토콜.  
- listener, talker 예제로 노드를 실행하고 rqt_graph를 실행하면 퍼블리셔,서브크스라이버 노드 확인 가능.  
- export RMW_IMPLEMENTATION=rmw_fastrtps_cpp 와 같이 ROS2를 지원하는 RMW를 여러가지 사용 가능. 서로 달라도 통신이 가능하다.  
- UDP 기반 멀티캐스트 통신이므로 별도 설정 없이는 같은 네트워크의 모든 노드가 연결된다. listener, talker가 자동으로 통신되는 이유인데, export ROS_DOMAIN_ID=11 와 같이 DDS의 domain을 변경하고 domain을 동일하게 맞춘 노드끼리 연결이 된다. 0~232 까지 사용 가능.  
#### turtlesim
ROS 학습용 패키지로 노드,토픽,서비스,액션,파라미터에 대한 기본적인 학습 및 CLI툴,rqt 툴 연동 체험.  
turtlesim 패키지에 포함된 노드들  
```
$ ros2 pkg executables turtlesim
turtlesim draw_square
turtlesim mimic
turtlesim turtle_teleop_key
turtlesim turtlesim_node
```
turtlesim 키보드 조종 예제 - 여기서 중요한 것은 두 노드간의 동작이 단순히 키보드 값을 전달하여 움직이는게 아니라 눌려진 키보드의 키값에 해당되는 병진 속도(linear velocity)와 회전 속도(angular velocity)를 geometry_msgs 패키지의 Twist 메시지 형태로 보내고 받는 다는 것이다.  
```
$ ros2 run turtlesim turtlesim_node
$ ros2 run turtlesim turtle_teleop_key
```
노드,토픽,서비스,액션 조회  
```
$ ros2 node list
$ ros2 topic list
$ ros2 service list
$ ros2 action list
$ rqt_graph
```
#### 토픽
비동기식 단방향 메시지 통신. 퍼블리셔-서브스크라이버 구조. msg 인터페이스 사용.
1. 노드 정보 확인
```
$ ros2 node info /turtlesim
```
2. 토픽 리스트 확인
```
$ ros2 topic list -t : -t는 각 메시지의 Type을 함께 표시
```
3. 토픽 정보 확인
```
$ ros2 topic info /turtle1/cmd_vel
```
4. 토픽 내용 확인
```
$ ros2 topic echo /turtle1/cmd_vel 입력하고 키보드로 움직여 보면 topic이 날아온다
```
5. 토픽 대역폭 확인
```
$ ros2 topic bw /turtle1/cmd_vel
```
6. 토픽 주기 확인
```
$ ros2 topic hz /turtle1/cmd_vel
```
7. 토픽 지연 시간 확인
```
$ ros2 topic delay /TOPIC_NAME
```
8. 토픽 발행
```
ros2 topic pub <topic_name> <msg_type> "<args>"
$ ros2 topic pub --once /turtle1/cmd_vel geometry_msgs/msg/Twist "{linear: {x: 2.0, y: 0.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: 1.8}}"
pub : 발행 --once : 한번 /turtle1/cmd_vel : 토픽 이름 geometry_msgs/msg/Twist : 토픽 타입 linear : 병진 속도 angular : 회전 속도
--once -> --rate 1 : 1Hz의 주기로 발행
```
9. bag 기록 - 발행하는 토픽을 파일 형태로 저장하고 불러와 재생하는 기능
```
ros2 bag record <topic_name1> <topic_name2> <topic_name3>
junq@DESKTOP-964Q3V3:~/Desktop$  ros2 bag record /turtle1/cmd_vel
[INFO]: Opened database 'rosbag2_2021_05_17-16_03_41/rosbag2_2021_05_17-16_03_41_0.db3' for READ_WRITE.
[INFO]: Listening for topics...
[INFO]: Subscribed to topic '/turtle1/cmd_vel'
[INFO]: All requested topics are subscribed. Stopping discovery...
```
10. bag 정보  
```
$ ros2 bag info rosbag2_2021_05_17-16_03_41/
Files:             rosbag2_2021_05_17-16_03_41_0.db3
Bag size:          16.8 KiB
Storage id:        sqlite3
Duration:          9.654s
Start:             May 17 2021 16:03:46.539 (1621235026.539)
End:               May 17 2021 16:03:56.193 (1621235036.193)
Messages:          8
Topic information: Topic: /turtle1/cmd_vel | Type: geometry_msgs/msg/Twist | Count: 8 | Serialization Format: cdr
```
11. bag 재생  
```
junq@DESKTOP-964Q3V3:~/Desktop$ ros2 bag play rosbag2_2021_05_17-16_03_41/
[INFO]: Opened database 'rosbag2_2021_05_17-16_03_41//rosbag2_2021_05_17-16_03_41_0.db3' for READ_ONLY.
```
12. ROS interface  
토픽, 서비스, 액션 에서 사용되는 데이터의 형태를 ROS 인터페이스라고 한다.
각각 msg, srv, action interface를 사용한다.  

13. 메시지 인터페이스
/turtle1/cmd_vel 토픽은 geometry_msgs/msgs/Twist 형태 Twist 데이터 형태를 자세히 보면 Vector3 linear과 Vector3 angular 이라고 되어 있다. 이는 메시지 안에 메시지를 품고 있는 것으로 Vector3는 다시 float64 형태에 x, y, z 값이 존재한다.
```
$ ros2 interface show geometry_msgs/msg/Twist
Vector3 linear
Vector3 angular
```
```
$ ros2 interface show geometry_msgs/msg/Vector3
float64 x
float64 y
float64 z
```
```
$ ros2 interface list - 모든 인터페이스 목록
$ ros2 interface packages - msg,srv,action 인터페이스를 담은 패키지 목록
$ ros2 interface package turtlesim - 패키지에 포함된 인터페이스
$ ros2 interface proto geometry_msgs/msg/Twist - 인터페이스 기본 형태 보여줌
```
#### 서비스
동기식 양방향 메시지 통신. 서버-클라이언트 구조. srv 인터페이스 사용
1. 서비스 목록 확인
```
$ ros2 service list -t
/clear [std_srvs/srv/Empty]
/kill [turtlesim/srv/Kill]
/reset [std_srvs/srv/Empty]
```
2. 서비스 타입 확인
```
$ ros2 service type /clear
std_srvs/srv/Empty
```
3. 서비스 찾기 - 특정 타입을 가진 서비스 확인
```
$ ros2 service find std_srvs/srv/Empty 
/clear
/reset
```
4. 서비스 요청
```
ros2 service call <service_name> <service_type> "<arguments>"
junq@DESKTOP-964Q3V3:~/Desktop$ ros2 service call /clear std_srvs/srv/Empty
- 거북이 궤적 지우기
junq@DESKTOP-964Q3V3:~/Desktop$ ros2 service call /kill turtlesim/srv/Kill "name: 'turtle1'"
- 거북이 제거
junq@DESKTOP-964Q3V3:~/Desktop$ ros2 service call /reset std_srvs/srv/Empty 
- 초기화
junq@DESKTOP-964Q3V3:~/Desktop$ ros2 service call /turtle1/set_pen turtlesim/srv/SetPen "{r: 255, g: 255, b: 255, width: 10}"
- 궤적 색, 두께 조정
junq@DESKTOP-964Q3V3:~/Desktop$ ros2 service call /spawn turtlesim/srv/Spawn "{x: 5.5, y: 7, theta: 1.57, name: 'raffaello'}"
- 거북이 생성
```
5. 서비스 인터페이스
```
junq@DESKTOP-964Q3V3:~/Desktop$ ros2 interface show turtlesim/srv/Spawn.srv
float32 x
float32 y
float32 theta
string name # Optional.  A unique name will be created and returned if this is empty
---
string name
```
#### 액션
비동기식+동기식 양방향 메시지 통신. 액션 클라이언트가 액션 목표를 지정하고 액션 서버가 목표를 받아 특정 태스크를 수행하면서 중간에 액션 피드백과 최종에 액션 결과를 전송. ROS2에서는 토픽과 서비스의 혼합으로 3개의 서비스, 2개의 토픽으로 구성. goal/feedback/result 데이터는 msg 및 srv 인터페이스의 변형으로 action 인터페이스이다.
<img src="https://cafeptthumb-phinf.pstatic.net/MjAyMDA4MzFfMTk0/MDAxNTk4ODQyNTQzMzU2.m5EI-vAFEqUp1RCH3N6AXQLQ4qtuEdeK5xoDI8dBFkog.l4uHomJU0HUHJCWRV2Cz07dyR3FIi-b8kLbmZo7remcg.PNG/Selection_060.png?type=w1600">  

거북이를 방향키가 아닌 G,B,V,C,D,E,R,T로 움직이는 것이 액션 목표이다.
```
[WARN]: Rotation goal received before a previous goal finished. Aborting previous goal
[INFO]: Rotation goal completed successfully
[INFO]: Rotation goal canceled
```
1. 노드 정보
```
$ ros2 node info /turtlesim
  Action Servers:
    /turtle1/rotate_absolute: turtlesim/action/RotateAbsolute
  Action Clients:
```
```
$ ros2 node info /teleop_turtle
  Action Servers:

  Action Clients:
    /turtle1/rotate_absolute: turtlesim/action/RotateAbsolute
```
rotate_absolute 액션은 5가지로 세분화
```
/turtle1/rotate_absolute/_action/send_goal: turtlesim/action/RotateAbsolute_SendGoal
/turtle1/rotate_absolute/_action/cancel_goal: action_msgs/srv/CancelGoal
/turtle1/rotate_absolute/_action/status: action_msgs/msg/GoalStatusArray
/turtle1/rotate_absolute/_action/feedback: turtlesim/action/RotateAbsolute_FeedbackMessage
/turtle1/rotate_absolute/_action/get_result: turtlesim/action/RotateAbsolute_GetResult
```
2. 액션 목록
```
$ ros2 action list -t
/turtle1/rotate_absolute [turtlesim/action/RotateAbsolute]
```
3. 액션 정보
```
$ ros2 action info /turtle1/rotate_absolute
Action: /turtle1/rotate_absolute
Action clients: 1
    /teleop_turtle
Action servers: 1
    /turtlesim
```
4. 액션 목표 전달
```
ros2 action send_goal <action_name> <action_type> "<values>"
$ ros2 action send_goal /turtle1/rotate_absolute turtlesim/action/RotateAbsolute "{theta: 1.5708}"
--feedback 옵션을 주면 남은 회전 량을 피드백으로 표시
```
5. 액션 인터페이스
```
$ ros2 interface show turtlesim/action/RotateAbsolute 
# The desired heading in radians
float32 theta
---
# The angular displacement in radians to the starting position
float32 delta
---
# The remaining rotation in radians
float32 remaining
```
#### 파라미터
각 노드에서 Parameter server를 실행시켜 외부의 Parameter client와 통신으로 파라미터를 변경하는 것으로 서비스와 동일하다. 서비스는 요청과 응답이 목적이면 파라미터는 노드 내 매개변수를 서비스 통신 방법으로 노드 내부, 외부에서 쉽게 지정, 변경(Set)하고 가져와서 사용(Get)할 수 있게 한다.  
<img src="https://cafeptthumb-phinf.pstatic.net/MjAyMDA5MDNfMjYx/MDAxNTk5MDkwMjIxNTg0.TnDl5CERabC3mqytAncMntf4Ba40lrduufzct1BGo34g.wf6_3Wicl6RH7yBOrPvcUQk1N64H8L29lKKmF-uRceMg.PNG/Selection_068.png?type=w1600">  
모든 노드가 자신만의 Parameter server를 가지고 있고, Parameter client도 포함시킬 수 있어서 자신과 다른 노드의 파라미터를 읽고 쓸 수 있다. 글로벌 매개변수처럼 사용할 수 있게되어 변화 가능한 프로세스 만들 수 있다. 각 파라미터는 yaml 파일 형태의 파라미터 설정 파일을 만들어 초기 파라미터 값 설정하고 노드 실행시 설정 파일을 불러와 사용할 수 있다.  
1. 파라미터 목록  
```
$ ros2 param list
/teleop_turtle:
  scale_angular
  scale_linear
  use_sim_time
/turtlesim:
  background_b
  background_g
  background_r
  use_sim_time
```
2. 파라미터 내용
```
$ ros2 param describe /turtlesim background_b
Parameter name: background_b
  Type: integer
  Description: Blue channel of the background color
  Constraints:
    Min value: 0
    Max value: 255
    Step: 1
```
3. 파라미터 읽기
```
ros2 param get <node_name> <parameter_name>
$ ros2 param get /turtlesim background_r
Integer value is: 69
$ ros2 param get /turtlesim background_g
Integer value is: 86
$ ros2 param get /turtlesim background_b
Integer value is: 255
```
4. 파라미터 쓰기
```
ros2 param set <node_name> <parameter_name> <value>
turtlesim 배경색 바꾸기
$ ros2 param set /turtlesim background_r 148
Set parameter successful
$ ros2 param set /turtlesim background_g 0
Set parameter successful
$ ros2 param set /turtlesim background_b 211
Set parameter successful
```
5. 파라미터 저장
```
$ ros2 param dump /turtlesim 
Saving to:  ./turtlesim.yaml
$ cat turtlesim.yaml 
/turtlesim:
  ros__parameters:
    background_b: 211
    background_g: 0
    background_r: 148
    use_sim_time: false
$ ros2 run turtlesim turtlesim_node --ros-args --params-file ./turtlesim.yaml
- 노드 실행 시 지정된 파라미터 값 이용
```
6. 파라미터 삭제
```
junq@DESKTOP-964Q3V3:~/Desktop$ ros2 param delete /turtlesim background_b
junq@DESKTOP-964Q3V3:~/Desktop$ ros2 param list /turtlesim 
  background_g
  background_r
  use_sim_time
```
#### ROS 도구
##### CLI tools  
개발환경 및 빌드, 테스트 툴 colcon / 데이터 기록, 재생, 관리 툴 ros2bag 외 20가지  
action, bag, component, daemon, doctor, extension_points, extensions, interface, launch, lifecycle, multicast, node, param, pkg, run, security, service, topic, wtf  
사용법 : ros2 [verbs] [sub-verbs] [options] [arguments]  
```
run : 특정 패키지의 특정 노드 실행 (1개. excutable에 따라 복수 노드도 실행 가능)  
launch : 특정 패키지의 특정 런치 파일 실행 (0 ~ 복수개의 노드)
```
ros2 pkg	
```
create 새로운 ROS 2 패키지 생성
executables 지정 패키지의 실행 파일 목록 출력
list 사용 가능한 패키지 목록 출력
prefix 지정 패키지의 저장 위치 출력
xml	지정 패키지의 패키지 정보 파일(xml) 출력
```
ros2 node	
```
info 실행 중인 노드 중 지정한 노드의 정보 출력
list 실행 중인 모든 노드의 목록 출력
```
ros2 topic
```
bw 지정 토픽의 대역폭 측정
delay 지정 토픽의 지연시간 측정
echo 지정 토픽의 데이터 출력
find 지정 타입을 사용하는 토픽 이름 출력
hz 지정 토픽의 주기 측정
info 지정 토픽의 정보 출력
list 사용 가능한 토픽 목록 출력
pub 지정 토픽의 토픽 발행
type 지정 토픽의 토픽 타입 출력
```
ros2 service
```
call 지정 서비스의 서비스 요청 전달
find 지정 서비스 타입의 서비스 출력
list 사용 가능한 서비스 목록 출력
type 지정 서비스의 타입 출력
```
ros2 action
```
info 지정 액션의 정보 출력
list 사용 가능한 액션 목록 출력
send_goal	지정 액션의 액션 목표 전송
```
ros2 interface
```
list 사용 가능한 모든 인터페이스 목록 출력
package 특정 패키지에서 사용 가능한 인터페이스 목록 출력
packages 인터페이스 패키지들의 목록 출력
proto 지정 패키지의 프로토타입 출력
show 지정 인터페이스의 데이터 형태 출력
```
ros2 param
```
delete 지정 파라미터 삭제
describe 지정 파라미터 정보 출력
dump 지정 파라미터 저장
get 지정 파라미터 읽기
list 사용 가능한 파라미터 목록 출력
set	지정 파라미터 쓰기
```
ros2 bag
```
info 저장된 rosbag 정보 출력
play rosbag 재생
record	rosbag 기록
```
ros2 extensions
```
(-a),(-v) ros2cli의 extension 목록 출력
```
ros2 extension_points
```
(-a),(-v) ros2cli의 extension point 목록 출력
```
ros2 daemon
```
start daemon 시작
status daemon 상태 보기
stop daemon 정지
```
ros2 multicast
```
receive multicast 수신
send multicast 전송
```
ros2 doctor,wtf(where's the file)
```
hello,(-r),(-rf),(-iw) ROS 설정 및 네트워크, 패키지 버전, rmw 미들웨어 등과 같은 잠재적 문제를 확인하는 도구
```
ros2 lifecycle
```
get 라이프사이클 정보 출력
list 지정 노드의 사용 가능한 상태천이 목록 출력
nodes 라이프사이클을 사용하는 노드 목록 출력
set 라이프사이클 상태 전환 트리거
```
ros2 component
```
list 실행 중인 컨테이너와 컴포넌트 목록 출력
load 지정 컨테이너 노드의 특정 컴포넌트 실행
standalone 표준 컨테이너 노드로 특정 컴포넌트 실행
types 사용 가능한 컴포넌트들의 목록 출력
unload 지정 컴포넌트의 실행 중지
```
ros2 security
```
create_key 보안키 생성
create_keystore 보안키 저장소 생성
create_permission 보안 허가 파일 생성
generate_artifacts 보안 정책 파일를 이용하여 보안키 및 보안 허가 파일 생성 
generate_policy 보안 정책 파일(policy.xml) 생성
list_keys 보안키 목록 출력
```
##### GUI 기반 RQT  
RQT는 플러그인 형태로 다양한 도구와 인터페이스를 구현할 수 있는 GUI 프레임워크이며 다양한 목적의 GUI 툴을 모아둔 ROS의 종합 GUI 툴박스이다.  
```
1. rqt를 실행하여 원하는 플러그인 실행.
2. $ ros2 run rqt_msg rqt_msg - rqt 관련 패키지들의 노드들을 하나씩 실행
3. $ rqt_graph, rqt_topic - 단축 명령어 이용
```
1. Node Graph
2. Topic Monitor
3. Message Publisher
4. Message Type Browser
5. Service Caller
6. Parameter(Dynamic) Reconfigure
7. Plot
8. Image View
9. Console
##### RViz  
3차원 시각화툴. 레이저, 카메라 등 센서 데이터 시각화. 로봇 외형과 계획된 동작 표현  
##### Gazebo  
3차원 시뮬레이터. 물리 엔진을 탑재하여 로봇, 센서, 환경 모델 등 지원.  
#### ROS2 표준 단위 : SI 유도 단위
길이: m 질량: kg 시간: s 전류: A 각: rad 주파수: Hz 힘: N 일률: W 전압: V 온도: C 자기장: T  
##### ROS2 축 방향 규칙  
모든 좌표계는 오른손 법칙으로 표현한다.  
로봇에서 사용하는 LiDAR 및 IMU, Torque 센서들도 제조사별로 서로 다른 좌표계를 사용할 수도 있으며 좌표 변환 API가 있더라도 기본 출력을 무엇인가를 미리 알아볼 필요가 있다. 예를 들어 IMU가 NED 타입(x north, y east, z down, relative to magnetic north) 이냐 ENU 타입(x-east, y-north, z-up, relative to magnetic north)이냐를 잘 살펴봐야하고 각 센서의 값을 사용하기에 전에 각 센서들의 좌표를 로봇 좌표에 맞추어 적절한 좌표 변환이 필요하다.  
1. 기본 3축 : x forward, y left, z up  
시각화 툴 RViz나 3차원 시뮬레이터 Gazebo에서 이러한 기본 3축의 표현에 있어서 RGB의 원색으로 표현하는데 순서대로 Red는 x 축, Green은 y축, Blue는 z축을 의미한다.  
2. ENU 좌표(east north up) : 큰 맵을 사용하는 드론, 실외 자율 주행 로봇에서 사용  
3. Suffix Frames : 기본 3축, ENU 좌표에서 벗어나는 경우 집미사를 이용하여 사용.접미사로 '_optical' '_ned' 가 있다.  
3-1) `_optical` 접미사  
컴퓨터 비전 분야의 경우, 카메라 좌표계로 많이 사용되는 z forward, x right, y down를 사용하게 되는데 이럴 경우에는 카메라 센서의 메시지에 `_optical` 접미사를 붙여 구분한다. 이때에는 z forward, x right, y down의 카메라 좌표계와 x forward, y left, z up의 로봇 좌표계 간의 TF(transform)가 필요하다.  
3-2) `_ned` 접미사  
실외에서 동작하는 시스템의 경우, 사용하는 센서 및 지도에 따라 ENU가 아닌 NED (north east down, ) 좌표계를 사용해야 할 때가 있다. 이때에는 `_ned` 접미사를 붙여 구분한다.  
##### ROS2 회전 표현 규칙  
1) 쿼터니언 (quaternion)  
- 간결한 표현방식으로 가장 널리 사용됨 (x, y, z, w)  
-특이점 없음 (No singularities)  
2) 회전 매트릭스 (rotation matrix)  
- 특이점 없음 (No singularities)  
3) 고정축 roll, pitch, yaw (fixed axis roll, pitch, yaw about X, Y, Z axes respectively)  
- 각속도에 사용  
#### ROS2 파일 시스템
ROS2의 응용프로그램은 패키지 단위로 개발,관리된다. 패키지는 노드를 하나 이상 포함하거나, 다른 노드를 실행하기 위한 launch와 같은 실행 및 설정 파일을 포함한다. 이러한 패키지는 메타패키지라는 공통된 목적의 패키지들을 모아둔 패키지 집합 단위로 관리되기도한다. Navigation2 메타패키지는 20여 개의 패키지로 구성. 각 패키지는 package.xml이라는 파일을 포함하고 있는데, XML파일은 패키지의 이름,저작자,라이선스,의존성 패키지 등의 정보를 담고있다. ROS2 빌드 시스템은 ament로 기본적으로 CMake를 이용하고 패키지 폴더에 CMakeLists.txt 파일에 빌드 설정을 기술한다.  
패키지 설치는 바이너리 설치, 소스 코드 설치 두가지가 있다.  
바이너리 설치 방법은 sudo apt install 해당 패키지 명만으로도 설치를 할 수 있으며 설치된 파일은 `/opt/ros/foxy` 에 저장되어 `ros2 run` 이나 `ros2 launch`로 해당 패키지내의 실행 가능한 노드를 실행시킬 수 있다.  
소스 코드 설치 방법은 사용자 작업폴더 (예: '~/robot_ws/src')에 `git clone` 명령어를 통해 원격의 리포지토리를 복사 후 빌드하는 방법이다.  
1. 기본 설치 폴더
```
/opt/ros/foxy
▪ /bin 실행 가능한 바이너리 파일  
▪ /cmake 빌드 설정 파일  
▪ /include 헤더 파일  
▪ /lib 라이브러리 파일  
▪ /opt 기타 의존 패키지  
▪ /share 패키지의 빌드, 환경 설정 파일  
▪ local_setup.* 환경 설정 파일  
▪ setup.* 환경 설정 파일  
```
2. 사용자 작업 폴더
```
/home/junq/robot_ws
▪ /build 빌드 설정 파일용 폴더
▪ /install msg, srv 헤더 파일과 사용자 패키지 라이브러리, 실행 파일용 폴더
▪ /log 빌드 로깅 파일용 폴더
▪ /src 사용자 패키지용 폴더
```
3. robot_ws/src 하위 파일
```
▪ /src C/C++ 코드용 폴더
▪ /include C/C++ 헤더 파일용 폴더 (폴더 안에는 각 패키지 이름별 폴더로 패키지별 헤더를 구분함)
▪ /param 파라미터 파일용 폴더
▪ /launch roslaunch에 사용되는 launch 파일용 폴더
▪ /패키지_이름_폴더 Python 코드용 폴더
▪ /test 테스트 코드 및 테스트 데이터용 폴더
▪ /msg 메시지 파일용 폴더
▪ /srv 서비스 파일용 폴더
▪ /action 액션 파일용 폴더
▪ /doc 문서용 폴더
▪ package.xml: 패키지 설정 파일 (REP-0140, REP-0149 참고)
▪ CMakeLists.txt: C/C++ 빌드 설정 파일
▪ setup.py: 파이썬 코드 환결 설정 파일
▪ README: 사용자 문서, github 리포지토리의 메인에 표시된다.
▪ CONTRIBUTING: 해당 패키지 개발에 공헌하는 방법을 기술하는 파일
▪ LICENSE: 이 패키지의 라이선스를 기술하는 파일
▪ CHANGELOG.rst: 이 패키지의 버전별 변경 사항 모음 파일 (REP-0132 참고)
```
#### ROS2 빌드 시스템, 빌드 툴
빌드 시스템은 단일 패키지를 대상으로 하며, 빌드 툴은 시스템 전체를 대상으로 한다. 단일 패키지 개념과 시스템 전체를 나누게 된 것은 의존성이 가장 크다. ROS는 코드의 재사용성을 위하여 패키지(메타패키지)와 노드 단위로 구성되어 있고 각 패키지는 다른 패키지와 상호 호환성을 위하여 의존성을 갖게 된다.  
빌드 시스템은 C++은 catkin과 ament_cmake를 사용하고 Python은 Python setuptools를 사용한다.  
ROS에서는 수많은 패키지가 함께 빌드하여 실행시키는 구조이기 때문에 각 패키지별로 서로 다른 빌드 시스템을 호출하고 패키지들의 종속성은 매우 얽혀있는 경우에는 이 얽혀있는 의존성 실타래를 풀고 토폴로지 순서대로 빌드 해야만 한다. ROS 빌드 툴로는 rosbuild, catkin_make, catkin_make_isolated, catkin_tools, ament_tools 그리고 현재 ROS 2 버전에서 널리 사용되고 있는 colcon이 있다.  
1. 패키지 생성
```
$ ros2 pkg create [패키지이름] --build-type [빌드 타입] --dependencies [의존하는패키지1] [의존하는패키지n]
RCL로 C++을 사용하면 ament_cmake, Python을 사용하면 ament_python. GUI 프로그램을 작성한다면 rqt plugin을 사용해야하니 ament_cmake
패키지를 작성할 때 ament 빌드 시스템에 꼭 필요한 CMakeLists.txt와 package.xml을 포함한 패키지 폴더를 생성한다.
$ ros2 pkg create my_first_rclcpp_pkg --build-type ament_cmake --dependencies rclcpp std_msgs
my_first_rclcpp_pkg
├── CMakeLists.txt
├── include
│   └── my_first_rclcpp_pkg
├── package.xml
└── src
```
2. 빌드
ROS2 특정 패키지 또는 전체 패키지를 빌드할 때는 colcon 빌드 툴을 사용한다. 소스코드가 있는 workspace로 이동하고 colcon build로 전체를 빌드하게 된다. 특정 패키지만 선택하여 빌드할 때는 --packages-select 옵션을 이용하고 symlink를 이용하려면 --symlink-install을 사용한다.  
```
$ cd ~/robot_ws && colcon build --symlink-install
$ cd ~/robot_ws && colcon build --symlink-install --packages-select [패키지 이름]
```
3. 빌드 시스템에 필요한 부가 기능  
1. vcstool 버전 컨트롤 시스템 툴  
2. rosdep 의존선 관리 툴  
3. bloom 바이너리 패키지 관리 툴





