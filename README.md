# opencv_install_cmake
opencv, cuda, cudnn 설치 방법이 잘 설명된 사이트 기록하고, 설치하면서 겪은 시행착오들 기록
<br>
<br> 만일 yolov3 용으로 사용하고 싶으면 반드시, 반드시, 반드시 OPENCV <= 3.4.0 만족해라!!!!
<br> 관련정보 : https://github.com/pjreddie/darknet/issues/1072 의 AlexeyAB 댓글 참조
<br>
<br> 그리고 4.0은 c api가 전혀 안되므로, 만일 opencv3으로 다운 그레이드 할 시에는 반드시 반드시 반드시 4.0관련 라이브러리는 아주 깔끔하게 지워라. 아래 블로그에 그 방법에 써져 있으니 참고.
<br>
<br>

## 1. 기본 설명
### 1) opencv 4.0.0 설치

참고 블로그 : https://webnautes.tistory.com/1030

 -----------------------------------------------------------------------------
<br>
<br>
 
### 2) cuda 9.0, cudnn 7 설치

 1. 여기있는 대로 cuda 설치  --->  https://hiseon.me/2018/03/11/cuda-install/  
 
 <br>
 
 cudnn은 여기대로 --> https://yunsangq.github.io/articles/2017-02/Ubuntu-16.04(64bit),-CUDA-8.0,-cuDNN-5.1-Install
 
<br>

 2. 환경변수 설정
```
export PATH=/usr/local/cuda-9.0/bin${PATH:+:${PATH}}
export LD_LIBRARY_PATH=${LD_LIBRARY_PATH}:/usr/local/cuda-9.0/lib64
``` 
<br>

 3. 위 내용을  영구적으로 설정
```
sudo nano /etc/environment
```
<br>

 4. 확인
```
echo $PATH
```
 -----------------------------------------------------------------------------
<br>
<br>

### 3) nvidia driver 설치
https://hiseon.me/2018/02/17/install_nvidia_driver/<br><br>

 -----------------------------------------------------------------------------
<br>
<br>

### 4) opencv cmake compile 옵션 모음
```
cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -D WITH_TBB=OFF -D WITH_IPP=OFF -D WITH_1394=OFF -D BUILD_WITH_DEBUG_INFO=OFF -D BUILD_DOCS=OFF -D INSTALL_C_EXAMPLES=ON -D INSTALL_PYTHON_EXAMPLES=ON -D BUILD_EXAMPLES=OFF -D BUILD_TESTS=OFF -D BUILD_PERF_TESTS=OFF -D WITH_GTK=ON -D OPENCV_EXTRA_MODULES_PATH=/home/ahnjaeyoung/opencv/opencv_contrib-4.0.0/modules -D WITH_V4L=ON -D WITH_FFMPEG=ON -D WITH_XINE=ON -D BUILD_NEW_PYTHON_SUPPORT=ON -D OPENCV_GENERATE_PKGCONFIG=ON -D WITH_CUDA=ON -D CUDA_ARCH_BIN=${ARCH_BIN} -D CUDA_ARCH_PTX="" -D ENABLE_FAST_MATH=ON -D CUDA_FAST_MATH=ON -D WITH_CUBLAS=ON -D WITH_LIBV4L=ON -D WITH_GSTREAMER=ON -D WITH_GSTREAMER_0_10=OFF -D WITH_QT=ON -D WITH_OPENGL=ON -D WITH_NVCUVID=ON -D PYTHON3_INCLUDE_DIR=/usr/include/python3.5m -D PYTHON3_NUMPY_INCLUDE_DIRS=/usr/lib/python3.5/dist-packages/numpy/core/include -D PYTHON3_PACKAGES_PATH=/usr/lib/python3.5/dist-packages -D PYTHON3_LIBRARY=/usr/lib/x86_64-linux-gnu/libpython3.5m.so ../
```
<br> 
<br> 참고 자료2 : https://github.com/cggos/dip_cvqt/issues/1
<br> 단, 여기에 ffmpeg 설치 과정이 생략 되어 있으니 참고 자료 2대로 ffmpeg 설치해랴
<br>


 -----------------------------------------------------------------------------
<br>
<br>

### 5) anaconda3 설치
http://sldofvge12.blogspot.com/2018/04/ubuntu-tensorflow-gpu-anaconda-pycharm.html <br>
근데 이 짓을 하니깐 파이썬 경로가 바뀌어서 tensorflow가 인식이 되지 않았었다. 이 사이트에서 처럼 nano에서 anaconda가 멋대로 추가한 PATH 변수를 comment해 버리자 

 -----------------------------------------------------------------------------
<br>
<br>


### 6) caffe 설치
https://hiseon.me/2018/04/23/caffe-build/ <br>

 -----------------------------------------------------------------------------
<br>
<br>

## 2. 버그 및 겪은 오류
### 1) namespace 오류
```
error: ‘FILTER_SCHARR’ was not declared in this scope
```

* issue : https://github.com/opencv/opencv_contrib/pull/1915 <br>
* 해결   : https://github.com/opencv/opencv_contrib/pull/1915/commits/675134eae93a4ce7c6e5d1214486bf0ca7ab89b2 <br>

이거 **내 잘못이 아니라** contrib의 modules/cvv/src/qtutil/filter/sobelfilterwidget.cpp의 코드 문제이다.<br>
<br>

### 2) python3.5 경로 오류
```
fatal error: numpy/ndarrayobject.h: No such file or directory No such file or directory
```
* 해결  : cmake -D 설정할 때 python3 경로가 실제와 안맞아서 그렇다. python3 -m site로 반드시 확인해라.
<br>

### 3) opnecv4 폴더
* 전에 없던 /usr/local/include/**opencv4**가 존재해서, .hpp파일들이 경로를 찾지 못하는 경우가 있다. opencv4/opencv2의 opencv2를 opencv4와 같은 위치에 있게 해야 한다.
<br>

### 4) pkg-config 지원 안함
* 더 이상 pkg-config는 지원안하므로, 앞으로 libs를 한번에 찾기가 힘
* 관련 내용 : https://github.com/opencv/opencv/issues/13154
<br>
<br>

## 3. 결론
 1. python 설치할 때 pip 사용 금지!!! 앞으로도 python 관련 lib 설치할 때 위 블로그 대로 해라.
 2. 이거 하다가 xterm이란 터미널 날렸더니 인터페이스가 아예 먹통이 되었다. 당황하지 말고 ctrl+alt+f1 누르고 ubuntu-desktop 설치.


