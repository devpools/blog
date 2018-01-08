---
title: 딥러닝 무식하게 정리해 보기 01
id: 4478
comment: false
categories:
  - deep learning
date: 2017-09-08 11:44:06
tags:
---

딥러닝, 인공지능 이야기를 하면 CNN, RNN 같은 알고리즘에 관한 글만 굉장히 많아서 조금 탑뷰에서 바라볼 수 있는 관점에서 전체 기술과 활용처를 분류하고 딥러닝 프레임워크에 대해서 알아 보았다. 딥러닝 프레임워크는 같이 프로젝트를 하고 있는 팀 로자미아 가 같이 정리를 해 주었다. Mabel, Jin, Alex 에게 감사를 드린다.

## [](http://keen.devpools.kr/2017/09/08/deeplearning-basic01/#AI-%EA%B8%B0%EC%88%A0%EC%9D%98-%EC%9D%91%EC%9A%A9-%EB%B6%84%EC%95%BC "AI 기술의 응용 분야")AI 기술의 응용 분야

딥러닝은 여러가지 알고리즘을 가지고 있지만 결국 하고자 하는 대부분의 일들이 군집(clustering)과 분류(classification) 라는 관점으로 귀결된다.
그에 따라 AI 기술 스택을 분류하는 방법은 여러가지가 있고 활용되는 분야도 다양하지만 아래와 같이 분류를 해보았다.
먼저 챗봇과 번역등에 사용되는 NLU등을 사용하는 Conversational AI.
두번째는 개와 고양이 구분하기 등에 많이 사용되는 Visual AI 분야.
마지막으로는 전통적으로 진행하던 데이타 분석을 하는 Analytic AI.

### [](http://keen.devpools.kr/2017/09/08/deeplearning-basic01/#Conversational-AI "Conversational AI")Conversational AI

