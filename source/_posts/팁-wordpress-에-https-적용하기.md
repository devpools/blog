---
title: '[팁] wordpress 에 https 적용하기'
tags:
  - https
  - wordpress
id: 610
categories:
  - Dev-Tips
date: 2017-02-08 17:57:12
---

크롬이 https 사이트가 아니면 warning을 띄우게 되었다는 사실이 알려지면서 많은 설치형 블로거들이 난감해 하시는 분들이 있어서 포스팅 합니다.

[caption id="attachment_612" align="aligncenter" width="648"]![](/images/2017/02/castle-265596_1920-1024x576.jpg) 크롬 도금의 자물쇠...[/caption]

세 단계 정도로 나뉘어서 해야할 일들이 있을 것입니다. (세 단계 모두 지정된 플랫폼들이 있어서 해당사항이 아니신 분들도 있을 거 같습니다. )

1.  호스팅 서비스 port 설정( Amazon 기준)
2.  웹서버 설정(  Ubuntu &amp; Apache2 설정 )
3.  인증서 설치( Let's Encrypt 설정)

&nbsp;

### 1\. 호스팅 서비스

아마존의 경우는 아마존 콘솔로 들어가셔서

Network &amp; Security -&gt; Security Groups 에 들어가셔서 서비스가 가지고 있는 정책에 맞는 그룹을 선택하신 후에 Inbound Edit을 선택하시고 https 포트로 들어올 수 있도록 작업을 해 주셔야 합니다.

[caption id="attachment_611" align="aligncenter" width="648"]![](/images/2017/02/https-devpools-00.jpg) 어디서든 들어 올 수 있게[/caption]

&nbsp;

∴이후 아래 설명될 3번 항목만 진행해도 적용이 될 거 같지만 혹시 다른 방법이나 self signed 된 인증서를 통한 설정을 알고 싶은 사람들을 위해서 2번을 설명합니다.

#### 2\. Let's Encrypt

Let's Encrypt는 무료라서 3개월에 한번씩 갱신해야 되지만 사용할 수 있는 인증서를 만들어 줍니다. 사이트에 들어가서 무언가를 다운로드 받아야 되나 살펴보면 Let's Encrypt 에서 바로 제공하는 것은 없고 자동화해 주는 툴이 있는 링크를 제공해 줍니다.

##### 2.1\. certbot

제가 찾아간 사이트는 바로 certbot이였습니다.

https://certbot.eff.org/

&nbsp;

여기서 작업을 하려면  ubuntu 버전 명령어를 통해 버전을 알아야 합니다.

<pre>$cat /etc/*releaseDISTRIB_ID=Ubuntu
DISTRIB_RELEASE=14.04
DISTRIB_CODENAME=trusty
DISTRIB_DESCRIPTION="Ubuntu 14.04.3 LTS"
NAME="Ubuntu"
VERSION="14.04.3 LTS, Trusty Tahr"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 14.04.3 LTS"
VERSION_ID="14.04"
HOME_URL="http://www.ubuntu.com/"
SUPPORT_URL="http://help.ubuntu.com/"
BUG_REPORT_URL="http://bugs.launchpad.net/ubuntu/"</pre>

&nbsp;

[caption id="attachment_616" align="alignnone" width="561"]![](/images/2017/02/https-devpools-01.jpg) certbot 좋아용[/caption]

&nbsp;

이렇게 선택하고 나면 해당 installation  페이지로 안내해 줍니다.

저의 경우는 다음 페이지가 되겠죠.

https://certbot.eff.org/#ubuntutrusty-apache

##### 2.2\. 인스톨

2.3\. 실행인스톨 하고 chmod로 실행할 수 있도록 만들어 줍니다.

<pre>$ ./certbot-auto</pre>

<div class="get-started"></div>

<div class="get-started">그리고 저 명령어를 한번 실행해 주면 자동적으로 아파치를 찾아서 바꿔줘야 할 옵션들을 자동으로 찾아서 바꿔줍니다.</div>

<div class="get-started">

[caption id="attachment_617" align="alignnone" width="648"]![](/images/2017/02/https-devpools-02.jpg) 이런 형태의 아웃풋들이 출력됩니다. 설정들을 지속적으로 물어봅니다.[/caption]

</div>

<div class="get-started"></div>

<div class="get-started">그러고 나면 warning 이 더 이상 뜨지 않습니다.</div>

<div class="get-started"></div>

<div class="get-started">![](/images/2017/02/https-devpools-03.jpg)</div>

<div class="get-started"></div>

##### 2.4\. 갱신

<div class="get-started">3개월에 한번씩 갱신하기 위해서는 아래의 명령어를 cron 잡에 등록해서 작업하시면 됩니다.</div>

<div class="get-started">
<pre>$./path/to/certbot-auto renew --quiet --no-self-upgrade</pre>
</div>

<div class="get-started"></div>

<div class="get-started">아래 내용은 2번까지 하신 분들은 크게 필요하지 않을지도 모르지만 혹시라도 https 만 적용하기 위해서 필요하신 분들을 위해서 남겨둡니다. Self-signed 인증서를 쓰는 경우인데 크롬 버전이 올라가면 이 경우는 에러는 여전히 뜰 것으로 예상됩니다.</div>

<div class="get-started"></div>

### 3\. 웹서버 설정(Manually)

우분투의 경우는 아파치, a2enmod등을 가지고 <del>생각보다 손쉽게</del> 작업을 진행할 수 있습니다.

아래의 명령어들은 혹시 권한이 필요한 경우가 생기면 sudo를 사용해서 진행하시면 됩니다.

##### 3.1\. SSL 모듈 설치

<pre>$sudo a2enmod ssl

$sudo service apache2 restart</pre>

##### 3.2\. Self-signed 인증서 만들기

a2enmod는 apache2 모듈설정을 자동화해주는 스크립트입니다. SSL을 설치하고 재시작해 줍니다.(http://man.he.net/man8/a2enmod)

    $sudo mkdir /etc/apache2/ssl`</pre>

    <pre class="code-pre ">`$sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout 
      /etc/apache2/ssl/self_signed.key 
      -out /etc/apache2/ssl/``self_signed``.crt`</pre>

    &nbsp;

    ##### 3.3\.  버추얼 호스트 설정파일 만들기

    <pre class="code-pre ">`$sudo vi /etc/apache2/sites-available/default-ssl.conf

로 파일을 열어서 아래와 같이 편집해 줍니다.

<pre><span style="font-size: 8pt;">&lt;IfModule mod_ssl.c&gt;
    &lt;VirtualHost _default_:443&gt;
        ServerAdmin <span class="highlight">ehrudxo@gmail.com</span>
        <span class="highlight">ServerName devpools.kr
</span>        <span class="highlight">ServerAlias www.devpools.kr
</span>        DocumentRoot <span class="highlight">/somewhere/</span>
        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
        SSLEngine on
        SSLCertificateFile <span class="highlight">/etc/apache2/ssl/self_signed.crt</span>
        SSLCertificateKeyFile <span class="highlight">/etc/apache2/ssl/self_signed.key</span>
        &lt;FilesMatch "\.(cgi|shtml|phtml|php)$"&gt;
                        SSLOptions +StdEnvVars
        &lt;/FilesMatch&gt;
        &lt;Directory /usr/lib/cgi-bin&gt;
                        SSLOptions +StdEnvVars
        &lt;/Directory&gt;
        BrowserMatch "MSIE [2-6]" \
                        nokeepalive ssl-unclean-shutdown \
                        downgrade-1.0 force-response-1.0
        BrowserMatch "MSIE [17-9]" ssl-unclean-shutdown
    &lt;/VirtualHost&gt;
&lt;/IfModule&gt;</span></pre>

##### 3.4\.  활성화 하기

이제 다음 명령어로 활성화 하고 재 시작해 줍니다.

<pre class="code-pre ">`$sudo a2ensite default-ssl.conf
$`sudo service apache2 restart</pre>

이제 접속해 보시면 잘 돌아가시는 걸 확인할 수 있습니다.

[caption id="attachment_619" align="alignnone" width="648"]![](/images/2017/02/https-devpools-04.jpg) 앗싸~[/caption]

&nbsp;