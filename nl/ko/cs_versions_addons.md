---

copyright:
  years: 2014, 2019
lastupdated: "2019-03-21"

keywords: kubernetes, iks, nginx, ingress controller

subcollection: containers

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}



# 클러스터 추가 기능 변경 로그

{{site.data.keyword.containerlong}} 클러스터는 IBM에 의해 자동으로 업데이트되는 추가 기능을 제공합니다. 일부 추가 기능에 대한 자동 업데이트를 사용 안함으로 설정하고 마스터 및 작업자 노드와 별도로 수동으로 업데이트할 수도 있습니다. 각 버전의 변경사항에 대한 요약은 다음 절에 나와 있는 표를 참조하십시오.
{: shortdesc}

## Ingress ALB 추가 기능 변경 로그
{: #alb_changelog}

{{site.data.keyword.containerlong_notm}} 클러스터에 있는 Ingress 애플리케이션 로드 밸런서(ALB)에 대한 빌드 버전 변경사항을 확인하십시오.
{:shortdesc}

Ingress ALB 추가 기능이 업데이트되면 모든 ALB 팟(Pod)에 있는 `nginx-ingress` 및 `ingress-auth` 컨테이너가 최신 빌드 버전으로 업데이트됩니다. 기본적으로 추가 기능에 대한 자동 업데이트가 사용으로 설정되어 있지만, 자동 업데이트를 사용 안함으로 설정하고 추가 기능을 수동으로 업데이트할 수 있습니다. 자세한 정보는 [Ingress 애플리케이션 로드 밸런서 업데이트](/docs/containers?topic=containers-update#alb)를 참조하십시오.

Ingress ALB 추가 기능의 빌드별 변경사항에 대한 요약은 다음 표를 참조하십시오.

<table summary="Ingress 애플리케이션 로드 밸런서 추가 기능에 대한 빌드 변경사항 개요">
<caption>Ingress 애플리케이션 로드 밸런서 추가 기능에 대한 변경 로그</caption>
<col width="12%">
<col width="12%">
<col width="41%">
<col width="35%">
<thead>
<tr>
<th>`nginx-ingress` / `ingress-auth` 빌드</th>
<th>릴리스 날짜</th>
<th>비파괴적 변경사항</th>
<th>파괴적 변경사항</th>
</tr>
</thead>
<tbody>
<tr>
<td>411 / 306</td>
<td>2019년 3월 21일</td>
<td>Go 버전을 1.12.1로 업데이트합니다.</td>
<td>-</td>
</tr>
<tr>
<td>410 / 305</td>
<td>2019년 3월 18일</td>
<td><ul>
<li>이미지 스캔의 취약점이 해결되었습니다.</li>
<li>{{site.data.keyword.appid_full}}에 대한 로깅이 향상되었습니다.</li>
</ul></td>
<td>-</td>
</tr>
<tr>
<td>408 / 304</td>
<td>2019년 3월 5일</td>
<td>-</td>
<td>로그아웃 기능, 토큰 만료 및 `OAuth` 권한 콜백과 관련된 권한 통합의 버그가 해결되었습니다. 이 수정사항은 [`appid-auth`](/docs/containers?topic=containers-ingress_annotation#appid-auth) 어노테이션을 사용하여 {{site.data.keyword.appid_full_notm}} 권한을 사용으로 설정한 경우에만 구현됩니다. 이 수정사항을 구현하기 위해 총 헤더 크기를 증가시키는 추가 헤더가 추가되었습니다. 사용자 고유의 헤더 크기 및 총 응답 크기에 따라 사용하는 [ 프록시 버퍼 어노테이션](/docs/containers?topic=containers-ingress_annotation#proxy-buffer)을 조정해야 할 수 있습니다.</td>
</tr>
<tr>
<td>406 / 301</td>
<td>2019년 2월 19일</td>
<td><ul>
<li>Go 버전을 1.11.5로 업데이트합니다.</li>
<li>이미지 스캔의 취약점이 해결되었습니다.</li>
</ul></td>
<td>-</td>
</tr>
<tr>
<td>404 / 300</td>
<td>2019년 1월 31일</td>
<td>Go 버전을 1.11.4로 업데이트합니다.</td>
<td>-</td>
</tr>
<tr>
<td>403 / 295</td>
<td>2019년 1월 23일</td>
<td><ul>
<li>ALB의 NGINX 버전을 1.15.2로 업데이트합니다.</li>
<li>IBM에서 제공하는 TLS 인증서는 이제 7일이 아닌 만료되기 37일 전에 자동으로 갱신됩니다.</li>
<li>{{site.data.keyword.appid_full_notm}} 로그아웃 기능이 추가됨: `/logout` 접두부가 {{site.data.keyword.appid_full_notm}} 경로에 있으면 쿠키가 제거되고 사용자를 로그인 페이지로 다시 보냅니다. </li>
<li>내부 추적 용도로 {{site.data.keyword.appid_full_notm}} 요청에 헤더가 추가되었습니다.</li>
<li>`app-id` 어노테이션이 `proxy-buffers`, `proxy-buffer-size` 및 `proxy-busy-buffer-size` 어노테이션과 함께 사용될 수 있도록 {{site.data.keyword.appid_short_notm}} 위치 지시문을 업데이트합니다.</li>
<li>정보 로그가 오류로 레이블되지 않도록 버그를 해결합니다.</li>
</ul></td>
<td>기본적으로 TLS 1.0 및 1.1을 사용 안함으로 설정합니다. 앱에 연결하는 클라이언트가 TLS 1.2를 지원하면 어떠한 조치도 필요하지 않습니다. 여전히 TLS 1.0 또는 1.1 지원이 필요한 레거시 클라이언트가 있는 경우 [이 단계](/docs/containers?topic=containers-ingress#ssl_protocols_ciphers)를 수행하여 수동으로 필요한 TLS 버전을 사용으로 설정하십시오. 클라이언트가 애플리케이션에 액세스하는 데 사용하는 TLS 버전을 확인하는 방법에 대한 자세한 정보는 이 [{{site.data.keyword.Bluemix_notm}} 블로그 게시물](https://www.ibm.com/blogs/bluemix/2018/11/ibm-cloud-kubernetes-service-alb-update-tls-1-0-and-1-1-disabled-by-default/)을 참조하십시오.</td>
</tr>
<tr>
<td>393 / 291</td>
<td>2019년 1월 9일</td>
<td>다중 {{site.data.keyword.appid_full_notm}} 인스턴스에 대한 지원이 추가되었습니다.</td>
<td>-</td>
</tr>
<tr>
<td>393 / 282</td>
<td>2018년 11월 29일</td>
<td>{{site.data.keyword.appid_full_notm}}에 대한 성능이 향상되었습니다.</td>
<td>-</td>
</tr>
<tr>
<td>384 / 246</td>
<td>2018년 11월 14일</td>
<td>{{site.data.keyword.appid_full_notm}}의 로깅 및 로그아웃 기능이 향상되었습니다.</td>
<td>`*.containers.mybluemix.net`에 대한 자체 서명 인증서가 자동으로 생성되어 클러스터에서 사용되는 LetsEncrypt 서명 인증서로 대체되었습니다. `*.containers.mybluemix.net` 자체 서명 인증서는 제거되었습니다.</td>
</tr>
<tr>
<td>350 / 192</td>
<td>2018년 11월 5일</td>
<td>Ingress ALB 추가 기능의 자동 업데이트를 사용 및 사용 안함으로 설정할 수 있는 지원이 추가되었습니다.</td>
<td>-</td>
</tr>
</tbody>
</table>
