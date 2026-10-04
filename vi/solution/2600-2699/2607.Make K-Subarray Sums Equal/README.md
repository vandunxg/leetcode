---
comments: true
difficulty: Medium
rating: 2071
source: Biweekly Contest 101 Q3
tags:
    - Greedy
    - Array
    - Math
    - Number Theory
    - Sorting
---

<!-- problem:start -->

# [2607. Make K-Subarray Sums Equal](https://leetcode.com/problems/make-k-subarray-sums-equal)

[中文文档](/solution/2600-2699/2607.Make%20K-Subarray%20Sums%20Equal/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <strong>được đánh chỉ số từ 0</strong> <code>arr</code> và một số nguyên <code>k</code>. Mảng <code>arr</code> là mảng vòng. Nói cách khác, phần tử đầu tiên của mảng là phần tử kế tiếp của phần tử cuối cùng, còn phần tử cuối cùng là phần tử đứng trước phần tử đầu tiên.</p>

<p>Bạn có thể thực hiện thao tác sau bao nhiêu lần tùy ý:</p>

<ul>
	<li>Chọn một phần tử bất kỳ trong <code>arr</code> và tăng hoặc giảm nó đi <code>1</code>.</li>
</ul>

<p>Trả về <em>số thao tác nhỏ nhất sao cho tổng của mỗi <strong>mảng con</strong> có độ dài </em><code>k</code><em> đều bằng nhau</em>.</p>

<p><strong>Mảng con</strong> là một phần liên tiếp của mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [1,4,1,3], k = 2
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> ta có thể thực hiện một thao tác trên chỉ số 1 để giá trị của nó bằng 3.
Mảng sau thao tác là [1,3,1,3]
- Mảng con bắt đầu tại chỉ số 0 là [1, 3], có tổng bằng 4
- Mảng con bắt đầu tại chỉ số 1 là [3, 1], có tổng bằng 4
- Mảng con bắt đầu tại chỉ số 2 là [1, 3], có tổng bằng 4
- Mảng con bắt đầu tại chỉ số 3 là [3, 1], có tổng bằng 4
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr = [2,5,5,7], k = 3
<strong>Đầu ra:</strong> 5
<strong>Giải thích:</strong> ta có thể thực hiện ba thao tác trên chỉ số 0 để giá trị của nó bằng 5 và hai thao tác trên chỉ số 3 để giá trị của nó bằng 5.
Mảng sau các thao tác là [5,5,5,5]
- Mảng con bắt đầu tại chỉ số 0 là [5, 5, 5], có tổng bằng 15
- Mảng con bắt đầu tại chỉ số 1 là [5, 5, 5], có tổng bằng 15
- Mảng con bắt đầu tại chỉ số 2 là [5, 5, 5], có tổng bằng 15
- Mảng con bắt đầu tại chỉ số 3 là [5, 5, 5], có tổng bằng 15
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= arr.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= arr[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các tổng của những cửa sổ kề nhau có độ dài $k$ bằng nhau dẫn đến điều kiện đơn giản $arr_i=arr_{i+k}$. Vì mảng là mảng vòng, các chỉ số cách nhau $k$ và cách nhau $n$ phải có cùng một giá trị.
>
> Định lý Bézout chia các chỉ số thành $\gcd(n,k)$ chuỗi độc lập. Các phần tử trong cùng một chuỗi phải trở thành một số; tổng chi phí $L_1$ đạt nhỏ nhất tại trung vị.
>
> Sắp xếp từng lớp dư, chọn trung vị của lớp đó rồi cộng các độ lệch.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def makeSubKSumEqual(self, arr: List[int], k: int) -> int:
        n = len(arr)
        g = gcd(n, k)
        ans = 0
        for i in range(g):
            t = sorted(arr[i:n:g])
            mid = t[len(t) >> 1]
            ans += sum(abs(x - mid) for x in t)
        return ans
```

#### Java

```java
class Solution {
    public long makeSubKSumEqual(int[] arr, int k) {
        int n = arr.length;
        int g = gcd(n, k);
        long ans = 0;
        for (int i = 0; i < g; ++i) {
            List<Integer> t = new ArrayList<>();
            for (int j = i; j < n; j += g) {
                t.add(arr[j]);
            }
            t.sort((a, b) -> a - b);
            int mid = t.get(t.size() >> 1);
            for (int x : t) {
                ans += Math.abs(x - mid);
            }
        }
        return ans;
    }

    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long makeSubKSumEqual(vector<int>& arr, int k) {
        int n = arr.size();
        int g = gcd(n, k);
        long long ans = 0;
        for (int i = 0; i < g; ++i) {
            vector<int> t;
            for (int j = i; j < n; j += g) {
                t.push_back(arr[j]);
            }
            sort(t.begin(), t.end());
            int mid = t[t.size() / 2];
            for (int x : t) {
                ans += abs(x - mid);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func makeSubKSumEqual(arr []int, k int) (ans int64) {
	n := len(arr)
	g := gcd(n, k)
	for i := 0; i < g; i++ {
		t := []int{}
		for j := i; j < n; j += g {
			t = append(t, arr[j])
		}
		sort.Ints(t)
		mid := t[len(t)/2]
		for _, x := range t {
			ans += int64(abs(x - mid))
		}
	}
	return
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}

func gcd(a, b int) int {
	if b == 0 {
		return a
	}
	return gcd(b, a%b)
}
```

#### TypeScript

```ts
function makeSubKSumEqual(arr: number[], k: number): number {
    const n = arr.length;
    const g = gcd(n, k);
    let ans = 0;
    for (let i = 0; i < g; ++i) {
        const t: number[] = [];
        for (let j = i; j < n; j += g) {
            t.push(arr[j]);
        }
        t.sort((a, b) => a - b);
        const mid = t[t.length >> 1];
        for (const x of t) {
            ans += Math.abs(x - mid);
        }
    }
    return ans;
}

function gcd(a: number, b: number): number {
    if (b === 0) {
        return a;
    }
    return gcd(b, a % b);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
