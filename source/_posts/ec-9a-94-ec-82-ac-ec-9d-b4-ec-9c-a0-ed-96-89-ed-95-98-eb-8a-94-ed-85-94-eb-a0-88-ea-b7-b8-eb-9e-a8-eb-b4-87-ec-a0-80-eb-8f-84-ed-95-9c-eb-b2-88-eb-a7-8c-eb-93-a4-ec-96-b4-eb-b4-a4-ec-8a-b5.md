---
title: 요사이 유행하는 텔레그램봇 저도 한번 만들어봤습니다.
tags:
  - telegram
  - 텔레그램
id: 143
categories:
  - Small talk
  - 미분류
date: 2016-03-21 09:31:24
---

최근들어 다시금 인기를 얻고 있는 텔레그램의 Bot을 한번 만들어보고자 합니다.

**Bot?**
텔레그램에서 제공하는 Bot은 텔레그램 계정 중 사람의 계정이 아닌 것을 말합니다. 쉽게 robot의 bot이라고 이해하셔도 좋을 것같습니다. Bot은 깃헙 같은 다른 서비스와 연결하여 푸시 알림이라던지, 토렌트의 다운로드가 완료되었다는 알림, 또는 가상의 게임, 소셜서비스 등, 사용자와 자동으로 상호작용할 수 있는 그런 서비스를 만들 수 있습니다. Bot 계정은 보통의 텔레그램 계정과는 다른 점이 있습니다. 예를들어 다른 사용자나 그룹에 대화를 먼저 시작할 수 없고, 계정은 반드시 Bot으로 끝나야 합니다.

Bot을 만들려면 어딘가에서 사용자의 메시지를 처리할 수 있는 서버가 필요하고, BotFather라는 텔레그램의 Bot을 통해서 나의 Bot계정을 만들 수 있습니다.

더 자세한 내용은 아래 링크에서 확인하세요.
[텔레그램 Bot 소개 (Bots: An introduction for developers)](https://core.telegram.org/bots)

**Bot 만들기.**
이제 Bot을 만들어볼건데, 텔레그램 bot샘플코드를 이용해서 프로그래밍의 시작인 Hello world를 만들겠습니다.
[텔레그램 bot 샘플코드](https://core.telegram.org/bots/samples)

먼저 준비물은
* 서버
* Bot 이름.

BotFather에게 말을 걸어야 합니다. [BotFather에게 연결하기](https://telegram.me/botfather)
(모바일로 말을 걸 수도 있지만, 데스크탑 앱을 이용하세요. 키보드가 있으니 더 편합니다.)

![Telegram 2016-03-21 08-15-27](http://nonamedeveloper.com/wp-content/uploads/2016/03/Telegram-2016-03-21-08-15-27-225x300.jpg)
말을 걸면 위와 같은 메시지를 보실 수 있습니다. BotFather가 무얼 할 수 있는지 간단한 설명을 보여주고 다음 메시지를 기다리게 됩니다. 이제 Bot계정을 만들어 보겠습니다. 친절하게도 "/"를 입력하면 사용할 수 있는 명령어 들을 보여줍니다. 지금은 새로 만들어야 하니까 "/newBot" 명령을 보내겠습니다.

name과 username을 지정하고 몇가지 대화를 거쳐 쉽게 Bot계정을 생성하실 수 있습니다. name은 bot이 표시될 이름이고, username은 ID라고 생각하시면 됩니다. 즉, name의 다른 bot과 중복될 수 있지만, username은 중복될 수 없습니다.
![Telegram 2016-03-21 08-26-22](http://nonamedeveloper.com/wp-content/uploads/2016/03/Telegram-2016-03-21-08-26-22-225x300.jpg)

![Telegram 2016-03-21 08-30-06](http://nonamedeveloper.com/wp-content/uploads/2016/03/Telegram-2016-03-21-08-30-06-274x300.jpg)

![Telegram 2016-03-21 08-30-37](http://nonamedeveloper.com/wp-content/uploads/2016/03/Telegram-2016-03-21-08-30-37-300x277.jpg)

Bot계정이 정상적으로 생성되면 API키를 전달받게됩니다. 이 키는 유출되거나, 잃어버리지 않도록 잘 보관하시기 바랍니다.

저는 ruby를 이용해서 만들건데, 다른 언어도 크게 다르지는 않을겁니다. 샘플코드에서 API키를 위에서 발급받은 키를 입력하고,
실행시키면 일단은 끝이 납니다. 간단하죠?
https://gist.github.com/ikaruce/2ee4088438bb45d25fbd

코드를 간단히 살펴보면 텔레그램 봇 데몬을 생성하고, 메시지 수신을 대기합니다. 그리고 들어오는 메시지에서 /greet라는 명령이 전달되면 Hello ~ 라는 메시지를 보내게 되는거죠. 만약 모르는 명령이 들어오면 나는 메시지를 모른다는 메시지를 보내는 간단한 코드입니다.

실행해서 메시지를 보내보면 아래와 같이 동작을 확인하실 수 있습니다.
![1\. ruby 2016-03-21 09-11-21](http://nonamedeveloper.com/wp-content/uploads/2016/03/1.-ruby-2016-03-21-09-11-21-300x190.jpg)

그럼 이제 연습삼아 생성했던 Bot은 삭제를 해야죠.
![Telegram 2016-03-21 09-18-10](http://nonamedeveloper.com/wp-content/uploads/2016/03/Telegram-2016-03-21-09-18-10-243x300.jpg)
'/deletebot'이라는 명령을 전송하고 나는 이 봇을 삭제합니다. 라고 확인 메시지를 보내면 만들었던 bot 계정은 사라지게 됩니다.