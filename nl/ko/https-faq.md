---

copyright:
  years: 2018
lastupdated: "2018-11-13"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{: faq: data-hd-content-type=‘faq’}

# HTTPS에 관한 FAQ 

## 내 CDN에서 와일드카드 인증서를 사용하는 HTTPS와 SAN 인증서를 사용하는 HTTPS의 차이점은 무엇입니까?
{: faq}

와일드카드 인증서를 사용하면 모든 고객이 벤더의 CDN 네트워크에 배치된 동일한 인증서를 사용합니다. IBM 접미부 `.cdnedge.bluemix.net`을 포함하는 CNAME을 사용하여 서비스에 액세스해야 합니다. 예를 들어 `https://www.example-cname.cdnedge.bluemix.net`입니다.

SAN 인증서의 경우 도메인 이름을 SAN 항목에 추가하여 여러 고객 도메인에서 단일 SAN 인증서를 공유합니다. 그러면 호스트 이름(예: `https://www.example.com`)을 사용하여 서비스에 액세스할 수 있습니다.

## 경로 재지정을 통한 도메인 유효성 검증을 수행하는 방법은 무엇입니까?
{: faq}

서버에 따라 다릅니다. Apache와 Nginx 서버의 도메인 유효성 검증을 완료하는 프로시저는 [HTTPS의 도메인 제어 유효성 검증 완료](how-to-https.html#redirect) 페이지에서 찾을 수 있습니다.

## 도메인 유효성 검증을 수행하는 데 시간이 얼마나 걸립니까?
{: faq}

도메인 유효성 검증은 일반적으로 2 - 4시간이 걸리지만, 유효성 검증에 사용하도록 선택한 메소드에 따라 달라집니다. CNAME을 사용한 DV 유효성 검증이 가장 빠르며, 일반적으로 1시간 미만이 소요됩니다. 표준 및 경로 재지정 메소드를 사용하는 DV는 인증 확인이 처리된 후 대개 ~4시간이 소요됩니다.

## DV SAN 인증서를 사용하는 CDN에 대한 HTTPS를 작성하고 사용 가능하도록 설정하는 데 시간이 얼마나 걸립니까?
{: faq}

HTTPS를 사용하기 위한 일반 요청은 초기 요청부터 실행까지 평균 3 - 9시간이 걸립니다.

## DV SAN 인증서를 사용하는 CDN을 삭제하는 데 시간이 얼마나 걸립니까?
{: faq}

CDN을 삭제하려면 모든 에지 서버에 있는 인증서에서 도메인을 제거해야 합니다. 이 프로세스는 완료하는 데 최대 8시간이 걸릴 수 있습니다.

## CDN에 DV SAN 인증서를 사용하는 데 연관된 추가 비용이 있습니까?
{: faq}

아니오. 와일드카드 인증서를 사용하는 HTTP 또는 HTTPS와 대조적으로 DY SAN 인증서 구성은 추가 요금 없이 제공됩니다.

## 와일드카드를 사용하는 HTTPS로 작성한 CDN에서 DV SAN 인증서를 사용하도록 업데이트할 수 있습니까?
{: faq}

아니오, 와일드카드 맵핑은 현재 SAN 인증서로 변경할 수 없습니다.

## 인증 기관은 무엇입니까?
{: faq}

인증 기관(CA)은 디지털 인증서를 발행하는 엔티티입니다.

## DV SAN 인증서를 발행하는 IBM Cloud CDN 서비스를 수행하는 CA는 어디입니까?
{: faq}

IBM Cloud CDN 서비스에서는 LetsEncrypt Certificate Authority를 사용합니다.

## IBM Cloud CDN에 대해 지원되는 SSL 인증서는 무엇입니까?
{: faq}

지원되는 SSL 인증서는 와일드카드 인증서와 도메인 유효성 검증(DV) SAN(Subject Alternate Name) 인증서입니다. SAN 인증서는 여러 고객 간에 공유됩니다. IBM Cloud CDN에서는 사용자 정의 인증서 업로드를 지원하지 않습니다.

## CDN과 관련하여 도메인 유효성 검증 인증 확인을 처리하도록 요청하는 이메일을 받았습니다. 지금 어떠한 작업을 수행해야 합니까?
{: faq}

도메인 유효성 검증은 CNAME, 표준 또는 직접의 세 가지 방법 중 하나로 처리할 수 있습니다.

해당 처리 방법에 대한 세부사항은 [HTTPS용 도메인 제어 유효성 검증 완료](how-to-https.html#how-to-https.html#initial-steps-to-domain-control-validation) 도메인을 참조하십시오.

## CDN의 도메인 유효성 검증을 위한 인증 확인을 처리하지 않으면 어떻게 됩니까?
{: faq}

맵핑 상태가 48시간이 넘게 DOMAIN_VALIDATION_PENDING 상태에 있으면 맵핑 작성이 취소되고 맵핑 상태가 CREATE_ERROR가 됩니다. 이 경우 작성 재시도 또는 맵핑 삭제를 선택할 수 있습니다.

## 와일드카드 인증서에서 CDN에 대한 도메인의 유효성을 검증해야 합니까?
{: faq}

아니오, CNAME만 사용하여 원본의 컨텐츠를 검색할 수 있습니다. `https://www.example-cname.cdnedge.bluemix.net`

## CDN에 대해 SAN 인증서 유형을 사용하는 경우 여전히 CNAME을 사용하여 내 서비스에 액세스할 수 있습니까?
{: faq}

아니오. SAN 인증서의 경우 사용자 정의 도메인 사용을 통해서만 원본의 컨텐츠에 액세스할 수 있습니다. `https://www.example.com`

## 내 IBM Cloud CDN 도메인이 모두 하나의 인증서에 추가됩니까?
{: faq}

반드시 그렇지는 않습니다. 인증서가 가장 효율적인 상태에 있도록 Akamai에서 인증서 선택을 처리합니다. 도메인은 여러 다른 인증서로 적절하게 분배되어 추가되므로 모든 도메인이 동일한 인증서에 있다고 보장할 수 없습니다.

## `dig`를 수행할 때 또는 내 CDN의 상태가 `인증서 요청 중`, `도메인 유효성 검증 보류 중` 또는 `인증서 배치 중` 상태에서 컨텐츠에 액세스할 때 "와일드카드"가 표시되는 이유는 무엇입니까?
{: faq}

DV SAN 인증서 요청 중 프로세스 중에, CDN의 DNS 레코드 체인은 임시로 와일드카드 인증서에 체인으로 연결됩니다. 프로세스가 완료될 때까지 이 와일드카드 인증서를 통해 임시로 컨텐츠를 제공합니다. 요청 프로세스가 완료되면 CDN의 DV SAN 인증서에 체인으로 연결되도록 DNS 레코드 체인이 업데이트됩니다.
