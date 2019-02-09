---
layout: post
title: "[Xcode] Theme 추가하기"
tags: 
  - Xcode
comments: true
published: true
---

개발자의 생산성에 영향을 끼치는 요소는 여러가지가 있습니다.

의자, 책상, 모니터, 키보드, 마우스와 같은 물리적인 것부터 형상관리 도구,  CI 툴, Snippet 관리도구 등 소프트웨어적인 것도 있습니다.

또하나 빼놓을 수 없는게 바로 에디터 환경입니다. 어떤 색을 배경으로하고 예약어, 지시어들을 어떻게 표시하는지 자신에게 맞게 설정해 놓으면 집중력을 높이고 피로감을 줄일 수 있습니다.

Xcode는 기본으로 10개의 Theme을 제공합니다.

![Xcode에서 제공하는 기본 Theme](https://lh3.googleusercontent.com/PPAFTVAcENozedHKthHhsyHfGfppTiVEYI7JAMjV6i45JG5LK2JJ-nj8tZXD7V_3K9tOxFdxaPJjV357GBRq6yb_j328ATAwZFJFr4zcmeJrfAn77TTmnab1yeO6Odatm2HL8WLAX9Bom_O6CD8gkMuZaYQI9H5v335S6Z4qfZNPlmO_VxCzAHUacnIRgCvp2Y9qtTdnU4aSLXSaKz8bVb5Bv4rKkbdaVEZNQMWcHmHOunwCUeW9XJYLqyb0eTFycv6zM3JvlR429Zijj768230H1Mh7ZeCH3fUA20eQvLLRhvzCX5eeQRc1Jx2PN1Z4OWE63iLXrzDHoUwwiHVdW-NRl7230AeO9QhCM_hG5yHBCtdIQgpnpFbhezJMa8lV_2QWzFFITvlwQXi2ve6wAClLH-bWq7Wb9j60PAzj_BlCs2zF4C5tASYm88xQRX5ak-pONy-GrwBDZNREaio3sLnNa4X1aSdoDWxlt9ObRj4b8FJ-x1ugtDMez6ozgpMgFnRnPQytRpF9l_i1lJEFN_CqTPg074riD6FUdI393sZ7yzJDqFGHIRE8ur58vK_uYh-MIgyJ9VIz1OCW7RJQ2AGyJo4YlHOUQ2KGL7jMJonYPBmAfSLOCHYyKS_Wn-Hz=w862-h669-no "Xcode에서 제공하는 기본 Theme"){: .center-image}

기본 Theme에는 흰바탕, 검정바탕, 노랑바탕 등 여러 Theme이 있지만, 이것 외의 Theme을 사용하고 싶을 때도 있습니다. 기본 Theme 외의 Theme을 사용하기 위해서는 Theme파일을 특정 디렉토리에 복사하면 됩니다.

이번 포스트에서는 특정 Theme을 추가하는 법을 알아보도록 하겠습니다.

 

https://github.com/hdoria/xcode-themes 이 링크를 클릭하면 아래와 같이 50여개의 Theme파일을 다운로드 할 수 있습니다.

![](https://lh3.googleusercontent.com/1koI_tpocSaAZtSIKW8nkbqGDz_9tbBQ67oYCwjW8oK05Aj1qn9U_gJo0_sBVqCorS4hIviFiiAynfa8c1YUfMp4HkIFAM-EtEACjm_n4bPMo8KL2fsigkYVJLXMakikOM4IGEvoMb3X-zzl5tElCpNvT3FG8wgFefkhExyp8wd7atfuuW4N1alEjKJUxkU7OE3kxXXn-YI_IvualI-X-vteBPwyOVgy-1kuvlV1m2q9wf1m_CR4vjWau0fN62plz7uVYwHzCrOrr8W82COHBGMj3Q-aX5iyADWLhVioTI8OTEs0J2iHmZKB5yu99EieETzIZ_PDutV1ME9s1ZySFGDcLjh-Wc71A9u7vKN1L-4XvR10LHR69UPpcqIsRHUMkOjywBxS-QL0qX6H0Minec38we9CtyM-N2QaQFd7_ZzOnJIQODk3LiU7XD2zRey67p86cHY6oYpo0S10hVqLg3ZamCDHkhI8Z8A0_DlRlKzz88FDDA325OJBuEf7MjJTVP4yIrpgLfmu3K8cV_b_xP-MU1jVNbpYrwsWdRrRSx6EK3F7M4X6lLGgV3wFxAhHAC91QnDf_Q4Bo8p33K-bKTLlCDMOtahntTL_uMsoXKCe0QuamYj2fNQZMliYeCaf=w1032-h967-no){: .center-image}

만약 이 50여개의 Theme 모두를 설치하고 싶다면 깃헙에 나와 있는데로 다운로드 받은 후, 쉘스크립트를 실행해서 설치하면 됩니다.

```sh
$ git clone https://github.com/hdoria/xcode-themes.git
$ cd xcode-themes
$ ./install.sh # And then restart Xcode
```
만약 일부 Theme만 설치하고 싶다면 Theme 묶음을 다운로드 해서 압축을 풀면 여러 Theme파일을 볼 수 있습니다. 
![](https://lh3.googleusercontent.com/VPZeYzSu0Il2hVDfNjy5hTZKyzS63D3-qX1ajyLLsukkh8KLSFZczsXULnrttgoQ6d7QAAuzvjnOAOaO-rra5oIQ7plCSsTfJPbOrpLcEXIW6f-UOfVhYRrRg237gkVGe-_3upXqK3bxgdW32jq-QjthIwrSZdQoqMJYau1AL3pJd_Q-BWfjt34LUz7vejSL4vg5Hdi2d4J4CXauDjs0ePf3y0k16F1DlcIeX2nxMi8wqo_jWKV2kbW0IRdvLHLxYDcB8d7kBeunVepbt2wDW5EzoK_6Cp0u5y4BQuPSFV-JkpPRd72Vpl9wHLRmaafxldLXflc3PyN8lDnfYtKuwJa8oGY5vmgcAssOUWyyViygyqVSHh2EdC69ihwwzB060VoZ4eMMMWAQ1uyvz-Kiqmzc9SxpZuCoq8lHlJaWtUVEPmy6qwgLygGr0pNnQ8leW7HpuwQRsobW1tmjDQ2TeeyaMFkcOPYu3qQPEFULM8ftlcVfNsobnciLNvENL3wZUbkifMeonM2tnG03ZrsKLcY0-VlCR7_ITtJV4koa0zbaiYWF9UQ5xf9n-093M0FIKkFj6LyeYp7O_wvaPWTmULc-xO6C18X0B1TfqixiEI9ovqI3ACSzts9TGg2ee4hu=w941-h820-no){: .center-image}

여기에서 원하는 Theme을 복사합니다. 저는 “WWDC2016.dvtcolortheme” 파일을 복사했습니다.

Finder 혹은 터미널에서 아래 디렉토리로 이동합니다.
```
~/Library/Developer/Xcode/UserData/FontAndColorThemes/
```

![](https://lh3.googleusercontent.com/kjs40PFKaRmA2x7uCAk89E3Ud6IqIoXu3AMMWS0JH0IASxglRQu18omT1R6XYZGGJfMyad0Z6QpJwvP--0LVRCjLGu-9bWeBvkpeeDyaEmYwf3hKyqbi3Qh_WzZv7AOzAdHMBHXpWLOszbzZbq0mcxhGWSPHn7-LHJtFvmmk7kR3n_zm-46Ls5HT8JeRNBZ-f5ks1ir4fSLZPuQH5k_vHlPrIgB_CU4u4-x57iR3II_fNx34ZT1D7fcZq_YL68XiawzDW-RzPTSIfx3BvyCWzfXvmLiY7tmicYcfzdPgB-ZRxFzNe7xBmP4Spf9tfvg-4_nCl-9ylM6laHcUm00lXW7jnTsvNNFV00syxutPrw2nLYARfr9ma8FP1YUFyLTa-8cpHLGWCILdnDagtJ_DZf-VyxtGk4KADiiVjhsoejBm4mo_h0L23hKgqHKJigiKVfkgUmeh7n1ajQJzrSRkGswH4YqQzF7wuKITzG_f0Y3TMm7OF9JMu2qk89wZ4YTZ5ljyYgTbCK6R0RrTw7wFgSOCNspdA3Q8rZCgBGDVdrlMHCOGd3w_hYwgW_gs-Ktqjqc3ihlPbIvCgbaBduKXm9ApCFiTzUvAUJrYvErGJnr4ZTBao7uiYtzenM9o0gZR=w774-h583-no){: .center-image}

이 디렉토리에 복사한 파일을 붙여 넣습니다.

![](https://lh3.googleusercontent.com/aYRkc4Tm4wEwlW1UwaaqH84Zzy7qu52gFBTHeFBRoMs45kTNkp6jsBpLxBaGxNPfPlM_4IlpkTaQU5IVo43HB_xb8gX3AKwX5MqKvFNbretxmUyiQFE92tM_pKFza6qtdgEaTGW3t0ddOjXoVQsSmcknvhy7edctBw72riIJSK9BKTXYqYrnqmFylguRnhk0M5bCUGAy-EKb_oXKrRHrydks2Zul9wGYr1YPYJmozny_ldkMozZFwTNqNJoqS12zgYpb1ZFlecHuPR2mXjqzbSQnDsWITpHJFHFU8Qcg-O40O2arm7tzCv7qu2psml-qSLfjJ_zZXIgkGPIpRLv1YVfgraK7vUdRRkEjW-WUvzMFl0SSFLtGUsYr4IoVUX9minIftS8je2ge68NtYm2y1EoTEpTJ5368Ce6UoLi38_wh0zMaShi89_eDxhN4UnsiAt3bbNVOjG2SX0cbVczer6O4fjLqc6zbRLYeoRATunl4WBTgNXQN9rXAVGenDKngdcWPMKeKp_8TCVdfhubeZpS3-KUqIHHhJXlz0GCGerZnnAJtIA5ue6rQX7NRth-La2Odb9KPiaqwTzXWFOXeg_JGOsDXFSu3S1-4nA9k8F_Ru1UYPdwBpLPxkQMKNeWw=w774-h583-no "복사된 Theme파일")
{: .center-image}
*복사된 Theme파일*
{: style="text-align: center;"}

이제 Xcode를 실행 중이라면 종료 후 재실행 합니다.

설정 메뉴의 Fonts & Colors 메뉴로 이동하면 아래와 같이 WWDC2016 Theme이 추가된 것을 확인하실 수 있을 것입니다.

![](https://lh3.googleusercontent.com/amh40msiJvuAyUCI0Ym9erHfmqwfEbC7-8xMavI1AUVrn95npME1qqYowHA-oYNUSraix35krXTGoWqq0mM_Q5NBzhgESjRzJM0j_HAgmqMkX3py9yYsC0gUNaCG_Sg3c8K0ED66ab0Hng1peaBgHRGbkzuuI39AiY4uvdv_dlLEMLlJjz99ty6qCbFkDWnKXZSgDDT5Y1o-JUFfucJkbZ-uyrwRoOE2pIkPL3DzE7HWXOWT-RO8TFdFjr8lC3uY2E4nSQbe--gBj_hbDfO9FnNyBRq1aae0eXhPzZ5weo9RKynTxgfMrjOOL8EAC1qbKWyi539EcTqm1oOynfPlbineRMxkrMaVclUs_OiJgB9ux3wp9GRueOT7Idid87Zm1NQjzxhFgdoS43acjn0LhdfgAfwpBkZbQ4YwHl86cUUGUvXYciDt1H3e7WDR_ti8yUjvJ3cY842IVjYbbXsvIKSjlH9vwzIw8Zc6z5G6oeG4bU8UWF785gZxHtgxvaiT7qAuy-3ByJ7DAFkWoi8jZLOfs7ZQ_pSIR8JSmfk24VDEgSRbJiw5oYRa5edaEj0MAfW6DU7EobFKLlMiMDpjW9j2FliG4ADKlw_3cI3VFU0Z3HVKgaPcvWVWRSvbMZhF=w862-h669-no){: .center-image}

참고로 저는 Xcode를 사용하면서 처음 3년은 흰색 배경의 Default Theme을 사용하다가 그 후 zvuc님의 권유로 “Tomorrow Night Bright“라는 검정색 배경의 Theme을 지금까지 사용하고 있습니다.

개인적으로 확실히 흰색 바탕 배경보다 검정 바탕 배경이, 야간에 작업하거나 장기간 일할 때 피로도가 덜한 것 같습니다. <del>사실은 검정색 바탕에 코딩하면 있어보여서…ㅋㅋㅋ</del>

또 예쁜 색깔로 조합이 되어 있어서 좋습니다. ㅎㅎㅎ

기본 Theme에 싫증나신 분, 좀 더 나에게 맞는 다른 Theme을 이용하고 싶으신 분은 외부 Theme을 설치해서 사용해 보시면 어떨까요?

이상으로 Xcode에 Theme추가하기 Post를 마치겠습니다.

 

One more thing!

Theme파일을 열어보면 아래와 같이 XML key-value로 이루어진 그냥 plist파일일 뿐이라는 것을 알 수 있습니다.

![](https://lh3.googleusercontent.com/Kd4eo5sGKaqcu4Ufcgnc14AtswfTDOdVOJNXrvLUS7mxJH7BlfVkS6_XdcW0ZzrM-8P193-JJsrvI3d6OjWgyjQ8YwboqigkhNkqvq_Dz6HtRmNA7zrNzJLRMvRyqRjIw3Opnr76Y2jGMiu5oHGMAtfWGRUa5bnSi6jqTZ4zG450jwNceJUwvkTSbMy24oSBG347auIzFyrFZqscY7xDzuEJ7oQGFRK7rIBN5nT1bKclGIavW0IvYoiDcYrsh30eHdW2NHSWijxlrHewSuOvws9ZfMui78DNU7ZGyxAcCy1kzrFfAoCb0rfQ2KmmYVsTuJ1c2WJNnoGQJ15uY5XOZ5124VDdC1fD2xzXshJ_JMr32CityO52610Wsj79t6919uhLE_-jNJ_SA96I_for3FDyUzMKIIBxv85RUm4BILdvNWb08BsCQLv3IHoZ_wrsciwFfd91w7xo590jz8Bkhjn6PhmP9wn7FXbxWUxWq1NmMAalVwbJj5PU-MePotDm181j45fXDN7E2m3yzRemAYzG9GDugIFUCqu2iw52RtT0qSBwAxtl6N0-Qfa0ASjeSDaK_AYJSEw2mezEas2C7keBB46-9C4VIWGHaNHoK2BzV6fK1gBkq_Uhxzdjj6Ih=w897-h900-no){: .center-image}