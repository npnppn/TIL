## Vue.js 3 버전에서 추가될 Composition API

 

 


(2020.03.03 기준)
지금의 Vue.js는 2.6.11 버전이며 3버전을 향해가고 있습니다.

 

 

컴포지션 API(Composition API)가 등장한다는 소식에 많은 관심을 받았었는데요, 어떤 내용인지 한 번 알아봅시다.

 

1. 개발환경 준비하기
 

[사전 요구 사항]

- Node.js (with NPM)

- Vue CLI

 

 

지금의 Vue.js는 2.x 버전에 머물러 있기 때문에 컴포지션 API를 사용할 수 없습니다.

하지만, 플러그인을 통해 Vue.js 3에 추가될 컴포지션 API를 사용할 수 있습니다.

 

https://github.com/vuejs/composition-api

 
vuejs/composition-api

Vue2 plugin for the Composition API. Contribute to vuejs/composition-api development by creating an account on GitHub.

github.com
 

 

 

우선 Vue CLI를 통해 프로젝트를 생성하고, 컴포지션 API를 사용하기 위한 플러그인을 설치합니다.

저는 vue3-example 이라는 이름으로 생성했습니다.

 

 

$ vue create vue3-example

 


이후 컴포지션 API를 사용하기 위한 플러그인을 추가로 설치합니다.

 

$ npm install @vue/composition-api --save


아래와 같이 main.js에 컴포지션 플러그인을 사용하기 위해 등록하는 과정이 필요합니다.

(이후 버전 3이 나온다면 이 과정은 필요하지 않을 것입니다)

 

import Vue from 'vue'
import VueCompositionApi from '@vue/composition-api'
import App from './App.vue'

Vue.use(VueCompositionApi)

new Vue({
  render: h => h(App)
}).$mount('#app')
view rawmain.js hosted with ❤ by GitHub
 

 

지금까지의 과정을 마무리했다면 컴포지션 API를 Vue 2에서 사용할 수 있습니다.

2. Composition API란?
 

컴포지션 API는 컴포넌트의 로직을 유연하게 구성할 수 있도록 하는 함수 기반의 API 입니다.

코드의 재사용성과 타입 추론(타입스크립트)이 크게 개선되었다고 합니다.

 

기존의 Vue 2 버전에선 믹스인(mixin)을 통해 컴포넌트 로직을 어느정도 재사용할 수 있었지만,

오버라이딩 문제나 다중 믹스인을 상속한다면 컴포넌트 관리가 어려워진다는 부분 등 아쉬운 부분이 많이 있었습니다.

 

무엇보다도 하나의 싱글 파일 컴포넌트(.vue 파일)에 로직을 구현하다보면 이상하게 가독성도 떨어지고 복잡해지는 문제가 발생했습니다.

 

아래와 같은 상황을 예로 들 수 있습니다.


위의 코드는 간단한 카운터 컴포넌트임을 짐작할 수 있습니다.

 

카운터의 값을 저장하는 count와, 값을 조작하기 위한 증가(increase), 감소(decrease) 메소드,

그리고 계산된 속성(computed)를 통해 '7 입니다' 와 같은 형식의 데이터를 제공하는 기능이 구현되어 있습니다.

 

하지만 모두 같은 기능을 위한 코드임에도 불구하고 이곳 저곳(data, computed, methods)에 흩어져있는 모습을 볼 수 있습니다.

 

지금의 컴포넌트는 조그만하고 복잡하지 않아서 나름 직관적이지만, 규모가 커지고 로직이 많아질수록 점점 사방에 흩어지게 될 것입니다.

(* 물론 그만큼 거대한 컴포넌트가 탄생하기 전에 적절히 쪼개는 설계가 필요하겠지만요)

 

이 외에도 하나만 존재하는 생명 주기 훅(Life Cycle Hook)에 많은 코드가 응집되는 등 여러 문제가 있을 수 있었습니다.

 

 

결국 이 코드 저 코드를 확인하느라 직관적인 확인이 어려울 수 있고 재사용성이 떨어지게 됩니다.

 

이러한 문제를 해결하기 위해 탄생한 것이 오늘 알아볼 컴포지션(Composition) API 입니다.
 

위의  Vue 2버전 코드를 컴포지션 API를 사용한다면 아래와 같이 변경할 수 있습니다.


다소 복잡해 보일수도 있지만 카운터 기능에 대한 값과 로직이 useCount 함수 내에 모두 모여있는 모습을 볼 수 있습니다.

얼핏 보면 리액트의 Hooks와 매우 유사한 느낌을 받을 수 있습니다.

 

지금은 하나의 카운터 기능만 존재하지만, 이후 컴포넌트에 다양한 기능이 생겨도

목적에 맞는 코드를 모듈화하여 기존의 단점을 보완할 수 있으며 더 유연하고 안전한 확장이 가능합니다.


 

