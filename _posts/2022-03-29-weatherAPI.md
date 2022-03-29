---
title: "weather API 사용해보기"   
date: 2022-03-29 23:38:28 -0400
categories: 공부
---

오늘은 [https://openweathermap.org](https://openweathermap.org) 를 소개해보려한다

## 괜히 해외 API 를 사용해야해? 공공 API 있잖아

이유가 다 있다..

기상청 공공 API 를 사용하려 본인도 시도했지만.... 내가 원한 거와 많이 달랐다..

1. 내가 원한 건 X, Y 좌표로 해당 위치의 날씨 조회하는거.
2. 실제 기상청은 시간까지 계산해야하고, 언제 날씨인지 등등 조건이 많아서 조금이라도 범위를 벗어나면 조회가 되지않았다
   
   ```json
   {
    "coord": {
   
        "lon": 126.924,
   
        "lat": 37.5007
   
    },
   
    "weather": [
   
        {
   
            "id": 800,
   
            "main": "Clear",
   
            "description": "clear sky",
   
            "icon": "01n"
   
        }
   
    ],
   
    "base": "stations",
   
    "main": {
   
        "temp": 280.47,
   
        "feels_like": 280.47,
   
        "temp_min": 279.05,
   
        "temp_max": 282.03,
   
        "pressure": 1022,
   
        "humidity": 79
   
    },
   
    "visibility": 10000,
   
    "wind": {
   
        "speed": 0.51,
   
        "deg": 20
   
    },
   
    "clouds": {
   
        "all": 0
   
    },
   
    "dt": 1648564735,
   
    "sys": {
   
        "type": 1,
   
        "id": 8105,
   
        "country": "KR",
   
        "sunrise": 1648502580,
   
        "sunset": 1648547472
   
    },
   
    "timezone": 32400,
   
    "id": 1948005,
   
    "name": "Kwangmyŏng",
   
    "cod": 200
   }
   ```
   
response 인데 정말 내가 원한 값들만 내려온다.


