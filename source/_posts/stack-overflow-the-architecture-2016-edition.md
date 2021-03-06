---
title: 'Stack Overflow : The Architecture - 2016 Edition'
id: 241
categories:
  - 미분류
date: 2016-04-04 20:04:40
tags:
---

지난 2월 17일 Stack Overflow의 시스템 관리자인 [Nick Craver의 블로그의 올라온 글](http://nickcraver.com/blog/2016/02/17/stack-overflow-the-architecture-2016-edition/)을 번역해 보려고 합니다.

[caption id="" align="aligncenter" width="601"]![](https://i.imgur.com/TVJUOXg.png) 주인장 허락 받은 글임.[/caption]

개발바보를 devpools로 번역한 부족한 개발자인 만큼, 이상한 부분은 댓글로 달아주시면  수정하도록 하겠습니다. :)

이 번역에는 구글번역기님이 많은 도움을 주실 예정입니다.

* * *

##### - Contents -

<div style="font-size: 10px;">

Ground Ruls

Ther Internets

Load Balancers(HAProxy)

Web Tier(IIS 8.5, ASP.Net MVC5.2.3, and .Net4.6.1)

Service Tier(IIS, ASP.Net MVC5.2.3, .Net4.6.1, and HTTP.SYS)

Cache &amp; Pub/Sub(Redis)

Websockets(NetGain)

Search(Elasticsearch)

Database(SQL Server)

Libraries

</div>

* * *

&nbsp;

이 포스트는 오랜 시간에 걸처시리즈로 연재될 Stack Overflow의 아키텍처에 대한 첫번째 글입니다.

벌써 다음 글이 포스팅 되었네요.

[Stack Overflow : A Technical Deconstruction](http://nickcraver.com/blog/2016/02/03/stack-overflow-a-technical-deconstruction/)

그럼 이번글 번역&amp;의역&amp;리뷰 시작합니다.

이 기능의 모든것에 대한 아이디어를 얻기 위해, Stack Overflow에서 평일에 업데이트를 시작했습니다. 그래서 여러분은 [2013년 11월의 이전 버전](http://nickcraver.com/blog/2013/11/22/what-it-takes-to-run-stack-overflow/)과 비교해 볼 수 있습니다. 여기 2013년 11월 12일과 비교한 2016년 2월 9일에 대한 하루동안에 통계자료가 있습니다.

*   **209,420,973 (+61,336,090) HTTP requests to our load balancer**
*   66,294,789 (+30,199,477) of those were page loads
*   1,240,266,346,053 (+406,273,363,426) bytes (1.24 TB) of HTTP traffic sent
*   569,449,470,023 (+282,874,825,991) bytes (569 GB) total received
*   3,084,303,599,266 (+1,958,311,041,954) bytes (3.08 TB) total sent
*   504,816,843 (+170,244,740) SQL Queries (from HTTP requests alone)
*   5,831,683,114 (+5,418,818,063) Redis hits
*   17,158,874 (not tracked in 2013) Elastic searches
*   3,661,134 (+57,716) Tag Engine requests
*   607,073,066 (+48,848,481) ms (168 hours) spent running SQL queries
*   10,396,073 (-88,950,843) ms (2.8 hours) spent on Redis hits
*   147,018,571 (+14,634,512) ms (40.8 hours) spent on Tag Engine requests
*   **1,609,944,301 (-1,118,232,744) ms (447 hours) spent processing in ASP.Net**
*   22.71 (-5.29) ms average (19.12 ms in ASP.Net) for 49,180,275 question page renders
*   11.80 (-53.2) ms average (8.81 ms in ASP.Net) for 6,370,076 home page renders

위의 통계자료를 보면, 2013년도보다 하루에 6100만건이나 더 많은 요청이 있었음에도 불구하고  ASP.Net의 급격한 처리시간 감소했다는 것에 대해 의문이 생길 것입니다.  그 이유는 [2015년 하드웨어 업그레이드](http://blog.serverfault.com/2015/03/05/how-we-upgrade-a-live-data-center/)와 어플리케이션 내부의 많은 성능 튜닝 때문입니다. 잊지마세요! [성능도 기능입니다:)](http://blog.codinghorror.com/performance-is-a-feature/) 혹시, 제가 언급한 것 이상의 하드웨어 성능이 궁금하셨다면 두려워하지 마세요. 다음 포스트는 모든 서버에 대한 [하드웨어 스펙의 부록](http://nickcraver.com/blog/2016/02/03/stack-overflow-a-technical-deconstruction/)이 될 겁니다. (아마 제가 살아있는 동안 쓰긴할 거예요; 이미 작성되서 링크 걸어놨습니다^^)

그래서 지난 2년동안 무엇이 바뀌었냐구요? 몇개의 서버와 네트워크 장비를 교체한거 말고는 특별한 건 없습니다. 여기 현재 사이트에서 운영하는 하드웨어의 목록이 있습니다.(2013년 이후로 달라진 점에 주목해 주세요)

*   4 Microsoft SQL Servers (new hardware for 2 of them)
*   11 IIS Web Servers (new hardware)
*   2 [Redis](http://redis.io/) Servers (new hardware)
*   3 Tag Engine servers (new hardware for 2 of the 3)
*   3 [Elasticsearch](https://www.elastic.co/) servers (same)
*   4 [HAProxy](http://www.haproxy.org/) Load Balancers (added 2 to support CloudFlare)
*   2 Networks (each a [Nexus 5596 Core](http://www.cisco.com/c/en/us/products/collateral/switches/nexus-5000-series-switches/data_sheet_c78-618603.html) + [2232TM Fabric Extenders](http://www.cisco.com/c/en/us/products/switches/nexus-2232tm-10ge-fabric-extender/index.html), upgraded to
*   10Gbps everywhere)
*   2 Fortinet [800C](http://www.fortinet.com/products/fortigate/enterprise-firewalls.html) Firewalls (replaced Cisco 5525-X ASAs)
*   2 Cisco [ASR-1001](http://www.cisco.com/c/en/us/products/routers/asr-1001-router/index.html) Routers (replaced Cisco 3945 Routers)
*   2 Cisco [ASR-1001-x](http://www.cisco.com/c/en/us/products/routers/asr-1001-x-router/index.html) Routers (new!)

Stack Overflow를 운영하기 위해 필요한 것은 무엇일까요? 2013도 이후로 크게 변경되지는 않았습니다. 하지만 최적화와 위에 언급된 새 하드웨어들 때문에 우리는 오직 하나의 웹서버가 필요했습니다. 우리는 성공적으로 몇번에 걸처 테스트 했습니다. 명확히 말하자면,  이것이 좋은 아이디어라는 것이 아니라, 동작한다는 것입니다.

우리는 규모의 아이디어에 대한 몇가지 기준을 가지고 있어 우리가 얼마나 멋진 웹페이지를 만들수 있는지를 들여다 보겠습니다. 몇가지 시스템은 완전히 독립되어서 존재하기 때문에, 아키텍처에 대한 결정은 보통 각 부분들이 전체에 얼마나 적합한지의 큰 그림 없이 이해가 됩니다. [이후의 여러 글](https://trello.com/b/0zgQjktX/blog-post-queue-for-stack-overflow-topics)들은 구체적으로 깊게 파 볼겁니다. 이 글은 하드웨어의 개요가 될 것입니다; 다음 포스트는 하드웨어의 세부사항에 대해 다룰 것입니다.

지금의 하드웨어가 어떻게 생겼는지 보고싶어하는 사람들을 위해 [2015년 2월 업그레이드](http://blog.serverfault.com/2015/03/05/how-we-upgrade-a-live-data-center/) 할 당시의 rack A의 사진이 있습니다.(이것과 같은 rack B도 있습니다.)

![](https://i.imgur.com/kturHDZ.jpg)

지금 이 사진을 보고 빠져드셨다면, 그주에 찍은 [256개의 이미지 앨범](http://imgur.com/a/X1HoY)이 있습니다.(이 숫자는 의도된 것은 아닙니다 ㅎㅎ)

지금부터 깊게 들여다 봅시다. 주요 시스템의 논리적 개요입니다.

![](https://i.imgur.com/GRU0ItF.png)

#### Ground RulEs

여기 전체적으로 적용되는 몇가지 규칙이 있습니다. 그래서 모든 설정을 반복할 필요가 없습니다.

*   모든 것은 중복 구성됩니다.
*   모든 서버와 네트워크 장비는 최소한 2x 10Gbps 연결이 있습니다.
*   모든 서버는 2개의 전원피드와 2개의 발전기와 2개의 유틸리티 피드에 의해 돌아가는 2개의 전원 공급장치가 있습니다.
*   모든 서버는 rack A와 rack B사이에 중복되는 파트너가 있습니다.
*   비록 내가 대부분 여기 뉴욕에 있는 데이터 센터에 대하여 이야기 하지만, 모든 서버와 서비스는 또다른 데이터 센터(콜로라도)를 통해 중복해서 구성되어 있습니다.

#### The Internets

먼저 우리가 찾아야 할 것은 [DNS](https://en.wikipedia.org/wiki/Domain_Name_System)(Domain Name System) 입니다. 우리 이것을 빨리찾게 하기 위해서 [CloudFlare](https://www.cloudflare.com/)에 맡깁니다.  CloudFlare는 더 가까운 DNS서버를 가지고 있기 떄문입니다. 그리고 우리는 API를 통해 DNS기록을 업데이트 하고 DNS로 호스팅을 수행합니다. 하지만 우리는 깊은 신뢰도 문제 때문에 여전히 DNS 서버를 가지고 있습니다.  세상에 대참사가 발생했을 때(아마 GPL, Punyon 또는 캐싱에 의한 대참사) 사람들이 여전히 프로그램을 그들의 마음에서 떠나 보낸다면, 우리는 그제서야 휙 던저버릴 것입니다.

우리의 비밀은신처를 찾은 후에 HTTP트래픽은 네가지 ISP(Level3, Zayo, Cogent, Lightower in NewYork)중에서 하나에 의해 발생하고 네가지 엣지 라우터 중 하나를 통해 흐릅니다. 우리는 트래픽의 흐름을 제어하고 가장 효율적으로 목적지에 도착하기 위해  몇가지 길을 제공하기 위해 [BGP(Border Gateway Protocol)](https://en.wikipedia.org/wiki/Border_Gateway_Protocol)를 사용하는 ISP를 자세히 들여다 봐야 합니다. ASR-1001과 ASR-10010X라우터는 2쌍이고, 각각 2개의 ISP를 제공합니다. 비록 두개의 라우터는 모두 물리적으로 같은 10Gbps의 네트워크일지 몰라도, 외부 트레픽은 로드 밸런서가 연결하는 VLAN과 분리됩니다. 라우터를 통해서 흐른 후에 로드밸런서로 갑니다.

저는 우리가 2개의 데이터 센터 사이에 10Gbps [MPLS](https://en.wikipedia.org/wiki/Multiprotocol_Label_Switching)(Multiprotocol Label Switching)를 가지고 있다고 언급할 수 있는 좋은 기회가 되었다고 생각합니다. 그러나 이것에 사이트를 제공하는데 직접 관여하진 않습니다. 저희는 만약의 돌발상황을 대비해서 데이터의 복제와 빠른 복구를 위해 이것을 사용합니다. "하지만 Nick, 그건 중복이 아니예요!" 여러분은 기술적으로 정확합니다.([the best kind of correct](https://www.youtube.com/watch?v=hou0lU8WMgo)) 그게 유일한 실패요소입니다. 하지만 기다리세요! 우리는 ISP를 통해 2개 이상의 시스템 대체 [OSPF](https://en.wikipedia.org/wiki/Open_Shortest_Path_First)루트를 가지고 있습니다.(MPLS가 #1이고, 비용지불에 의한 #2와 #3이 있습니다.) 각 셋은 콜로라도에 일치하는 장비에 더 빠른 연결을 요청합니다. 그리고 그것들은 트래픽을 각 장애 조차 상황 사이에 나누어 줍니다. 우리는 두가지를 연결하고 4가지 경로를 가질 수 있습니다.

&nbsp;

#### Load Balancers(HAProxy)

저희는 리눅스를 선호하기 때문에 로드 밸런서는 [CentOX 7](https://www.centos.org/)기반으로 [HAProxy](http://www.haproxy.org/) 1.5.15를 사용하고 있습니다. TLS(SSL)도 지원합니다. [HTTP/2](https://en.wikipedia.org/wiki/HTTP/2)가 지원되는 HAProxy1.7의 출시를 기다리고 있습니다.

듀얼 10Gbps LACP 네트워크 링크를 가진 다른 서버들과는 달리, 각 로드 밸런서는 10Gbps 2쌍을 가지고 있습니다: 외부 네트워크를 위한 것과 DMZ를 위한 것. 이 박스들은 64GB 실행하거나 더많이 효율화하기 위해 그 이상의 메모리를 SSL 협상을 처리합니다.  우리가 재사용 메모리에서 TLS 세션을 더 많이 캐싱할 때, 이전과 동일한 연속된 접속에 대해서는 재계산을 덜할 수 있습니다. 꽤 싼 RAM이 주어졌을 때, 이것은 쉬운 선택입니다.

로드 밸런서는 간단하게 설치할 수 있습니다. 다양한 IP의 다른 사이트와 주로 호스트헤더에 기반한 다양한 백엔드 경로를 듣고 있습니다. 우리가 여기에서 주목해야할 것은 속도제한과 HAProxy시스템로그 메시지에 있는 헤드 캡처이고 그래서 모든 단일 요청에 대해 성능메트릭스를 기록할 수 있습니다. 이것도 나중에 이야기 할 것입니다.

#### Web Tier(IIS 8.5, ASP.Net MVC5.2.3, and .Net4.6.1)

로드밸런서는 트래픽을 "primary"(01-09)라고 불리는 9개의 서버와 2개의 "dev/meta"(10-11, 스테이징 환경)로 나누어 줍니다. 프라이머리 서버는 마지막 2개의 서버에서  Stack Overflow, Career 그리고 meta.stackoverlfow.com과 meta.stackexchange.com를 제외한 Stack Exchange 사이트를 운영합니다. 프라이머리 Q&amp;A Application은 그자체로 멀티테넌시를 지원한다. 이것은 하나의 어플리케이션이 모든 Q&amp;A사이트에 대한 요청을 받는다는 것을 의미합니다.  우리는 하나의 서버에서 하나의 어플리케이션 풀의 전체의 Q&amp;A 네트워크를 실행할 수 있습니다. Career, API v2,  모바일 API 등과 같은 다른 어플리케이션은 분리됩니다.  아래는 IIS에서 primary와 dev계층의 모습입니다.

![](https://i.imgur.com/pim6K0V.png)

Observer(내부적으로 사용하는 모니터링 대쉬보드)에서 웹계층을 가로지르는 Stack Overflow의 분포의 모습입니다.

![](https://i.imgur.com/9CbOdoj.png)

그리고 사용관점에서 본 웹서버의 모습입니다.

![](https://i.imgur.com/6bbugFc.png)

&nbsp;

#### Service Tier(IIS, ASP.Net MVC5.2.3, .Net4.6.1, and HTTP.SYS)

웹서버는 서비스계층과 매우 유사합니다. 또한 Windows 2012R2에서 IIS 8.5를 실행시킵니다. 이 계층은 생산웹계층과 다른 내부적시스템을 지원하기 위해 내부적 서비스를 운영해 왔습니다. 이 두가지는 태그엔진을 우녕ㅇ하고 http.sys를 기반으로 하는 Stack Server와 Providence API(IIS-based)입니다. 재미있는 사실은 2분 간격으로 질문목록을 새로고침할 때, Stack Server가 L2 와 L3를 제압하기 때문에 저는 각2개의 프로세스에서 소켓을 분리시키기 위해 관계를 세팅해야 합니다.

서비스계층은 중복성을 필요로하는 태그엔진과 벡엔드 API를 떠받치고 있지만, 모든 것을 9배 중복하는 것을 의미하진 않습니다. 예를들면 데이터베이스로부터 매n분마다 변경되는 보스트와 태그들을 로딩하는 것은 그렇게 가벼운 작업이 아닙니다. 우리는 웹계층에서 9번 로딩하는 것을 원하지 않습니다. 3이면 충분하고 우리에게 충분히 안전성을 제공합니다. 우리는 또한 태그엔진과 엘라스틱 인덱싱 작업을 최적화를 위해서 이계층의 하드웨어에서 환경을 다르게 설정합니다.  태그 엔진은 상대적으로 복잡하고 포스트에 연관성이 높습니다.  /questions/tagged/java에 방문했을 때, 질문에 맞는 것을 보기 위해 태그 엔진을 검색합니다. 이것은 /search의 밖에서 태그에 부합하는 모든 것을 의미합니다.

Cache &amp; Pub/Sub([REDIS](http://redis.io/))

우리는 Redis를 사용하고 Redis는 견고합니다. 한달에 1600억 ops를 처리하면서도 모든 인스턴스는 CPU를 2%이하로 사용합니다. 보통은 훨씬더 적게 사용합니다.![](https://i.imgur.com/hvszb0x.jpg)

저희는 Redis의 L1/L2 캐쉬 시스템을 사용합니다. L1"은 웹서버 또는 어플의 HTTP캐쉬입니다. L2는 레디스로 돌아가서 값을 가져옵니다. 값은 [Protobuf format](https://developers.google.com/protocol-buffers/)에 저장됩니다. 클라이언트로 StackExchange.Redis를 사용합니다. 하나의 웹서버가 L1과 L2 둘다에서 캐쉬하는 것을 잊을 때, 이것은 소스로 부터 값을 복구합니다.(데이터베이스 쿼리, API call 등) 그리고 로컬 캐쉬와 레디스에 그결과를 넣습니다. 값을 원하는 다음 서버는 L1을 놓쳐도 됩니다. 그러나 데이터베이스 쿼리나 API call을 통해 저장한 L2/Redis에서 값을 찾을 겁니다.

우리는 또한 Q&amp;A사이트를 운영합니다. 각각의 사이트는 사이트 자체의 L1/ L2캐쉬를 가지고 있습니다.

레디스는 캐쉬로만 사용하지 않습니다. 레디스는 하나의 서버가 메시지를 출판하고 모든 다른 구독자가 그것을 받아 볼 수 있는 출판과 구독의 매커니즘을 가지고 있습니다. 우리는 웹서버를 제거할 때 이 매커니즘을 다른 서버의 L1캐쉬를 비우기 위해 사용합니다.  하지만 웹소켓이라는 또다른 방법이 있습니다.

#### Websockets([NETGAIN](https://github.com/StackExchange/NetGain))

실시간 업데이트를 위해 웹소켓을 사용합니다. (예를 들면 topbar의 알림, 투표수,  newnav의 수, 새 답변과 댓글 그리고 다른 몇가지들을 실시간으론 처리하기 위해 말이죠.)

웹소켓 서버는 웹티어에서 실행시키기 위해 raw 소켓(개발자가 새로운 프로토콜을 설계하여 구현하거나, 패킷을 세밀하게 조작할 때 사용하는 소켓. 커널 수준에서만 다룰 수 있었던 패킷의 헤더 등을 직접 구현하거나 조작할 수 있다.)을 사용합니다. 우리의 오픈소스 라이브러리인 [StackExchange.NetGain](https://github.com/StackExchange/NetGain)에서 raw 소켓은 매우 가벼운 어플리케이션입니다. 동시접속자가 가장 많은 때는 약 500,000개의 웹소켓 연결이 발생합니다. 재미있는 사실은 이 브라우저중 일부는 18개월동안 열려 있다는 점입니다. 우리는 왜 그런지 확실히 모릅니다. 누군가는 .NetGain 개발자들이 여전히 활동하는지 체크해봐야 합니다. 아래는 이번주 동시접속 웹소켓 패턴의 모습입니다.

![](https://i.imgur.com/bCrrf3t.png)

왜 웹소켓이냐구요? 웹소켓은 우리가가진 규모에서 폴링하는 것보다 훨씬 더 효율적이기 때문입니다. 우리는 단순히 더 적은 자원으로 더 많은 데이터를 밀어 넣을 수 있습니다. 반면에 사용자들은 더많은 인스턴스를 가지게 됩니다. 이 방법도 문제가 없는 것은 아닙니다. 로드 밸러서에 임시포트와 파일 처리의 고갈은 [우리가 나중에 다뤄 볼 재미있는 이슈](https://trello.com/c/7nv66g78/58-websockets)입니다.

#### Search([Elasticsearch](https://www.elastic.co/products/elasticsearch))

스포일러 : 이 부분에서 많이 흥미로운 것은 없습니다. 웹계층은 엘라스틱서치1.4에 대항하여 고성능의 StackExchange.Elastic클라이언트를 이용하여 꽤 단순한 검색을 합니다. 대부분의 것과 달리, 우리가 가지고 있는 API의 일부를 노출하는 것이기 때문에 우리는 오픈소스에 대한 계획이 없습니다. 저는 오히려 더 많이 노출시키는 것이 개발자들을 혼란스럽게 할 것이라고 생각합니다.

각각의 엘라스틱 클러스터는 3개의 노드를 가지고 있고, 각 싸이트는 각자의 인덱스를 가지고 있습니다.  Career는 추가적으로 적은 인덱스를 갖고 있습니다. 우리의 3개의 서버 클러스터는 SSD스토리지, 192GB의  RAM, 듀얼 10Gbps의 네트워크를 각각 가지고 있는 병균의 것들보다 조금 느립니다.

태그엔진을 호스팅하는 Stack서버에서 같은 어플리케이션 도메인은 엘라스틱 서치에서 아이템들을 인덱싱합니다. 우리는 여기서 [ROWVERSION in SQL Server](https://msdn.microsoft.com/en-us/library/ms182776.aspx)처럼 몇가지 간단한 트릭을 가지고 있습니다. 이것은 순차적인것처럼 행동하기 때문에 변화하는 아에템들을 쉽게 붙잡고 인덱싱할 수 있습니다.

우리가 SQL-full-text검색과 같은 것을 사용하지 않고 엘라스틱서치를 사용하는 주된 이유는 확장성과 메모리 할당 때문입니다. SQL CPU는 엘라스틱에 비해 비교적 비쌉니다. Solr는 왜 안되냐구요? 저희는 전체의 네트워크에서 검색하기 원합니다.(한번에 많은 인덱스를 걸어) 그리고 이것은 결정시간을 지원하지 않습니다. 우리가 2.x버전을 가직 사용하지 않는 이유는 type에 대한 주된 변화입니다. 업그레이드를 위해서는 모든 것을 다시 인덱싱하는 것이 필요합니다. 그러기에 우리는 시간이 부족합니다.

#### Databases (SQL Server)

저희는 SQL Server를 [single source of truth](https://en.wikipedia.org/wiki/Single_source_of_truth)(SSOT, 모든 핵심 데이터는 코드에서 한 번만 등장해야 한다)로 사용합니다. Elastic과 Redis의 모든 데이터는 SQL Server에서 가져옵니다. 우리는 [AlwaysOn Availablility Group](https://msdn.microsoft.com/en-us/library/hh510230.aspx)(데이터베이스 미러링에 대한 엔터프라이즈 수준의 대안을 제공하는 고가용성 및 재해 복구 솔루션)과 함께 2개의 SQL Server 클러스터를 실행합니다. 각 클러스터는 하나의 마스터를 가지고 뉴욕에 하나의 복제본을 가지고 있고, 또 콜로라도(우리의 DR 데이터 센터)에 또하나의 복제본을 가지고 있습니다. 모든 복제본은 동기화되어 있습니다.

첫번째 클러스터는 각 RAM 384GB, PCle SSD 4TB, 2x12 core를 가진 Dell R720xd서버 셋입니다. 이 서버는 Stack Overflow, Site(나쁜 이름입니다. 나중에 설명하겠습니다.), PRIZM, 모바일 데이터베이스를 호스팅합니다.

두번째 클러스터는 각 RAM 768GB, PCle SSD 6TB, 2x8core를 가진 Dell R730xd 서버 셋입니다. 이 클러스터는 [Careers](http://careers.stackoverflow.com/), [Open ID](https://openid.stackexchange.com/), [Chat](https://chat.stackoverflow.com/), [Exception log](https://github.com/NickCraver/StackExchange.Exceptional), 다른 모든 Q&amp;A사이트(예를 들면 [Super User](http://superuser.com/), [Server Fault](http://superuser.com/) 등)를 포함하여 다른 모든 것을 실행시킵니다.

데이터베이스계층에서 CPU의 사용은 우리가 매우 적게 유지하려는 부분입니다. 그러나 사실상 캐쉬문제 때문에 순간 높아질 때가 있습니다.  지금은 NY-SQL02와 04 가 마스터이고, 01과 03가 SSD업그레이드 하는 동안 재시작되는 복제본입니다. 아래는 지난 24시간 동안의 데이터베이서 서버의 모습입니다.

![](https://i.imgur.com/CyE3rEX.png)

우리의 SQL사용법은 꽤 단순합니다. 단순하다는 것은 빠르다는 것입니다. 비록 몇가지 쿼리가 미쳐 날뛸 순 있지만,  SQL과의 상호작용은 꽤 단순합니다. 우리는 일부 과거의 시스템인 [Linq2Sql](https://msdn.microsoft.com/en-us/library/bb425822.aspx)를 가지고 있지만 새로운 개발은 [Dapper](https://github.com/StackExchange/dapper-dot-net)와 우리의 오픈소스인 [POCOs](https://en.wikipedia.org/wiki/Plain_Old_CLR_Object)를 사용한 Micro-ORM을 사용합니다. Stack Overflow는 데이터베이스에서 한번만 저장하는 절차를 가지고 있는데 이것은 코드에 마지막 흔적만 이동시키려는 의도입니다.

#### Libraries

직접적으로 여러분에게 도움이되는 것은 제가 위에 언급했던 것처럼 몇가지 장비를 변경하는 것입니다.  그리고 아래는 저희가 사용하는 .Net 라이브러리들의 오픈 소스의 리스트입니다다. 아래 오픈소스들은 핵심 비지니스 가치는 없지만 전세계의 개발자들에게 는 도움을 줄 수 있기 때문에 공개합니다. 여러분이 유용함을 발견할 수 있기를 바랍니다.

*   [Dapper](https://github.com/StackExchange/dapper-dot-net) (.Net Core) - High-performance Micro-ORM for ADO.Net
*   [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) - High-performance Redis client
*   [MiniProfiler](http://miniprofiler.com/) - Lightweight profiler we run on every page (also supports Ruby, Go, and Node)
*   [Exceptional](https://github.com/NickCraver/StackExchange.Exceptional) - Error logger for SQL, JSON, MySQL, etc.
*   [Jil](https://github.com/kevin-montrose/Jil) - High-performance JSON (de)serializer
*   [Sigil](https://github.com/kevin-montrose/sigil) - A .Net CIL generation helper (for when C# isn’t fast enough)
*   [NetGain](https://github.com/StackExchange/NetGain) - High-performance websocket server
*   [Opserver](https://github.com/opserver/Opserver/tree/overhaul) - Monitoring dashboard polling most systems directly and feeding from Orion, Bosun, or WMI as well.
*   [Bosun](http://bosun.org/) - Backend monitoring system, written in Go

다음은 우리의 코드를 실행하는 구체적인 하드웨어 리스트입니다. 그 후 우리는 [다음 목록](https://trello.com/b/0zgQjktX/blog-post-queue-for-stack-overflow-topics)으로 넘어갈 것입니다. 지켜봐 주세요:)