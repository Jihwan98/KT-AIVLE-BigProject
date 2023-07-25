# 🐶 강아지킴

![LogoView](https://github.com/Jihwan98/KT-AIVLE-BigProject/assets/76936390/c19821f8-b426-4f31-83a1-9ab7ca0ce334)

### 📱Android APK 파일

<img src="https://github.com/Jihwan98/KT-AIVLE-BigProject/assets/76936390/cf1aa9a9-359e-4329-93cb-b94a7af6a6ef" width="200">

## 😊 Members

#### 곽채원, 김민수, 박예은, 류홍규, 박지환, 이윤열, 이태훈

## 📜 서비스 내용

**반려견의 피부 질환을 AI가 진단하고, 수의사에게 물어볼 수 있는 서비스입니다.**

🌱 **반려인 시나리오**

1. 반려견의 피부 질환 의심부위를 찍고 업로드한다.
2. 해당 의심부위의 피부 질환을 AI가 진단한 결과를 알려준다.  
   2-1. AI가 예상한 질환에 대한 ChatGPT의 설명을 볼 수 있다.
3. 추가적인 질문사항이 있는 경우 Q&A 게시판에 질문을 올린다.  
   3-1. 질문당 하나의 ChatGPT의 답변이 작성된다.
4. 수의사의 답변을 확인한다.
5. 자신이 작성한 질문 내역을 확인할 수 있다.

🌱 **수의사 시나리오**

1. AI 피부 질환 진단 서비스는 마찬가지로 사용할 수 있다.
2. Q&A 게시판에 올라오는 질문들에 답변을 작성한다.
3. 답변 개수와 퀄리티를 바탕으로 메인페이지에 병원 광고를 게시할 수 있으며,  
   사용자들의 AI 진단 후 추천 병원으로 광고할 수 있다.

## 🛠 기술 스택

🌟 **[Front](./frontend)**

-   Android Studio (Java)

🌟 **[Backend](https://github.com/Jihwan98/KT-AIVLE-BigProject-Back)**

-   Django
-   AWS (EC2, RDS, S3)
-   MySQL
-   NGINX

## 🖥 개발 내용

**Frontend**는 **Android Studio**를 사용하여 **Android APP**을 구현했습니다.  
**Frontend**에서 **Backend**로의 통신은 **Retrofit2**를 이용하여 구현했습니다.

**Backend**는 **Django REST Framwork** 를 사용하여 **REST API**를 구현하였고,  
**Backend 서버**의 경우 **AWS EC2**를 사용하고, DB와 Storage를 **RDS**와 **S3**로 분리해서 구현했습니다.  
DB는 **MySQL**을 사용했고, API 서버는 **NGINX**로 배포 하였습니다.

![전체 아키텍처](https://github.com/Jihwan98/KT-AIVLE-BigProject/assets/76936390/d7795a8d-b50e-4301-b23d-d58b8d8661f3)

### ✅ API 명세서

API에 대한 내용들은 Notion에 API 명세서를 작성하여 관리했습니다. [[API 명세서 링크]](https://www.notion.so/957e66a93eee468b9ad01613f041ea0a?pvs=21)
![api 명세서 캡처 다크모드](https://github.com/Jihwan98/KT-AIVLE-BigProject/assets/76936390/b353e489-9e9d-4f16-bd75-72b80ddbf274)

### ✅ 로그인 관련 구현

Django REST Auth를 활용하여 기본적인 회원가입, 로그인, 로그아웃 등을 구현했습니다.

아래와 같은 Field들을 추가하여 Custom User를 구현했습니다.

-   Email을 로그인 시 사용 (기본은 Username)
-   수의사 여부 Field 추가
-   프로필 이미지 Field 추가 : Default로 pydenticon 이미지 사용

✔️ **JWT Token**

로그인할 때에는 `AccessToken` 과 `RefreshToken` 을 발급해 해당 토큰으로 사용자 정보를 확인할 수 있도록 구현했습니다.

✔️ **기타 User 관련 API**

회원가입 시 Email 중복 확인을 하는 API와, 비밀번호를 까먹었을 때 해당 아이디로 Email을 전송하여 비밀번호를 초기화할 수 있는 API를 구현했습니다. Front에서와 마찬가지로 비밀번호는 `SHA256` 암호화를 수행한 후 전달되도록 구현했습니다.

| 비밀번호 초기화 메일 | 비밀번호 초기화 화면 |
| --- | --- |
| <img src="https://github.com/Jihwan98/KT-AIVLE-BigProject/assets/76936390/b1225247-9d66-4248-96ab-6d981993c382" width="200"/> | <img src="https://github.com/Jihwan98/KT-AIVLE-BigProject/assets/76936390/ed32b264-be95-4bc1-99f5-94fe2d1039d9" width="200"> |



### ✅ Pet, Hospital API

User 별로 Pet 정보를 등록하는 API와, 수의사일 경우 Hospital 정보를 등록하는 API를 구현했습니다.

Pet과 Hospital 은 UserID와 외래키로 연결되어있어, 로그인한 정보를 바탕으로 데이터 생성 시 자동으로 UserID를 가져오도록 구현했습니다.

**✔ Hospital 광고 API**

메인 화면에 답변 작성이 우수한 병원들을 광고하고 있습니다. 지금은 답변에 대한 Rank 알고리즘이 없어 ChatGPT를 제외한 병원들에서 랜덤하게 보여주고 있습니다.

### ✅ Picture, Question, Answer API

우리의 핵심 기능인 사진을 찍어서 AI 진단을 받는 부분과 Q&A 게시판 부분을 담당하는 API 입니다.

**✔ Picture**

Picture는 유저의 PetID를 외래키로 가지기 때문에 사진을 찍어서 올릴 때 자신의 Pet만을 선택하도록 구현했습니다. 사진을 올린 후에는 AI 모델의 결과가 DB에 저장되도록 했습니다.

이후 해당 병명에 대한 설명을 ChatGPT API를 통해 받아와 DB에 저장하고 사용자에게 보여줍니다.

**✔ Question**

Question의 경우 마찬가지로 PictureID를 외래키로 가지기 때문에 해당 유저의 사진에만 접근하여 질문을 등록하도록 하였고, 조회는 누구나 가능하지만 쓰기, 수정, 삭제 기능은 유저 본인만 가능하도록 구현했습니다.

그리고 질문 제목과 내용에 대한 검색기능을 구현해 게시글을 검색할 수 있도록 했습니다.

또한, Question이 등록될 때 해당 질문 내용을 바탕으로(질문 제목, 내용, AI 모델 예상 질환, AI 모델 예상 Confidence) ChatGPT에게 답변을 받을 수 있도록 했습니다. ChatGPT의 경우 응답이 오는 시간이 대략 10초 이상 걸리기 때문에 쓰레드로 구현하여 질문이 등록되는 것에 지연이 발생하지 않도록 했습니다.

**✔ Answer**

Answer의 경우 수의사의 경우에만 답변을 달 수 있도록 구현하였습니다.

### ✅ AWS 서버 배포

AWS 서버 배포는 uwsgi와 Nginx를 통해 배포했습니다. Nginx를 통해 배포했음에도 AI모델이 돌아가고 동시에 다른 API 요청이 들어오면 서버가 죽는 경우가 발생하여, uwsgi 서버 부하 관련 설정을 변경하여 서버가 안정적으로 돌아갈 수 있도록 했습니다.

### ✅ 아키텍처, ERD, Service Flow, UI/UX 흐름도

| 아키텍처 | ERD | Service Flow | UI/UX 흐름도 |
| --- | --- | --- | --- |
| ![architecture](https://github.com/Jihwan98/KT-AIVLE-BigProject/assets/76936390/5613ec52-f891-4a2a-8e1d-441a49408033) | ![erd](https://github.com/Jihwan98/KT-AIVLE-BigProject/assets/76936390/e54a8176-6a59-4cda-b17f-46f5651a43ba) | ![서비스 플로우](https://github.com/Jihwan98/KT-AIVLE-BigProject/assets/76936390/7ad92375-6fd3-40c1-b11f-3428ab9c6fd4) | ![UI/UX](https://github.com/Jihwan98/KT-AIVLE-BigProject/assets/76936390/5fcbe597-6895-4940-bfa8-ae427d36d5d1) |

### ✅ AI

무증상 및 6가지의 피부 질환을 포함하여 7 Class로 분류하는 Flow를 가지고 있습니다.
![ppt4](https://github.com/Jihwan98/KT-AIVLE-BigProject/assets/76936390/863570b9-4de0-49da-a85f-b0b3feecf1b2)

✔️ **데이터**  
[AI HUB 반려동물 피부 질환 데이터](https://www.aihub.or.kr/aihubdata/data/view.do?currMenu=115&topMenu=100&aihubDataSe=realm&dataSetSn=561)를 사용했습니다.  
~~AI HUB 데이터에서 무증상 데이터를 제공하지 않고 있어 **DALLE API**를 사용해 **_무증상 데이터를 생성_**하여 학습을 진행했습니다.~~

-   뒤늦게 반려동물 무증상 데이터가 AI HUB에 업데이트되었고 이를 6월 23일에 확인하여 이후 해당 데이터로 학습을 진행하였습니다.
-   모델의 성능이 좋지 않아 데이터를 확인해본 결과, 중복되는 데이터가 여러 클래스에 들어가있고 개수를 채우기 위해 같은 이미지가 반복되는 등의 문제를 확인했습니다.
-   중복되는 데이터를 삭제하고 각 피부 질환의 특질에 따라 데이터를 직접 분류하는 정제 과정을 거쳤습니다.

✔️ **모델**  
모델은 pretrained InceptionV3를 사용했습니다.<br>

**✔ 모델 선정과정**

-   Unet
    ![ppt3](https://github.com/Jihwan98/KT-AIVLE-BigProject/assets/76936390/296df0bc-a369-4888-bc4d-7c0bba873999)

마스킹 이미지가 작은 경우 환부로 인식하기 어려운 문제가 있었습니다.<br>
unet에서는 픽셀 주위의 local region(패치)를 입력으로 각 픽셀의 label을 예측하는데, 패치가 작아 환부에 대한 localization 성능은 향상되었으나(98.81%의 accuracy), 네트워크는 매우 작은 context만 보게 되어 context 파악 성능이 매우 떨어지는 것을 확인할 수 있었습니다. <br>
(Unet의 경우 localization accuracy와 context(sementaic information) 사이는 trade off 관계)

-   YOLOv5

![ppt7](https://github.com/Jihwan98/KT-AIVLE-BigProject/assets/76936390/256633b8-856c-412f-82ab-215627ae1e9f)


마찬가지로 바운딩 박스가 작은 경우 객체 인식 자체를 잘 못하는 문제가 있었습니다.<br>
데이터 특성상 세그멘테이션 모델보다는 환부 주변 부위까지 함께 고려하는 분류 모델이 적합​하다고 판단했습니다.

-   사전학습 분류모델 검토

VGG16, MobileNetV3, EfficientNet-B0, Resnet-50, InceptionV3, Inception-resnetV2모델을 검토하였습니다.<br>
![ppt8](https://github.com/Jihwan98/KT-AIVLE-BigProject/assets/76936390/fd12e335-c0da-4ef7-92b6-024338344526)

InceptionV3, Inception-resnetV2의 성능이 가장 좋은 것을 확인하였습니다.<br>
모바일 환경을 고려하여 좀 더 가벼운 모델인 InceptionV3로 최종 결정하였습니다.<br>

**✔ 고도화 및 최적화**

-   고도화

데이터셋 정제과정을 거치다보니 줄어든 데이터를 보강하기위해 Data augmentation기법을 활용하였습니다.<br>
모델 학습속도를 높이고 과적합를 방지하기 위해 213layer까지 동결하되 Batchnormalization layer는 동결하지 않았습니다.<br>

-   최적화

기존의 313layer중 layer갯수를 줄여가면서 성능변화를 확인하였습니다.<br>
256layer까지는 밑단을 삭제하여도 유사한 성능을 내는 것을 확인하였습니다.<br>
이를 채택한 결과 기존의 Parameter기준 약 42% 경량화했습니다.(2219만-->1278만)<br>

![ppt11](https://github.com/Jihwan98/KT-AIVLE-BigProject/assets/76936390/3a4842e3-3be3-4d26-b144-818897b69267)

![ppt12](https://github.com/Jihwan98/KT-AIVLE-BigProject/assets/76936390/89a94acc-68b0-438c-81d4-ad37b42da8c4)

## 👀 서비스 화면

<img src="https://github.com/Jihwan98/KT-AIVLE-BigProject/assets/76936390/6b1ecd69-5c64-451b-baee-0f9f56a37ff3" width="200"/><br>
어플리케이션을 설치하면 위와 같은 아이콘이 나타납니다.<br><br>
<img src="https://github.com/Jihwan98/KT-AIVLE-BigProject/assets/76936390/6b58083e-7d40-4689-bc86-51eb89b41dbc" width="200"/>&nbsp;&nbsp;
<img src="https://github.com/Jihwan98/KT-AIVLE-BigProject/assets/76936390/90b67417-601f-4821-9141-620b86973878" width="200"/><br>
아이콘을 눌러 실행하면 로딩화면을 거치고 로그인 화면으로 이동합니다.<br><br>
<img src="https://github.com/Jihwan98/KT-AIVLE-BigProject/assets/76936390/64435bf2-6a31-44de-a270-92a9c4666ca0" width="200"/>&nbsp;&nbsp;
<img src="https://github.com/Jihwan98/KT-AIVLE-BigProject/assets/76936390/dfbdc944-3906-42b9-b6f4-c5ccd63df293" width="200"/>&nbsp;&nbsp;
<img src="https://github.com/Jihwan98/KT-AIVLE-BigProject/assets/76936390/a946c42b-f22d-4dbc-bd33-f2005fdfc45f" width="200"/><br>
로그인 화면에서 회원가입 버튼을 누르면 위와 같은 화면으로 이동합니다. <br>
수의사 버튼을 누르면 병원 정보를 입력할 수 있는 화면이 나타나고, <br>
약관 확인하기 문구를 클릭하면 약관을 확인 할 수 있는 화면으로 이동니다.<br><br>
<img src="https://github.com/Jihwan98/KT-AIVLE-BigProject/assets/76936390/6637cca5-e8fe-4d5d-b385-8a7e21781f09" width="200"/>&nbsp;&nbsp;
<img src="https://github.com/Jihwan98/KT-AIVLE-BigProject/assets/76936390/e02b11c2-5857-48ff-a916-7aa9bd6a1c0d" width="200"/><br>
로그인 화면에서 비밀번호 초기화 문구를 클릭하면 이메일을 입력할 수 있는 화면이 나타나고 <br>
초기화 이메일 보내기 버튼을 누르면 위와 같은 메일을 받을 수 있습니다.<br><br>
<img src="https://github.com/Jihwan98/KT-AIVLE-BigProject/assets/76936390/1e7257d9-0543-4d09-8569-5997319bb3c4" width="200"/>&nbsp;&nbsp;
<img src="https://github.com/Jihwan98/KT-AIVLE-BigProject/assets/76936390/c40940d1-b970-4822-a331-500561d31b0c" width="200"/>&nbsp;&nbsp;
<img src="https://github.com/Jihwan98/KT-AIVLE-BigProject/assets/76936390/7cdb82b3-b45f-4917-bfcb-35a9a9478327" width="200"/><br>
로그인이나 회원가입이 성공하면 메인화면으로 이동합니다.<br>
답변을 많이 한 병원들 중 하나가 하단에 노출됩니다.<br>
설문조사 버튼을 누르면 구글폼으로 이동합니다.<br>
전화상담 버튼을 누르면 휴대전화의 전화화면으로 이동하고 병원 번호로 다이얼을 걸어둡니다.<br><br>
<img src="https://github.com/Jihwan98/KT-AIVLE-BigProject/assets/76936390/a371f154-d47b-4921-8423-3b1a0249afc8" width="200"/>&nbsp;&nbsp;
<img src="https://github.com/Jihwan98/KT-AIVLE-BigProject/assets/76936390/6af68889-fc9c-4a78-be64-0339ae183c15" width="200"/>&nbsp;&nbsp;
<img src="https://github.com/Jihwan98/KT-AIVLE-BigProject/assets/76936390/30589332-cf41-4b3a-bd11-ba23b7f02875" width="200"/><br>
메인화면에서 피부 질환 진단하기 버튼을 누르면 해당하는 동물을 선택하고 <br>
사진을 촬영하거나 갤러리에서 선택하여 이미지를 업로드하는 화면으로 이동합니다.<br>
첫번째 사진은 선택 전 화면, 두번째 사진은 사진촬영 버튼으로 넘어간 화면, <br>
세번째 사진은 사진선택 버튼으로 넘어간 화면, 마지막은 선택 후 화면입니다.<br><br>
<img src="https://github.com/Jihwan98/KT-AIVLE-BigProject/assets/76936390/30213c7c-ac87-488d-8893-402eaf59978c" width="200"/>&nbsp;&nbsp;
<img src="https://github.com/Jihwan98/KT-AIVLE-BigProject/assets/76936390/b92106d5-09f3-44a0-8241-e6b951cc2e9b" width="200"/>&nbsp;&nbsp;
<img src="https://github.com/Jihwan98/KT-AIVLE-BigProject/assets/76936390/726fff9f-75f6-49b0-b9df-148944ae461d" width="200"/>&nbsp;&nbsp;
<img src="https://github.com/Jihwan98/KT-AIVLE-BigProject/assets/76936390/4ce99c6f-def3-4b6f-8d54-9dc00229a3db" width="200"/>&nbsp;&nbsp;
<img src="https://github.com/Jihwan98/KT-AIVLE-BigProject/assets/76936390/780286f8-ce86-4d6f-bd66-60199d29e162" width="200"/><br>
사진을 선택한 후 사진 등록 버튼을 누르면 진단 결과를 보여주는 화면으로 이동합니다. <br>
기다리면 첫번째 사진 처럼 AI 진단 결과를 보여주고 <br>
진단 결과가 나온 후 더 기다리면 두번째 사진처럼 설명이 도착했다고 메시지를 보여줍니다.<br>
이후 AI 진단 버튼을 누르면 진단 결과에 대한 GPT의 설명을 보여줍니다. <br>
진단 결과가 나온 뒤 질문등록 버튼을 누르면 마지막 사진처럼 질문을 등록할 수있는 화면을 보여줍니다. <br><br>
<img src="https://github.com/Jihwan98/KT-AIVLE-BigProject/assets/76936390/548e5ad1-9a67-42ea-b575-e8b952445294" width="200"/>&nbsp;&nbsp;
<img src="https://github.com/Jihwan98/KT-AIVLE-BigProject/assets/76936390/70852765-498e-4b04-9d34-64372077d26c" width="200"/>&nbsp;&nbsp;
<img src="https://github.com/Jihwan98/KT-AIVLE-BigProject/assets/76936390/6bd49ef3-4fb7-4b76-b167-da420e165bc3" width="200"/>&nbsp;&nbsp;
<img src="https://github.com/Jihwan98/KT-AIVLE-BigProject/assets/76936390/4232b7cd-b9b7-4a87-821e-1276f71685b6" width="200"/>&nbsp;&nbsp;
<img src="https://github.com/Jihwan98/KT-AIVLE-BigProject/assets/76936390/0e86ce3d-5a55-4098-9e86-f7ca5eed61cf" width="200"/>&nbsp;&nbsp;<br>
질문등록을 마치면 메인화면으로 돌아오게 됩니다.<br>
하단 네비게이션의 질문게시판을 클릭하면 게시판화면이 나타나게됩니다.<br>
게시물을 클릭하면 찍은 사진, AI진단결과, GPT의 설명, 질문이 보여지게됩니다.<br>
답변 또한 하단에 달리게 되는데 우선 GPT가 진단결과와 질문을 기반으로 답변을 해주고 <br>
추가적으로 수의사 분들이 답변을 해주실 수 있습니다.<br>
또한 마지막 사진 처럼 게시글을 검색 할 수 있습니다.<br><br>
<img src="https://github.com/Jihwan98/KT-AIVLE-BigProject/assets/76936390/824dfe1c-54ca-4e30-83b3-850422edd320" width="200"/>&nbsp;&nbsp;
<img src="https://github.com/Jihwan98/KT-AIVLE-BigProject/assets/76936390/cd73df20-55c2-49d8-a634-7fb6f6d7eace" width="200"/>&nbsp;&nbsp;
<img src="https://github.com/Jihwan98/KT-AIVLE-BigProject/assets/76936390/03cc750c-578e-4dd2-9b7d-42a8ab4c8501" width="200"/>&nbsp;&nbsp;
<img src="https://github.com/Jihwan98/KT-AIVLE-BigProject/assets/76936390/9c74a106-4ef6-44a8-9f25-e31d5c87d771" width="200"/>&nbsp;&nbsp;
<img src="https://github.com/Jihwan98/KT-AIVLE-BigProject/assets/76936390/9a46883e-852b-45d9-8440-e0fa2aa019f2" width="200"/>&nbsp;&nbsp;<br>
메인화면이나 Q&A게시판에서 하단 네비게이션의 내 정보 버튼을 누르면 첫번째 사진과 같은 화면으로 이동합니다.<br>
해당 화면에서 프로필 사진 변경 버튼을 누르면 유저 프로필 사진을 바꿀 수 있고<br>
플러스 모양을 눌러 내 반려동물 추가화면으로 이동하여 반려동물을 추가 할 수 있습니다.<br>
세번째와 다섯번째 사진처럼 수정버튼을 눌러 반려동물, 병원정보 수정화면으로 이동 할 수 있습니다.<br>
이 때 병원 정보는 일반회원이면 보이지 않습니다.<br>
삭제 버튼, 로그아웃 문구, 회원탈퇴 문구의 경우 누르면 확인화면으로 재확인 후 기능을 수행합니다.<br><br>

<img src="https://github.com/Jihwan98/KT-AIVLE-BigProject/assets/76936390/261a5a52-11f6-4e72-b130-e0e2681874f2" width="200"/><br>
내 정보 화면에서 게시글 작성 내역 문구를 누르거나 메인화면에서 나의 질문 버튼을 누르면<br>
내가 작성한 질문글 목록을 볼 수 있고 클릭하면 QnA게시글 화면으로 이동하여 해당하는 게시글을 보여주게 됩니다.
