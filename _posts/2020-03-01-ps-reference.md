---
title: 모르면 못푸는 PS용 C++ 레퍼런스
date: 2020-03-01
tags:
  - Algorithmn
  - PS
  - STL
keywords:
  - Algorithmn
  - PS
  - STL
---

# Headers
### bit/stdc++.h

```cpp
#include <bit/stdc++.h>
```
<details>
<summary>코드 보기</summary>
<div markdown="1">

```cpp
// cpp includes used for precompiling -*- cpp -*-
 
// Copyright (C) 2003-2013 Free Software Foundation, Inc.
//
// This file is part of the GNU ISO cpp Library.  This library is free
// software; you can redistribute it and/or modify it under the
// terms of the GNU General Public License as published by the
// Free Software Foundation; either version 3, or (at your option)
// any later version.
 
// This library is distributed in the hope that it will be useful,
// but WITHOUT ANY WARRANTY; without even the implied warranty of
// MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
// GNU General Public License for more details.
 
// Under Section 7 of GPL version 3, you are granted additional
// permissions described in the GCC Runtime Library Exception, version
// 3.1, as published by the Free Software Foundation.
 
// You should have received a copy of the GNU General Public License and
// a copy of the GCC Runtime Library Exception along with this program;
// see the files COPYING3 and COPYING.RUNTIME respectively.  If not, see
// <http://www.gnu.org/licenses/>.
 
/** @file stdc++.h
 *  This is an implementation file for a precompiled header.
 */
 
// 17.4.1.2 Headers
 
// C
#ifndef _GLIBCXX_NO_ASSERT
#include <cassert>
#endif
#include <cctype>
#include <cerrno>
#include <cfloat>
#include <ciso646>
#include <climits>
#include <clocale>
#include <cmath>
#include <csetjmp>
#include <csignal>
#include <cstdarg>
#include <cstddef>
#include <cstdio>
#include <cstdlib>
#include <cstring>
#include <ctime>
 
#if __cplusplus >= 201103L
#include <ccomplex>
#include <cfenv>
#include <cinttypes>
#include <cstdalign>
#include <cstdbool>
#include <cstdint>
#include <ctgmath>
#include <cwchar>
#include <cwctype>
#endif
 
// cpp
#include <algorithm>
#include <bitset>
#include <complex>
#include <deque>
#include <exception>
#include <fstream>
#include <functional>
#include <iomanip>
#include <ios>
#include <iosfwd>
#include <iostream>
#include <istream>
#include <iterator>
#include <limits>
#include <list>
#include <locale>
#include <map>
#include <memory>
#include <new>
#include <numeric>
#include <ostream>
#include <queue>
#include <set>
#include <sstream>
#include <stack>
#include <stdexcept>
#include <streambuf>
#include <string>
#include <typeinfo>
#include <utility>
#include <valarray>
#include <vector>
 
#if __cplusplus >= 201103L
#include <array>
#include <atomic>
#include <chrono>
#include <condition_variable>
#include <forward_list>
#include <future>
#include <initializer_list>
#include <mutex>
#include <random>
#include <ratio>
#include <regex>
#include <scoped_allocator>
#include <system_error>
#include <thread>
#include <tuple>
#include <typeindex>
#include <type_traits>
#include <unordered_map>
#include <unordered_set>
#endif
```

</div>
</details>


### ios
```cpp
#include <ios>
#import <ios>
```
* 숏코딩용. std I/O 정도만 포함.


# Functions
## 원소를 수정하는 함수
### sort

```cpp
#include <algorithm>
void sort(begin, end, [comparator]);
```
* 컨테이너 오름차순 정렬.
* O(NlogN)

```cpp
sort(vec.begin(), vec.end(), [](const &T a, const &T b){
    return a < b;
});
```
* 람다식을 이용한 사용자 정의 Comparator 사용.
* *`comp(a, b) == comp(b, a)`이면 에러 발생, a == b일때는 임의의 순서 배정 필요*

```cpp
#include <functional>
sort(vec.begin(), vec.end(), greater<T>());
```
fuctional 헤더의 greater Comparator를 사용해 **내림차순 정렬**.

```cpp
bool operator< (const &T a, const &T b) {
    return a < b;
}

sort(vec.begin(), vec.end());
```
* 사용자 정의 `<`연산자를 사용.
* *`sort`는 순서 비교에 `<`만 사용*


### reverse

