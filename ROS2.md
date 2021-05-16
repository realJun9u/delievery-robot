### ROS2 
- ROS2 용어 정리
Node : 최소 단위의 실행 가능한 프로세스. 하나의 실행 가능한 프로그램  
Package : 하나 이상의 노드, 노드 실행을 위한 정보 등을 묶어 놓은 것. 패키지의 묶음을 메타패키지.  
Message : 노드간의 데이터를 주고 받는 방식은 메시지 통신이다. 메시지는 integer, floating point, boolean, string 같은 변수형태. 메시지안에 메시지가 있는 데이터 구조, 메시지들의 배열 같은 구조도 사용가능.  
Topic : publish-subscribe 개념 방식. 단방향, 연속적 통신. 보내는 쪽이 퍼블리셔 노드, 받는 쪽이 서브스크라이버 노드가 된다. 1:N, N:1, N:N 통신 가능. 센서와 같은 일방적으로 데이터를 보낼 때 주로 사용. ROS에서 가장 많이 사용한다.  
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
turtlesim 키보드 조종 예제  
```
$ ros2 run turtlesim turtlesim_node
$ ros2 run turtlesim turtle_teleop_key
여기서 중요한 것은 두 노드간의 동작이 단순히 키보드 값을 전달하여 움직이는게 아니라 눌려진 키보드의 키값에 해당되는 병진 속도(linear velocity)와 회전 속도(angular velocity)를 geometry_msgs 패키지의 Twist 메시지 형태로 보내고 받는 다는 것이다.
```
노드,토픽,서비스,액션 조회
```
$ ros2 node list
$ ros2 topic list
$ ros2 service list
$ ros2 action list
$ rqt_graph
```
