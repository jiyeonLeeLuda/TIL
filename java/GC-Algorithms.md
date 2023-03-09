# GC algorithms

**updated 2023.03.09**
참고

- [Java 가비지 수집 알고리즘 이해](https://developersjournal.in/understanding-java-garbage-collection-algorithms/)
<hr/>

## GC - YoungRegion

1. Young Region - 이름에서 알 수 있듯이 젊은 영역은 최근에 생성된 개체를 포함하는 힙 영역입니다. Young 지역 자체는 더 많은 지역으로 세분화됩니다.
   1. Eden 공간 - 초기 생성 시 개체는 첫 번째 가비지 수집까지 힙의 Eden 영역에 저장됩니다.
   2. survivor (서바이버) 공간 - 가비지 컬렉션에서 살아남은 객체는 서바이버 영역으로 승격됩니다. 세대별 수집기에는 가비지 수집기 효율성을 개선하기 위한 목적으로 여러 생존 영역이 있습니다. 가비지 수집 중에 여전히 Eden 공간 또는 점유된(occupied:가득차 있는) 서바이버 공간에 있는 참조되는 개체는 빈 생존자 공간으로 복사 또는 이동합니다.

## 3가지 가비지 수집 알고리즘.

- Sweeping
- Compacting
- Copying

Sweeping

<img src="https://developersjournal.in/wp-content/uploads/2017/09/gc-algo-mark-sweep-300x110.png">

> After the marking phase, we have the memory space which is occupied by visited and unvisited objects. All of the unvisited objects are considered free and can be re-used to allocate new objects. It is simple, but because the dead objects are not necessarily next to each other, we end up having a fragmented memory. That’s not bad, but if no single region is large enough to accommodate the allocation, the allocation is still going to fail (with an OutOfMemoryError in Java).

마킹 단계가 끝나면 방문한 객체와 방문하지 않은 객체가 차지하는 메모리 공간이 생깁니다. 방문하지 않은 객체는 모두 비어 있는 것으로 간주되며 새 객체를 할당하는 데 재사용할 수 있습니다. 간단하지만, 죽은 객체가 반드시 서로 옆에 있는 것은 아니기 때문에 결국 파편화된 메모리를 갖게 됩니다. 나쁘지는 않지만 **할당을 수용할 수 있을 만큼 충분히 큰 영역이 하나도 없는 경우** 할당은 여전히 실패하게 됩니다(Java에서는 OutOfMemoryError가 발생함).

Compacting

<img src="https://developersjournal.in/wp-content/uploads/2017/09/gc-algo-mark-sweep-compact-300x165.png">

> 알고리즘마크-스윕-컴팩트 알고리즘은 마크 및 스윕(조각화된 메모리)의 단점을 해결합니다. 이 알고리즘은 표시된 모든 객체를 메모리 영역의 시작 부분으로 이동합니다. 힙을 압축하는 것은 무료가 아니며 몇 가지 단점도 있습니다. 이 접근 방식은 모든 오브젝트를 새로운 위치로 복사하고 해당 오브젝트의 모든 참조를 업데이트해야 하므로 GC 일시 중지 시간이 길어집니다. 이러한 접근 방식을 사용하면 여유 공간의 위치를 항상 알 수 있으며 조각화 문제도 발생하지 않습니다.

Copying

<img src="https://developersjournal.in/wp-content/uploads/2017/09/gc-algo-mark-copy.png">

> Mark and Copy algorithms are very similar to the Mark and Compact as they too relocate all live objects. The important difference is that the target of relocation is a different memory region as a new home for survivors. The previously occupied region is considered to be free. The advantages of this algorithm is that copying can occur simultaeously with marking during the same phase. Next time mark-copy is executed, all the alive objects are moved back to the previous memory region. As you can imagine, this of course leads to a memory compaction. Unfortunately, it requires additional extra region large enough to fit all live objects at any given point in time.

마크 및 복사 알고리즘은 모든 라이브 오브젝트를 재배치한다는 점에서 마크 및 컴팩트와 매우 유사합니다. 중요한 차이점은 재배치 대상이 생존자의 새로운 보금자리로 다른 메모리 영역이라는 점입니다. 이전에 점유했던 영역은 비어 있는 것으로 간주됩니다. 이 알고리즘의 장점은 같은 단계에서 마킹과 복사를 동시에 수행할 수 있다는 것입니다. 다음에 마크 복사가 실행되면 살아있는 모든 개체는 이전 메모리 영역으로 다시 이동합니다. 상상할 수 있듯이 이것은 당연히 메모리 압축으로 이어집니다. 안타깝게도 특정 시점의 모든 라이브 오브젝트를 수용하기 위해 충분히 큰 추가 영역이 필요합니다.

## Survivor 공간은 왜 2개일까?

- Mark and Copy algorithms을 수행하기 위해서 설계되었다.
- GC는 죽은 객체를 청소하기위해 Mark, Sweep,compaction 즉, 마킹하고(삭제할 객체를 확인 및 표시) 쓸어내고(힙에서 삭제), 압축하는 (빈 공간 처리, 디스크 조각모음을 통해 큰 여유공간 확보하는 정리 작업) 3가지 작업을 한다.
- 이 중 Mark and Copy 알고리즘은 OutOfMemoryError를 방지하기 위해 마킹과 동시에 복사를 통해 디스크 공간을 최적화 하는 작업을 수행한다.
- 결과적으로 Survior 1 공간에 디스크 조각모음을 하면서 살아있는 객체를 안전하게 보관 하면서, servior 2 공간을 비워둠으로써 다음 GC에서 Mark and Copy가 가능하도록 준비한다. 또한 servior 한쪽 영역이 가득차 Old 영역으로 객체를 이관할 때에도 다른 한쪽이 비어있음으로 minor GC가 발생 하더라도 안전하게 쓰기 작업이 가능할 것이라 유추 된다.