```cpp
#include <algorithm>
void reverse(begin, end);
```
* 컨테이너의 순서 역전
* O(N)

```cpp(
reverse(vec.begin(), vec.end());
```


### unique

```cpp
#include <algorithm>
T *iter = unique(begin, end);
```
* 컨테이너의 중복 요소를 제거. 제거하고 남은 공간은 쓰레기값으로 남고, 쓰레기값의 시작점 이터레이터를 반환.
* 정렬과 무관하게 연달아 나온 두 원소 중 하나를 제거.
* O(N)

```cpp
vec.erase(unique(vec.begin(), vec.end()), vec.end());
```
* 컨테이너에서 중복 요소를 제거하고, 남은 쓰레기값을 삭제.


### merge
> *I have never tasted `merge`.*


### fill, fill_n

```cpp
#include <algorithm>
void fill(begin, end, value);
void fill_n(begin, size, value);
```
* 컨테이너의 모든 값에 대입. **다차원 배열 불가능**
* O(N)

```cpp
fill(vec.begin(), vec.end(), 375);
fill_n(vec.begin(), vec.size(), 375);
```


### memset

```cpp
#include <cstring>
void memset(begin, value, size);
```
메* 모리에 값을 셋팅. **다차원 배열 가능**

```cpp
memset(arr, 0, size(arr));
```
* 배열의 모든 값을 0으로 맵핑. 

```cpp
memset(arr, -1, size(arr));
```
* 배열의 모든 값을 0(0xFF)으로 맵핑.


## 탐색 관련 함수
### find (algorithm)

```cpp
#include <algorithm>
T *iter = find(begin, end, value);
```
* 컨테이너 안에서 값을 찾고 그 이터레이터를 리턴. **없으면 last를 리턴**.
* O(N)

```cpp
int x = *find(vec.begin(), vec.end(), 523);
```


### find (string)
```cpp
#include <string>
size_t pos = str.find(value, [start]);
```
* 문자열 안에서 값을 찾고 그 위치를 리턴. **없으면 string::npos(-1)를 리턴**.
* O(N)

```cpp
int p = str.find('a');
```
* 단일 문자 `'a'`를 찾고 그 위치를 리턴.

```cpp
int p = str.find("aa");
```
* 문자열 `"aa"`를 찾고 그 위치를 리턴.

```cpp
int p = str.find("aa", start);
```
* start번째 문자부터 문자열 `"aa"`를 찾고 그 위치를 리턴.


### binary_search

