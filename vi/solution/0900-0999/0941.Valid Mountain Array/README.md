---
comments: true
difficulty: Easy
tags:
    - Array
---

<!-- problem:start -->

# [941. Valid Mountain Array](https://leetcode.com/problems/valid-mountain-array)

[中文文档](/solution/0900-0999/0941.Valid%20Mountain%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>arr</code>, trả về <em><code>true</code> khi và chỉ khi đây là mountain array hợp lệ</em>.</p>

<p>Nhắc lại, arr là mountain array khi và chỉ khi:</p>

<ul>
	<li><code>arr.length &gt;= 3</code></li>
	<li>Tồn tại <code>i</code> sao cho <code>0 &lt; i &lt; arr.length - 1</code> và:
	<ul>
		<li><code>arr[0] &lt; arr[1] &lt; ... &lt; arr[i - 1] &lt; arr[i] </code></li>
		<li><code>arr[i] &gt; arr[i + 1] &gt; ... &gt; arr[arr.length - 1]</code></li>
	</ul>
	</li>
</ul>
<img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0900-0999/0941.Valid%20Mountain%20Array/images/hint_valid_mountain_array.png" width="500" />
<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Input:</strong> arr = [2,1]
<strong>Output:</strong> false
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Input:</strong> arr = [3,5,5]
<strong>Output:</strong> false
</pre><p><strong class="example">Ví dụ 3:</strong></p>
<pre><strong>Input:</strong> arr = [0,3,2,1]
<strong>Output:</strong> true
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= arr[i] &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Two Pointers

<!-- thinking:start -->

> **Tư duy**
>
> Mountain array tăng nghiêm ngặt rồi giảm nghiêm ngặt, đồng thời có độ dài ít nhất $3$. Duyệt từ hai đầu vào trong cho đến khi dãy ngừng tăng hoặc giảm; hai con trỏ phải gặp nhau tại đỉnh, không được dừng ở đầu mút. Mỗi con trỏ chỉ đi qua mảng một lần.

<!-- thinking:end -->

Đầu tiên, kiểm tra độ dài mảng có nhỏ hơn $3$ không. Nếu có thì chắc chắn không phải mountain array, nên trả về `false` ngay.

Sau đó, dùng pointer $i$ duyệt từ trái sang phải đến khi tìm được vị trí $i$ sao cho $arr[i] > arr[i + 1]$. Tiếp theo, dùng pointer $j$ duyệt từ phải sang trái đến khi tìm được vị trí $j$ sao cho $arr[j] > arr[j - 1]$. Nếu $i = j$, mảng $arr$ là mountain array.

Độ phức tạp thời gian là $O(n)$, với $n$ là độ dài mảng. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def validMountainArray(self, arr: List[int]) -> bool:
        n = len(arr)
        if n < 3:
            return False
        i, j = 0, n - 1
        while i + 1 < n - 1 and arr[i] < arr[i + 1]:
            i += 1
        while j - 1 > 0 and arr[j - 1] > arr[j]:
            j -= 1
        return i == j
```

#### Java

```java
class Solution {
    public boolean validMountainArray(int[] arr) {
        int n = arr.length;
        if (n < 3) {
            return false;
        }
        int i = 0, j = n - 1;
        while (i + 1 < n - 1 && arr[i] < arr[i + 1]) {
            ++i;
        }
        while (j - 1 > 0 && arr[j - 1] > arr[j]) {
            --j;
        }
        return i == j;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool validMountainArray(vector<int>& arr) {
        int n = arr.size();
        if (n < 3) {
            return false;
        }
        int i = 0, j = n - 1;
        while (i + 1 < n - 1 && arr[i] < arr[i + 1]) {
            ++i;
        }
        while (j - 1 > 0 && arr[j - 1] > arr[j]) {
            --j;
        }
        return i == j;
    }
};
```

#### Go

```go
func validMountainArray(arr []int) bool {
	n := len(arr)
	if n < 3 {
		return false
	}
	i, j := 0, n-1
	for i+1 < n-1 && arr[i] < arr[i+1] {
		i++
	}
	for j-1 > 0 && arr[j-1] > arr[j] {
		j--
	}
	return i == j
}
```

#### TypeScript

```ts
function validMountainArray(arr: number[]): boolean {
    const n = arr.length;
    if (n < 3) {
        return false;
    }
    let [i, j] = [0, n - 1];
    while (i + 1 < n - 1 && arr[i] < arr[i + 1]) {
        i++;
    }
    while (j - 1 > 0 && arr[j] < arr[j - 1]) {
        j--;
    }
    return i === j;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
