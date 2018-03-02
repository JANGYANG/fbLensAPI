fbLens API 사용법 정리
======================

# 1. 주 사용 목적
  fbLens 서비스를 위한 안드로이드 및 하이브리드 등의 어플리케이션과 웹 앱간의 데이터 통신을 위해 사용된다.

# 2. 주요 사항

 ##  2.1 BaseURL
        http://www.fblens.com/api/

 ##  2.2 Request 및 Response
  ##### 2.2.1. 무조건 JSON 형태로 보내며 받을때도 JSON 형태로 받는다.
  ##### 2.2.2. Request 시의 Header 의 Content-Type 은 application/json 이어야 한다.
  ##### 2.2.3. 편의상 모든 request 는 Post 방식을 취한다. (원래는 REST에 맞춰 해야하지만, 빠른 개발과 실용성을 생각해서 그냥 POST를 쓰기로 했습니다...ㅠ)

# 3. 일반적인 사용법
  ## 3.1. baseURL 뒤에 원하는 서비스에 해당하는 URI 를 덧붙인 곳으로 요청한다.
  #### 예시
  - http://www.fblens.com/api/SearchUser
  - http://www.fblens.com/api/SearchTeam
  - http://www.fblens.com/api/RegisterUser

 ## 3.2 데이터를 보낼 때, 이름은 꼭 정해진 규칙을 따른다.(대소문자 구분)
    userName  --> OK!

    username  --> NO!
    user_name --> NO!

 ## 3.3 Response 를 잘 활용한다!
    //response JSON 예시
    //java 언어 사용시에는 JsonObject 객체를 사용하거나,
    //JavaScript 사용시에는 잘 받는다!(JavaScript는 걍 변수에 저장하면 지가 알아서 제이슨 객체화(?)됨)
    
    {
	"userUID": "188bbe73-916f-401b-9502-5581869ecb13",
	"userName": "데이터베이스",
	"job": "대학생",
	"birth": "2017-11-22",
	"height": 173,
	"weight": 62,
	"teamUID": "231ac76a-aac3-44e4-831c-6fba64a3e7e4",
	"position": ["CFW", "ACM", "CM", "DCM", "CD"],
	"region": [{
		"mainRegion": "서울특별시",
		"subRegion": "강남구"
	}, {
		"mainRegion": "서울특별시",
		"subRegion": "강북구"
	}]
}

# 4. 주의 사항
 ### 4.1 ONLY POST!

 ### 4.2 ONLY JSON!

 ### 4.3 ONLY CamelCase!

***
# API
드디어 API 관한 내용입니다.
위의 언급사항을 주의하여 사용해주시기 바랍니다.

## SearchUser
 ##### URI : SearchUser
 ##### 목적 : 선수 단 한명을 검색

  ##### Request
    userUID : String
  ##### Resonse
    userUID   : String
    userName  : String
    job       : String
    birth     : String
    height    : int
    weight    : int
    teamUID   : String    //팀이 가입이 안되었을 시에 포함되지 않음.
    position  : Array
    region    : Json Array
  #### Example
  Request
  ```
  {
    "userUID" : "188bbe73-916f-401b-9502-5581869ecb13"
  }
  ```
 Response
```
{
  "userUID": "188bbe73-916f-401b-9502-5581869ecb13",
  "userName": "데이터베이스",
  "job": "대학생",
  "birth": "2017-11-22",
  "height": 173,
  "weight": 62,
  "teamUID": "231ac76a-aac3-44e4-831c-6fba64a3e7e4",
  "position": ["CFW", "ACM", "CM", "DCM", "CD"],
  "region": [
    {
      "mainRegion": "서울특별시",
      "subRegion": "강남구"
    },
    {
      "mainRegion": "서울특별시",
      "subRegion": "강북구"
    }
  ]
} 
```

## SearchUsers
 ##### URI : SearchUsers
 ##### 목적 : 조건에 맞는 여러명의 선수를 검색
 ##### 비고
 * 이름으로 검색하거나 지역으로 검색하여 여러명의 선수 목록을 얻고 싶을때 사용.
 * 이름으로 검색시 정확히 일치 하지 않고, 글자 하나만 일치하더라도 검색됨.
  ##### Request
  ```
  userName : String
  ```
  or
  ```
  mainRegion  : String
  subRegion   : String
  ```
  ##### Resonse
  ```
  //비고 위의 SearchUser 에서 결과가 Array 형태로 옵니다.
  ```
  #### Example
  Request
  ```
  {
    "userName" : "이"
  }
  ```
  ```
  {
    "userName" : "이우"
  }
  ```
  ```
  {
    "mainRegion"  : "경기도",
    "subRegion"   : "여주시"
  }
  ```
  ```
  {
    "mainRegion"  : "서울특별시",
    "subRegion"   : "중구"
  }
  ```
 Response
