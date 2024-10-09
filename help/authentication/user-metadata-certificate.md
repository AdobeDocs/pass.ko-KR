---
title: 암호화를 위한 사용자 메타데이터 인증서
description: 암호화를 위한 사용자 메타데이터 인증서
exl-id: 9fd68885-7b3a-4af0-a090-6f1f16efd2a2
source-git-commit: 8c5203aa4a897a5e119a9886afc64a1b1556ee4f
workflow-type: tm+mt
source-wordcount: '381'
ht-degree: 0%

---


# 암호화를 위한 사용자 메타데이터 인증서

>[!NOTE]
>
>이 페이지의 컨텐츠는 정보용으로만 제공됩니다. 이 API를 사용하려면 Adobe의 현재 라이선스가 필요합니다. 허가되지 않은 사용은 허용되지 않습니다.

암호화된 사용자 메타데이터 Adobe Pass 인증 통합의 경우 개인/공개 키 쌍이 있어야 합니다.

이 문서에서는 Adobe Pass 인증에서 사용할 공개 키 인증서를 생성하는 프로세스에 대해 설명합니다. 여기에 설명된 프로세스는 OpenSSL 툴킷을 사용합니다.

## 인증서 생성 프로세스 연습(#generation)

1. OpenSSL 툴킷(http://www.openssl.org)을 다운로드하여 설치합니다.

1. CSR(인증서 서명 요청) 생성:

   * 키 쌍을 생성합니다.  명령/터미널 창을 열고 다음 명령을 실행합니다.

     ```bash
     openssl genrsa -des3 -out mycompany-license.key 2048
     ```

   * CSR을 생성합니다. 명령줄에서 다음을 실행합니다.

     ```bash
     openssl req -new -key mycompany-license.key -out mycompany-license.csr -batch
     ```

     개인 키의 암호를 입력하라는 메시지가 표시됩니다.

   * 개인 키 및 암호의 백업 사본을 만듭니다. 샘플 CSR:

     ```
     -----BEGIN CERTIFICATE REQUEST-----
     MIIBnTCCAQYCAQAwXTELMAkGA1UEBhMCU0cxETAPBgNVBAoTCE0yQ3J5cHRvMRIw
     EAYDVQQDEwlsb2NhbGhvc3QxJzAlBgkqhkiG9w0BCQEWGGFkbWluQHNlcnZlci5l
     eGFtcGxlLmRvbTCBnzANBgkqhkiG9w0BAQEFAAOBjQAwgYkCgYEAr1nYY1Qrll1r
     uB/FqlCRrr5nvupdIN+3wF7q915tvEQoc74bnu6b8IbbGRMhzdzmvQ4SzFfVEAuM
     MuTHeybPq5th7YDrTNizKKxOBnqE2KYuX9X22A1Kh49soJJFg6kPb9MUgiZBiMlv
     tb7K3CHfgw5WagWnLl8Lb+ccvKZZl+8CAwEAAaAAMA0GCSqGSIb3DQEBBAUAA4GB
     AHpoRp5YS55CZpy+wdigQEwjL/wSluvo+WjtpvP0YoBMJu4VMKeZi405R7o8oEwi
     PdlrrliKNknFmHKIaCKTLRcU59ScA6ADEIWUzqmUzP5Cs6jrSRo3NKfg1bd09D1K
     9rsQkRc9Urv9mRBIsredGnYECNeRaK5R1yzpOowninXC
     -----END CERTIFICATE REQUEST-----
     ```

1. CSR을 인증 기관(CA)에 보냅니다(예: Verisign).

1. CA는 .p7b 형식(PKCS#7, Cryptographic Message Syntax Standard)으로 인증서를 전송합니다

1. .p7b 인증서 배포 개인 키를 사용하여 PKCS#7(.p7b) 파일을 PKCS#12(PFX 파일, Personal Information Exchange Syntax Standard)로 변환하고 PEM 파일(연결된 인증서 컨테이너 파일)을 생성합니다.

   * PKCS#7 파일을 임시 PEM 파일로 변환합니다. 명령줄에서 다음을 실행합니다.

     ```
     openssl pkcs7 -in mycompany-license.p7b -inform DER -out mycompany-license-temp.pem -outform PEM -print_certs
     ```

   * 임시 PEM 파일을 PFX 파일로 변환합니다.  명령줄에서 다음을 실행합니다.

     ```
     openssl pkcs12 -export -inkey mycompany-license.key -in mycompany-license-temp.pem -out mycompany-license.pfx -passin pass:private_key_password -passout pass:pfx_password
     ```

   * 임시 PEM 파일을 최종 PEM 파일로 변환합니다. 명령줄에서 다음을 실행합니다.

     ```
     openssl x509 -in mycompany-license-temp.pem -inform PEM -out mycompany-license.pem -outform PEM
     ```

1. 최종 PEM 파일을 Adobe으로 전송하여 구성하십시오.

   * PEM 파일을 최종적으로 수신해야 하는 사람은 통합/유효성 검사에 할당된 Adobe 지원 엔지니어입니다. 해당 사용자와 직접 협력하지 않는 경우 Adobe 담당자로부터 파일을 받을 사용자를 확인할 수 있습니다.
   * Adobe은 기본 인증서와 백업 인증서를 모두 지원합니다. 어떤 식으로든 기본 인증서가 손상되면 인증서를 취소하고 보조 인증서로 전환하여 앱에서 요청자 ID에 서명할 수 있습니다. 이렇게 하면 고객의 영향을 최소화하면서 프로덕션에서 인증서 간의 원활한 전환을 보장합니다.
   * Adobe이 PEM 파일을 받으면 인증 엔지니어가 서버측 구성에 이 파일을 추가하고 파일 수신을 확인합니다.
