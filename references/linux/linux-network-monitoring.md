# 리눅스 네트워크 모니터링 정리

## 1. iftop
- 네트워크 활동에 대한 간단한 개요를 얻는 데 사용
- 평균 2, 10, 40 초마다 네트워크 사용 대역폭 업데이트를 표시
    ```shell
    $ sudo apt install iftop
    $ sudo iftop
    ```
    ![image](https://user-images.githubusercontent.com/64345968/139362393-5602e7c1-07b9-4efa-a044-2ab996a5019d.png)

<br />

## 2. nload
- 네트워크 트래픽과 대역폭 사용을 실시간으로 모니터링
- 그래프를 사용하여 인바운드 및 아웃 바운드 트래픽을 모니터링
- 전송 된 데이터의 총량 및 최소/최대 네트워크 사용량과 같은 정보도 표시
    ```shell
    $ sudo apt install nload
    $ sudo nload
    ```
    ![image](https://user-images.githubusercontent.com/64345968/139362928-01d0b0ad-c504-4050-a95f-20db472e4729.png)

<br />

## 3. nethogs
- 실행되는 각 프로세스 또는 애플리케이션별로 실시간 네트워크 트래픽 대역폭 사용량을 모니터링하는 텍스트 기반 도구
    ```shell
    $ sudo apt install nethogs
    $ sudo nethogs [인터페이스 이름] #인터페이스 이름 : ifconfig로 확인
    ```
    ![image](https://user-images.githubusercontent.com/64345968/139363845-a4c0e80f-7f2f-49d5-a87a-1cb4f671bb6a.png)

<br />

## 4. bmon
- 네트워크 대역폭 사용률 및 속도 추정기를 모니터링하기위한 간단한 명령 줄 도구
- 네트워크 통계를 캡처하여 사용자가 친숙한 형식으로 시각화
    ```shell
    $ sudo apt install bmon
    $ sudo bmon
    ```
    ![image](https://user-images.githubusercontent.com/64345968/139606712-dc6744ce-b969-4742-91b9-c7ede7a5fd39.png)

<br />

## 5. iptraf
- IP 트래픽 모니터
- 인터페이스별 세부 상태 정보
- TCP/UDP 포트별 또는 패킷 사이즈별 구분
- 맥 주소별 상태 정보
    ```shell
    $ sudo apt-get install iptraf
    $ sudo iptraf-ng
    ```
    ![image](https://user-images.githubusercontent.com/64345968/139606872-40b9b8e6-53ff-4a2a-a387-d22a40f62428.png)

    ![image](https://user-images.githubusercontent.com/64345968/139606888-c64b2eec-5fb1-455b-9520-9bffbae0fec6.png)