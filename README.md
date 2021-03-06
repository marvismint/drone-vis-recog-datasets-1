# drone-vis-recog-datasets-1
상황인지 이미지(영상) 어노테이션   

## 데이터 개요
- 규격 : 해상도 1920x1080px 이상/ 15fps /30초 길이의 동영상  
- 데이터 구조: 하나의 영상 클립(폴더)당 450장의 이미지(jpg)
- 각 영상클립은 고유의 영상 ID를 가지며 하위에 고유의 이미지르ID를 가진 450개의 이미지 파일로 구성
```
(예시) 영상 파일 및 폴더 구조 : t1_video/t1_video_#####/t1_video_#####_#####.jp
```

## 사람, 사물 어노테이션
1.일반 규칙  
- 작업자는 각 이미지에서 바운딩 박스 좌표를 어노테이션하고 클래스ID와 고유ID를 라벨링함. 
- "사랑, 소화기, 소화전, 차량, 자전거, 오토바이" 등으로 한정
- 전체 이미지에 등장하는 인물의 수 및 사물의 수를 클래스별로 중복 없이 라벨링함.  
- 차량의 경우, 객체가 이동수단으로 기능이 식벽가능해야함 (지하철은 차량으로 보지 않음)  
![스크린샷, 2020-11-26 16-32-42](https://user-images.githubusercontent.com/11286586/100320769-09a96c00-3005-11eb-9d54-752b3e0f9418.png)

1-1. 바운딩 박스
- 이미지에 나타나는 대상이 25px 이상일 경우 1개로 처리하고 대상이 겹치거나 화면에서 잘리더라도 크기를 만족하면 이를 포함하여 처리함
- 클래스ID와 객체 ID 겹침 및 잘림 여부를 함께 라벨링함
- 동일한 대상은 하나의 영상 내 여러 이미지에 등장하더라도 동일한 객체 ID를 가짐

1-2. 바운딩 박스 라벨링
- 필수로 표기해야 하는 메타데이터의 예시
- 하나의 이미지 당 예시 중 노란 음역 및 박스의 정보가 반드시 포함된 메타데이터를 생성하고 나머지 정보는 별도의 파일 또는 폴더명으로 표기함
![스크린샷, 2020-11-26 16-31-59](https://user-images.githubusercontent.com/11286586/100320724-f39bab80-3004-11eb-81e3-ddc985caeb13.png)
![스크린샷, 2020-11-26 16-32-08](https://user-images.githubusercontent.com/11286586/100320728-f5656f00-3004-11eb-8db7-8677806b21fc.png)  


2.세부 규칙  

2.1 바운딩박스 조건  
- 바운딩 박스는 가능한 여백이 없도록 상하좌우 최극단에 맞는 박스를 사용함
- 하나의 객체에 하나의 박스만 사용
- 객체의 일부가 박스 영역을 벗어나지 않음
- 장애물이나 화면 경계에서 일부만 보이는 경우, 영상내 표현되는 객체가 25px이상이고 육안으로 확인가능할 경우에만 표기
- 객체의 크기는 동영상 내 해당 객체가 최초 등장 후 좌상단, 우하단 좌표로 표현되는 영역의 단축 기준으로 25px 이상이여야 인정
![스크린샷, 2020-11-26 16-42-30](https://user-images.githubusercontent.com/11286586/100321693-6ce7ce00-3006-11eb-9536-b6e28cf53e06.png)

2.2 동일인의 판단
- 영상 중 사라졌다 나타나는 사람이 동일인지 판단이 어려운경우 다수 작업자 투포료 동일인 여부를 판정함

2.3 최소 검수 조건
- 폴더 당 450개의 이미지 파일이 존재하여 사람의 오류의 확률이 높기 때문에 다수의 검수자가 여러 단계에 걸쳐 반복 검수하는 다단계 검수 절차를 진행

3.데이터 비식별화
- 공개를 목적으로 하는 데이터이므로 개인정보 보호를 위해 육안으로 식별 가능한 사람의 얼굴, 차량의 번호판 등 민감한 정보를 흐림 처리.
![스크린샷, 2020-11-26 16-45-46](https://user-images.githubusercontent.com/11286586/100321959-dc5dbd80-3006-11eb-9478-8db5b2b61504.png)

## 데이터셋 통계
- **해당 표는 본 저장소의 데이터 통계로서, 전체 통계 및 촬영 조건 통계 등 상세 내용은 [객체통계.xlsx](./객체통계.xlsx) 파일을 참고할 것.**    
___
- **객체 통계**

  | **객체**                      | **클래스 아이디** | **객체 수** |
  | ----------------------------- | :-------------: | :-----------: |
  | **Person(사람)**              | c_1 | 355 |
  | **Fire Extinguisher(소화기)** | c_2 | 1 |
  | **Fire Hydrant(소화전)**      | c_3 | 13 |
  | **Car(차량)**                 | c_4 | 598 |
  | **Bicycle(자전거)**           | c_5 | 39 |
  | **Motorcycle(오토바이)**      | c_6 | 56 |
  | **합계**                      |      | **1062** |
___

## 알림
- 본 데이터셋은 과학기술정보통신부의 인공지능산업원천기술개발 사업(과제번호: 2019-0-01763)의 일환으로 구축되었으며, 공개 S/W로 제공됨.
- 데이터셋은 대용량 다운로드를 고려하여 참여 업체당 1개씩, 총 3개의 저장소로 분리하여 저장하였으며, 각 저장소 마다 70개씩 전체 210개의 비디오 클립이 있음.
- 본 저장소에는 1번째부터 50번째까지의 70개 비디오 클립에 대한 데이터셋이 저장되어 있음.
- 저장소 2 링크:
- 저장소 3 링크: https://github.com/MTCom-AI/drone-vis-recog-datasets-3
