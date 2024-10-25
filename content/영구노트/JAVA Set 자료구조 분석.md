---
tags:
  - 완성
  - JAVA
  - 자료구조
aliases:
  - Set
  - set
date: 2024-05-13
title: JAVA Set 자료구조 분석
---
작성 날짜: 2024-05-13
작성 시간: 12:55


----
## 내용(Content)

### Set

>[!summary]
>Set은 Unique하게 내부 데이터를 유지하는 자료구조이다.

Set을 활용하면 중복되지 않고 유일한 데이터 값들을 저장할 수 있다. Set 구현체에 따라 여러 용도로 Set 인터페이스를 사용가능하다.

자주 많이 사용하는 Set 구현체의 종류로는 HashSet, TreeSet, LinkedHashSet이 있다.

### TreeSet

>[!summary]
>TreeSet은 이진 트리 구조로 이루어진 자료구조이다.

TreeSet은 내부적으로 이진 트리 구조로 되어 있다. 그래서 TreeSet에 사용될 클래스는 Comparable을 구현해야 한다. 

>[!caution]
>Comparable을 구현해야 class는 비교 가능해진다. 내부적으로 값들의 크기를 비교하고 이진트리를 구성하기위해서는 비교 가능한 class여야 한다.

### HashSet

>[!summary]
>HashCode를 활용해 유일성을 판단하고 값들을 저장한다. 이 때 데이터들의 입력 순서나 정렬은 보장하지 않는다.

HashSet은 기본적으로 Hash Code를 활용한다. 왠만한 Collection의 클래스는 Equals/HashCode가 이미 구현되어 있기 때문에 쉽게 사용할 수 있다. 커스텀 클래스(Value Object)를 만들어 활용하는 경우 Equals/HashCode를 따로 구현해서 사용해주면 된다. 

### LinkedHashSet

>[!summary]
>HashCode를 활용하는 것은 HashSet과 같지만 입력한 값들의 저장 순서를 보장한다.

일반적으로 HashSet을 사용하지만, 입력한 순서를 보장해야 하는 경우 LinkedHashSet을 사용하면 된다. 

### TreeSet vs HashSet vs LinkedHashSet

TreeSet과 HashSet, 그리고 LinkedHashSet를 [[JMH|Java MicroBenchmark Harness]] 를 이용해서 성능 비교를 해보자.

```java
@State(Scope.Benchmark)  
@BenchmarkMode(Mode.AverageTime)  
@OutputTimeUnit(TimeUnit.MICROSECONDS)  
public class HashSetBenchmark {  
  
    @Param({"1000", "10000"})  
    private static int NUMBERS;  
  
    private static final Set<Integer> hashSet = new HashSet<>();  
  
    @Setup  
    public void init() {  
       for (int i  = 0; i < NUMBERS; i++) {  
          hashSet.add(i);  
       }  
    }  
  
    @Benchmark  
    public void putElements(Blackhole bh) {  
       Set<Integer> set = new HashSet<>();  
       for (int i = 0; i < NUMBERS; i++) {  
          bh.consume(set.add(i));  
       }  
       bh.consume(set);  
    }  
  
    @Benchmark  
    public void containsElements(Blackhole bh) {  
       for (int i = 0; i < NUMBERS; i++) {  
          bh.consume(hashSet.contains(i));  
       }  
    }  
}
```

```java
@State(Scope.Benchmark)  
@BenchmarkMode(Mode.AverageTime)  
@OutputTimeUnit(TimeUnit.MICROSECONDS)  
public class LinkedHashSetBenchmark {  
  
    @Param({"1000", "10000"})  
    private static int NUMBERS;  
  
    private static final Set<Integer> linkedHashSet = new LinkedHashSet<>();  
  
    @Setup  
    public void init() {  
       for (int i = 0; i < NUMBERS; i++) {  
          linkedHashSet.add(i);  
       }  
    }  
  
    @Benchmark  
    public void putElements(Blackhole bh) {  
       Set<Integer> set = new LinkedHashSet<>();  
       for (int i = 0; i < NUMBERS; i++) {  
          bh.consume(set.add(i));  
       }  
       bh.consume(set);  
    }  
  
    @Benchmark  
    public void containsElements(Blackhole bh) {  
       for (int i = 0; i < NUMBERS; i++) {  
          bh.consume(linkedHashSet.contains(i));  
       }  
    }  
}
```

