---
comments: true
difficulty: Hard
tags:
    - Stack
    - Greedy
    - Array
    - Sorting
    - Monotonic Stack
---

<!-- problem:start -->

# [768. Max Chunks To Make Sorted II](https://leetcode.com/problems/max-chunks-to-make-sorted-ii)

[中文文档](/solution/0700-0799/0768.Max%20Chunks%20To%20Make%20Sorted%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>arr</code>.</p>

<p>Ta chia <code>arr</code> thành một số <strong>chunk</strong> (tức các đoạn), rồi sắp xếp riêng từng chunk. Sau khi nối chúng lại, kết quả phải bằng mảng đã sắp xếp.</p>

<p>Hãy trả về <em>số chunk lớn nhất có thể tạo ra để sắp xếp mảng</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [5,4,3,2,1]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong>
Chia thành hai chunk trở lên sẽ không tạo ra kết quả cần tìm.
Ví dụ, chia thành [5, 4], [3, 2, 1] sẽ tạo ra [4, 5, 1, 2, 3], vốn chưa được sắp xếp.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [2,1,3,4,4]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong>
Ta có thể chia thành hai chunk, chẳng hạn [2, 1], [3, 4, 4].
Tuy nhiên, chia thành [2, 1], [3], [4], [4] là cách tạo được nhiều chunk nhất.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length &lt;= 2000</code></li>
	<li><code>0 &lt;= arr[i] &lt;= 10<sup>8</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Monotonic stack

<!-- thinking:start -->

> **Tư duy**
>
> Chia một mảng (có thể chứa phần tử trùng lặp) thành nhiều chunk nhất sao cho sau khi sắp xếp riêng từng chunk, ta thu được mảng đã sắp xếp. $n\le 2000$.
>
> Giá trị lớn nhất của các chunk phải không giảm. Nếu giá trị mới nhỏ hơn đỉnh stack, ta cần gộp các chunk trước đó: pop khi đỉnh lớn hơn giá trị mới, rồi push lại giá trị lớn nhất cũ.
>
> Mỗi giá trị còn lại trong stack tương ứng với một chunk; độ dài stack là đáp án.

<!-- thinking:end -->

Theo đề bài, khi duyệt từ trái sang phải, mỗi chunk có một giá trị lớn nhất và các giá trị lớn nhất này tăng đơn điệu (không giảm). Ta có thể dùng stack để lưu các giá trị lớn nhất của từng chunk. Kích thước stack cuối cùng là số chunk tối đa có thể sắp xếp.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài của $\textit{arr}$.

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
            if (stk.isEmpty() || stk.peek() <= v) {
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
        for (int& v : arr) {
            if (stk.empty() || stk.top() <= v)
                stk.push(v);
            else {
                int mx = stk.top();
                stk.pop();
                while (!stk.empty() && stk.top() > v) stk.pop();
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
	var stk []int
	for _, v := range arr {
		if len(stk) == 0 || stk[len(stk)-1] <= v {
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
    for (let v of arr) {
        if (stk.length === 0 || v >= stk[stk.length - 1]) {
            stk.push(v);
        } else {
            let mx = stk.pop()!;
            while (stk.length > 0 && stk[stk.length - 1] > v) {
                stk.pop();
            }
            stk.push(mx);
        }
    }
    return stk.length;
}
```

#### Rust

```rust
impl Solution {
    pub fn max_chunks_to_sorted(arr: Vec<i32>) -> i32 {
        let mut stk = Vec::new();
        for &v in arr.iter() {
            if stk.is_empty() || v >= *stk.last().unwrap() {
                stk.push(v);
            } else {
                let mut mx = stk.pop().unwrap();
                while let Some(&top) = stk.last() {
                    if top > v {
                        stk.pop();
                    } else {
                        break;
                    }
                }
                stk.push(mx);
            }
        }
        stk.len() as i32
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Prefix maximum và suffix minimum

<!-- thinking:start -->

> **Tư duy**
>
> Cách dùng stack chưa thể hiện rõ điều kiện chia. Chỉ số $i$ là điểm chia khi và chỉ khi $\max(arr[:i])\le\min(arr[i:])$.
>
> Ta tính prefix maximum và duyệt suffix minimum từ phải sang trái để kiểm tra bất đẳng thức này. Ban đầu, xem toàn bộ mảng là một chunk.

<!-- thinking:end -->

Ta muốn chia mảng độ dài $n$ thành nhiều chunk sao cho sau khi sắp xếp riêng từng chunk, toàn bộ mảng vẫn được sắp xếp.

Xét hai chunk liền kề:

- chunk bên trái: `left_chunk`
- chunk bên phải: `right_chunk`

Nếu điều kiện sau đúng:

`max(left_chunk)` <= `min(right_chunk)`

điều đó có nghĩa là:

- mọi phần tử trong chunk bên trái đều nhỏ hơn hoặc bằng mọi phần tử trong chunk bên phải
- do đó, sau khi sắp xếp riêng hai chunk, ta vẫn có thể nối chúng để được một mảng đã sắp xếp toàn cục

Vì vậy, với mỗi chỉ số $i$ thỏa mãn:

$$
1 \le i < n
$$

ta kiểm tra xem điều kiện sau có đúng không:

$$
\max(arr[:i]) \le \min(arr[i:])
$$



Nếu đúng, chỉ số $i$ có thể là một điểm chia hợp lệ.

---

Để kiểm tra điều kiện trên hiệu quả, ta tiền xử lý:

- `prefix_maxs[j]`

biểu diễn:

$$
\max(arr[:j + 1])
$$

tức là prefix maximum.

- `suffix_min[j]`

biểu diễn:

$$
\min(arr[j:])
$$

tức là suffix minimum.

Tiếp theo:

1. Duyệt mảng từ trái sang phải để tính mọi prefix maximum
2. Duyệt mảng từ phải sang trái để tính mọi suffix minimum
3. Với mỗi chỉ số $i$, kiểm tra xem điều kiện sau có đúng không:

`prefix_maxs[i - 1]` <= `suffix_min[i]`



Nếu đúng thì:

- mọi phần tử bên trái đều nhỏ hơn hoặc bằng mọi phần tử bên phải
- do đó, có thể chia mảng tại chỉ số $i$

Cuối cùng, đếm tất cả các điểm chia hợp lệ.

Lưu ý:

Ngay cả khi mảng giảm nghiêm ngặt, toàn bộ mảng vẫn có thể được xem là một chunk hợp lệ.

Vì vậy, đáp án cuối cùng bằng số điểm chia hợp lệ cộng $1$.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của $\textit{arr}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxChunksToSorted(self, arr: list[int]) -> int:
        prefix_maxs = []  # Max of each arr[:i + 1] where 0 <= i < len(arr).

        for num in arr:
            if not prefix_maxs:
                prefix_maxs.append(num)
                continue

            prefix_maxs.append(max(num, prefix_maxs[-1]))

        max_chunks = 1  # Base case.
        suffix_min = arr[-1]  # Min of arr[i:] where 0 <= i < len(arr).

        for idx in range(len(arr) - 1, 0, -1):
            if arr[idx] < suffix_min:
                suffix_min = arr[idx]

            if prefix_maxs[idx - 1] <= suffix_min:
                max_chunks += 1

        return max_chunks
```

#### C++

```cpp
class Solution {
public:
    int maxChunksToSorted(vector<int>& arr) {
        vector<int> prefixMaxs; // Max of each arr[:i + 1]. 0 <= i < arr.size().

        for (const auto& num : arr) {
            if (prefixMaxs.empty()) {
                prefixMaxs.push_back(num);
                continue;
            }

            prefixMaxs.push_back(max(prefixMaxs.back(), num));
        }

        int maxChunks = 1; // Base case.
        int suffixMin = arr.back(); // Min of arr[i:]. 0 <= i < arr.size().

        for (int idx = arr.size() - 1; idx >= 1; idx--) {
            if (arr[idx] < suffixMin)
                suffixMin = arr[idx];

            if (prefixMaxs[idx - 1] <= suffixMin)
                maxChunks++;
        }

        return maxChunks;
    }
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
