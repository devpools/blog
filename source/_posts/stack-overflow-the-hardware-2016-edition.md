---
title: 'Stack Overflow: The Hardware - 2016 Edition'
id: 302
categories:
  - Small talk
date: 2016-04-18 19:25:48
tags:
---

발번역 입니다;( 간절히 첨삭 부탁드립니다.

* * *

&nbsp;

> Stack Overflower's architecture의 두번째 글입니다. 이전포스트는 [Stack Overflow: The Architecture - 2016 Edition](http://nickcraver.com/blog/2016/02/17/stack-overflow-the-architecture-2016-edition/)  (번역본 링크는 [여기](http://devpools.kr/2016/04/04/stack-overflow-the-architecture-2016-edition/))에서 확인하세요 :)

누가 하드웨어를 사랑한다고 생각하나요? 내가 그렇습니다. 이건 내블로그입니다. 내가 이겼어요. 만약 여러분이 하드웨어를 사랑하지 않는다면 브라우저를 닫으세요!

아직 안닫으셨네요. 좋습니다! 브라우저가 미치게 느릴때, 몇가지 새로운 하드웨어에 대해 생각해 봐야 합니다.

여러번 다시 말하지만, [성능 또한 기능입니다](http://blog.codinghorror.com/performance-is-a-feature/).(전 포스트에서도 엄청 강조했었습니다.^^)  <span style="line-height: 1.75;">여러분의 코드가 하드웨어만큼 빠르기 때문에, 하드웨어는 확실히 문제가 됩니다. 다만 다른 플랫폼처럼 스택 오버플로우의 아키텍쳐도  계층을 가지고 있습니다. 우리에게 하드웨어는 기초 계층(최하단의 계층)이고, 하드웨어를 가지고 많은 고급진 일들을할 수 있습니다.(누군가 다른 서버를 운영하는 것 처럼) 하드웨어는 직간접적으로 비용이들지만 그건 이 포스트에서 보는 관점은 아닙니다. </span>[그건 추후에 비교할 것입니다. ](https://trello.com/c/4e6TOnA7/87-on-prem-vs-aws-azure-etc-why-the-cloud-isn-t-for-us)<span style="line-height: 1.75;"> 지금부터 저는 우리의 인프라에 대한 자세한 목록과 이것을 비교 목적을 이야기하고자합니다.</span>

이 시리즈의 많은 포스트에서, 저는 많은 숫자와 스펙을 보여드릴 것입니다. 제가 "우리의 SQL 서버의 CPU이용율은 대략 5-10%입니다."라고 이야기할 때,  좋아 그런데 5-10%가 어느정도지?라고 생각할 수 있습니다. 그때가 레퍼런스가 필요하나 시점입니다. 이 하드웨어 리스트는 이러한 질문에 대한 대답과 비교에 대한 리소스를 제공할 것입니다.

* * *

##### – Contents –

How We Do Hardware

Servers Running Stack Overflow &amp; Stack Exchange Sites

Servers for Other Bits

* * *

#### How We Do Hardware

저는 하드웨어 스펙을 혼자 정하지 않습니다. 하드웨어 스펙을 정할 때, 저는 주로 George Beech([@GABeech](https://twitter.com/GABeech))와 함께 일합니다. 저희는 각 하드웨어의 스펙을 목적에 맞게 정하려고 노력합니다.  우리는 이과정에서 혼자가 아닙니다. 우리는 최적의 스펙을 갖기 위해 하드웨어가 어떻게 운영되는지 알 필요가 있습니다. 우리는 다른 개발자들과 함께 일할 것입니다. 그리고 다른 사이트의 신뢰성 있는 엔지니어들을 수용할 것입니다.

우리는 또한 이 시스템에서 무엇이 최고인가를 생각합니다. 각 서버는 독립되어있지 않습니다. 어떻게 이것이 전반적인 아키텍처에 적합한지는 명확한 고려사항입니다. 어떤 서비스가 이 플랫폼에 적용될수 있는가? 이 데이터스토어인가? 로그시스템인가?  최소한의 적은 변화는 거의 변화가 없는 것을 관리하는 고유의 값이 있습니다.

하드웨어의 스펙을 정할 때, 무수히 많은 요구사항을 고려합니다. 한번도 저희의 고려사항을 적어내려가 본적이 없습니다. 지금 한번 해보도록 하죠.

*   이건 scale-up과 scale-out의 문제인가?(하나의 큰 하드웨어를 살까 아님 작은 것을 여러개 살까?)
*   얼마나 많은 여분이 필요하가?/원하는가?(얼마나 많은 헤드룸과 시스템 대체작동 가능성이 있는가?)
*   스토리지:

    *   서버/어플리케이션이 디스크에 접근할 것인가?(OS 드라이브 이외에도 어떠한 것이 필요한가?)

            *   만약 필요하다면, 얼마나 많이 필요한가?(얼마나 많은 대역폭과 얼마나 많은 작은 파이들이 필요한가? SSD 또한 필요한가?)
        *   만약 SSD가 필요하다면, 무엇을  SSDs, 쓰기로드(write load)는 무엇인가? (Intel S3500/3700s?에 대해서 이야기할 것인가? 아님 P360x? P3700s?)

                    *   얼마나 많은 SSD용량이 필요한가?(그리고 HDD를 가진 2티어 솔루션이어야 하는가?)
            *   이 데이터는 완전히 일시적 데이터인가?(SSD는 더 저렴하고 더 적합한 커페시터가 없는 SSD인가?)

        *   스토리지는 확장이 필요할 것인가?(우리는 1U/10-bay 서버를 가지고 있난그 아니면 2U26-bay 서버를 가지고 있는가?)
    *   데이터웨어하우스의 타입 시나리오인가?(.35' 드라이브를 찾고 있는가? 만약 그렇다면 2U당 12 또는 16 드라이브는 어떤가?)

            *   이 스토리지는 120W TDP의 한계를 가진 3.5' q백플레인 상호 보완적인가?

        *   직접 디스크를 노출하는 것이 필요한가?(컨트롤러를 통과하게 해야 하는가?)

*   메모리:

    *   얼마나 많은 메모리가 필요한가?(우리는 무엇을 구입해야 하는가?)
    *   얼마나 많은 메모리를 사용할 수 있는가?(무엇을 구매하는 것이 합리적인가?)
    *   후에 더 많은 메모리가 필요할 것이라고 생각되는가?(어떤 메모리 채널 환경설정을 바꿔야 하는가?)
    *   메모리 접근이 많은 어플리케이션인가?(클럭 속도를 최대로 하길 원하는가?)

            *   고도의 병렬접근을 하고 있는가?(우리는 DIMM(dual in-line memory module, 여러개의 DRAM칩을 회로 기판 위에 탑재한 메모리 모듈)을 통해같은 공간에서 더 많은 접근을 하는 것을 원하는가?)

*   CPU:

    *   어떤 종류의 접근을 생각하고 있는가?(우리는 기본 CPU나 파워가 필요한가?)
    *   무거운 병렬처리를 하는가?(더 적고 빠른 코어를 원하는가? 아니면 더 많고 느린 코어를 원하는가?)

            *   어떤 방법인가? 무거운 L2/L3캐시인가?(성능을 위해 거대한 L3캐쉬가 필요한가?)

        *   거의 싱글 코어의 성능인가?(최대의 클럭을 원하는가?)

            *   만약 그렇다면, 한번에 얼마나 많이 처리하는가?

*   네트워크:

    *   여분의 10Gb네트워크 연결이 필요한가?(로드밸런서 같은은"through"머신인가?)
    *   송수신 버퍼(Tx/Rx 버퍼)에 얼마나 균형이 필요한가?(어떤 CPU코어가 최고의 균형을 계산하는가?)

*   Redundancy:

    *   우리는 장애복구 데이터 센터가 필요한가?

            *   우리는 같은 번호가 필요한가? 또는 적은 중복은 허용하는가?

*   우리는 전원 코드가 필요한가? No. No. 우리는 필요하지 않다.

#### Servers Running Stack Overflow &amp; Stack Exchange Sites

각 서버는 아래의 스펙을 가지고 있습니다.

*   특별한 경우가 아니라면 OS 드라이브를 포함하지 않다. 대부분의 서버는 RAID1(**R**edundant **A**rray of **I**nexpensive/**I**ndependent **D**isk:RAID의 주 사용 목적은 크게 **무정지 구현**(안정성)과 **고성능 구현**으로 구분된다. 무정지 구현을 극도로 추구하면 RAID 1, 고성능 구현을 극도로 추구하면 RAID 0이 되며, RAID 5, 6은 둘 사이에서 적당히 타협한 형태.)에서 250 혹은 500GB SATA HDD 쌍입니다.  비록 부팅시간의 대부분은 드라이브 속도에 독립적이지 않더라도(예를들어 메모리의 768GB를 체크합니다.) 부팅시간은 고려대상이 아니다.
*   모든 서버는active/active [LACP](https://en.wikipedia.org/wiki/Link_aggregation#Link_Aggregation_Control_Protocol)의 2개 또는 그 이상의 10Gb네트워크 링크로 연결되어 있다.
*   모든 서버는 208V 단일전원으로 동작한다. All servers run on 208V single phase power (2개의 PDU로부터 공급받는 2개의 PSU를 통해).
*   뉴욕에 있는 모든 서버들은 케이블 암(arm)을 가지고 있고 덴버의 모든 서버들은 가지고 있지 않다.(현지 엔지니어들의 선호)
*   모든 서버들은 [iDRAC](http://en.community.dell.com/techcenter/systems-management/w/wiki/3204.dell-remote-access-controller-drac-idrac) connection (via the management network)과 a KVM connection를 가지고 있다.

###### Network

*   2x Cisco Nexus [5596UP](http://www.cisco.com/c/en/us/products/switches/nexus-5596up-switch/index.html) core switches (96 SFP+ ports each at 10 Gbps)
*   10x Cisco Nexus [2232TM](http://www.cisco.com/c/en/us/products/switches/nexus-2232tm-10ge-fabric-extender/index.html) Fabric Extenders (**2 per rack** - each has 32 BASE-T ports each at 10Gbps + 8 SFP+ 10Gbps uplinks)
*   2x Fortinet [800C](http://www.fortinet.com/products/fortigate/enterprise-firewalls.html) Firewalls
*   2x Cisco [ASR-1001](http://www.cisco.com/c/en/us/products/routers/asr-1001-router/index.html) Routers
*   2x Cisco [ASR-1001-x](http://www.cisco.com/c/en/us/products/routers/asr-1001-x-router/index.html) Routers
*   6x Cisco [2960S-48TS-L](http://www.cisco.com/c/en/us/support/switches/catalyst-2960s-48ts-l-switch/model.html) Management network switches (**1 Per Rack** - 48 1Gbps ports + 4 SFP 1Gbps)
*   1x Dell [DMPU4032](http://accessories.us.dell.com/sna/productdetail.aspx?c=us&amp;l=en&amp;s=bsd&amp;cs=04&amp;sku=A7546775) KVM
*   7x Dell [DAV2216](http://accessories.us.dell.com/sna/productdetail.aspx?c=us&amp;l=en&amp;s=bsd&amp;cs=04&amp;sku=A7546777) KVM Aggregators (**1–2 per rack** - each uplinks to the DPMU4032)

_Note: _각각 FEX는 80Gbps의 코어에 업링크 대역폭을 가지고 있고 코어들은 그들 사이에 160Gbps포트 채널을 가지고 있습니다. 더많은 최신 설치 때문에 덴버 데이터 센터의 하드웨어는 약간 더 최신화 되었습니다. 4개의 라우터들은 _[ASR-1001-x](http://www.cisco.com/c/en/us/products/routers/asr-1001-x-router/index.html) 모델이고 2개의 코어는 각각 <em>96 SFP+ 10Gbps ports와 8 QSFP+ 40Gbps ports를 가진 __[Cisco Nexus 56128P](http://www.cisco.com/c/en/us/products/switches/nexus-56128p-switch/index.html)입니다. 이 것은 뉴욕에서 했던 것처럼<em>16x 10Gbps ports를 사용하는 것 대신 <em>4x 40Gbps links코어를 결합할 수 있기 때문에 _</em>미래의 확장에 대비해  10Gbps 포트를 절약할 수 있습니다. </em></em>

<div class="pics-stack-overflow-the-hardware-2016-edition">

이것은 뉴욕 데이터 센터의 네트워크 장비 입니다.

![](http://i.imgur.com/7mPs4mT.jpg)

</div>

<div class="pics-stack-overflow-the-hardware-2016-edition">

…그리고 덴버에서:

![](http://i.imgur.com/AB4ath5.jpg)

제가 믿고 일하는 엔지니어 [Mark Henderson](https://twitter.com/thefarseeker)이 이 포스트를 위한 현재의 사진 한장 보내기 위해 뉴욕DC 로 특별한 여행을 떠났습니다.

</div>

###### SQL Servers (Stack Overflow Cluster)

*   2 Dell [R720xd](http://www.dell.com/us/business/p/poweredge-r720xd/pd) Servers, each with:
*   Dual [E5-2697v2](http://ark.intel.com/products/75283/Intel-Xeon-Processor-E5-2697-v2-30M-Cache-2_70-GHz) Processors (12 cores @2.7–3.5GHz each)
*   384 GB of RAM (24x 16 GB DIMMs)
*   1x Intel [P3608](http://www.intel.com/content/www/us/en/solid-state-drives/solid-state-drives-dc-p3608-series.html) 4 TB NVMe PCIe SSD (RAID 0, 2 controllers per card)
*   24x Intel [710](http://ark.intel.com/products/56584/Intel-SSD-710-Series-200GB-2_5in-SATA-3Gbs-25nm-MLC) 200 GB SATA SSDs (RAID 10)
*   Dual 10 Gbps network (Intel X540/I350 NDC)

#### SQL Servers (Stack Exchange “…and everything else” Cluster)

*   2 Dell [R730xd](http://www.dell.com/us/business/p/poweredge-r730xd/pd) Servers, each with:
*   Dual [E5-2667v3](http://ark.intel.com/products/83361/Intel-Xeon-Processor-E5-2667-v3-20M-Cache-3_20-GHz) Processors (8 cores @3.2–3.6GHz each)
*   768 GB of RAM (24x 32 GB DIMMs)
*   3x Intel [P3700](http://ark.intel.com/products/79620/Intel-SSD-DC-P3700-Series-2_0TB-12-Height-PCIe-3_0-20nm-MLC) 2 TB NVMe PCIe SSD (RAID 0)
*   24x 10K Spinny 1.2 TB SATA HDDs (RAID 10)
*   Dual 10 Gbps network (Intel X540/I350 NDC)

_Note: 덴버 SQL 하드웨어는 뉴욕에 있는 서버와 비슷하지만 1개의 SQL서버가 있습니다. _

뉴욕에 있는 SQL서버들은 2월에 PCle SSD로 업그레이드 한 것처럼 보입니다.

<div class="pics-stack-overflow-the-hardware-2016-edition">![](http://i.imgur.com/aUqoSuM.jpg)</div>

###### Web Servers

*   11 Dell [R630](http://www.dell.com/us/business/p/poweredge-r630/pd) Servers, each with:
*   Dual [E5-2690v3](http://ark.intel.com/products/81713/Intel-Xeon-Processor-E5-2690-v3-30M-Cache-2_60-GHz) Processors (12 cores @2.6–3.5GHz each)
*   64 GB of RAM (8x 8 GB DIMMs)
*   2x Intel [320](http://ark.intel.com/products/56567/Intel-SSD-320-Series-300GB-2_5in-SATA-3Gbs-25nm-MLC) 300GB SATA SSDs (RAID 1)
*   Dual 10 Gbps network (Intel X540/I350 NDC)

![](http://i.imgur.com/2DNxSuB.jpg)

###### Service Servers (Workers)

*   2 Dell [R630](http://www.dell.com/us/business/p/poweredge-r630/pd) Servers, each with:

    *   Dual [E5-2643 v3](http://ark.intel.com/products/81900/Intel-Xeon-Processor-E5-2643-v3-20M-Cache-3_40-GHz) Processors (6 cores @3.4–3.7GHz each)
    *   64 GB of RAM (8x 8 GB DIMMs)

*   1 Dell [R620](http://www.dell.com/us/business/p/poweredge-r620/pd) Server, with:

    *   Dual [E5-2667](http://ark.intel.com/products/64589/Intel-Xeon-Processor-E5-2667-15M-Cache-2_90-GHz-8_00-GTs-Intel-QPI) Processors (6 cores @2.9–3.5GHz each)
    *   32 GB of RAM (8x 4 GB DIMMs)

*   2x Intel [320](http://ark.intel.com/products/56567/Intel-SSD-320-Series-300GB-2_5in-SATA-3Gbs-25nm-MLC) 300GB SATA SSDs (RAID 1)
*   Dual 10 Gbps network (Intel X540/I350 NDC)

_Note: NY-SERVICE03는 교체할만큼 오래되지 않았기 때문에 여전히 R620입니다. 이건 올해 업그레이드 될 예정입니다._

###### Redis Servers (Cache)

*   2 Dell [R630](http://www.dell.com/us/business/p/poweredge-r630/pd) Servers, each with:
*   Dual [E5-2687W v3](http://ark.intel.com/products/81909/Intel-Xeon-Processor-E5-2687W-v3-25M-Cache-3_10-GHz) Processors (10 cores @3.1–3.5GHz each)
*   256 GB of RAM (16x 16 GB DIMMs)
*   2x Intel [520](http://ark.intel.com/products/66250/Intel-SSD-520-Series-240GB-2_5in-SATA-6Gbs-25nm-MLC) 240GB SATA SSDs (RAID 1)
*   Dual 10 Gbps network (Intel X540/I350 NDC)

###### Elasticsearch Servers (Search)

*   3 Dell [R620](http://www.dell.com/us/business/p/poweredge-r620/pd) Servers, each with:
*   Dual [E5-2680](http://ark.intel.com/products/64583/Intel-Xeon-Processor-E5-2680-20M-Cache-2_70-GHz-8_00-GTs-Intel-QPI) Processors (8 cores @2.7–3.5GHz each)
*   192 GB of RAM (12x 16 GB DIMMs)
*   2x Intel [S3500](http://ark.intel.com/products/75685/Intel-SSD-DC-S3500-Series-800GB-2_5in-SATA-6Gbs-20nm-MLC) 800GB SATA SSDs (RAID 1)
*   Dual 10 Gbps network (Intel X540/I350 NDC)

###### HAProxy Servers (Load Balancers)

*   2 Dell [R620](http://www.dell.com/us/business/p/poweredge-r620/pd) Servers (CloudFlare Traffic), each with:

    *   Dual [E5-2637 v2](http://ark.intel.com/products/81900/Intel-Xeon-Processor-E5-2643-v3-20M-Cache-3_40-GHz) Processors (4 cores @3.5–3.8GHz each)
    *   192 GB of RAM (12x 16 GB DIMMs)
    *   6x Seagate [Constellation 7200RPM](http://www.amazon.com/SEAGATE-ST91000640NS-Constellation-6-0Gb-internal/dp/B004HZEF2I) 1TB SATA HDDs (RAID 10) (Logs)
    *   Dual 10 Gbps network (Intel X540/I350 NDC) - Internal (DMZ) Traffic
    *   Dual 10 Gbps network (Intel X540) - External Traffic

*   2 Dell [R620](http://www.dell.com/us/business/p/poweredge-r620/pd) Servers (Direct Traffic), each with:

    *   Dual [E5-2650](http://ark.intel.com/products/64590/Intel-Xeon-Processor-E5-2650-20M-Cache-2_00-GHz-8_00-GTs-Intel-QPI) Processors (8 cores @2.0–2.8GHz each)
    *   64 GB of RAM (4x 16 GB DIMMs)
    *   2x Seagate [Constellation 7200RPM](http://www.amazon.com/SEAGATE-ST91000640NS-Constellation-6-0Gb-internal/dp/B004HZEF2I) 1TB SATA HDDs (RAID 10) (Logs)
    *   Dual 10 Gbps network (Intel X540/I350 NDC) - Internal (DMZ) Traffic
    *   Dual 10 Gbps network (Intel X540) - External Traffic

_Note:  이 서버는 따로 주문되었고, 그 결과 다른 스펙을 가지고 있습니다. 또한 두개의 CloudFlare로드 밸런서는 CloudFlare의 [Railgyn](https://www.cloudflare.com/railgun/)의 메모리에 캐쉬된 설치(현재 더이상 운영되지 않는)를 위한 더많은 메모리를 가지고 있습니다. _

<div class="pics-stack-overflow-the-hardware-2016-edition">

서비스, 레디스, 검색 글고 로드밸런서는 모두 1U서버입니다. 아래는 뉴욕지부 데이터 센터의의 서버 모습입니다.

![](http://i.imgur.com/jwiUUGW.jpg)

</div>

#### Servers for Other Bits

직간접적으로 트래픽을 제공하는데 참여하지 않는 서버들이 있습니다. 이 서버들은 다른쪽으로 연관이 있거나 모니터링, 로그 저장, 백업 등 본질적이지 않은 목적을 위한 서버들입니다.

이 포스트는 후에 있을 [다른 시리즈](http://nickcraver.com/blog/2016/02/03/stack-overflow-a-technical-deconstruction/)들의 위한 많은 부가 정보들을 제공합니다. 그래서 저는 흥미로운 백그라운드 서버 또한 이 포스트 내용에 포함시켰습니다.

###### VM Servers (VMWare, Currently)

*   2 Dell [FX2s](http://www.dell.com/us/business/p/poweredge-fx/pd) Blade Chassis, each with 2 of 4 blades populated

    *   4 Dell [FC630](http://www.dell.com/us/business/p/poweredge-fx/pd#Misc) Blade Servers (2 per chassis), each with:

            *   Dual [E5-2698 v3](http://ark.intel.com/products/81900/Intel-Xeon-Processor-E5-2643-v3-20M-Cache-3_40-GHz) Processors (16 cores @2.3–3.6GHz each)
        *   768 GB of RAM (24x 32 GB DIMMs)
        *   2x 16GB SD Cards (Hypervisor - no local storage)

        *   Dual 4x 10 Gbps network (FX IOAs - BASET)

*   1 EqualLogic [PS6210X](http://www.dell.com/us/business/p/equallogic-ps6210-series/pd) iSCSI SAN

    *   24x Dell 10K RPM 1.2TB SAS HDDs (RAID10)
    *   Dual 10Gb network (10-BASET)

*   1 EqualLogic [PS6110X](http://www.dell.com/us/business/p/equallogic-ps6110x/pd) iSCSI SAN

    *   24x Dell 10K RPM 900GB SAS HDDs (RAID10)
    *   Dual 10Gb network (SFP+)

![](http://i.imgur.com/Zladxtn.jpg)

더 주목할만한 서버들이 있습니다. 백그라운드 작업을 처리하고 로깅, 대량의 데이터 저장 등에 관한일을 돕습니다.

###### Machine Learning Servers (Providence)

이 서버는 99% 유휴상태입니다. 하지만 야간에 처리하는 무거운 작업을 수행합니다. 또한 이 서버는 큰규모의 데이트셋을 필요로하는 새로운 알고리즘을 테스트하기 위한 내부적으로 데이터센터 장소 역할을 합니다.

*   2 Dell [R620](http://www.dell.com/us/business/p/poweredge-r620/pd) Servers, each with:
*   Dual [E5-2697 v2](http://ark.intel.com/products/75283/Intel-Xeon-Processor-E5-2697-v2-30M-Cache-2_70-GHz) Processors (12 cores @2.7–3.5GHz each)
*   384 GB of RAM (24x 16 GB DIMMs)
*   4x Intel [530](http://ark.intel.com/products/75336/Intel-SSD-530-Series-480GB-2_5in-SATA-6Gbs-20nm-MLC) 480GB SATA SSDs (RAID 10)
*   Dual 10 Gbps network (Intel X540/I350 NDC)

###### Maching Learning Redis Servers (Still Providence)

이건 Providence의 Redis 데이터 저장소입니다. 일반적인 설정은 하나의 마스터, 하나의 슬레이브 그리고 우리의 ML알고리즘의 최신버전을 테스팅 용도로 사용되는 하나의 인스턴스 입니다. Q&amp;A사이트를 제공하기위해 사용되지 않는 동안 이 데이터는 직업매칭과 사이드바 직업 목록을 만들 때 사용되었습니다.

*   3 Dell [R720xd](http://www.dell.com/us/business/p/poweredge-r720xd/pd) Servers, each with:
*   Dual [E5-2650 v2](http://ark.intel.com/products/75269/Intel-Xeon-Processor-E5-2650-v2-20M-Cache-2_60-GHz) Processors (8 cores @2.6–3.4GHz each)
*   384 GB of RAM (24x 16 GB DIMMs)
*   4x Samsung [840 Pro](http://www.samsung.com/semiconductor/products/flash-storage/client-ssd) 480 GB SATA SSDs (RAID 10)
*   Dual 10 Gbps network (Intel X540/I350 NDC)

###### Logstash Servers (For ya know…logs)

Logstash 클러스터는(스토리지로 엘라스틱 서치를 사용함) 모든 로그를 저장합니다. 우리는 여기에  HTTP로그를 복제할 계획이지만 성능 이슈가 있습니다. 그러나 우리는 모든 네트워크 디바이스 로그와 시스템로그 그리고 윈도우와 리눅스 시스템의 로그들을 여기에 집계할 수 있어 빠르게 네트워크 오버뷰를 볼 수 있고 이슈를 검색할 수 있습니다. 또한 Bosun(Stack Exchange의 모니터링, 알람 시스템입니다.)의 화제 경고에 대한 추가적 정보를 얻기 위한 데이터소스로 사용됩니다. 전체 클러스터의 저장소는 6x12x4 = 288 TB입니다.

*   6 Dell [R720xd](http://www.dell.com/us/business/p/poweredge-r720xd/pd) Servers, each with:
*   Dual [E5-2660 v2](http://ark.intel.com/products/75272/Intel-Xeon-Processor-E5-2660-v2-25M-Cache-2_20-GHz) Processors (10 cores @2.2–3.0GHz each)
*   192 GB of RAM (12x 16 GB DIMMs)
*   12x 7200 RPM Spinny 4 TB SATA HDDs (RAID 0 x3 - 4 drives per)
*   Dual 10 Gbps network (Intel X540/I350 NDC)

###### HTTP Logging SQL Server

HTTP로깅 SQL 서버는 SQL데이터베이스에 우리 로드밸런서의 모든 단일 HTTP 히트수를 기록하는 서버입니다.(시스템로그를 통해 HAProxy로부터 보냅니다.) 우리는 URL, 쿼리, 사용자 Agent, SQL/Redis의 시간 등과 같은 최상위 비트만을 여기에 기록합니다.- 그래서 이것은 모두 클러스터된 컬럼스토어의 인덱스가 됩니다. 우리는 이 기록을 사용자 이슈나 검출 봇 등을 위해 사용합니다.

*   1 Dell [R730xd](http://www.dell.com/us/business/p/poweredge-r730xd/pd) Server with:
*   Dual [E5-2660 v3](http://ark.intel.com/products/81706/Intel-Xeon-Processor-E5-2660-v3-25M-Cache-2_60-GHz) Processors (10 cores @2.6–3.3GHz each)
*   256 GB of RAM (16x 16 GB DIMMs)
*   2x Intel [P3600](http://ark.intel.com/products/80995/Intel-SSD-DC-P3600-Series-2_0TB-2_5in-PCIe-3_0-20nm-MLC) 2 TB NVMe PCIe SSD (RAID 0)
*   16x Seagate [ST6000NM0024](http://www.seagate.com/internal-hard-drives/enterprise-hard-drives/hdd/enterprise-capacity-3-5-hdd/?sku=ST6000NM0024) 7200RPM Spinny 6 TB SATA HDDs (RAID 10)
*   Dual 10 Gbps network (Intel X540/I350 NDC)

###### Development SQL Server

우리는 개발자가 제품을 가능한 많이 시뮬레이션하는 것을 좋아합니다. SQL서버를 구입한 이후로 생산 프로세스를 업그레이드 했습니다. 올해 이후 스택오버플로의 클러스터를 업그레이드 하면서 동시에 이박스를 2U솔루션으로 바꿀 것입니다.

*   1 Dell [R620](http://www.dell.com/us/business/p/poweredge-r620/pd) Server with:
*   Dual [E5-2620](http://ark.intel.com/products/64594/Intel-Xeon-Processor-E5-2620-15M-Cache-2_00-GHz-7_20-GTs-Intel-QPI) Processors (6 cores @2.0–2.5GHz each)
*   384 GB of RAM (24x 16 GB DIMMs)
*   8x Intel [S3700](http://ark.intel.com/products/71916/Intel-SSD-DC-S3700-Series-800GB-2_5in-SATA-6Gbs-25nm-MLC) 800 GB SATA SSDs (RAID 10)
*   Dual 10 Gbps network (Intel X540/I350 NDC)

<div class="pics-stack-overflow-the-hardware-2016-edition">

이 것이 이 사이트를 제공하기 위한 하드웨어 입니다. 우리는 물론 로깅, 모니터링, 백업 등과 같은 백그라운드 작업을 위해 다른 서버를 가지고 있습니다. 만약에 여러분이 어떤 다른 시스템의 스펙에 대해 궁금하다면, 댓글로 질문한다면 저는 기쁘게 자세히 대답해 드릴 수 있습니다.([링크: 여기에 댓글을!!!](http://nickcraver.com/blog/2016/03/29/stack-overflow-the-hardware-2016-edition/)) 여기에 몇주전에 뉴욕에 설치한 전체 하드웨어의 모습입니다.

![](http://i.imgur.com/OmjzY1C.jpg)

</div>

[다음 글](http://nickcraver.com/blog/2016/02/03/stack-overflow-a-technical-deconstruction/)은 뭐냐구요? 제가 지금 포스팅하는  이 시리즈가 담는 내용은 많은 사람들이 알고 싶어하는 것입니다. [트렐로](https://trello.com/b/0zgQjktX/blog-post-queue-for-stack-overflow-topics)에가서 보면, [배포](https://trello.com/c/bh4GZ30c/25-deployment)가 가장 흥미로운 주제처럼 보입니다. 그래서 다음은 어떻게 코드가 개발자의 컴퓨터로부터 제품이되는지의 과정을 쓸 것 같습니다. 데이터베이스 마이그레이션, 빌드, CI인프라 그리고 어떻게 개발환경이 셋업되는지를 이야기 할 것입니다.