또한 setup 훅이 추가된 것을 볼 수 있는데 컴포지션 API를 사용하기 위한 진입점 즉, 초기화 지점입니다.

기존의 생명주기와 비교해보자면, beforeCreate 이전에 setup이 호출됩니다.

 

컴포지션 API를 사용하는 경우 beforeCreate, created 훅 대신 setup을 사용하면 됩니다.

 

3. Composition API 살펴보기 - 반응형 데이터
Vue 2에서의 변경 가능한(Mutable) 반응형 데이터는 아래와 같이 data에 존재하는 값들이 이에 해당했습니다.


여기서 message는 반응형이며 변경 가능한 데이터입니다.

(computed의 경우 반응형이지만, 읽기만 가능함)

 

반응형 데이터는 값이 변경됨에 따라 이를 감지하고 해당 값에 종속된 작업(Side Effect)이 수행됩니다.

예를 들면 위의 message 값이 변경되면, DOM에 존재하는 div의 내부 텍스트가 알아서 변경되는 경우를 의미합니다.

 

 

컴포지션 API에서는 2가지 유형(reactive, ref)의 변경 가능한 반응형 데이터를 만들 수 있습니다.

(아래 코드들은 간결한 설명을 위해 setup 메소드 없이 작성된 코드입니다. 코드를 직접 돌려보시려면 setup이 포함된 온전한 형태를 만들어주세요)


첫 번째는 reactive 값입니다.

컴포지션 API의 reactive를 통해 생성할 수 있으며 오직 객체만 받습니다.

reactive는 인자로 받은 객체와 완전히 동일한 프록시 객체를 반환하고,

생성된 객체는 Vue 2의 Vue.observable()로 생성한 것과 동일하다고 합니다.

 

값에 접근하는 방법은 원본 객체에 접근하는 것과 동일하며 reactiveValue.name을 통해 tom이라는 값을 받을 수 있습니다.

 

 

또한 reactive를 통해 생성된 객체는 모두 깊은(Deep) 감지를 수행하기 때문에

객체가 중첩된 상황에서도 반응형 데이터를 쉽게 조작하고 처리할 수 있습니다.

 

 

 ref를 알아보기 전에 하나 짚어보자면, reactive로 생성한 값은 프록시 객체라고 했지만 ES6에 추가된 프록시와는 다릅니다.


ES6의 프록시 객체는 원본 데이터와 다른 별도의 프록시 객체를 생성하지만

 reactive로 생성한 프록시 객체는 원본 객체와 완전히 동일합니다.

(원본 객체 내부에 Vue 옵저버만 추가하여 그대로 반환하기 때문)

 

 

 

 

두 번째는 ref 값 입니다.

이 또한 reactive와 동일하게 컴포지션 API의 ref를 통해 객체를 생성할 수 있으며

reactive는 객체를 받는데 반해 ref는 모든 원시타입(Primitive) 값을 포함한 여러가지 타입의 값을 받을 수 있습니다.

 

 원본 값은 ref 객체의 value 속성을 통해 접근할 수 있으며 값을 변경할 때에도 value 속성에 접근하여 조작해야 합니다.

 

 

 

여기서 한 가지 의아한 점이 있습니다.

reactive는 객체만 받아 값을 생성할 수 있지만,

ref는 원시타입 모두(객체 포함)에 해당하는 값을 생성할 수 있다고 했습니다.

 

그럼 reactive는 필요 없는 것 아닌가요..?라고 생각할 수도 있지만 공식 문서상에 ref의 값으로

객체가 전달될 경우 reactive 메소드를 통해 깊은(Deep) 감지를 수행한다고 합니다.

 

 reactive와 ref값을 템플릿에 표시하는 방법은 기존의 버전 2와 동일합니다.


단순히 콧수염 태그에 값을 넣어주기만 하면 됩니다.

 

ref 값을 직접 참조할 땐 value 속성으로 접근했었지만, 템플릿에 사용하는 경우에는 value 속성 접근없이 ref 값 자체를 사용합니다.

 

또한 setup 훅에서 값을 반환할 때 속성명을 변경하여 사용할 수 있습니다.

(reactiveValue => first, refValue => second)

 

reactive와 ref?

이해를 돕기위한 간소화된 그림 자료입니다 (파랑: 속성, 보라: 옵저버)
ref 객체는 원본 값을 value라는 속성에 담아두고 변경을 감시하는 객체이며,

reactive는 원본 객체 자체에 변경을 감지하는 옵저버를 추가하여 그대로 반환한 값 입니다.

 

값의 특성에 따라 적절히 선택하여 사용하면 될 듯 합니다.

 

아직 저 본인도 컴포지션 API를 계속 배워가고 있기 때문에

이후 포스팅에서 reactive와 ref에 대한 내용을 더 자세히 다뤄보도록 하겠습니다.

 

 

isRef, toRefs
 

