# python_crawling_study
for studying crawling 

1. 웹 브라우저로 HTML을 오픈했는데 글자 깨짐 
- head 안에 <meta charset="utf-8">
- 유니코드 사용 - 전세계 문자를 정의 해놨음

2. 태그에는 속성을 넣을 수 있다
<img src ="dimg.png" width="100" height="100">

img: 태그
src: 속성이름 (여기선 이미지니까 이미지파일임)
width: 넓이
height: 높이 

필요한 태그는 html 사전 
: https:devloper.ozilla.org/ko/docs/Web/HTML/Element


★css 언어 적용하기
1. 적용할 태그에 style 속성으로 넣기
: ex) <td style="text-align:center;color:blue">
text-align, color : 프로퍼티
center(가운데정렬), blue, :값 
2. HTML 문서 <head> 안에 <style> 태그 넣기 
3. HTML 문서 <head> 안에 css 파일 링크하기

★css 프로퍼티: color, font-size, font-family, text-align

★웹크롤링 과정 3단계
1. Fetching(내용 가져오기)
2. Parsing (구조파악)
3. Extraction(정보골라내기)

★HTML에서 class 나 id 속성이 웹 크롤링 시 데이터 추출에 유용한 이유?>
: class나 id는 css(디자인)을 위해 사용되지만, 
특정 html 요소를 고유하게 식별하거나 그룹화 가능하기 때문

★BeautifulSoup 객체에서 .get_text()나 .string 메서드를 호출하면 
해당요소의 시작 태그부터 끝 태그 사이의 모든 내용을 가져오되 
포함된 하위 태그들은 제거하고 순수한 텍스트만 남겨준다.




★<crawling tip> 1
1. 크롬 브라우저 활용 방법 
Ctrl+shift+i(오픈 크롬 개발자 모드) -> 마우스로 원하는 부분 선택 
->태그랑 class 선택해서 원하는 데이터 추출

#res는 requests 라이브러리 안에 정의된 Response 클래스의 객체(인스턴스)임
#인스턴스: 어떤 클래스에서 만들어졌는지, 강조할 때 쓰는 말임 

2.추출한 거 또 추출 
- find()로 더 크게 감싸는 HTML 태그로 추출하고
- 다시 추출된 데이터에서 find_all()로 원하는 부분 추출
-> find()는 첫번 째 일치하는 요소만 반환 , find_all()은 일치하는 모든 요소 리스트로 반환 

3. 데이터 전처리 함수 strip(), split() 쓰기
- 리스트 번호 접근 후, enumerate로 숫자 추가하기 or 공백 제거 etc..  



★<crawling tip> 2 (css selector <css선택자> . . 사용하기 꽤나 유용)
1. 기존에는 find함수를 썼지만 , selcet() 안에 태그 또는 css class 이름 넣어주기 
2. 결과 값은 리스트 형태로 반환 됨 . (items로 slect메서드 받아주고 리스트 형태로 출력하면 되겠지?) 
(첫번째 데이터만 얻고자 할 때는 select_one() 사용하기 -> 해당 아이템 객체가 리턴 된다

-(1) 하위태그 띄워쓰기로 select 하기 , 바로 아래 태그는 > 꺾새 표시로 ㄲ 2칸 차이 나면 안됨 바로아래여야만
-(2) .class 이름 선택
-(3) #id 이름 검색  
-(4) 클래스가 여러개인 경우 : 태그.클래스이름1.클래스이름2

-> select() 안에 select 하는 중요한 방법 . .


★기존에는 urllib + bs4 많이 사용, 최근 들어 requests + bs4 많이 사용
   기존 코드 중 일부가 urllib 사용하니.. 간단한 사용법만 알아둡시다.
-> requests 라이브러리를 사용해서 크롤링 진행하고,  문제 있어 보이는 경우만
     urllib 사용 추천

★클라이언트와 서버간에 웹페이지를 가져오기 위해선  requests 라이브러리를 써서
요청하고 실제 네트워크에서 요청이 되고 응답이 되는 관련된 특별한 규격이 있음
: Http라는 프로토콜 규격 

★여러 페이지를 크롤링 하는 기법
