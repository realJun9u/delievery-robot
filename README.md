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

2020-03-24 SW 환경 구성
- 라즈베리파이, 카메라 모듈 사용 환경 설정  
> 라즈베리파이 Imager를 이용하여 2021-03-04 버전 라즈비안 설치, 카메라 모듈 연결하여 촬영됨을 확인  
- Opencv 설치
> opencv 컴파일 과정중에 opencv 4.5.1 빌드 오류가 생기는데 이는 opencv 4.5.1 이 2020-12-22 릴리즈된 버전으로 그 때의 라즈비안 버전이 필요할 것으로 생각된다. opencv 4.5.1 버전이 릴리즈 될 때 라즈비안이 2020-12-02 버전인데 2020-08-20 버전에서도 된다고 하는데, 그 버전들을 구할 수 있을지는 모르겠다. 현재 홈페이지에서 2021-01-11 버전을 구할 수 있어서  이 버전으로 다시 설치하여 해보려 한다.
PCB : Printed Circuit Board, 인쇄 회로 기판
