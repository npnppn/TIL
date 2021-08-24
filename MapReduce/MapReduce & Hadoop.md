## MapReduce Phase 3단계
- 맵 페이즈 : key value쌍 형태로 결과 출력하고, 같은 key를 가진 key value쌍은 같은 머신으로 보내짐  
- 셔플링 페이즈 : 맵 페이즈에서 각각 머신으로 보내진 key value 쌍을 key 를 이용해서 정렬한 후에, 각각의 key마다 같은 key를 가진 key value 쌍을 모아서 (value-list)를 만든다.  
다음에, key,value-list 형태로 key에 따라서 여러 머신에 분산해서 보냄.  
- 리듀스 페이즈 : 모든 머신에서 셔플링 페이즈가 끝나면 시작됨.  
각각의 key,value-list 쌍마다 리듀스 함수가 호출되며 하나의 리듀스함수가 끝나면 다음 key,value-list쌍에 리듀스함수가 호출됨. 출력이 있으면 key value 형태로 출력

## Hadoop
맵리듀스 프레임워크의 오픈소스  
- 빅데이터를 수천대의 값싼 컴퓨터에 병렬 처리하기 위해 분산
- 주요 구성 요소: MapReduce(소프트웨어의 수행(컴퓨팅)을 분산) , HDFS(데이터를 분산)  
- 한개의 Namenode와 여러개의 Datanode
- 자바로 맵리듀스 알고리즘 구현
- 맵리듀승 알고리즘 디자인에서 Combine함수를 사용하면 좋다!!

## 전체적인 개요
- Mapper와 Reducer는 각 머신에서 독립적으로 수행된다.
- 필요하다면 setup() and cleanup()을 수행할 수 있다.  
setup() : 모든 map 함수들이 공유하는 자료구조를 초기화 -> 첫 Map함수나 Reduce함수가 호출되기 전에 맨 먼저 수행  
cleanup() : 모든 map 함수들이 공유하는 자료구조의 결과를 출력 -> 마지막 Map함수나 Reduce함수가 끝나고 나면 수행
