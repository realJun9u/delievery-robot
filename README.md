# delievery-robot

## 사용 기술

### 참고
휠 - https://m.blog.naver.com/PostView.nhn?blogId=hdh7485&logNo=20118847355&proxyReferer=https:%2F%2Fwww.google.com%2F => 옴니휠 4개 쓰자  
모터 - https://www.mfgkr.com/archives/5108 => DC 모터 쓰자  
모터 드라이버 역할? - http://www.makeshare.org/bbs/board.php?bo_table=Parts&wr_id=26  
라즈베리파이 모터제어 - https://digital-play.tistory.com/24?category=940925  
라즈베리파이 모터제어 단점 - http://www.makeshare.org/bbs/board.php?bo_table=raspberrypi&wr_id=69 =>  
아두이노 라즈베리파이 연동 - https://blog.naver.com/PostView.nhn?blogId=3demp&logNo=221399859161&parentCategoryNo=&categoryNo=52&viewDate=&isShowPopularPosts=true&from=search  


### 시간별 진행 상황  
6.25  
우분투 설치, ros noetic 설치, rplidar 연결 완료  
라즈베리파이4 우분투 설치: https://blog.naver.com/roboholic84/221701573539  + SD카드 용량이 64GB이상이면 ext4로 포맷되어 FAT32로 포맷한 후 이미지를 설치해야한다.  
우분투 서버 20.04 LTS GUI 설치: https://velog.io/@kyoung99u/Ubuntu-GUI-%EC%84%A4%EC%B9%98   
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
```
![image](https://user-images.githubusercontent.com/78460105/123499975-87098d80-d675-11eb-9006-c118b6d62d38.png)  

