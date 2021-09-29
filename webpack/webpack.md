1. Webpack 등장 배경 ✨
1-1. 페이지 로딩 속도 저하 🚶‍♀️


웹 사이트를 개발할 때 JS, CSS, IMG 등 수많은 리소스 파일이 생겨납니다.

위 사진은 GitHub 페이지를 로딩했을 때 다운로드 된 파일입니다. Type 을 보면 script, stylesheet, jpeg 등 다양한 파일이 있습니다.

완성된 웹사이트를 로딩했을 때 많은 파일이 다운로드 된다는 것을 알 수 있습니다.

많은 파일이 다운로드 되는 것의 단점은, 서버와의 접속이 많을수록 웹 로딩은 느려진다는 것입니다.

1-2. JS 모듈화 이슈 📂
<script src="/js/vendor/vendor01.js"></script>
<script src="/js/module01.js"></script>
<script src="/js/lib/library01.js"></script>
<script src="/js/module02.js"></script>
<script src="/js/module03.js"></script>
위 코드는 일반적인 방식인 태그로 자바스크립트를 모듈화하는 예제입니다.

이렇게 사용하면 여러 개의 파일을 로딩하더라도 같은 스코프를 공유하기 때문에 전역 변수의 충돌이 발생할 수 있습니다.

그리고 라이브러리 로딩 순서에 따른 이슈, 복잡도에 따른 관리 문제도 생길 수 있습니다.

1-3. Bundler 등장 🎊


위와 같은 두 문제를 해결하기 위해 등장한 도구가 바로 번들러입니다.

번들러란 필요한 의존성을 추적하여 해당하는 의존성들을 그룹핑 해주는 도구입니다.

이러한 번들러 중 가장 많이 사용되는 도구가 바로 웹팩입니다.

1-4. Bundler의 장점 👍


웹팩의 공식 사이트에 있는 사진입니다. 이렇게 애플리케이션을 구성하는 몇십, 몇백 개의 자원들을 하나의 파일로 병합 및 압축해 주는 동작을 모듈 번들링이라고 합니다.

이처럼 번들러를 사용하면 여러 개의 파일을 하나로 묶어주기 때문에 네트워크 접속의 부담을 줄여 더 빠른 서비스를 제공할 수 있습니다.

또한, Module 을 사용하여 매우 복잡하고 긴 코드를 작성할 때 사용 용도에 따라 파일 단위로 구분한 뒤, 다른 파일에서 해당 클래스나 함수가 필요할 때 가져와서 사용할 수 있도록 해줍니다. 외부 라이브러리의 의존성도 쉽게 관리할 수 있습니다.



2. 웹팩의 4가지 주요 속성 🔑
2-1. entry 🚪
entry 속성은 웹팩에서 웹 자원을 변환하는 데 필요한 진입점이자 자바스크립트 파일 경로입니다.

entry 속성에서 번들링하고 싶은 파일들을 선언합니다.

여기에 지정된 파일은 웹 애플리케이션의 전반적인 구조와 내용이 담겨 있어야 합니다.

왜냐하면 웹팩이 entry 속성에 지정된 파일을 가지고 웹 애플리케이션에서 사용되는 모듈들의 연관 관계를 이해하고 분석하기 때문입니다.

웹팩은 엔트리를 통해서 필요한 모듈들을 로딩하고, 하나의 파일로 묶는 과정을 진행합니다.

2-2. output 🎯
output 속성은 웹팩을 돌리고 난 결과물의 파일 경로를 의미합니다.

번들된 결과물을 처리할 위치를 output에 기록합니다.

2-3. loader 🚩
웹팩이 웹 애플리케이션을 해석할 때 JS 파일이 아닌 HTML, CSS, IMG, 폰트 등을 변환할 수 있도록 도와주는 속성입니다.

웹팩은 모든 파일을 모듈로 관리하지만 JS 만 알고 있어서 다른 파일들을 웹팩이 이해할 수 있도록 변경해주는 것이 로더의 역할입니다.

2-4. plugin 🔗
웹팩의 기본적인 동작에 추가적인 기능을 제공하는 속성입니다.

로더랑 비교하면 로더는 파일을 해석하고 변환하는 과정에 해당하고 플러그인은 해당 결과물의 형태를 바꾸는 과정에 해당합니다.



3. entry와 output 📃
3-1. module 🧩


웹팩은 JS 파일과 IMG 파일 등 모두 모듈로 인식합니다.