컴포지션 API에는 단순한 반응형 데이터 뿐만 아니라 어떤 유형의 값인지 확인하기 위한 isRef,

reactive 값을 ref 값으로 변환하는 toRefs가 존재합니다.

 

isRef는 아래와 같이 ref 값과 reactive 값에 따라 다르게 처리해야 할 경우 사용합니다.


 ref 값의 경우 value 속성을 통해 데이터에 접근하므로 ref인 경우 obj.value를 출력하고,

ref가 아닌 경우(reactive) obj를 그대로 출력하도록 구현된 모습입니다.

 

 

toRefs는 아래와 같은 상황에서 사용합니다.


좌표와 같은 묶여있는 데이터의 경우 ref 값을 개별로 생성하기보단, 하나의 reactive 객체 값을 생성하여 사용합니다.

위의 경우엔 x, y 속성을 가지고 있는 객체를 reactive 값으로 생성하고 setup에서 불러오는 모습입니다.

 

하지만 useMousePosition의 reactive 객체를 구조 분해하여 재할당 할 경우 반응형으로 동작하지 않습니다.

위의 경우 setup에서 x, y로 분해하는 과정을 의미합니다.

 

기존 객체를 분해하여 각각 할당하기 때문에 반응형으로 동작하지 않는 것은 당연한 일입니다.

이러한 문제를 해결하기 위해 toRefs가 존재합니다.


위와 같이 reactive를 toRefs를 통해 ref 값으로 변환하면 구조 분해 할당을 수행해도 반응형이 그대로 유지됩니다.

 

x, y를 지닌 객체를 x를 참조하는 ref하나, y를 참조하는 ref하나 이렇게 총 2개의 ref로 변환할 뿐입니다.

 

4. Composition API 살펴보기 - 계산된 속성 (computed)
 

계산된 속성(computed) 또한 컴포지션 API를 통해 간단히 사용할 수 있습니다.


computed는 getter 함수가 반환하는 값에 대해 변경 불가능한 반응형 데이터를 반환합니다.

 

getter 함수는 위 코드에서 computed의 인자로 전달되는 () => myValue * 2 에 해당합니다.

value2x는 myValue에 2를 곱한 값을 제공합니다.

 

5. Composition API 살펴보기 - 변경 감시 (watch)
 

변경 감시(watch)도 컴포지션 API를 통해 간단히 사용할 수 있으며,

Vue 2 버전의 옵션과 동일한 옵션을 사용할 수 있습니다.

(deep, immediate)


해당 코드는 유저 리스트에 3명이 존재하고, 그 중 첫 번째 항목의 name값 뒤에 3초마다 느낌표를 추가하는 코드입니다.

 

watch를 통해 반응형 값의 상태 변화를 쉽게 감지할 수 있습니다.

watch는 첫 번째 인자의 getter가 반환하는 반응형 값 혹은 ref값이 변경될 경우 두 번째 인자로 전달된 콜백을 실행합니다.

 

위의 userList는 배열 안에 유저 객체가 포함되어 객체가 중첩된 데이터 구조를 갖는데 이런 데이터의 경우

위의 코드처럼 watch의 세 번째 인자로 deep 옵션을 활성화한 옵션을 전달하여 내부 값의 변경도 감지할 수 있습니다.

(deep 외에도 immediate 옵션이 존재합니다)

 

6. Composition API 살펴보기 - 생명 주기 훅 (Life Cycle Hooks)
 

Vue 2 버전에서는 생명 주기 훅을 아래와 같이 사용했습니다.


이 또한 컴포지션 API를 사용하여 바꿀 수 있습니다.

 


생명 주기 훅에 해당하는 메소드의 인자로 해당 주기 때 호출될 콜백 함수를 전달하여 사용합니다.

 

기존의 생명 주기 훅 이름 앞에 on 수식어가 붙은 형식의 메소드로 구성되어 있어 익숙하신 분들은 금방 사용하실 수 있을 것 같습니다.

다만 생명 주기 훅 이름이 변경된 경우가 있으며 아래 목록을 참고해주세요.

 

(좌: Vue 2, 우: 컴포지션 API)

 

beforeCreate -> setup

created -> setup

beforeMount -> onBeforeMount

mounted -> onMounted

beforeUpdate -> onBeforeUpdate

updated -> onUpdated

beforeDestroy -> onBeforeUnmount

destroyed -> onUnmounted

errorCaptured -> onErrorCaptured

 

강조 표시한 부분 외엔 변경된 사항은 없습니다.

 

 

 

또한 기존에는 컴포넌트당 하나의 생명 주기 훅을 사용했지만,

컴포지션 API를 사용할 경우 아래와 같이 여러개로 나누어 사용할 수 있습니다.

 

 


 하나의 mounted 훅 안에 모두 들어갔어야 할 코드들을

각 기능(something 1 ~ 3)별로 나누어 코드를 작성할 수 있습니다.

https://geundung.dev/102 블로그를 참고하였습니다.