```
// 사람 이름 '이'로 검색했을시에 결과
[{
	"userUID": "188bbe73-916f-401b-9502-5581869ecb13",
	"userName": "데이터베이스",
	"birth": "2017-11-22",
	"height": 173,
	"weight": 62,
	"teamUID": "231ac76a-aac3-44e4-831c-6fba64a3e7e4",
	"position": ["CFW", "ACM", "CM", "DCM", "CD"],
	"region": [{
		"mainRegion": "서울특별시",
		"subRegion": "강남구"
	}, {
		"mainRegion": "서울특별시",
		"subRegion": "강북구"
	}]
}, {
	"userUID": "5dcb8cdf-75ac-47c2-9e94-7680d0b8430a",
	"userName": "이민규",
	"birth": "1992-07-20",
	"height": 173,
	"weight": 62,
	"teamUID": "4494fe1b-a8a5-4dee-8ee0-af2b8f6acaa0",
	"position": ["LFW", "ACM", "RWM"],
	"region": [{
		"mainRegion": "서울특별시",
		"subRegion": "노원구"
	}, {
		"mainRegion": "서울특별시",
		"subRegion": "중구"
	}]
}, {
	"userUID": "79045c6e-eb1f-4850-aec8-0ae8b9ff8871",
	"userName": "이우경",
	"birth": "1992-05-03",
	"height": 173,
	"weight": 62,
	"teamUID": "4494fe1b-a8a5-4dee-8ee0-af2b8f6acaa0",
	"position": ["LFW", "ACM", "RWM", "DCM", "RB", "CM"],
	"region": [{
		"mainRegion": "서울특별시",
		"subRegion": "중구"
	}, {
		"mainRegion": "서울특별시",
		"subRegion": "강남구"
	}]
}, {
	"userUID": "83fc5676-9804-48e8-907b-b6e23dab4085",
	"userName": "이상현",
	"height": 0,
	"weight": 0,
	"position": [],
	"region": []
}, {
	"userUID": "946151b2-700d-40d8-8c5c-8b2618e02c9f",
	"userName": "이창준",
	"birth": "1979-05-01",
	"height": 177,
	"weight": 67,
	"position": ["ACM", "CM"],
	"region": [{
		"mainRegion": "서울특별시",
		"subRegion": "광진구"
	}]
}, {
	"userUID": "29ff0e05-6fe2-4cab-bcac-ce1b0c6d5abf",
	"userName": "이민규",
	"birth": "1992-09-10",
	"height": 173,
	"weight": 63,
	"position": ["LFW"],
	"region": [{
		"mainRegion": "경기도",
		"subRegion": "안산시"
	}, {
		"mainRegion": "서울특별시",
		"subRegion": "서대문구"
	}]
}, {
	"userUID": "c4c006dd-7bf0-4e17-bac9-1d62ac4e20c7",
	"userName": "이천수",
	"birth": "1997-02-27",
	"height": 189,
	"weight": 78,
	"position": ["CFW"],
	"region": [{
		"mainRegion": "제주도",
		"subRegion": "제주시"
	}]
}]
```

## SearchTeam
 ##### URI : SearchTeam
 ##### 목적 : 단 한 팀만을 검색

  ##### Request
    teamUID : String
  ##### Resonse
    teamUID   : String
    teamName  : String
    teamBirth : String
    captainUID: String
    formation : Array
    region    : JsonArray
  #### Example
  Request
  ```
  {
    "teamUID" : "231ac76a-aac3-44e4-831c-6fba64a3e7e4"
  }
  ```
 Response
```
{
	"teamUID": "231ac76a-aac3-44e4-831c-6fba64a3e7e4",
	"teamName": "데이터베이스",
	"teamBirth": "2017",
	"captainUID": "188bbe73-916f-401b-9502-5581869ecb13",
	"formation": ["4-4-2", "3-4-1-2"],
	"region": [{
		"mainRegion": "인천광역시",
		"subRegion": "남동구"
	}, {
		"mainRegion": "서울특별시",
		"subRegion": "중구"
	}]
} 
```

## SearchTeams
 ##### URI : SearchTeams
 ##### 목적 : 조건에 해당하는 팀들을 검색
 ##### 비고
 * 이름으로 검색하거나 지역으로 검색하여 여러 개의 팀 목록을 얻고 싶을때 사용.
 * 이름으로 검색시 정확히 일치 하지 않고, 글자 하나만 일치하더라도 검색됨.
 * 현재는 검색시에 대소문자 구분하나, 대소문자 구분없이 검색 가능하도록 업데이트 할 예정.
  ##### Request
  ```
  teamName : String
  ```
  or
  ```
  mainRegion  : String
  subRegion   : String
  ```
  ##### Resonse
  ```
  //비고 위의 SearchTeam 에서 결과가 Array 형태로 옵니다.
  ```
  #### Example
  Request
  ```
{
	"teamName": "안"
}
  ```
  ```
  {
    "TeamName" : "FC"
  }
  ```
  ```
  {
    "mainRegion"  : "경기도",
    "subRegion"   : "여주시"
  }
  ```
  ```
  {
    "mainRegion"  : "서울특별시",
    "subRegion"   : "중구"
  }
  ```
 Response
```
// 팀이름 '안'로 검색했을시에 결과
[{
	"teamUID": "4494fe1b-a8a5-4dee-8ee0-af2b8f6acaa0",
	"teamName": "부러진안경",
	"teamBirth": "2015",
	"captainUID": "79045c6e-eb1f-4850-aec8-0ae8b9ff8871",
	"formation": ["4-4-2", "4-3-3"],
	"region": [{
		"mainRegion": "서울특별시",
		"subRegion": "중구"
	}, {
		"mainRegion": "강원도",
		"subRegion": "원주시"
	}]
}, {
	"teamUID": "daa77e1a-1592-4fe9-97c8-4ed1fe020581",
	"teamName": "딱딱한안경",
	"teamBirth": "1987",
	"captainUID": "3fd93e07-4072-40e6-bcc7-57e6afdb61fd",
	"formation": ["3-4-1-2"],
	"region": [{
		"mainRegion": "서울특별시",
		"subRegion": "강남구"
	}]
}]
```