```java
@State(Scope.Benchmark)  
@BenchmarkMode(Mode.AverageTime)  
@OutputTimeUnit(TimeUnit.MICROSECONDS)  
public class TreeSetBenchmark {  
  
    @Param({"1000", "10000"})  
    private static int NUMBERS;  
  
    private static final Set<Integer> treeSet = new TreeSet<>();  
  
    @Setup  
    public void init() {  
       for (int i = 0; i < NUMBERS; i++) {  
          treeSet.add(i);  
       }  
    }  
  
    @Benchmark  
    public void putElements(Blackhole bh) {  
       Set<Integer> set = new TreeSet<>();  
       for (int i = 0; i < NUMBERS; i++) {  
          bh.consume(set.add(i));  
       }  
       bh.consume(set);  
    }  
  
    @Benchmark  
    public void containsElements(Blackhole bh) {  
       for (int i = 0; i < NUMBERS; i++) {  
          bh.consume(treeSet.contains(i));  
       }  
    }  
  
}
```

이렇게 3개의 자료구조에 대한 JMH 코드를 작성했다. 간략하게 설명하자면 내부적으로 자료구조를 선언하고 `@Param`을 이용해 NUMBERS가 1000, 10000 변수를 가지도록 설정한다. 그리고 benchmark에서 측정 이전에 `@Setup`을 이용해 미리 set 변수들을 초기화해두고 `@Benchmark` 테스트를 통해 benchmark 테스트 코드를 작성해준다. 위의 벤치마크의 경우
Fork=1, warnup=2, iterations=2로 설정해두었다.

그 결과를 python으로 시각화한 코드다.

```python
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
from pprint import pprint



# 파일 읽기

with open('results.txt', 'r', encoding="utf-8") as f:
	lines = f.readlines()

# '±' 기호 제거
lines = [line.replace('±', '') for line in lines]


# 새로운 파일에 쓰기
with open('new_results.txt', 'w', encoding="utf-8") as f:
	f.writelines(lines)

data = pd.read_csv('new_results.txt', sep="\s+")

pprint(data)

data['BenchmarkGroup'] = data['Benchmark'].str.split('.').str[0]
data['BenchmarkType'] = data['Benchmark'].str.split('.').str[1]

pprint(data)


# 오차 막대 추가
g = sns.catplot(x='(NUMBERS)', y='Score', hue='BenchmarkGroup', col='BenchmarkType', data=data, kind='bar', ci="sd")


# 각 서브플롯에 대해 반복
for i, ax in enumerate(g.axes.flat):
	# 막대에 수치 추가
	for bar in ax.patches:
		ax.text(bar.get_x() + bar.get_width() / 2, bar.get_height(), f'{bar.get_height():.2f}',

				ha='center', va='bottom')
	# y축 레이블 변경
	ax.set(ylabel="Score (us/op)")

plt.show()
```

결과는 다음과 같다.

![[Pasted image 20240513210702.png]]

생각보다 LinkedHashSet과 HashSet 성능 차이는 크지 않다. TreeSet은 내부적으로 정렬을 수행하기 떄문에 시간복잡도가 크고, 더 느리다. TreeSet이 일반적으로 HashSet보다 데이터가 적으면 빠르다는데, 1000개면 크지 않다고 생각하는데 성능차이가 나는걸 보면, 이진 트리의 이점을 챙기는 것이 아니라면 HashSet을 사용하는 것이 낫다고 생각한다. contains의 경우 20배나 차이가 나고 입력 역시 4배 가까이 차이나기 때문이다.

Bruce Eckel이란 분이 테스트한 자료도 있으니 https://www.artima.com/weblogs/viewpost.jsp?thread=122295 를 참고해보자

## 질문 & 확장

(없음)

## 출처(링크)

- https://www.artima.com/weblogs/viewpost.jsp?thread=122295
- https://ysjee141.github.io/blog/quality/java-benchmark/
## 연결 노트

- [[JMH]]








