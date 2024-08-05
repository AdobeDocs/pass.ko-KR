---
title: 헤더 - X-Device-Info
description: REST API V2 - 헤더 - X-Device-Info
source-git-commit: 4d1ce1301d6baf7309e8ee52c43b02403aa2fab9
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 0%

---


# 헤더 - X-Device-Info {#header-x-device-info}

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

## 개요 {#overview}

<b>X-Device-Info</b> 요청 헤더에 실제 스트리밍 장치와 관련된 클라이언트 정보(장치, 연결 및 응용 프로그램)가 포함되어 있습니다.

## 구문 {#syntax}

<table>
   <tr>
      <td style="background-color: #DEEBFF;" colspan="2"><b>X-Device-Info</b>: &lt;device_information&gt;</td>
   </tr>
   <tr>
      <td>헤더 유형</td>
      <td>요청 헤더</td>
   </tr>
   <tr>
      <td>표준</td>
      <td>아니요</td>
   </tr>
</table>

## 지시문 {#directives}

<b>&lt;device_information></b>

다음 표에 필요한 것으로 표시된 특성을 적어도 포함하는 JSON 요소의 `Base64-encoded` 값입니다.

<table>
    <tr>
        <th style="background-color: #EFF2F7; width: 15%;">키</th>
        <th style="background-color: #EFF2F7;">설명</th>    
        <th style="background-color: #EFF2F7; width: 15%;">현재 상태</th>
        <th style="background-color: #EFF2F7;">가능한 값</th>
    </tr>
    <tr>
        <td>기본 하드웨어 유형</td>
        <td>장치의 기본 하드웨어 유형입니다.</td>
        <td></td>
        <td>
            값이 제한됩니다.
            <ul>
                <li>카메라</li>
                <li>DataCollectionTerminal</li>
                <li>데스크탑</li>
                <li>포함된 네트워크 모듈</li>
                <li>eReader</li>
                <li>게임콘솔</li>
                <li>GeolocationTracker</li>
                <li>안경</li>
                <li>MediaPlayer</li>
                <li>휴대폰</li>
                <li>결제 단말기</li>
                <li>플러그인 모뎀</li>
                <li>셋톱 박스</li>
                <li>TV</li>
                <li>태블릿</li>
                <li>WirelessHotspot</li>
                <li>손목시계</li>
                <li>알 수 없음</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>모델</td>
        <td>장치의 모델 이름.</td>
        <td><i>필수</i></td>
        <td>예: iPhone, SM-G930V, AppleTV 등</td>
    </tr>
    <tr>
        <td>버전</td>
        <td>디바이스 버전.</td>
        <td></td>
        <td>예: 2.0.1 등</td>
    </tr>
    <tr>
        <td>제조업체</td>
        <td>장치의 제조 회사/조직.</td>
        <td></td>
        <td>예: 삼성, LG, ZTE, 화웨이, 모토로라, Apple 등</td>
    </tr>
    <tr>
        <td>공급업체</td>
        <td>장치의 판매 회사/조직.</td>
        <td></td>
        <td>예: Apple, Samsung, LG, Google 등</td>
    </tr>
    <tr>
        <td>osName</td>
        <td>장치의 운영 체제(OS) 이름.</td>
        <td><i>필수</i></td>
        <td>
            값이 제한됩니다.
            <ul>
                <li>Android</li>
                <li>Chrome</li>
                <li>리눅스</li>
                <li>Mac</li>
                <li>OS X</li>
                <li>OpenBSD</li>
                <li>Roku 운영 체제</li>
                <li>Windows</li>
                <li>iOS</li>
                <li>tvOS</li>
                <li>webOS</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>osFamily</td>
        <td>장치의 운영 체제(OS) 그룹 이름입니다.</td>
        <td></td>
        <td>
            값이 제한됩니다.
            <ul>
                <li>Android</li>
                <li>BSD</li>
                <li>리눅스</li>
                <li>플레이스테이션</li>
                <li>Roku 운영 체제</li>
                <li>Symbian</li>
                <li>티젠</li>
                <li>Windows</li>
                <li>iOS</li>
                <li>tvOS</li>
                <li>macOS</li>
                <li>webOS</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>osVendor</td>
        <td>장치의 운영 체제(OS) 공급업체.</td>
        <td></td>
        <td>
            값이 제한됩니다.
            <ul>
                <li>Amazon</li>
                <li>Apple</li>
                <li>Google</li>
                <li>LG</li>
                <li>Microsoft</li>
                <li>모질라</li>
                <li>닌텐도</li>
                <li>노키아</li>
                <li>Roku</li>
                <li>Samsung</li>
                <li>소니</li>
                <li>Tizen 프로젝트</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>osVersion</td>
        <td>장치의 운영 체제(OS) 버전.</td>
        <td></td>
        <td>예: 10.2, 9.0.1 등</td>
    </tr>
    <tr>
        <td>browserName</td>
        <td>브라우저의 이름입니다.</td>
        <td></td>
        <td>
            값이 제한됩니다.
            <ul>
                <li>Android 브라우저</li>
                <li>Chrome</li>
                <li>Edge</li>
                <li>Firefox</li>
                <li>Internet Explorer</li>
                <li>Opera</li>
                <li>Safari</li>
                <li>시몽키</li>
                <li>Symbian 브라우저</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>browserVendor</td>
        <td>브라우저의 빌드 회사/조직입니다.</td>
        <td></td>
        <td>
            값이 제한됩니다.
            <ul>
                <li>Amazon</li>
                <li>Apple</li>
                <li>Google</li>
                <li>Microsoft</li>
                <li>모토로라</li>
                <li>모질라</li>
                <li>넷스케이프</li>
                <li>닌텐도</li>
                <li>노키아</li>
                <li>Samsung</li>
                <li>소니 에릭슨</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>browserVersion</td>
        <td>장치의 브라우저 버전.</td>
        <td></td>
        <td>예: 60.0.3112</td>
    </tr>
    <tr>
        <td>userAgent</td>
        <td>장치의 사용자 에이전트.</td>
        <td></td>
        <td>예: Mozilla/5.0 (Macintosh, Intel Mac OS X 10_12_3) AppleWebKit/602.4.8 (KHTML, 예: Gecko) 버전/10.0.3 Safari/602.4.8</td>
    </tr>
    <tr>
        <td>displayWidth</td>
        <td>장치의 실제 화면 너비입니다.</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td>displayHeight</td>
        <td>장치의 실제 화면 높이입니다.</td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td>displayPi</td>
        <td>장치의 실제 화면 픽셀 밀도입니다.</td>
        <td></td>
        <td>예: 294</td>
    </tr>
    <tr>
        <td>대각 화면 크기</td>
        <td>디바이스의 물리적 화면 대각선 치수(인치)입니다.</td>
        <td></td>
        <td>예: 5.5, 10.1</td>
    </tr>
    <tr>
        <td>connectionIp</td>
        <td>HTTP 요청을 전송하는 데 사용되는 장치의 IP입니다.</td>
        <td></td>
        <td>예: 8.8.4.4</td>
    </tr>
    <tr>
        <td>connectionPort</td>
        <td>HTTP 요청을 전송하는 데 사용되는 장치의 포트입니다.</td>
        <td></td>
        <td>예: 53124</td>
    </tr>
    <tr>
        <td>connectionType</td>
        <td>네트워크 연결 유형입니다.</td>
        <td></td>
        <td>예: WiFi, LAN, 3G, 4G, 5G</td>
    </tr>
    <tr>
        <td>connectionSecure</td>
        <td>네트워크 연결 보안 상태입니다.</td>
        <td></td>
        <td>
            값이 제한됩니다.
            <ul>
                <li>true - 보안 네트워크의 경우</li>
                <li>false - 공용 핫스팟의 경우</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>applicationId</td>
        <td>애플리케이션의 고유 식별자.</td>
        <td></td>
        <td>예: CNN</td>
    </tr>