```cpp
#include <algorithm>
bool include = binary_search(begin, end, value, [comparator]);
```
* 정렬된 컨테이너에서 값을 탐색하고 찾았는지를 반환.
* Comparator는 [`sort`](#sort)와 동일.
* O(logN)

```cpp
if (binary_search(vec.begin(), vec.end(), 618))
  cout << "Find!";
```


### lower_bound

```cpp
#include <algorithm>
T *iter = lower_bound(begin, end, value, [comparator]);
```
* 정렬된 컨테이너에서 이진탐색을 하고 찾은 원소의 이터레이터를 반환, 못 찾으면 value보다 큰 값(다음 위치)의 이터레이터를 반환.
* $$[0, pivot]$$ 에서 가장 작은 값의 이터레이터.
* Comparator는 [`sort`](#sort)와 동일.
* O(logN)


### upper_bound

```cpp
#include <algorithm>
T *iter = upper_bound(begin, end, value, [comparator]);
```
* 정렬된 컨테이너에서 이진탐색을 하고 value를 초과하는 값의 이터레이터를 반환.
* $$(pivot, N]$$ 에서 가장 작은 값의 이터레이터.
* Comparator는 [`sort`](#sort)와 동일.
* O(logN)


### equal_range

```cpp
#include <algorithm>
pair<T*, T*> iterPair = equal_range(begin, end, value, [comparator]);
```
* 정렬된 컨테이너에서 이진탐색을 하고 lower_bound와 upper_bound를 pair로 반환.
* $$[lower\_bound, upper\_bound)$$ 범위 이터레이터 리턴.
* Comparator는 [`sort`](#sort)와 동일.
* O(logN)

```cpp
auto p = equal_range(vec.begin(), vec.end(), 762);
cout << distance(p.first, p.second); // == (p.second - p.first)
```
이진탐색을 하고 value와 일치하는 원소의 갯수를 출력. `count(LB ≤ value < RB)`


## 수학 관련 함수
### min, max

```cpp
#include <algorithm>
T min(a, b, [comparator]);
T min(init_list, [comparator]);
T max(a, b, [comparator]);
T max(init_list, [comparator]);
```
* 매개변수의 최솟값이나 최댓값을 반환.
* Comparator는 [`sort`](#sort)와 동일.

```cpp
int mn = min({ 3, 4, 5 });
int mx = max({ 3, 4, 5 });
```
`initializer_list`를 사용하여 최솟값/최댓값 반환.


### min&#95;element, max&#95;element

```cpp
#include <algorithm>
T *iter = min_element(a, b, [comparator]);
T *iter = min_element(init_list, [comparator]);
T *iter = max_element(a, b, [comparator]);
T *iter = max_element(init_list, [comparator]);
```
* 매개변수의 최솟값이나 최댓값의 이터레이터를 반환.
* Comparator는 [`sort`](#sort)와 동일.

```cpp
int mn = *min_element({ 3, 4, 5 });
int mx = *max_element({ 3, 4, 5 });
```
`initializer_list`를 사용하여 최솟값/최댓값의 이터레이터를 반환.


### ceil, round, floor

```cpp
#include <cmath>
T ceil(value);
T round(value);
T floor(value);
```
* `ceil`: value보다 큰 가장 가까운 정수로 올림
* `round`: value와 가장 가까운 정수로 반올림
* `floor`: value보다 작은 가장 가까운 정수로 내림

```cpp
int H = (int)ceil(log2(N));
int tree_size = 1 << (H+1);
```
`ceil`를 사용하여 트리의 노드 수를 구함.


### next_permutation

```cpp
#include <algorithm>
bool next_permutation(begin, end, [comparator]);
```
* 컨테이너 순열의 다음 번째 순열로 변환, 더 이상 진행할 수 없으면(내림차순 순열이면) `false` 반환.
* 2종류의 원소만 사용한 배열을 사용하면 조합을 구할 수도 있다.
* Comparator는 [`sort`](#sort)와 동일.

```cpp
do {
  // ...
} while (next_permutation(vec.begin(), vec.end()));
```

```cpp
// 5C2 구하기
vector<int> vec(5);
vector<bool> filter = { false, false, false, true, true };
do {
  for (int i = 0; i < 5; ++i) {
    if (filter[i]) {
      // vec[i] is selected.
    }
  }
} while (next_permutation(filter.begin(), filter.end()));
```
중복되는 원소를 가진 `filter`의 순열을 구하고, 여기에 대응되는 `vec`의 원소를 선택해서 중복을 허용한다.


### prev_permutation

```cpp
#include <algorithm>
bool prev_permutation(begin, end, [comparator]);
```
* 컨테이너 순열의 이전 번째 순열로 변환, 더 이상 진행할 수 없으면(오름차순 순열이면) `false` 반환.
* 2종류의 원소만 사용한 배열을 사용하면 조합을 구할 수도 있다.
* Comparator는 [`sort`](#sort)와 동일.

```cpp
do {
  // ...
} while (prev_permutation(vec.begin(), vec.end()));
```

```cpp
// 5C2 구하기
vector<int> vec(5);
vector<bool> filter = { false, false, false, true, true };
do {
  for (int i = 0; i < 5; ++i) {
    if (filter[i]) {
      // vec[i] is selected.
    }
  }
} while (prev_permutation(filter.begin(), filter.end()));
```
중복되는 원소를 가진 `filter`의 순열을 구하고, 여기에 대응되는 `vec`의 원소를 선택해서 중복을 허용한다.


### &#95;&#95;builtin&#95;popcount

```cpp
unsigned int __builtin_popcount(uint32 value);
```
* 32bit unsigned int에 대하여, 1인 bit의 수를 반환.
* ⚠️ **GCC환경에서만 동작.** ⚠️
* bitmasking에 대한 더 자세한 내용은 다른 포스트로 추가 예정.
<!--
더 공부해보고 정리해서 추가하기
[Count bits 1 on an integer as fast as GCC __builtin__popcount(int) - Stack Overflow](https://stackoverflow.com/questions/51387998/count-bits-1-on-an-integer-as-fast-as-gcc-builtin-popcountint)
[마크다운으로 주석 처리 가능](http://www.secmem.org/blog/2019/10/19/handy-function-about-bit/)
[비트마스크 사용법](https://sunnyholic.com/88)
-->


---

References
* http://www.cplusplus.com/
* https://en.cppreference.com/