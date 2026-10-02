---
comments: true
difficulty: Easy
rating: 1151
source: Biweekly Contest 27 Q1
tags:
    - Array
    - Hash Table
    - Sorting
---

<!-- problem:start -->

# [1460. Make Two Arrays Equal by Reversing Subarrays](https://leetcode.com/problems/make-two-arrays-equal-by-reversing-subarrays)

[中文文档](/solution/1400-1499/1460.Make%20Two%20Arrays%20Equal%20by%20Reversing%20Subarrays/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng số nguyên <code>target</code> và <code>arr</code> có cùng độ dài. Trong một bước, bạn có thể chọn bất kỳ <strong>mảng con không rỗng</strong> nào của <code>arr</code> và đảo ngược nó. Bạn được phép thực hiện bao nhiêu bước tùy ý.</p>

<p>Trả về <code>true</code> <em>nếu bạn có thể biến </em><code>arr</code><em> thành </em><code>target</code><em>&nbsp;hoặc trả về </em><code>false</code><em> trong trường hợp ngược lại</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> target = [1,2,3,4], arr = [2,4,1,3]
<strong>Output:</strong> true
<strong>Explanation:</strong> Có thể thực hiện các bước sau để biến arr thành target:
1- Đảo ngược mảng con [2,4,1], arr trở thành [1,4,2,3]
2- Đảo ngược mảng con [4,2], arr trở thành [1,2,4,3]
3- Đảo ngược mảng con [4,3], arr trở thành [1,2,3,4]
Có nhiều cách để biến arr thành target, đây không phải cách duy nhất.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> target = [7], arr = [7]
<strong>Output:</strong> true
<strong>Explanation:</strong> arr đã bằng target mà không cần đảo ngược.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> target = [3,7,9], arr = [3,7,11]
<strong>Output:</strong> false
<strong>Explanation:</strong> arr không chứa giá trị 9 nên không thể biến đổi thành target.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>target.length == arr.length</code></li>
	<li><code>1 &lt;= target.length &lt;= 1000</code></li>
	<li><code>1 &lt;= target[i] &lt;= 1000</code></li>
	<li><code>1 &lt;= arr[i] &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Việc đảo ngược các mảng con có thể tạo ra mọi hoán vị. Vì $n\le 1000$, hai mảng có thể được đưa về giống nhau khi và chỉ khi chúng giống nhau sau khi sắp xếp.

<!-- thinking:end -->

Nếu hai mảng bằng nhau sau khi sắp xếp, thì có thể biến chúng thành cùng một mảng bằng cách đảo ngược các mảng con.

Do đó, chúng ta chỉ cần sắp xếp hai mảng rồi kiểm tra xem hai mảng đã sắp xếp có bằng nhau hay không.

Độ phức tạp thời gian là $O(n \times \log n)$, còn độ phức tạp không gian là $O(\log n)$, trong đó $n$ là độ dài của mảng `arr`.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canBeEqual(self, target: List[int], arr: List[int]) -> bool:
        return sorted(target) == sorted(arr)
```

#### Java

```java
class Solution {
    public boolean canBeEqual(int[] target, int[] arr) {
        Arrays.sort(target);
        Arrays.sort(arr);
        return Arrays.equals(target, arr);
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool canBeEqual(vector<int>& target, vector<int>& arr) {
        sort(target.begin(), target.end());
        sort(arr.begin(), arr.end());
        return target == arr;
    }
};
```

#### Go

```go
func canBeEqual(target []int, arr []int) bool {
	sort.Ints(target)
	sort.Ints(arr)
	return reflect.DeepEqual(target, arr)
}
```

#### TypeScript

```ts
function canBeEqual(target: number[], arr: number[]): boolean {
    target.sort();
    arr.sort();
    return target.every((x, i) => x === arr[i]);
}
```

#### JavaScript

```js
function canBeEqual(target, arr) {
    target.sort();
    arr.sort();
    return target.every((x, i) => x === arr[i]);
}
```

#### Rust

```rust
impl Solution {
    pub fn can_be_equal(mut target: Vec<i32>, mut arr: Vec<i32>) -> bool {
        target.sort();
        arr.sort();
        target == arr
    }
}
```

#### PHP

```php
class Solution {
    /**
     * @param Integer[] $target
     * @param Integer[] $arr
     * @return Boolean
     */
    function canBeEqual($target, $arr) {
        sort($target);
        sort($arr);
        return $target === $arr;
    }
}
```

#### C

```c
int compare(const void* a, const void* b) {
    return (*(int*) a - *(int*) b);
}

bool canBeEqual(int* target, int targetSize, int* arr, int arrSize) {
    qsort(target, targetSize, sizeof(int), compare);
    qsort(arr, arrSize, sizeof(int), compare);
    for (int i = 0; i < targetSize; ++i) {
        if (target[i] != arr[i]) {
            return false;
        }
    }
    return true;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Đếm

<!-- thinking:start -->

> **Tư duy**
>
> Phương pháp 1 sắp xếp các mảng. Các giá trị nằm trong khoảng từ $1$ đến $1000$, vì vậy chỉ cần so sánh tần suất xuất hiện là đủ và thời gian chạy là tuyến tính.

<!-- thinking:end -->

Chúng ta nhận thấy miền giá trị của các phần tử mảng trong đề bài là $1 \sim 1000$. Do đó, chúng ta có thể dùng hai mảng `cnt1` và `cnt2` có độ dài $1001$ để ghi lại số lần mỗi phần tử xuất hiện tương ứng trong các mảng `target` và `arr`. Cuối cùng, chúng ta chỉ cần kiểm tra xem hai mảng này có bằng nhau hay không.

Chúng ta cũng có thể chỉ dùng một mảng `cnt`. Duyệt qua hai mảng `target` và `arr`. Với `target[i]`, tăng `cnt[target[i]]`, còn với `arr[i]`, giảm `cnt[arr[i]]`. Cuối cùng, kiểm tra xem tất cả phần tử trong mảng `cnt` có bằng $0$ hay không.

Độ phức tạp thời gian là $O(n + M)$, còn độ phức tạp không gian là $O(M)$. Trong đó, $n$ là độ dài của mảng `arr`, còn $M$ là miền giá trị của các phần tử mảng. Trong bài này, $M = 1001$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canBeEqual(self, target: List[int], arr: List[int]) -> bool:
        return Counter(target) == Counter(arr)
```

#### Java

```java
class Solution {
    public boolean canBeEqual(int[] target, int[] arr) {
        int[] cnt1 = new int[1001];
        int[] cnt2 = new int[1001];
        for (int v : target) {
            ++cnt1[v];
        }
        for (int v : arr) {
            ++cnt2[v];
        }
        return Arrays.equals(cnt1, cnt2);
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool canBeEqual(vector<int>& target, vector<int>& arr) {
        vector<int> cnt1(1001);
        vector<int> cnt2(1001);
        for (int& v : target) {
            ++cnt1[v];
        }
        for (int& v : arr) {
            ++cnt2[v];
        }
        return cnt1 == cnt2;
    }
};
```

#### Go

```go
func canBeEqual(target []int, arr []int) bool {
	cnt1 := make([]int, 1001)
	cnt2 := make([]int, 1001)
	for _, v := range target {
		cnt1[v]++
	}
	for _, v := range arr {
		cnt2[v]++
	}
	return reflect.DeepEqual(cnt1, cnt2)
}
```

#### TypeScript

```ts
function canBeEqual(target: number[], arr: number[]): boolean {
    const n = target.length;
    const cnt = Array(1001).fill(0);
    for (let i = 0; i < n; i++) {
        cnt[target[i]]++;
        cnt[arr[i]]--;
    }
    return cnt.every(v => !v);
}
```

#### JavaScript

```js
function canBeEqual(target, arr) {
    const n = target.length;
    const cnt = Array(1001).fill(0);
    for (let i = 0; i < n; i++) {
        cnt[target[i]]++;
        cnt[arr[i]]--;
    }
    return cnt.every(v => !v);
}
```

#### Rust

```rust
impl Solution {
    pub fn can_be_equal(mut target: Vec<i32>, mut arr: Vec<i32>) -> bool {
        let n = target.len();
        let mut cnt = [0; 1001];
        for i in 0..n {
            cnt[target[i] as usize] += 1;
            cnt[arr[i] as usize] -= 1;
        }
        cnt.iter().all(|v| *v == 0)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
