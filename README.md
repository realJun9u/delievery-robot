# delievery-robot
Line Trace, OCR

## 사용 기술
1. OCR (Optical Character Recognition, 광학 문자 인식) : 사람이 쓰거나 기계로 인쇄한 문자의 영상을 이미지 스캐너로 획득하여 기계가 읽을 수 있는 문자로 변환하는 기술. 이미지 스캔으로 얻을 수 있는 문서의 활자 영상을 컴퓨터가 편집가능한 문자코드 등의 형식으로 변환하는 소프트웨어로써 일반적으로 OCR 이라고 하며, 인공지능이나 기계 시각의 연구분야로 시작되었다.
2. OpenCV(Open Source Computer Vision) : 실시간 컴퓨터 비전을 목적으로 한 프로그래밍 라이브러리이다. 원래는 인텔이 개발하였다. 실시간 이미지 프로세싱에 중점을 둔 라이브러리이다. 인텔 CPU에서 사용되는 경우 속도의 향상을 볼 수 있는 IPP(Intel Performance Primitives)를 지원한다. 이 라이브러리는 윈도, 리눅스 등에서 사용 가능한 크로스 플랫폼이며 오픈소스 BSD 허가서 하에서 무료로 사용할 수 있다. OpenCV는 TensorFlow , Torch / PyTorch 및 Caffe의 딥러닝 프레임워크를 지원한다.
```
주요 알고리즘
- 이진화(binarization)
- 노이즈 제거
- 외곽선 검출(edge detection)
- 패턴인식
- 기계학습(machine learning)
- ROI(Region Of Interest) 설정
- 이미지 변환(image warping)
- 하드웨어 가속
```

### 참고
휠 - https://m.blog.naver.com/PostView.nhn?blogId=hdh7485&logNo=20118847355&proxyReferer=https:%2F%2Fwww.google.com%2F => 옴니휠 4개 쓰자  
모터 - https://www.mfgkr.com/archives/5108 => DC 모터 쓰자  
모터 드라이버 역할? - http://www.makeshare.org/bbs/board.php?bo_table=Parts&wr_id=26  
라즈베리파이 모터제어 - https://digital-play.tistory.com/24?category=940925  
라즈베리파이 모터제어 단점 - http://www.makeshare.org/bbs/board.php?bo_table=raspberrypi&wr_id=69 =>  
아두이노 라즈베리파이 연동 - https://blog.naver.com/PostView.nhn?blogId=3demp&logNo=221399859161&parentCategoryNo=&categoryNo=52&viewDate=&isShowPopularPosts=true&from=search  


### 시간별 진행 상황
2021-03-08 First team meeting  
- 개발 방향 설정, 라인 맵 구상, 로봇의 배달 동작 방식 구상  

2021-03-11 아이디어 세미나 피드백  
- 어떤 기술을 중점적으로 다룰 것 인가?
> 라인 트레이스
> QR코드를 교차로에 깔아서 QR코드 리더기를 통해 위치 정보나 이동해야 할 정보를 줄 수 있다.  

> 화면 인식
> 화면 정보를 통해 위치 정보를 알고 그에 맞는 이동을 설정한다.  

- 이동할 경로는 고정되어 있고 물류를 왔다갔다 하게되니 회전할 필요가 없는 옴니휠을 이용해보는 건?
> 회전없이 전 방향 이동이 가능하여 사용하면 좋을 것 같다.  

2021-03-17 팀 미팅 작품 동작, 견적 구상  
- 적외선 센서를 통한 라인트레이서 + 카메라를 이용한 위치 인식 후 이동
> 모터를 구동할 때 4개의 모터 필요

PCB : Printed Circuit Board, 인쇄 회로 기판