그래서 JS 코드에서 import 구문을 사용해서 IMG 와 CSS 파일을 가져올 수 있는 것입니다.

모듈을 지원하지 않는 브라우저에서도 웹팩을 사용하면 모듈을 적용할 수 있습니다.

위에서 본 사진을 기준으로 왼쪽 위 .js 파일을 entry 라고 합니다. entry를 기준으로 모든 모듈을 찾은 후 하나의 코드로 만들어 줍니다.

따라서 오른쪽 그림을 보면 하나의 .js 파일이 생성된 것을 볼 수 있습니다. 이렇게 만들어진 결과물을 output 이라고 합니다.

3-2. 실습해보기 💻
간단한 html 파일과 js 파일을 작성한 후 실습을 진행하겠습니다.

파일 구조

node_modules
src
 ┣ app.js
 ┗ math.js
index.html
package-lock.json
package.json
app.js

import { sum } from './math.js';

console.log(sum(1, 2));
math.js

export function sum(a, b) {
  return a + b;
}
index.html

<!DOCTYPE html>
<html lang="en">
  <head>
    <title>webpack</title>
  </head>
  <body>
    <script type="module" src="./src/app.js"></script>
  </body>
</html>
엔트리와 아웃풋을 실습하기 위해 패키지를 설치하겠습니다.

npm install -D webpack webpack-cli


4. 웹팩을 실행할 때 필수 3가지 옵션 💿
패키지를 설치한 후 아래 명령어를 실행하면 사용 가능한 옵션과 사용 방법에 관해 확인할 수 있습니다.

node_modules/.bin/webpack --help
기본값으로 줘야 하는 3가지 옵션에 대해 살펴보겠습니다.

4-1. mode 🎮
mode 옵션은 웹팩에 내장된 최적화 방법 중 어느 것을 사용할지 웹팩에게 알려주는 역할을 합니다.

module.exports = {
  mode: 'production' | 'development' | 'none',
}
production : DefinePlugin 의 process.env.NODE_ENV 를 production 으로 설정합니다.

development : DefinePlugin 의 process.env.NODE_ENV 를 development 로 설정합니다.

none : 기본 최적화 옵션으로 설정합니다.

mode 에 따라 웹팩 설정을 다르게 실행하고 싶다면 객체 대신 함수를 반환해야 합니다.

var config = {
  entry = './app.js'
};

module.exports = (env, argv) => {
  if (argv.mode === 'development') {
    config.entry = './dev/app.js';
  };

  if (argv.mode === 'production') {
  };

  if (argv.mode === 'none') {
  };

  return config;
};
4-2. entry 🚪
entry 옵션은 애플리케이션 번들링을 시작할 지점을 의미합니다. 만약 배열을 값으로 전달하면, 배열에 담긴 모든 요소가 번들링됩니다.

HTML 페이지마다 하나의 포인트를 가집니다. 그래서 SPA(Single Page Application)은 하나의 포인트를 가지게 되고, MPA(Multiple Page Application)은 페이지별로 하나의 포인트를 가지게 됩니다.

module.exports = {
  // SPA
  entry: './index.js'

  // MPA
  entry: {
    main: './main.js',
    contact: './contact.js'
  }
}
2번 항목의 웹팩의 4가지 주요 속성 중 엔트리에 대해 설명할 때 작성했던 내용입니다.

엔트리에 지정된 파일은 웹 애플리케이션의 전반적인 구조와 내용이 담겨 있어야 합니다.

예시를 통해 알아보겠습니다.

아래와 같은 SPA 의 index.js 코드가 있다면 index.js 파일에서 다른 js 파일도 불려져 사용되고 있어서 웹팩을 실행하면 해당 파일들의 내용까지 해석하여 파일을 빌드해줄 것입니다.

import LoginView from './LoginView.js';
import HomeView from './HomeView.js';
import PostView from './PostView.js';

function initApp() {
  LoginView.init();
  HomeView.init();
  PostView.init();
}

initApp();


위와 같이 모듈 간의 의존 관계가 생기는 구조를 디펜던시 그래프(Dependency Graph)라고 합니다.

4-3. output 🎯
번들링 결과물을 저장할 경로입니다.

최상위 output 키는 웹팩으로 번들링하거나 로드하는 번들, assets 그리고 다른 것들을 어떻게, 어디에 내보내야 하는지 설정하는 옵션들을 가지고 있습니다.