![conversation goes on](http://keen.devpools.kr/images/playmobil-451203_640.jpg)

Conversational AI 에는 기본적으로 NLU(Naturla Language Understanding)을 그 근간으로 한다. 오픈 소스중에선 Rasa.ai 같은 프레임워크가 존재하고 오픈 서비스로는 wit.ai 와 api.ai 가 있고 제품으로 유명한 것은 IBM의 conversational 엔진이 유명하다. 일반적인 개발자들이 가장 많이 만나는 것은 페이스북을 가지고 챗봇을 한번 만들어보다가 만나게 되는 것이 NLU 와의 처음 만남이 된다고 볼 수 있다. 이 NLU 엔진은 여러가지 기능을 가지는데 기본적으로 전체의 맥락을 판단하고 키워드를 뽑아내는 게 으뜸된 기능인데 이것을 인텐트와 엔터티라는 업계 용어로 지칭한다.
맥락 혹은 화행에는 기본적으로 의도가 들어가 있고 감정이 들어가 있어서 이런 감성을 파악해 내는 데 word2vec doc2vec 같은 툴들이 사용된다. 한글의 경우는 영어와 다른 구조를 가지고 있기 때문에 형태소를 분석해야 하고 mecab-ko 나 은전한잎 같은 툴들이 이용된다.

여기까지는 일반적인 NLU에 대한 이야기만 한 것이고, 챗봇을 위해서는 다이알로그의 룰이 존재하기 마련이라 룰 매니저 혹은 다이알로그 매니저들이 존재한다. 얘룰 들면 페이스북 챗봇을 만들 때 이런 말이 들어오면 이렇게 대답해야지.. 라고 만드는 대화 구성이 그런 역할을 하는 것이다. 이런 다이알로그 개념이 들어가면 한 문장의 문맥이 아닌 전체 문맥을 파악해야하는 기능과 랭킹 시스템을 구축해야 한다.

조금 더 나아가면 보통 STT(Speech To Text), TTS(Text To Speech) 로 알고 있는 음성자동 시스템이랑 연결해서 사용자의 음성을 듣고 텍스트로 변환해 준 뒤에 그것을 구축해 놓은 챗봇 시스템과 연결해 구축하는 작업들이 생겨나고 있다.

### [](http://keen.devpools.kr/2017/09/08/deeplearning-basic01/#AI-for-media "AI for media")AI for media

![AI for media](http://keen.devpools.kr/images/abstract-1233873_640.jpg)

얼굴 인식, 피플 카운팅 등에 사용되는 Face Recognition 과 딥러닝 기반의 Object Recognition 은 미디어들을 통해 사람을 인식하거나 장면등을 파악해서 시스템과 연결하는 일들을 할 때 많이 사용된다. 딥러닝 기반 객체 인식은 보통 CNN 알고리즘으로 유명한데 최근에는 여러가지 알고리즘이 더 들어간 경우가 많고 YOLO Darknet 같은 경우는 인식률과 속도에서 다른 범용 툴을 압도한다.
피플 카운팅을 응용하면 히트맵 히트존 등의 기술등으로 확장이 가능하다.
OCR 프로그램은 기존에도 존재했던 것들인 많은데, 딥러닝 기반으로 프로젝트들이 변경되고 있다. 구글 tesseract 도 LSTM 알고리즘의 4.0 버전을 테스트 중이다.

### [](http://keen.devpools.kr/2017/09/08/deeplearning-basic01/#Analytics "Analytics")Analytics

기존의 데이터들을 분석하는 툴 기반이라 딥러닝 보다는 기존 스파크와 하둡 기반의 빅데이터 툴들이 더 많은 일들을 해 주는 부분들이다.

## [](http://keen.devpools.kr/2017/09/08/deeplearning-basic01/#AI-%EA%B8%B0%EB%B0%98-%EA%B8%B0%EC%88%A0-%EC%8A%A4%ED%83%9D-%EB%B3%84%EB%A1%9C-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90-1 "AI 기반 기술 스택 별로 알아보자 (1)")AI 기반 기술 스택 별로 알아보자 (1)

### [](http://keen.devpools.kr/2017/09/08/deeplearning-basic01/#%EC%9D%B8%EA%B3%B5%EC%A7%80%EB%8A%A5%EC%9D%84-%EC%9C%84%ED%95%9C-%ED%94%84%EB%A1%9C%EC%84%B8%EC%84%9C%EC%9D%98-%EB%B0%9C%EC%A0%84 "인공지능을 위한 프로세서의 발전")인공지능을 위한 프로세서의 발전

![템빨은 언제나 진리](http://keen.devpools.kr/images/nvidia-1201077_640.jpg)

딥러닝은 기본적으로 다층 레이어의 많은 학습을 요구하기 때문에 단순한 계산이 순식간에 많이 일어나는 것을 특징으로 한다. 그렇기에 병렬처리 컴퓨팅이 가장 중요한 핵심기능이 되어서 기존 CPU이외의 다른 대안들이 필요하다.
그런 의미에서 프로세서 유니트(코어) 각각의 성능은 떨어지지만 프로세서 유니트 수가 압도적으로 많은 GPU 가 각광을 받았다. 특히 엔비디아 CUDA (병렬컴퓨팅 플랫폼의 API모델) 의 등장은 프로세서 경쟁에 불을 붙였다.
참조 : [http://www.epnc.co.kr/news/articleView.html?idxno=75603](http://www.epnc.co.kr/news/articleView.html?idxno=75603)

#### [](http://keen.devpools.kr/2017/09/08/deeplearning-basic01/#GPGPU-General-Purpose-GPU "GPGPU - General Purpose GPU")GPGPU - General Purpose GPU

범용 GPU를 이야기 하는 것으로 AI 용으로만 사용될 때는 TPU라는 이름을 쓰기도 한다.(Tensor Processing Unit) NVIDIA의 GPGPU를 극대화 하기 위한 CUDA 기술을 공개했고 동시에 범용 GPU 기능이 주목 받기 시작했다

#### [](http://keen.devpools.kr/2017/09/08/deeplearning-basic01/#GPGPU%EB%A5%BC-%EB%84%98%EC%96%B4%EC%84%9C "GPGPU를 넘어서")GPGPU를 넘어서

구글의 딥러닝의 학습속도를 향상시키기 위해 자체 디자인한 반도체 칩셋(ASIC)를 적용되면서 더 각광 받기 시작함. 구글의 ASIC를 TPU(Tensor Processing Unit) 이라 부르고 GPGPU의 범주에 놓기도 한다.
MS는 같은 목적으로 FPGA 칩을 클라우드 데이타 센터에 탑재해 Azure 서비스에도 이용하고 있는데, FPGA는 저전력의 강점도 가지고 있다.

#### [](http://keen.devpools.kr/2017/09/08/deeplearning-basic01/#Neuromorphic-Processor "Neuromorphic Processor")Neuromorphic Processor

신경구조와 유사한 프로세서가 차후 프로세서로 각광을 받고 있고 IBM 같은 회사들이 준비하고 있다.
링크 : [http://www.research.ibm.com/cognitive-computing/neurosynaptic-chips.shtml#fbid=rXQq5aX-WkP](http://www.research.ibm.com/cognitive-computing/neurosynaptic-chips.shtml#fbid=rXQq5aX-WkP)

### [](http://keen.devpools.kr/2017/09/08/deeplearning-basic01/#%EB%94%A5%EB%9F%AC%EB%8B%9D-%EC%84%9C%EB%B9%84%EC%8A%A4%EB%A5%BC-%EC%9C%84%ED%95%9C-%EC%9D%B8%ED%94%84%EB%9D%BC "딥러닝 서비스를 위한 인프라")딥러닝 서비스를 위한 인프라

![요즘엔 서버실에 갈 일이 있어야지](http://keen.devpools.kr/images/server-2160321_640.jpg)

크게 딥러닝 전용 인프라는 클라우드와 판매형 인프라로 나뉘어서 볼 수 있을 것 같은데 기존의 아마존 AWS, 마이크로소프트의 Azure, 구글 클라우드 같은 빅3는 이미 클라우드 인프라를 가지고 있다.
AWS의 경우는 클라우드 AMI(Amazon Machin Image)도 제공하고 있어서 굉장히 편리하게 쓸 수 있다. AMI라는 것은 미리 만들어진 이미지 같은 개념으로 볼 수 있다. NVidia 의 경우는 자사의 GPU를 이용한 클라우드 서비스를 하고 있는 것이 흥미롭다.
판매형 인프라는 기존의 서버 벤더들과 비슷한 형태를 취하고 있는데 웨이브 컴퓨팅은 하나의 모델을 제안하는 데 텐서플로 같은 소프트웨어에 특화되어 만들어져 있다.이에 비해 Penguin computing 은 몇가지 옵션들을 더 제공하고 있다.

## [](http://keen.devpools.kr/2017/09/08/deeplearning-basic01/#%EB%A7%BA%EC%9C%BC%EB%A9%B0 "맺으며")맺으며

딥러닝, 인공지능 등을 바라볼때 탑뷰로 어떤 기술이 있는지를 알아보는 과정을 거치고 있다. 다음 번엔 개발자에게는 가장 중요한 딥러닝 프레임워크들을 다뤄보도록 하겠다.