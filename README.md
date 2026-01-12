# Python Web Crawling (실무 중심 정리)

> **목표**: 내꺼 만들기
---

## 1. HTML 기본 개념 (크롤링 관점)

### 1️⃣ 문자 인코딩 (글자 깨질 때)

* HTML을 브라우저로 열었는데 한글이 깨지면 인코딩 문제일 가능성이 큼
* `<head>` 안에 아래 메타 태그가 있으면 정상

```html
<meta charset="utf-8">
```

* UTF-8 = 유니코드 기반 (전 세계 문자 표현 가능)
* 크롤링 시에도 `response.encoding = 'utf-8'` 설정이 필요할 수 있음

---

### 2️⃣ 태그와 속성

```html
<img src="dimg.png" width="100" height="100">
```

* `img` : 태그 이름
* `src`, `width`, `height` : 속성(attribute)
* **크롤링 포인트**: 필요한 정보는 대부분 *태그의 속성* 또는 *태그 안 텍스트*

 HTML 태그 사전 (필수)

* [https://developer.mozilla.org/ko/docs/Web/HTML/Element](https://developer.mozilla.org/ko/docs/Web/HTML/Element)

---

## 2. CSS 기본 (셀렉터 이해가 핵심)

### CSS 적용 방법 3가지 (알아만 두면 됨)

1. 태그에 직접 적용 (inline)

```html
<td style="text-align:center; color:blue">
```

2. `<head>` 안에 `<style>` 태그
3. `<head>` 안에 외부 CSS 파일 링크

 자주 보이는 CSS 프로퍼티

* `color`, `font-size`, `font-family`, `text-align`

---

## 3. 웹 크롤링 3단계 (매우 중요)

1. **Fetching** : 웹 페이지 가져오기 (`requests.get()`)
2. **Parsing** : HTML 구조 파악 (`BeautifulSoup`)
3. **Extraction** : 필요한 데이터만 추출 (`find`, `select` 등)

 이 흐름이 머릿속에 자동으로 떠올라야 함

---

## 4. class / id 가 중요한 이유

* 원래 목적: **CSS 디자인용**
* 크롤링에서는:

  * 특정 요소를 **고유하게 식별(id)**
  * 여러 요소를 **그룹화(class)** 가능

 그래서 데이터 추출할 때 class / id 를 최우선으로 본다

---

## 5. BeautifulSoup 핵심 메서드

### 텍스트 추출

* `.get_text()`
* `.string`

✔ 시작 태그 ~ 끝 태그 사이의 **순수 텍스트만 반환**
✔ 내부에 다른 태그가 있어도 제거됨

---

## 6. 크롤링 실전 TIP ① (개발자도구 활용)

### 크롬 개발자도구

* `Ctrl + Shift + I`
* 마우스로 원하는 요소 선택
* **태그 / class / id 확인 → 그대로 코드에 사용**

---

### Response 객체 개념

```python
res = requests.get(url)
```

* `res`는 `requests.Response` 클래스의 **인스턴스**
* 인스턴스 = 어떤 클래스에서 만들어진 객체라는 걸 강조할 때 쓰는 말

---

### 1️⃣ 추출한 것에서 또 추출하기 (실무 패턴)

1. `find()`로 큰 덩어리 선택
2. 그 안에서 `find_all()`로 세부 데이터 추출

```python
box = soup.find('div', class_='box')
items = box.find_all('li')
```

* `find()` → 첫 번째 하나만 반환
* `find_all()` → 리스트로 반환

---

### 2️⃣ 데이터 전처리

* `strip()` : 공백 제거
* `split()` : 문자열 분리
* `enumerate()` : 인덱스 번호 붙이기

 크롤링의 절반은 **전처리**

---

## 7. 크롤링 실전 TIP ② (CSS Selector)

>  **select() / select_one() 적극 추천**

### 기본 특징

* `select()` → 리스트 반환
* `select_one()` → 요소 하나 반환

---

### CSS Selector 사용법

1️⃣ 하위 태그

```python
soup.select('div ul li')
```

2️⃣ 바로 아래 자식 태그

```python
soup.select('div > ul')
```

(⚠️ 중간 태그 있으면 안 됨)

3️⃣ class 선택

```python
soup.select('.title')
```

4️⃣ id 선택

```python
soup.select('#content')
```

5️⃣ 클래스 여러 개

```python
soup.select('div.box.active')
```

---

## 8. requests vs urllib

* 과거: `urllib + BeautifulSoup`
* 현재 실무: **`requests + BeautifulSoup`**

 기본은 `requests`
 문제 생길 때만 `urllib` 참고

---

## 9. HTTP 개념 (아주 최소한)

* 클라이언트 ↔ 서버 통신 규칙 = **HTTP 프로토콜**
* `requests.get()` = HTTP 요청
* 서버 응답 = HTML, JSON 등

---

## 10. 여러 페이지 크롤링 패턴

```python
for num in range(10):
    url = base_url + str(num)
    res = requests.get(url)
```

* 페이지 번호를 문자열로 변환 후 URL에 붙이기
* 첫 페이지 예외 처리 필요한 경우 `if num == 0` 처리

---

##  한 줄 요약

> **개발자도구 → class/id 찾기 → select() → 전처리**

이 루트만 자동화되면 실무 크롤링은 충분함 


## 11. 데이터 추출해서 엑셀 파일로 읽기
```import openpyxl

def write_excel_template(filename, sheetname, listdata):
    #엑셀 하나 만들어서, 시트 이름 정하고, 리스트 데이터를 행 단위로 쓰고, 저장”
    
    excel_file = openpyxl.Workbook()  #workbook생성, 기본 sheet 자동 생성
    excel_sheet = excel_file.active#.active로 sheet가져오기
    
    excel_sheet.column_dimensions['A'].width = 100
    excel_sheet.column_dimensions['B'].width = 20
    
    if sheetname != '': #시트 이름 비어 있으면 기본값 유지, 있으면 교체  
        excel_sheet.title = sheetname
    
    for item in listdata:  #list안에 또 다른 list
        #[["이름", "가격"], ["상품1", 10000],["상품2", 20000]] 리스트 데이터 구조 예시 
        excel_sheet.append(item)
        #각각의 행을 뽑아서 append
    excel_file.save(filename)
    excel_file.close()

write_excel_template('tmp.xlsx', '상품정보', product_lists)


### 엑셀 파일 읽기 전체 코드
import openpyxl

excel_file = openpyxl.load_workbook('tmp.xlsx') #tmp.xlsx 파일을 열어서 workbook 객체 형성
#여러가지 sheet 존재 
excel_sheet = excel_file.active  
#Workbook
# └── Worksheet (excel_sheet)
 #     └── Cell 들
# excel_sheet = excel_file.get_sheet_by_name('IT뉴스')

for row in excel_sheet.rows: 
    print(row[0].value, row[1].value) #row[0] :셀 객체, .value 는 그 셀에 들어 있는 실제 값

excel_file.close()

```
