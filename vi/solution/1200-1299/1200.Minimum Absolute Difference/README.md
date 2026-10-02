---
comments: true
difficulty: Easy
rating: 1198
source: Weekly Contest 155 Q1
tags:
    - Array
    - Sorting
---

<!-- problem:start -->

# [1200. Minimum Absolute Difference](https://leetcode.com/problems/minimum-absolute-difference)

[中文文档](/solution/1200-1299/1200.Minimum%20Absolute%20Difference/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên <code>arr</code> gồm các giá trị <strong>khác nhau</strong>, hãy tìm mọi cặp phần tử có độ chênh lệch tuyệt đối nhỏ nhất trong tất cả các cặp.</p>

<p>Trả về danh sách các cặp theo thứ tự tăng dần, mỗi cặp <code>[a, b]</code> thỏa mãn:</p>

<ul>
	<li><code>a, b</code> là các phần tử của <code>arr</code></li>
	<li><code>a &lt; b</code></li>
	<li><code>b - a</code> bằng độ chênh lệch tuyệt đối nhỏ nhất giữa hai phần tử bất kỳ trong <code>arr</code></li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [4,2,1,3]
<strong>Đầu ra:</strong> [[1,2],[2,3],[3,4]]
<strong>Giải thích: </strong>Độ chênh lệch tuyệt đối nhỏ nhất là 1. Liệt kê mọi cặp có độ chênh lệch bằng 1 theo thứ tự tăng dần.</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [1,3,6,10,15]
<strong>Đầu ra:</strong> [[1,3]]
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [3,8,-10,23,19,-4,-14,27]
<strong>Đầu ra:</strong> [[-14,-10],[19,23],[23,27]]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= arr.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>6</sup> &lt;= arr[i] &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Duyệt mọi cặp có độ phức tạp $O(n^2)$, quá chậm với $n \le 10^5$.
>
> Sau khi sắp xếp, khoảng cách giữa hai giá trị bất kỳ ít nhất bằng tổng các khoảng cách giữa những phần tử kề nhau nằm giữa chúng. Vì vậy, độ chênh lệch tuyệt đối nhỏ nhất chỉ có thể xuất hiện giữa hai phần tử liền kề.
>
> Vì vậy, ta sắp xếp $arr$, duyệt các hiệu giữa phần tử liền kề để tìm giá trị nhỏ nhất $mi$, rồi thu thập mọi cặp kề nhau có hiệu bằng $mi$. Sắp xếp giúp thu hẹp tập ứng viên; hai lượt duyệt tuyến tính sẽ tìm giá trị nhỏ nhất và gom các cặp kết quả.

<!-- thinking:end -->

Theo mô tả bài toán, ta cần tìm độ chênh lệch tuyệt đối nhỏ nhất giữa hai phần tử bất kỳ trong mảng $arr$. Vì vậy, trước tiên ta sắp xếp $arr$, rồi duyệt các phần tử liền kề để tìm độ chênh lệch nhỏ nhất $mi$.

Cuối cùng, ta duyệt lại các phần tử liền kề để tìm mọi cặp có độ chênh lệch bằng $mi$.

Độ phức tạp thời gian là $O(n \times \log n)$, độ phức tạp không gian là $O(\log n)$. Trong đó, $n$ là độ dài mảng $arr$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumAbsDifference(self, arr: List[int]) -> List[List[int]]:
        arr.sort()
        mi = min(b - a for a, b in pairwise(arr))
        return [[a, b] for a, b in pairwise(arr) if b - a == mi]
```

#### Java

```java
class Solution {
    public List<List<Integer>> minimumAbsDifference(int[] arr) {
        Arrays.sort(arr);
        int n = arr.length;
        int mi = 1 << 30;
        for (int i = 0; i < n - 1; ++i) {
            mi = Math.min(mi, arr[i + 1] - arr[i]);
        }
        List<List<Integer>> ans = new ArrayList<>();
        for (int i = 0; i < n - 1; ++i) {
            if (arr[i + 1] - arr[i] == mi) {
                ans.add(List.of(arr[i], arr[i + 1]));
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
    vector<vector<int>> minimumAbsDifference(vector<int>& arr) {
        sort(arr.begin(), arr.end());
        int mi = 1 << 30;
        int n = arr.size();
        for (int i = 0; i < n - 1; ++i) {
            mi = min(mi, arr[i + 1] - arr[i]);
        }
        vector<vector<int>> ans;
        for (int i = 0; i < n - 1; ++i) {
            if (arr[i + 1] - arr[i] == mi) {
                ans.push_back({arr[i], arr[i + 1]});
            }
        }
        return ans;
    }
};
```

#### Go

```go
func minimumAbsDifference(arr []int) (ans [][]int) {
	sort.Ints(arr)
	mi := 1 << 30
	n := len(arr)
	for i := 0; i < n-1; i++ {
		if t := arr[i+1] - arr[i]; t < mi {
			mi = t
		}
	}
	for i := 0; i < n-1; i++ {
		if arr[i+1]-arr[i] == mi {
			ans = append(ans, []int{arr[i], arr[i+1]})
		}
	}
	return
}
```

#### TypeScript

```ts
function minimumAbsDifference(arr: number[]): number[][] {
    arr.sort((a, b) => a - b);
    let mi = 1 << 30;
    const n = arr.length;
    for (let i = 0; i < n - 1; ++i) {
        mi = Math.min(mi, arr[i + 1] - arr[i]);
    }
    const ans: number[][] = [];
    for (let i = 0; i < n - 1; ++i) {
        if (arr[i + 1] - arr[i] === mi) {
            ans.push([arr[i], arr[i + 1]]);
        }
    }
    return ans;
}
```

#### Rust

```rust
impl Solution {
    pub fn minimum_abs_difference(mut arr: Vec<i32>) -> Vec<Vec<i32>> {
        arr.sort();
        let n = arr.len();

        let mut mi = i32::MAX;
        for i in 1..n {
            mi = mi.min(arr[i] - arr[i - 1]);
        }

        let mut ans = Vec::new();
        for i in 1..n {
            if arr[i] - arr[i - 1] == mi {
                ans.push(vec![arr[i - 1], arr[i]]);
            }
        }

        ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
