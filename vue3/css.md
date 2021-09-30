Flex에 관한 오해
“Grid가 나온 이상, Flex는 구시대의 유물일 뿐이다.”

글쎄..?

더 나중에 나온 스펙인 Grid로도 Flex와 똑같이 구현할 수 있는 경우가 많지만, Grid로는 구현이 어려운 레이아웃이거나 Flex를 쓰는게 더 편리한 경우도 있습니다. 그리고 마소가 싼 똥 버렸지만 우리는 여전히 지원해줘야 하는 인터넷 익스플로러(IE)같은 경우는 Flex는 표준 스펙을 지원하지만 Grid는 legacy(고인물) 버전만 지원하기 때문에, 둘 다 잘 알아두고 적재적소에 활용하는 것이 가장 좋다고 생각돼요.

Flex의 속성들은,

- 컨테이너에 적용하는 속성
- 아이템에 적용하는 속성

이렇게 2가지로 나뉜다!!

### 1) 컨테이너에 적용하는 속성

1. display: flex;

1. flex-direction; → 배치 방향 설정


1. flex-wrap; → 줄넘김 처리 설정


1. flex-flow; → 방향과 wrap을 한번에 해줄 수 있는 설정
2. justify-content → 메인축 방향 정렬


이렇게 6종류 있는데.. 이름보면 느낌 알제?

1. align-items → 수직축 방향 정렬


쭈욱 늘어나는 느낌으로!

justify-content: center;

align-item: center;

이렇게 두개를 쓰면 아이템을 한가운데에 정렬!

1. align-content → 여러 행 정렬

### 2) 아이템에 적용하는 속성

1. flex-basis → 아이템의 기본 크기를 설정
2. flex-grow → flex-basis보다 크기 늘려주는.. 쭈욱 아이템이 늘어남



0일 때


1일 때

1. flex-shrink → flex-basis보다 크기 줄여주는.. 쭈욱 아이템이 줄어듬
2. flex → flex-grow, flex-shrink, flex-basis를 한 번에 쓸 수 있는 축약형 속성
3. align-self → 수직축으로 아이템 정렬
4. order → 시각적 나열 순서
5. z-index → 숫자 클수록 위로 나타남  



display:table : 다른 요소를 table태그 속성으로 바꿔주는 역할  
display:table-cell : 다른 요소를 tr, td 속성으로 바꿔주는 역할  
table표 안에 있는 것처럼 바꿔서 쉽게 정렬 할 수 있게 할때 쓰인다.  
td에서 정렬은 text-align:center; vertical-align:middle; 으로 간단하게 가로/세로정렬이 가능하다.
