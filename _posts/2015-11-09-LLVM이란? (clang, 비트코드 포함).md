---
layout: post
title: "LLVM이란? (clang, 비트코드 포함)"
tags: 
  - Xcode
comments: true
published: true
---

[if kakao 2018 개발자 컨퍼런스](https://if.kakao.com)에 다녀왔습니다.
행사는 삼성동 COEX 그랜드볼룸에서 진행했습니다.

![](https://farm2.staticflickr.com/1900/44483742701_9980ca3ab0_k.jpg){: .center-image}
*COEX 그랜드볼룸*
{: style="text-align: center;"}


iOS나 OSX용 앱을 개발하시는 분은 한번 쯤 LLVM이라는 단어를 들어보셨을 겁니다. Xcode 프로젝트 설정 화면에서도 LLVM 관련 여러 옵션을 설정하는 것을 본적이 있으실 겁니다. 이번 포스트에서는 LLVM이 무엇인지 알아보도록 하겠습니다.

llvm option in xcode
Xcode의 LLVM 설정
컴파일러

LLVM을 이야기 하기에 앞서 컴파일러에 대해서 잠시 이야기 하고자 합니다. 우리는 앱을 개발하기 위해 소스코드를 작성합니다. 그리고 나서 작성한 소스코드를 빌드한 후 실행하게 되죠. 왜 직접 실행하지 않고 이런 과정을 거쳐야 하는 것일까요?

소스코드는 사람이 이해할 수 있는 형태의 언어로 작성된 코드입니다. 기계(iPhone, iPad, Macbook 등)가 이 소스코드를 실행하려면 소스코드를 기계가 이해할 수 있는 형태 즉, 바이너리나 기계어로 변환해야 합니다. 이 변환을 하는 것이 컴파일러의 역할입니다.

section .text
global _start ;must be declared for linker (ld)

_start: ;tell linker entry point

mov edx,len ;message length
mov ecx,msg ;message to write
mov ebx,1 ;file descriptor (stdout)
mov eax,4 ;system call number (sys_write)
int 0x80 ;call kernel

mov eax,1 ;system call number (sys_exit)
int 0x80 ;call kernel

section .data

msg db ‘Hello, world!’,0xa ;our dear string
len equ $ – msg ;length of our dear string

(위) 리눅스에서 “Hello world”를 출력하는 어셈블리어 코드

컴파일러는 보통 Frontend, Optimzer, Backend의 세 가지 구성요소를 가지고 있습니다.

classical compiler
일반적 컴파일러의 구조
Frontend : 어휘분석(Lexical analysis), 구문분석(Syntax analysis), 의미분석(Semantic analysis), 중간코드 생성(Immediate code generation)
*소스코드 파싱, 에러체크, 언어에 맞는 Abstract Syntax Tree(AST)작성
Optimizer : 런타임시 성능향상을 위해 중복계산 제거 및 기타 여러 변환 실행
Backend : 각 코드를 타켓 아키텍처에 맞는 인스트럭션 셋(Instruction Set)으로 매핑해 실행코드 생성
하지만 일반적인 컴파일러에서 이 3가지 구성요소는 나눠서 실행되지 않고 단일 바이너리에 의해 한번에 실행됩니다.

그렇다면 만약 컴파일러가 다양한 언어로 작성된 소스코드를 다양한 아키텍처를 타겟으로 컴파일하고자 한다면 몇개의 컴파일러가 필요할 까요?

타겟 아치텍처 개수(N) X 소스 언어 개수(M), 즉 N x M개의 컴파일러가 필요하게 됩니다.

이런 비효율성을 개선하여 인터페이스와 라이브러리의 재사용성을 위해 나온 것이 LLVM입니다.

LLVM이란?

LLVM은 최초 일리노이 대학의 Chris Lattner의 “an innovative infrastructure for optimising compilers”를 주제로한 석사논문 프로젝트에서 출발했습니다. Chris Lattner는 2005년 애플에 새로 신설된 컴파일러 팀에서 일하게 됩니다. Chris Lattner는 2010년 Swift 언어 개발을 시작한 사람이기도 합니다.
(*LLVM은 Low-Level Virtual Machine의 약자로, 가상기계용의 bitcode를 만들어내기 때문에 붙여진 이름이었지만 현재는 이 약어를 사용하지 않고 그냥 LLVM이라고 부릅니다.)

LLVM은 기존의 기존의 컴파일러의 3가지 주요 구성요소를 분리해서 각각의 부분들을 조합할 수 있게 디자인 했습니다.

llvm
Frontend, Optimizer, Backend를 분리한 LLVM의 디자인
LLVM이 기존의 컴파일러의 3가지 구성요소를 분리했다는 것 외에 가장 다른 점은 바로 비트코드라 불리는 중간코드(IR : Intermediate Representation)를 생성한다는 것입니다. 그래서 Frontend에서는 비트코드를 생성하고 Backend에서는 이 코드를 받아서 타겟에 맞는 결과물을 생성합니다. 이 비트코드를 사용함으로 얻는 장점은 효율성/확장성입니다.
만약 일반적인 컴파일러가 새로운 언어를 지원한다고 가정해 보고 그 언어를 K라고 해보겠습니다. K언어를 x86아키텍처, PowerPC, ARM등 각기 다른 아키텍처의 기계어를 생성하려면 3개의 컴파일러를 작성해야 하고 컴파일러 각각의 Frontend, Optimiser, Backend를 구현해 주어야 합니다. 하지만 LLVM을 이용하면 K언어의 Frontend 부분만 개발하면 여러 아키텍처를 타겟으로 빌드할 수 있습니다.
(*clang이란 말을 들어보셨을 겁니다. clang은 LLVM에서 C, C++, Objective-C같은 C언어 계열의 Frontend입니다.)

또 LLVM 덕분에 GCC(GNU Compiler Collection)와 비교했을때, 컴파일 속도는 2배 빨라지고, 빌드된 실행파일의 크기도 2~30% 줄일 수 있었습니다. 실행 속도 또한 파일의 크기가 줄어든 것과 비례에 빨라졌습니다. 뿐만아니라 LLVM가 있었기때문에 Block을 지원하고 GCD(Grand Central Dispatch)가 가능해졌습니다. Xcode의 다양한 Instruments 기능과 Xcode IDE와의 연동도 역시 LLVM 덕분입니다.

realtime check
IDE에서의 실시간 문법체크도 LLVM을 사용하기에 가능합니다.
앞으로의 변화

비트코드로 앱스토어에 전송된 앱은 사용자가 앱스토어에서 해당 앱을 다운로드 받을 때 사용자가 사용하고 있는 플랫폼에 맞게 빌드되어서 전송될 수 있습니다.

bit code setting
Bitcode Setting
현재 iOS9부터 비트코드(LLVM IR) 제출이 기본 설정이고, Watch앱은 의무입니다. 지금 시점에서 애플이 무엇을 계획하고 있는지는 모르지만 새로운 CPU 아키텍처를 준비하고 있거나, Watch앱의 CPU가 완전히 바뀔 수도 있을 것 같습니다. 아니면 ARM용 맥이 나온다거나… 혹은 아이패드 프로에 OS X이 올라갈지도 모르겠습니다.

아무튼 애플은 다음에 어떤 사업을 하던 플랫폼의 통합과 확장의 유연성을 확보해서 다른 어떤 경쟁사보다도 빠르게 좋은 제품을 만들 수 있는 유리한 고지를 선점하고 있는 것으로 보입니다.

이상으로 LLVM에 대해서 알아보았습니다.

[참  고]
1. http://www.aosabook.org/en/llvm.html#fig.llvm.rtc
2. https://en.wikipedia.org/wiki/LLVM
3. http://social.gabia.com/index.php?document_srl=11970&mid=tech
4. https://ko.wikipedia.org/wiki/컴파일러
5. http://asm.sourceforge.net/intro/hello.html
6. http://jeonghodot.blogspot.kr/2013/03/llvm-3.html
7. http://www.albireo.net/threads/9970/