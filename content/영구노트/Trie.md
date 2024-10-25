---
tags:
  - 완성
  - 알고리즘
  - Trie
  - 자료구조
aliases:
  - 트라이
  - radix
title: Trie
date: 2024-01-29
---
작성 날짜: 2024-01-29
작성 시간: 23:42


----
## 내용(Content)
### Trie
>[!summary] Trie
>문자열을 효과적으로 저장하고 효율적으로 탐색하기 위해 만들어진 트리 형태의 자료 구조


트라이는 트리 형태로 문자열들을 저장한다. 예를 들어 kor, korea, car 3개의 단어를 가지는 트라이 구조의 트리 생성 과정을 알아보자

**우선 첫번쨰로 kor를 넣어보자**
![[Trie kor step 1(draw).svg|400]]

head에서 부터 k, o, r 차례로 만들고 최종적으로 r 노드에서 단어가 만들어졌다고 표시한다.

**두번째로 korea를 넣어보자**
![[Trie korean step 2(draw).svg|400]]

세 번째로 car를 넣어보자
![[Trie car step 3(draw).svg|400]]

보면 동일한 prefix면 노드를 공유하게 된다. 만약 여기서 kos 라는 단어를 쓴다면 ko 까지는 같고 s만 추가되는 식일 것이다. 이러한 특징 때문에 접두어 트리라고도 하며, 해당 자료 구조에서 prefix나 단어 존재 여부를 찾을 때 효과적으로 찾을 수 있다.

prefix의 경우 prefix만큼만 탐색을 진행하며 word 존재 여부가 궁금하면 문자열 끝까지 탐색하고 중간에 노드가 없으면 안되며 있다면 위의 그림처럼 빨간 노드(is_word)인지 확인 작업만 하면 된다.
### Trie 특징
- 여러 문자열을 가지고 있는 경우 탐색이 빠르다.
	- 최소 문자열의 길이 L만큼만 탐색을 진행한다. $O(L)$
	- 트리 형태로 탐색하기 때문에 여러 문자열 비교가 필요가 없다.
- 각 글자를 노드 형태로 저장하기 떄문에 메모리를 많이 차지한다.

### 용도
Trie는 접두어 존재 여부를 판별할 때 굉장히 효율적으로 활용할 수 있다. 뿐만 아니라 단어의 존재 여부나, 순차적으로 단어를 분할하는 경우(접두어 느낌으로) 효과적일 수 있다.

>[!example] 순차적 단어 분할 예시
>leetscode란 단어가 있고, 적절한 분할을 통해서 해당 dictionary: [leet, code]가 있는지 찾아보는 작업을 진행한다고 가정하자
>
>이 경우 leet / s / code로 분할하면 될 것이다. 이 때 leet라는 덩어리가 dictionary에 있는지 확인하기 위해서는 l, le, lee, leet 이런식으로 점차 확장하며 탐색할 것이다. 근데 문제는 이 4개가 모두 prefix이며 이 경우 4가지 경우의 수에 대해 dictionary의 모든 원소에 대하여 startsWith를 수행하는 것 보다 Trie 자료 구조를 활용하면 효율적으로 문제를 풀 수 있다.

### 구현
```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_word = False


class Trie:
    def __init__(self):
        self.root = TrieNode()
    

    def insert(self, word):
        node = self.root
        for ch in word:
            if ch not in node.children:
                node.children[ch] = TrieNode()
            node = node.children[ch]
        node.is_word = True
    

    def search(self, word):
        node = self.root
        for ch in word:
            if ch not in node.children:
                return False
            node = node.children[ch]
        return node.is_word
    
    
    def startsWith(self, word):
        node = self.root
        for ch in word:
            if ch not in node.children:
                return False
            node = node.children[ch]
        return True
```

```java
class Trie {

    private final TrieNode root;

    public Trie() {
        this.root = new TrieNode();
    }
    
    public void insert(String word) {
        TrieNode node = root;
        for (char ch : word.toCharArray()) {
            int idx = ch - 'a';
            if (node.children[idx] == null) {
                node.children[idx] = new TrieNode();
            }
            node = node.children[idx];
        }
        node.is_word = true;
    }
    
    public boolean search(String word) {
        TrieNode node = root;
        for (char ch : word.toCharArray()) {
            int idx = ch - 'a';
            if (node.children[idx] == null) {
                return false;
            }
            node = node.children[idx];
        }
        return node.is_word;
    }
    
    public boolean startsWith(String prefix) {
        TrieNode node = root;
        for (char ch : prefix.toCharArray()) {
            int idx = ch - 'a';
            if (node.children[idx] == null) {
                return false;
            }
            node = node.children[idx];
        }
        return true;
    }

    private static class TrieNode {
        private final TrieNode[] children = new TrieNode[26];
        private boolean is_word;
    }
}
```

java의 경우 아스키 코드를 활용하는 것이 구현이 더 쉬워 Node children을 hashMap 대신 배열을 사용했다. 배열이 좀 더 작고 빠르긴 하다.

## 질문 & 확장

(없음)

## 출처(링크)
- https://leetcode.com/problems/implement-trie-prefix-tree/description/

## 연결 노트
- [[2707. Extra Characters in a String]]









