---
comments: true
difficulty: Easy
rating: 1378
source: Weekly Contest 129 Q1
tags:
    - Greedy
    - Array
---

<!-- problem:start -->

# [1013. Partition Array Into Three Parts With Equal Sum](https://leetcode.com/problems/partition-array-into-three-parts-with-equal-sum)

[中文文档](/solution/1000-1099/1013.Partition%20Array%20Into%20Three%20Parts%20With%20Equal%20Sum/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>arr</code>. Trả về <code>true</code> nếu có thể chia mảng thành ba phần <strong>không rỗng</strong> có tổng bằng nhau.</p>

<p>Cụ thể, có thể chia mảng nếu tìm được các chỉ số <code>i + 1 &lt; j</code> sao cho <code>(arr[0] + arr[1] + ... + arr[i] == arr[i + 1] + arr[i + 2] + ... + arr[j - 1] == arr[j] + arr[j + 1] + ... + arr[arr.length - 1])</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [0,2,1,-6,6,-7,9,1,2,0,1]
<strong>Đầu ra:</strong> true
<strong>Giải thích: </strong>0 + 2 + 1 = -6 + 6 - 7 + 9 + 1 = 2 + 0 + 1
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [0,2,1,-6,6,7,9,-1,2,0,1]
<strong>Đầu ra:</strong> false
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [3,3,6,5,-2,2,5,1,-9,4]
<strong>Đầu ra:</strong> true
<strong>Giải thích: </strong>3 + 3 = 6 = 5 - 2 + 2 + 5 + 1 - 9 + 4
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= arr.length &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>-10<sup>4</sup> &lt;= arr[i] &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Duyệt và tính tổng

<!-- thinking:start -->

> **Tư duy**
>
> Thử mọi cặp vị trí cắt có độ phức tạp $O(n^2)$. Có thể chia thành ba phần bằng nhau chỉ khi tổng toàn mảng chia hết cho $3$ và ta tạo được ít nhất ba đoạn liên tiếp, mỗi đoạn có tổng $s=\textit{sum}/3$.
>
> Cộng dồn từ trái sang phải và đặt lại tổng mỗi khi đạt $s$ để đếm các đoạn như vậy. Các đoạn dư có thể gộp vào phần cuối, nên chỉ cần đếm được ít nhất $3$ đoạn.
>
> Nếu tổng chia cho $3$ còn dư, ta trả về false; nếu không, duy trì tổng phần hiện tại và số phần đã tìm được trong một lượt duyệt.

<!-- thinking:end -->

Trước tiên, tính tổng toàn mảng và kiểm tra xem tổng có chia hết cho 3 không. Nếu không, trả về $\textit{false}$.

Nếu có, gọi $\textit{s}$ là tổng của mỗi phần. Dùng biến $\textit{cnt}$ để đếm số phần đã tìm được và biến $\textit{t}$ để lưu tổng phần hiện tại. Ban đầu, $\textit{cnt} = 0$ và $\textit{t} = 0$.

Sau đó duyệt mảng. Với mỗi phần tử $x$, cộng $x$ vào $\textit{t}$. Nếu $\textit{t}$ bằng $s$, ta đã tìm được một phần; tăng $\textit{cnt}$ thêm một và đặt lại $\textit{t}$ về 0.

Cuối cùng, kiểm tra xem $\textit{cnt}$ có lớn hơn hoặc bằng 3 không.

Độ phức tạp thời gian là $O(n)$, trong đó $n$ là độ dài mảng $\textit{arr}$. Độ phức tạp không gian là $O(1)$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canThreePartsEqualSum(self, arr: List[int]) -> bool:
        s, mod = divmod(sum(arr), 3)
        if mod:
            return False
        cnt = t = 0
        for x in arr:
            t += x
            if t == s:
                cnt += 1
                t = 0
        return cnt >= 3
```

#### Java

```java
class Solution {
    public boolean canThreePartsEqualSum(int[] arr) {
        int s = Arrays.stream(arr).sum();
        if (s % 3 != 0) {
            return false;
        }
        s /= 3;
        int cnt = 0, t = 0;
        for (int x : arr) {
            t += x;
            if (t == s) {
                cnt++;
                t = 0;
            }
        }
        return cnt >= 3;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool canThreePartsEqualSum(vector<int>& arr) {
        int s = accumulate(arr.begin(), arr.end(), 0);
        if (s % 3) {
            return false;
        }
        s /= 3;
        int cnt = 0, t = 0;
        for (int x : arr) {
            t += x;
            if (t == s) {
                t = 0;
                cnt++;
            }
        }
        return cnt >= 3;
    }
};
```

#### Go

```go
func canThreePartsEqualSum(arr []int) bool {
	s := 0
	for _, x := range arr {
		s += x
	}
	if s%3 != 0 {
		return false
	}
	s /= 3
	cnt, t := 0, 0
	for _, x := range arr {
		t += x
		if t == s {
			cnt++
			t = 0
		}
	}
	return cnt >= 3
}
```

#### TypeScript

```ts
function canThreePartsEqualSum(arr: number[]): boolean {
    let s = arr.reduce((a, b) => a + b);
    if (s % 3) {
        return false;
    }
    s = (s / 3) | 0;
    let [cnt, t] = [0, 0];
    for (const x of arr) {
        t += x;
        if (t == s) {
            cnt++;
            t = 0;
        }
    }
    return cnt >= 3;
}
```

#### Rust

```rust
impl Solution {
    pub fn can_three_parts_equal_sum(arr: Vec<i32>) -> bool {
        let sum: i32 = arr.iter().sum();
        let s = sum / 3;
        let mod_val = sum % 3;
        if mod_val != 0 {
            return false;
        }

        let mut cnt = 0;
        let mut t = 0;
        for &x in &arr {
            t += x;
            if t == s {
                cnt += 1;
                t = 0;
            }
        }

        cnt >= 3
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
