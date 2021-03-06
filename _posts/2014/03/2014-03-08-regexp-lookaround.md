---
layout: post
title: "[Regex]전방탐색과 후방탐색"
description: ""
category: "Regex"
tags: [regex, 정규식, 정규표현식, 전방탐색, 후방탐색, lookaround, lookahead, lookbehind ]
---
{% include JB/setup %}

## 전방탐색과 후방탐색

원하는 문자를 검색하기 위하여 정규식을 사용하였지만 어디서부터 문자를 찾을지 정해야할때도 있습니다. 이때 전후방탐색(lookaround)를 사용해야합니다.

### 전방탐색 - 앞에서 찾기

전방탐색(lookahead)패턴은 일치 영역을 발견해도 그 값을 반환하지 않는 패턴을 말합니다. 전방탐색은 실제로는 하위 표현식이며, 하위 표현식과 같은 형식으로 작성됩니다. 전방탐색 패턴의 구문은 `?=`로 시작하고 등호(=) 다음에 일치할 텍스트가 오는 하위 표현식입니다.

<div class="alert-info"><strong>팁</strong> 일부 정규 표현식 문서에 일치하는 영역을 반환하는 동작을 표현할 때 <code>소비한다(consume)</code>라는 용어를 사용합니다. 전방탐색은 <code>소비하지 않는다(not consume)</code>라고 말합니다.</div>

다음 예제에서 어떻게 사용하는지 살펴봅니다.

예문)

	http://www.forta.com
    https://mail.forta.com
    ftp://ftp.forta.com

정규 표현식)

	.+(?=:)

결과)

	http
    https
    ftp

예제에 나열된 URL에서 프로토콜은 :을 기준으로 분리가 되며 .+패턴은 프로토콜과 일치하고 (?=:)는 :과 일치합니다. 이때 :를 찾되 :앞에 있는 문자열을 찾으라고 하며 :는 소비되지 않습니다.

그렇다면 ?=가 없다면 어떻게 동작하는지 다음 예제를 봅니다.

예문)

	http://www.forta.com
    https://mail.forta.com
    ftp://ftp.forta.com

정규 표현식)

	.+(:)

결과)

	http
    https
    ftp

하위표현식 :과 일치하고 문자열을 소비하여 위에서 나온 결과와 같이 나타납니다.

### 후방탐색 - 뒤에서 찾기

텍스트를 반환하기 전에 뒤쪽을 탐색하는 것은 후방탐색(lookbehind)이라고 합니다. 후방탐색 연산은 ?<=입니다.

기본적으로 후방탐색의 사용방법은 전방 탐색과 같습니다. 하위 표현식 안에서 사용하고 일치할 텍스트 앞에 옵니다. 

다음 예제에서 어떻게 사용하는지 살펴봅시다.

예문)

    ABC01: $23.45
    HGG42: $5.31
    CFMX1: $899.00
    XTC99: $69.96
    Total items found: 4

정규 표현식)

    (?<=\$)[0-9.]+

결과)
    
    23.45
    5.31
    899.00
    69.96

$ 기호와 일치하지만 소비하지 않고 뒤에 숫자만 반환합니다.

### 전방탐색과 후방탐색 함께 사용하기

예문)

    <HEAD>
    <TITLE>Ben Forta`s Homepage</TITLE>
    </HEAD>

정규 표현식)
    
    (?<=\<[tT][iI][tT][lL][eE]\>).*(?=\<\/[tT][iI][tT][lL][eE]\>)

결과)
    
    Ben Forta`s Homepage

`(?<=\<[tT][iI][tT][lL][eE]\>)`는 후방탐색 작업으로 `<TITLE>`과 일치하며 소비하지 않습니다.. `(?=\<\/[tT][iI][tT][lL][eE]\>)`도 같은 방식으로 `</Title>`과 일치하며, 소비하지 않습니다. 따라서 제목 텍스트만 반환합니다..


### 부정형 전후방탐색

앞에서 봤던 전방탐색과 후방탐색은 찾고자 하는 부분의 앞뒤를 특별히 지정하고 싶을 때 주로 사용되며 이러한 방법들은 긍정형 전방탐색, 긍정형 후방탐색이라고 합니다. 실제로 일치하는 텍스트를 찾기 때문이죠.

부정형 전방탐색은 앞쪽에서 지정한 패턴과 일치하지 않는 텍스트를 찾고 부정형 후방탐색도 마찬가지로 뒤쪽에서 지정한 패턴과 일치하지 않는 텍스트를 찾습니다.

부정형 전후방탐색에서 부정형을 나타낼때는 등호(=) 대신 느낌표(!)를 사용합니다.

종류 | 설명
--------|------
(?=) | 긍정형 전방탐색
(?!) | 부정형 전방탐색
(?<=) | 긍정형 후방탐색
(?<!) | 부정형 후방탐색


<br/><div class="alert-info"><strong>팁</strong> : 전후방탐색을 지원하는 정규 표현식은 긍정형, 부정형을 지원합니다.</div>


다음 예제에 가격과 수량을 나타내는 숫자들이 있습니다. 긍정형에서는 가격, 부정형에서는 수량을 얻을 것입니다.

예문)

    I paid $30 for 100 apples,
    50 oranges, and 60 pears.
    I saved $5 on this order.

#### 긍정형

정규 표현식)

    (?<=\$)\d+

결과)

    30
    5

\d+는 하나 이상 연속된 숫자와 일치하고 (?<=\$)는 긍정형 후방탐색이며, \$로 $ 기호를 찾습니다. 하지만 소비하지 않으므로 가격만 일치합니다.

#### 부정형

정규 표현식)
    
    \b(?<!\$)\d+\b

결과)

    100
    60

\d+는 하나 이상 연속된 숫자와 일치하고 (?!\$)는 부정형 후방탐색이므로 앞에 $ 기호가 없는 숫자와 일치합니다. 이때 \b를 사용하여 경계를 표시하여 $30에 0이 일치할 수 있는 경우를 제외시켜 수량만 얻을 수 있도록 합니다.