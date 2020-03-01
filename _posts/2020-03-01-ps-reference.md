---
title: 모르면 못푸는 PS용 C++ 레퍼런스
date: 2020-03-01
tags:
  - Algorithmn
  - PS
  - STL
---

# Headers
### bit/stdc++.h

```C++
#include <bit/stdc++.h>
```
<details>
<summary>코드 보기</summary>
<div markdown="1">

```C++
// C++ includes used for precompiling -*- C++ -*-
 
// Copyright (C) 2003-2013 Free Software Foundation, Inc.
//
// This file is part of the GNU ISO C++ Library.  This library is free
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
 
// C++
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
```C++
#include <ios>
#import <ios>
```
숏코딩용. std I/O 만 포함.


# Functions
## 원소를 수정하는 함수
### sort

```C++
#include <algorithm>
void sort(begin, end, [comparator]);
```
컨테이너 오름차순 정렬. <br/>
O(NlogN)

```C++
sort(vec.begin(), vec.end(), [](const &T a, const &T b){
    return a < b;
});
```
람다식을 이용한 사용자 정의 Comparator 사용 <br/>
*`comp(a, b) == comp(b, a)`이면 에러 발생, a == b일때는 임의의 순서 배정 필요*

```C++
#include <functional>
sort(vec.begin(), vec.end(), greater<T>());
```
fuctional 헤더의 greater Comparator를 사용해 **내림차순 정렬**.

```C++
bool operator< (const &T a, const &T b) {
    return a < b;
}

sort(vec.begin(), vec.end());
```
사용자 정의 `<`연산자를 사용. <br/>
*`sort`는 순서 비교에 `<`만 사용*


### reverse

```C++
#include <algorithm>
void reverse(begin, end);
```
컨테이너의 순서 역전 <br/>
O(N)

```C++(
reverse(vec.begin(), vec.end());
```

### unique


### merge


### memset

```C++
#include <cstring>
void memset(begin, value, size);
```
메모리에 값을 셋팅. **다차원 배열 가능**

```C++
memset(arr, 0, size(arr));
```
배열의 모든 값을 0으로 맵핑. 

```C++
memset(arr, -1, size(arr));
```
배열의 모든 값을 0(0xFF)으로 맵핑.


### fill, fill_n

```C++
#include <algorithm>
void fill(begin, end, value);
void fill_n(begin, size, value);
```
컨테이너의 모든 값에 대입. **다차원 배열 불가능** <br/>
O(N)

```C++
fill(vec.begin(), vec.end(), 375);
fill_n(vec.begin(), vec.size(), 375);
```

## 탐색 관련 함수
### find (algorithm)

```C++
#include <algorithm>
T *iter = find(begin, end, value);
```
컨테이너 안에서 값을 찾고 그 이터레이터를 리턴. **없으면 last를 리턴**. <br/>
O(N)

```C++
int x = *find(vec.begin(), vec.end(), 523);
```


### find (string)
```C++
#include <string>
size_t pos = str.find(value, [start]);
```
문자열 안에서 값을 찾고 그 위치를 리턴. **없으면 string::npos(-1)를 리턴**. <br/>
O(N)

```C++
int p = str.find('a');
```
단일 문자 `'a'`를 찾고 그 위치를 리턴.

```C++
int p = str.find("aa");
```
문자열 `"aa"`를 찾고 그 위치를 리턴.

```C++
int p = str.find("aa", start);
```
start번째 문자부터 문자열 `"aa"`를 찾고 그 위치를 리턴.


### binary_search


### upper_bound


### lower_bound


### equal_range


## 수학 관련 함수
### min, max


### min_ element, max_ element


### next_permutation


### prev_permutation


### make_heap


### sort_heap


### push_heap


### pop_heap


### __builtin _popcount