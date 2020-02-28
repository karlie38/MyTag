# MyTag : Restaurant keyword extractor
맛집 태그 추출기 

## 목적 및 활용
블로그 리뷰를 이용한 식당 키워드 추출

## 따로 install이 필요한 패키지

* numpy
* pandas
* matplotlib
* sklearn
* scipy
* Mecab
* ckonlpy
* soynlp
* soykeyword

## 프로세스
1. Crawl_Naver_blog_Itaewon.ipynb -> 크롤링
	- 블로그 리뷰 제목과 본문의 일부만을 크롤링
	- Input Shape
		- 네이버 음식점 메인 소개 페이지
		- 네이버 음식점 블로그 리뷰 페이지: 끝이 tabPage = 숫자 형태로 끝나는 꼴이어야함
		- 음식점 이름
		- ex. 홍대에 위치한 '다락투'라는 음식점
		<pre>
		<code>
		main_url ='https://store.naver.com/restaurants/detail?id=31202900&tab=main'
		tab_url = 'https://store.naver.com/restaurants/detail?id=31202900&tab=fsasReview&tabPage=1'
		data = crawl_rest(main_url, tab_url, 12, '다락투')
		</code>
		</pre>
		
		
2. MyTag_Extractor.ipynb -> 전처리 및 태그 추출
	- 이용한 로직: extractor가 각각 계산한 단어의 중요도(score)에 가중치를 통한 키워드 추출 
	- 3가지 extractor 조합
		- textrank
		- tf-idf
		- soyextractor
	- 가중치를 줄 태그 우선순위
		- 기준: 사람들이 음식점을 인지(기억)할 때, 메타 정보는 무엇인가
		- 아래와 같이 크게 3가지의 카테고리를 잡고 해당 태그가 아래 카테고리에 포함되면 extractor가 계산한 score 값에 가중치를 더해 합산
		1. 음식
			- food_dic.txt 에 음식사전 구축
		2. 목적
			- 남자친구, 부모님 등의 누구와 함께 가는지에 대한 정보
			- 기념일, 생일, 데이트 등의 목적 정보
		3. 그외
			- 분위기, 테라스 등의 음식점 내부 환경을 말해주는 정보 포함
			- 미슐랭, 블루리본 등의 음식점의 외부 환경(평가)를 말해주는 정보 포함

## 결과

1. 꿀밤
>	- 드라마 이태원 클래쓰(박서준, 김다미 주연) 촬영 장소
>	- 마이태그 결과: '포차', '웹툰', '드라마', '술집', '박서준', '라운지', '단밤', '혼술', '광진', '평일', '원작', '클럽', '바스타', '핫플레이스', '분위기', '칵테일', '작가', '친구'
	
2. 라페름
>	- 샐러드 및 브런치 집
>	- https://store.naver.com/restaurants/detail?id=37160963
>	- 마이태그 결과: '연어', '샐러드', '친구', '스테이크', '한강진', '건강', '브런치', '주말', '건강식', '한남', '다이어트', '아보카도', '푸드', '카페', '슈퍼', '치킨'
  
 3. 원인어밀리언
 > 	- 인스타그램에서 유명한 카페
 > 	- https://store.naver.com/restaurants/detail?id=37793452
 > 	- 마이태그 결과: '핑크', '카페', '한강진', '커피', '분위기', '인스타', '핫플레이스', '디저트', '사진', '동반', '애견', '친구', '데이트', '파스타'
