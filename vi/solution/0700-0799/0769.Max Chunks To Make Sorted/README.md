---
comments: true
difficulty: Medium
tags:
    - Stack
    - Greedy
    - Array
    - Sorting
    - Monotonic Stack
---

<!-- problem:start -->

# [769. Max Chunks To Make Sorted](https://leetcode.com/problems/max-chunks-to-make-sorted)

[中文文档](/solution/0700-0799/0769.Max%20Chunks%20To%20Make%20Sorted/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>arr</code> có độ dài <code>n</code>, là một hoán vị của các số nguyên trong phạm vi <code>[0, n - 1]</code>.</p>

<p>Ta chia <code>arr</code> thành một số <strong>chunk</strong> (tức các đoạn), rồi sắp xếp riêng từng chunk. Sau khi nối chúng lại, kết quả phải bằng mảng đã sắp xếp.</p>

<p>Trả về <em>số chunk lớn nhất có thể chia để sắp xếp mảng</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [4,3,2,1,0]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong>
Chia thành hai chunk trở lên sẽ không cho ra kết quả cần tìm.
Ví dụ, chia thành [4, 3], [2, 1, 0] sẽ cho kết quả [3, 4, 0, 1, 2], không phải mảng đã sắp xếp.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [1,0,2,3,4]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong>
Ta có thể chia thành hai chunk, chẳng hạn [1, 0], [2, 3, 4].
Tuy nhiên, chia thành [1, 0], [2], [3], [4] là cách tạo được nhiều chunk nhất.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == arr.length</code></li>
	<li><code>1 &lt;= n &lt;= 10</code></li>
	<li><code>0 &lt;= arr[i] &lt; n</code></li>
	<li>Các phần tử trong <code>arr</code> đều <strong>khác nhau</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy + Duyệt một lượt

<!-- thinking:start -->

> **Tư duy**
>
> Mảng là một hoán vị của $0..n-1$. Có thể cắt sau vị trí $i$ khi và chỉ khi prefix chứa đúng các giá trị $\{0,\ldots,i\}$.
>
> Điều này tương đương với việc giá trị lớn nhất trong prefix bằng $i$. Chỉ cần duyệt một lượt và đếm các vị trí thỏa mãn.

<!-- thinking:end -->

Vì $\textit{arr}$ là hoán vị của $[0,..,n-1]$, nếu giá trị lớn nhất $\textit{mx}$ trong các phần tử đã duyệt bằng chỉ số hiện tại $i$, ta có thể cắt tại đây và tăng đáp án lên 1.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(1)$, trong đó $n$ là độ dài của mảng $\textit{arr}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxChunksToSorted(self, arr: List[int]) -> int:
        mx = ans = 0
        for i, v in enumerate(arr):
            mx = max(mx, v)
            if i == mx:
                ans += 1
        return ans
```

#### Java

```java
class Solution {
    public int maxChunksToSorted(int[] arr) {
        int ans = 0, mx = 0;
        for (int i = 0; i < arr.length; ++i) {
            mx = Math.max(mx, arr[i]);
            if (i == mx) {
                ++ans;
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxChunksToSorted(vector<int>& arr) {
        int ans = 0, mx = 0;
        for (int i = 0; i < arr.size(); ++i) {
            mx = max(mx, arr[i]);
            ans += i == mx;
        }
        return ans;
    }
};
```

#### Go

```go
func maxChunksToSorted(arr []int) int {
	ans, mx := 0, 0
	for i, v := range arr {
		mx = max(mx, v)
		if i == mx {
			ans++
		}
	}
	return ans
}
```

#### TypeScript

```ts
function maxChunksToSorted(arr: number[]): number {
    const n = arr.length;
    let ans = 0;
    let mx = 0;
    for (let i = 0; i < n; i++) {
        mx = Math.max(arr[i], mx);
        if (mx == i) {
            ans++;
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_chunks_to_sorted(arr: Vec<i32>) -> i32 {
        let mut ans = 0;
        let mut mx = 0;
        for i in 0..arr.len() {
            mx = mx.max(arr[i]);
            if mx == (i as i32) {
                ans += 1;
            }
        }
        ans
    }
}
```

#### C

```c
#define max(a, b) (((a) > (b)) ? (a) : (b))

int maxChunksToSorted(int* arr, int arrSize) {
    int ans = 0;
    int mx = -1;
    for (int i = 0; i < arrSize; i++) {
        mx = max(mx, arr[i]);
        if (mx == i) {
            ans++;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Monotonic Stack

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 dựa trên điều kiện $value=index$, nên không áp dụng được khi có phần tử trùng lặp. Monotonic stack lưu giá trị lớn nhất của các chunk tương tự bài 768.
>
> Khi gặp giá trị mới nhỏ hơn, ta gộp các chunk trước đó; độ dài stack chính là đáp án.

<!-- thinking:end -->

Lời giải thứ nhất có một số hạn chế: nếu mảng có phần tử trùng lặp thì không thể tìm được đáp án chính xác bằng cách đó.

Theo đề bài, khi duyệt từ trái sang phải, mỗi chunk có một giá trị lớn nhất và các giá trị lớn nhất này tăng đơn điệu. Ta có thể dùng stack để lưu giá trị lớn nhất của từng chunk. Kích thước stack cuối cùng là số chunk tối đa có thể sắp xếp.

Cách này không chỉ giải được bài toán này mà còn áp dụng cho bài 768. Max Chunks To Make Sorted II. Bạn có thể tự thử.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{arr}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxChunksToSorted(self, arr: List[int]) -> int:
        stk = []
        for v in arr:
            if not stk or v >= stk[-1]:
                stk.append(v)
            else:
                mx = stk.pop()
                while stk and stk[-1] > v:
                    stk.pop()
                stk.append(mx)
        return len(stk)
```

#### Java

```java
class Solution {
    public int maxChunksToSorted(int[] arr) {
        Deque<Integer> stk = new ArrayDeque<>();
        for (int v : arr) {
            if (stk.isEmpty() || v >= stk.peek()) {
                stk.push(v);
            } else {
                int mx = stk.pop();
                while (!stk.isEmpty() && stk.peek() > v) {
                    stk.pop();
                }
                stk.push(mx);
            }
        }
        return stk.size();
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maxChunksToSorted(vector<int>& arr) {
        stack<int> stk;
        for (int v : arr) {
            if (stk.empty() || v >= stk.top()) {
                stk.push(v);
            } else {
                int mx = stk.top();
                stk.pop();
                while (!stk.empty() && stk.top() > v) {
                    stk.pop();
                }
                stk.push(mx);
            }
        }
        return stk.size();
    }
};
```

#### Go

```go
func maxChunksToSorted(arr []int) int {
	stk := []int{}
	for _, v := range arr {
		if len(stk) == 0 || v >= stk[len(stk)-1] {
			stk = append(stk, v)
		} else {
			mx := stk[len(stk)-1]
			stk = stk[:len(stk)-1]
			for len(stk) > 0 && stk[len(stk)-1] > v {
				stk = stk[:len(stk)-1]
			}
			stk = append(stk, mx)
		}
	}
	return len(stk)
}
```

#### TypeScript

```ts
function maxChunksToSorted(arr: number[]): number {
    const stk: number[] = [];

    for (const x of arr) {
        if (stk.at(-1)! > x) {
            const top = stk.pop()!;
            while (stk.length && stk.at(-1)! > x) stk.pop();
            stk.push(top);
        } else stk.push(x);
    }

    return stk.length;
}
```

#### JavaScript

```js
function maxChunksToSorted(arr) {
    const stk = [];

    for (const x of arr) {
        if (stk.at(-1) > x) {
            const top = stk.pop();
            while (stk.length && stk.at(-1) > x) stk.pop();
            stk.push(top);
        } else stk.push(x);
    }

    return stk.length;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
