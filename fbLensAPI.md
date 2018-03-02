fbLens API 사용법 정리
======================

# 1. 주 사용 목적
fbLens 서비스를 위한 안드로이드 및 하이브리드 등의 어플리케이션과 웹 앱간의 데이터 통신을 위해 사용된다.

# 2. 주요 Default 사항
## 2.1 BaseURL
  http://www.fblens.com/api/
## 2.2 Request 및 Response
  ### 1. 무조건 JSON 형태로 보내며 받을때도 JSON 형태로 받는다.
  ### 2. Request 시의 Header 의 Content-Type 은 application/json 이어야 한다.
  ### 3. 편의상 모든 request 는 Post 방식을 취한다. (귀찮아서 죄송합니다)
## 2.3 일반적인 사용법
  ### 1. baseURL 뒤에 원하는 서비스에 해당하는 URI 를 덧붙인 곳으로 요청한다.
    ex. http://www.fblens.com/api/SearchUser