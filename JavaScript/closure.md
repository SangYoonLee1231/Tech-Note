## JavaScript - 클로저(Closure)

<p data-ke-size="size16">우선 클로저를 쉽게 설명하면, <b>함수가 자신이 탄생한 환경(스코프)을 기억하는</b> 것을 말합니다. 이로 인해 자신이 탄생한 외부 함수가 메모리에서 정리되어도 그 값을 기억하고 접근할 수 있습니다. 좀 더 깊이 있게 설명하면, 클로저는 <b>내부 함수의 코드와 렉시컬 환경을 함께 기억하는 함수 객체</b>를 말합니다. 이 정의에는 헷갈리는 개념들이 다수 포함되어 있기 때문에, 아래에 좀 더 쉽게 풀어서 설명해 보겠습니다.</p>
<p data-ke-size="size16">&nbsp;</p>
<hr contenteditable="false" data-ke-type="horizontalRule" data-ke-style="style6" />
<p data-ke-size="size16">우선 예시 코드부터 살펴보겠습니다.</p>
<pre id="code_1750662031471" class="javascript" data-ke-language="javascript" data-ke-type="codeblock"><code>function outerFunction(outerVariable) {
  return function innerFunction(innerVariable) {
    console.log('Outer Variable: ' + outerVariable);
    console.log('Inner Variable: ' + innerVariable);
  };
}

const newFunction = outerFunction('outside'); // (1)
newFunction('inside'); // (2)</code></pre>

<p data-ke-size="size16">&nbsp;</p>
<p data-ke-size="size16">outerFunction의 내부에 innerFunction이 정의되어 있습니다. 그리고 (1)에서 newFunction 상수를 선언하고 outerFunction 함수를 호출하여 여기서 반환된 innerFunction 함수를 할당해줍니다. 매개변수는 'outside'이고요. 이렇게 함수를 값처럼 할당하는 것이 가능한 이유는 자바스크립트가 일급 객체 언어이기 때문입니다. 이 내용은 다음 포스트에서 좀 더 자세히 다룰 예정입니다. 보통 (1)의 코드가 실행되면 outerFunction의 호출이 끝나기 때문에 이에 해당하는 메모리 공간은 회수됩니다.&nbsp;</p>
<p data-ke-size="size16">&nbsp;</p>
<p data-ke-size="size16">이제 다음 줄 (2)을 보겠습니다. newFunction을 호출하는 코드이네요. newFunction엔 내부 함수 innerFunction가 연결되어 있으므로, 매개변수가 'inside'인 innerFunction이 여기서 실행된다고 예상할 수 있습니다. 이 때, 주목할 점은 'innerFunction 내의 outerVariable을 읽을 수 있는가?'입니다. outerVariable은 외부 함수의 매개변수 이므로 (1)줄이 실행되면 메모리에서 사라져 읽을 수 없습니다. 그렇지만 결과는 '읽을 수 있다'입니다.</p>
<p data-ke-size="size16">&nbsp;</p>
<p data-ke-size="size16">왜냐하면 바로 <b>클로저의 원리</b>로 인해 <b>innerFunction은 자신이 생성된 환경인 outerFunction의 스코프를, 정확하게는 outerFunction<b>의</b> 렉시컬 환경을 기억하고 있기 때문</b>입니다. 따라서 outerFunction의 실행 자원이 메모리에서 사라져도 이 렉시컬 환경은 사라지지 않습니다. 이에 따라 innerFunction은 외부 함수의 변수인 outerVariable에도 접근할 수 있는 것입니다.</p>
<p data-ke-size="size16">&nbsp;</p>
<p data-ke-size="size16">그렇다면 렉시컬 환경은 무엇을 가리키는 개념일까요?</p>
<p data-ke-size="size16">이를 이해하려면 자바스크립트의 실행 컨텍스트 개념을 알고 있어야 합니다. 실행 컨텍스트는 소스 코드를 실행하기 위해 미리 만들어두는 환경입니다. 작업실로도 비유할 수 있겠네요. 전역 함수는 전역 실행 컨텍스트를 생성하고 지역 함수는 지역 실행 컨텍스트를 생성합니다. 이 실행 컨텍스트는 변수와 함수를 저장하는 객체, this 포인터, 스코프 체인 이렇게 3가지로 구성되어 있습니다. 여기서 변수와 함수를 저장하는 객체, 스코프 체인 이 둘을 묶어 렉시컬 환경이라 부릅니다. 실행 컨텍스트에 대한 자세한 내용은 추후에 더욱 자세히 다룰 예정입니다. 실행 컨텍스트의 개념을 잘 모르시는 분들은 우선 렉시컬 환경을 '함수의 변수들을 저장해놓은 공간' 이 정도로만 이해하시면 충분할 듯 합니다.</p>
<p data-ke-size="size16">&nbsp;</p>
<hr contenteditable="false" data-ke-type="horizontalRule" data-ke-style="style6" />
<p data-ke-size="size16">마지막으로 클로저가 어떻게 활용되는 지를 언급하고 포스트를 마치겠습니다.</p>
<p data-ke-size="size16">&nbsp;</p>
<p data-ke-size="size16">클로저는 변수와 함수의 접근 범위를 제어하고 특정 데이터와 상태를 유지하기 위해 자주 활용됩니다. 크게 세 가지 대표적인 사용 사례로 나누어 설명드릴 수 있습니다.</p>
<p data-ke-size="size16">&nbsp;</p>
<p data-ke-size="size16"><b>첫째, 데이터 은닉에 활용됩니다.</b>&nbsp;클로저는 외부에서 접근할 수 없는 비공개 변수와 함수를 만들 수 있습니다. 이를 통해 데이터를 은닉하여 외부 접근을 막고, 데이터 무결성을 유지할 수 있습니다. 예를 들어, 특정 함수 내부에서만 접근 가능한 변수를 생성하고, 이를 조작할 수 있는 함수만 외부로 노출하여 안전하게 데이터를 관리할 수 있습니다.</p>
<p data-ke-size="size16">&nbsp;</p>
<p data-ke-size="size16"><b>둘째, 비동기 작업에 활용됩니다.</b>&nbsp;클로저는 비동기 작업에서 이전의 실행 컨텍스트를 유지해야 할 때 유용합니다. 콜백 함수가 비동기적으로 실행될 때 클로저를 사용하면 함수 실행 시점의 변수를 참조할 수 있습니다.</p>
<pre class="javascript"><code>function createLogger(name) {
  return function() {
    console.log(`Logger: ${name}`);
  };
}

const logger = createLogger('MyApp');
setTimeout(logger, 1000); // 1초 후에 'Logger: MyApp' 출력
</code></pre>

<p data-ke-size="size16">위의 예시에서 클로저가&nbsp;name&nbsp;변수('MyApp')를 저장하여 1초 후에도 해당 값이 유지되어 출력됩니다.</p>
<p data-ke-size="size16">&nbsp;</p>
<p data-ke-size="size16"><b>셋째, 모듈 패턴을 구현하는 데 활용됩니다.</b>&nbsp;모듈 패턴은 특정 기능을 캡슐화하고, 외부에 공개하고자 하는 부분만 선택적으로 노출하여 코드의 응집력을 높이고, 유지보수성을 향상시키는 패턴입니다. 클로저를 활용하면 필요한 함수와 데이터만 외부로 노출함으로써 모듈 패턴을 쉽게 구현할 수 있습니다.</p>
<p data-ke-size="size16">&nbsp;</p>

<br/>
