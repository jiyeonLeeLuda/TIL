# ArrayList vs LinkedList (시간복잡도)

**updated 2023.03.24**

참고 자료

- [JAVA Collection 시간 복잡도/특징 정리](https://www.grepiu.com/post/9)
- [ArrayList vs LinkedList 시간 복잡도](https://yeon-kr.tistory.com/152)
<hr/>

## LinkedList의 시간복잡도

```
시간복잡도
add             : O(1)
remove          : O(1)
get             : O(n)
Contains        : O(n)
iterator.remove : O(1)
java 1.2에 추가, thread-safe 보장 안함
특징 : 데이터를 저장하는 각 노드가 이전 노드와 다음 노드의 상태(주소값) 만 알고 있다.
   - 데이터 추가/삭제시 빠름
   - 데이터 검색시 처음부터 노드를 순화해야 되기 때문에 느림
   - 리스트를 이루는 노드의 데이터는 각각 규칙성 없이 메모리에 저장됨 (ArrayList와 상반되는 특징) -> 지역성의 특징
```

## ArrayList의 시간복잡도

```
시간복잡도
add             : O(1)
remove          : O(n)
get             : O(1)
Contains        : O(n)
iterator.remove : O(n)
java 1.2에 추가, thread-safe 보장 안함
 특징 :  데이터 추가,삭제를 위해 임시 배열을 생성해 데이터를 복사
   - 대량의 자료를 추가/삭제시 복사가 일어 나게 되어 성능 저하를 일으킴
   - 데이터의 인덱스를 가지고 있어 데이터 검색시 빠름
   - LinkedList와 다르게, 메모리상에 배열로 공간을 차지하고 있다. (상관된 데이터들이 모여있다.)
```

## 언제 ArrayList vs LinkedList를 쓰면 좋을까?

(작성중)
