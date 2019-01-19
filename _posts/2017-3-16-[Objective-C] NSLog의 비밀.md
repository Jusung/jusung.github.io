---
layout: post
title: "[Objective-C] NSLog의 비밀"
tags: 
  - Objective-C
comments: true
published: true
---

NSLog로 긴 문자열을 출력하다가 문자열이 계속 잘려서 나오는 것을 보게 되었습니다.
아래는 제가 사용했던 문자열입니다.

> What is Lorem Ipsum?
 Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book. It has survived not only five centuries, but also the leap into electronic typesetting, remaining essentially unchanged. It was popularised in the 1960s with the release of Letraset sheets containing Lorem Ipsum passages, and more recently with desktop publishing software like Aldus PageMaker including versions of Lorem Ipsum.
>
> Why do we use it?
 It is a long established fact that a reader will be distracted by the readable content of a page when looking at its layout. The point of using Lorem Ipsum is that it has a more-or-less normal distribution of letters, as opposed to using 'Content here, content here', making it look like readable English. Many desktop publishing packages and web page editors now use Lorem Ipsum as their default model text, and a search for 'lorem ipsum' will uncover many web sites still in their infancy. Various versions have evolved over the years, sometimes by accident, sometimes on purpose (injected humour and the like).
>
> Where does it come from?
 Contrary to popular belief, Lorem Ipsum is not simply random text. It has roots in a piece of classical Latin literature from 45 BC, making it over 2000 years old. Richard McClintock, a Latin professor at Hampden-Sydney College in Virginia, looked up one of the more obscure Latin words, consectetur, from a Lorem Ipsum passage, and going through the cites of the word in classical literature, discovered the undoubtable source. Lorem Ipsum comes from sections 1.10.32 and 1.10.33 of "de Finibus Bonorum et Malorum" (The Extremes of Good and Evil) by Cicero, written in 45 BC. This book is a treatise on the theory of ethics, very popular during the Renaissance. The first line of Lorem Ipsum, "Lorem ipsum dolor sit amet..", comes from a line in section 1.10.32.
>
> The standard chunk of Lorem Ipsum used since the 1500s is reproduced below for those interested. Sections 1.10.32 and 1.10.33 from "de Finibus Bonorum et Malorum" by Cicero are also reproduced in their exact original form, accompanied by English versions from the 1914 translation by H. Rackham.
>
> Where can I get some?
 There are many variations of passages of Lorem Ipsum available, but the majority have suffered alteration in some form, by injected humour, or randomised words which don't look even slightly believable. If you are going to use a passage of Lorem Ipsum, you need to be sure there isn't anything embarrassing hidden in the middle of text. All the Lorem Ipsum generators on the Internet tend to repeat predefined chunks as necessary, making this the first true generator on the Internet. It uses a dictionary of over 200 Latin words, combined with a handful of model sentence structures, to generate Lorem Ipsum which looks reasonable. The generated Lorem Ipsum is therefore always free from repetition, injected humour, or non-characteristic words etc.

이 문자가 NSLog로 찍어 보면 이렇게 일부분만 출력 됐습니다.
![](https://farm2.staticflickr.com/1907/44134741875_2bfbfb5b07_o.png)
*NSLog 결과값* [(크게보기)](https://farm2.staticflickr.com/1907/44134741875_2bfbfb5b07_o.png)
{:style="text-align: center" target="_blank"}

왜 이럴까 왜 이럴까? 고민하며 다른 문자들을 소스로 `NSLog`를 찍어봤는데 모두 전체가 다 출력되지 않고 잘려서 출력되는 것을 확인했습니다. 그러다 문득 출력되는 문자의 개수를 확인해 봐야겠다는 생각이 들었습니다. 헐… 아니나 다를까! 테스트한 소스의 결과 모두 공통적으로 1023개의 문자만 출력되는 것이었습니다!!

![](https://farm2.staticflickr.com/1912/44998015272_e404d0683e_o.png)

프로그래밍을하는 사람이면 친숙한 숫자 2^10 = 1,024입니다. 그래서 감을 잡았죠. 이거 `NSLog`에 버퍼든 뭐든 뭔가 있구나! 그러다 최종적으로 iOS10과 Xcode 8에 들어오면서 ASL(Apple System Log)가 Unified Logging이라는 새 로깅 시스템으로 바뀌었다는 사실을 확인했습니다.

아래는 애플 문서에서 관련 내용이 명시돼 있는 부분입니다.[^1]

>  Important
> 
Unified logging is available in iOS 10.0 and later, macOS 10.12 and later, tvOS 10.0 and later, and watchOS 3.0 and later, and supersedes ASL (Apple System Logger) and the Syslog APIs. Historically, log messages were written to specific locations on disk, such as /etc/system.log. The unified logging system stores messages in memory and in a data store, rather than writing to text-based log files.

*번역 : iOS 10.0 macOS 10.12, watchOS 3.0 혹은 이후 버전부터 ASL과 Syslog API대신 Unified logging을 이용 가능합니다. 대대로 로그 메시지는 /etc/system.log와 같은 디스크의 특정위치에 쓰여 졌습니다. unified logging system은 메시지를 문자기반의 로그 파일에 쓰는것 대신 메모리와 데이터 소스에 저장합니다.*


> Important
>
Log message lines greater than the system’s maximum message length are truncated when stored by the logging system. Complete messages are visible when using the log command-line tool to view a live stream of activity. Bear in mind, however, that streaming log data is an expensive activity.

*번역 : 시스템의 최대 메시지 길이를 초과하는 로그 메시지는 로깅 시스템에 저장될 때 잘리게 됩니다. 전체 메시지는 로그 CLI를 이용하면 볼 수 있습니다. 인내를 부탁드립니다. 로그데이터*를 스트리밍하는 것은 비싼 작업입니다.

그리고 @Hot_Leaks라는 분의 도움으로 시스템의 최대 메시지 길이의 제한이 SDK의 헤더(source:<os/log.h>)에 1024로 명시돼 있다는 것이 확인되었습니다.[^2]

```
/*!  
 * @function os_log  
 *   
 * ...  
 *  
 * There is a physical cap of 1024 bytes per log line for dynamic content,  
 * such as %s and %@, that can be written to the persistence store.  
 * All content exceeding the limit will be truncated before it is  
 * written to disk.  
 *
 * ... 
 *
 */  
#define os_log(log, format, ...)    os_log_with_type(log, OS_LOG_TYPE_DEFAULT, format, ##__VA_ARGS__)
```

그렇다면 해결방안은 무엇일까요?

두 가지가 있습니다. 하나는 애플 문서에 나와있는대로 로그 CLI툴을 이용하거나 아래와 같이 XCode에서 `NSLog`대신 C함수 `printf`를 사용하는 것입니다.

`printf(“%s”,[html UTF8String]);`

![](https://farm2.staticflickr.com/1973/44134742245_1e80bc54f2_o.png)
*문자가 전부 출력된 모습* [(크게보기)](https://farm2.staticflickr.com/1973/44134742245_1e80bc54f2_o.png)
{:style="text-align: center" target="_blank"}

이상 `NSLog`의 비밀(?)에 대해서 알아봤습니다. 이제 문자열을 NSLog 출력했는데 전부다 출력되지 않는다고 당황하지 않으셔도 됩니다.

[^1]: [Apple API Reference – About the Logging System](https://developer.apple.com/reference/os/logging?language=objc)
[^2]: [StackOverflow - NSLog on devices in iOS 10 / Xcode 8 seems to truncate? Why？](http://stackoverflow.com/a/40283623/424937)