</table>


## 예시 {#examples}

```JSON
// Device information
// {
//  "primaryHardwareType" : "MobilePhone",
//  "model":"SM-S901U",
//  "vendor":"samsung",
//  "version":"r0q",
//  "manufacturer":"samsung",
//  "osName":"Android",
//  "osVersion":"14"
// }
 
// BASE64-encoded
// ewogICJwcmltYXJ5SGFyZHdhcmVUeXBlIiA6ICJNb2JpbGVQaG9uZSIsCiAgIm1vZGVsIjoiU00tUzkwMVUiLAogICJ2ZW5kb3I
// iOiJzYW1zdW5nIiwKICAidmVyc2lvbiI6InIwcSIsCiAgIm1hbnVmYWN0dXJlciI6InNhbXN1bmciLAogICJvc05hbWUiOiJBbmRyb
// 2lkIiwKICAib3NWZXJzaW9uIjoiMTQiCn0=
 
X-Device-Info: ewogICJwcmltYXJ5SGFyZHdhcmVUeXBlIiA6ICJNb2JpbGVQaG9uZSIsCiAgIm1vZGVsIjoiU00tUzkwMVUiLAogICJ2ZW5kb3IiOiJzYW1zdW5nIiwKICAidmVyc2lvbiI6InIwcSIsCiAgIm1hbnVmYWN0dXJlciI6InNhbXN1bmciLAogICJvc05hbWUiOiJBbmRyb2lkIiwKICAib3NWZXJzaW9uIjoiMTQiCn0=
```
