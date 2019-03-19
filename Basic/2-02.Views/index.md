---
layout: laravel
title: Laravel
subtitle: View
submenus:
    -
        name: 뷰-Views
        link: /Laravel/Basic/Views
    -
        name: 뷰 생성하기
        link: /Laravel/Basic/Views/Creating
    -
        name: 데이터 전달하기
        link: /Laravel/Basic/Views/DataViews
    -
        name: 뷰 컴포저
        link: /Laravel/Basic/Views/Composers
---

## View
---

* 뷰 생성하기
* 뷰에 데이터 전달하기
    - 모든 뷰에서 데이터 공유하기
* 뷰 컴포저

뷰는 MVC 패턴에서 컨트롤러가 처리한 결과를 사용자가 확인을 할 수 있도록 화면을 생성해 주는 역할을 합니다.

컨트롤러에서 뷰를 호출할 수도 있고, 직접 URL 라우팅에서 뷰를 호출할 수도 있습니다.
<br>

## 뷰의 분리
---
라라벨은 MVC 프래임워크 패턴을 응용을 하였습니다. 이로 인하여 뷰는 컨트롤러 / 애플리케이션 로직과 분리되어 역할을 수행합니다.
<br>

## 뷰 파일
---
라라벨의 뷰파일들은 resources/views 디렉토리에 위치합니다. 