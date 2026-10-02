---
comments: true
difficulty: Medium
rating: 1332
source: Weekly Contest 192 Q2
tags:
    - Array
    - Two Pointers
    - Sorting
---

<!-- problem:start -->

# [1471. The k Strongest Values in an Array](https://leetcode.com/problems/the-k-strongest-values-in-an-array)

[中文文档](/solution/1400-1499/1471.The%20k%20Strongest%20Values%20in%20an%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>arr</code> và một số nguyên <code>k</code>.</p>

<p>Một giá trị <code>arr[i]</code> được gọi là mạnh hơn một giá trị <code>arr[j]</code> nếu <code>|arr[i] - m| &gt; |arr[j] - m|</code>, trong đó <code>m</code> là <strong>trung tâm</strong> của mảng.<br />
Nếu <code>|arr[i] - m| == |arr[j] - m|</code>, thì <code>arr[i]</code> được gọi là mạnh hơn <code>arr[j]</code> nếu <code>arr[i] &gt; arr[j]</code>.</p>

<p>Trả về <em>danh sách <code>k</code></em> giá trị mạnh nhất trong mảng. Có thể trả về đáp án <strong>theo bất kỳ thứ tự nào</strong>.</p>

<p><strong>Trung tâm</strong> là giá trị ở giữa trong một danh sách số nguyên đã được sắp xếp. Cụ thể hơn, nếu độ dài danh sách là n, trung tâm là phần tử ở vị trí <code>((n - 1) / 2)</code> trong danh sách đã sắp xếp <strong>(đánh chỉ số từ 0)</strong>.</p>

<ul>
	<li>Với <code>arr = [6, -3, 7, 2, 11]</code>, <code>n = 5</code> và trung tâm được lấy bằng cách sắp xếp mảng <code>arr = [-3, 2, 6, 7, 11]</code>; trung tâm là <code>arr[m]</code>, trong đó <code>m = ((5 - 1) / 2) = 2</code>. Trung tâm là <code>6</code>.</li>
	<li>Với <code>arr = [-7, 22, 17,&thinsp;3]</code>, <code>n = 4</code> và trung tâm được lấy bằng cách sắp xếp mảng <code>arr = [-7, 3, 17, 22]</code>; trung tâm là <code>arr[m]</code>, trong đó <code>m = ((4 - 1) / 2) = 1</code>. Trung tâm là <code>3</code>.</li>
</ul>

<div class="simple-translate-system-theme" id="simple-translate">
<div>
<div class="simple-translate-button isShow" style="background-image: url(&quot;moz-extension://8a9ffb6b-7e69-4e93-aae1-436a1448eff6/icons/512.png&quot;); height: 22px; width: 22px; top: 266px; left: 381px;">&nbsp;</div>

<div class="simple-translate-panel " style="width: 300px; height: 200px; top: 0px; left: 0px; font-size: 13px;">
<div class="simple-translate-result-wrapper" style="overflow: hidden;">
<div class="simple-translate-move" draggable="true">&nbsp;</div>

<div class="simple-translate-result-contents">
<p class="simple-translate-result" dir="auto">&nbsp;</p>

<p class="simple-translate-candidate" dir="auto">&nbsp;</p>
</div>
</div>
</div>
</div>
</div>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> arr = [1,2,3,4,5], k = 2
<strong>Output:</strong> [5,1]
<strong>Explanation:</strong> Trung tâm là 3, các phần tử của mảng được sắp xếp theo độ mạnh là [5,1,4,2,3]. 2 phần tử mạnh nhất là [5, 1]. [1, 5] cũng là đáp án <strong>được chấp nhận</strong>.
Lưu ý rằng mặc dù |5 - 3| == |1 - 3| nhưng 5 mạnh hơn 1 vì 5 &gt; 1.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> arr = [1,1,3,5,5], k = 2
<strong>Output:</strong> [5,5]
<strong>Explanation:</strong> Trung tâm là 3, các phần tử của mảng được sắp xếp theo độ mạnh là [5,5,1,1,3]. 2 phần tử mạnh nhất là [5, 5].
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> arr = [6,7,11,7,6,8], k = 5
<strong>Output:</strong> [11,8,6,6,7]
<strong>Explanation:</strong> Trung tâm là 7, các phần tử của mảng được sắp xếp theo độ mạnh là [11,8,6,6,7,7].
Mọi hoán vị của [11,8,6,6,7] đều là đáp án <strong>được chấp nhận</strong>.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr.length &lt;= 10<sup>5</sup></code></li>
	<li><code>-10<sup>5</sup> &lt;= arr[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= k &lt;= arr.length</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp

<!-- thinking:start -->

> **Tư duy**
>
> Độ mạnh là khoảng cách đến median, nếu bằng nhau thì ưu tiên giá trị lớn hơn. $n\le 10^5$. Sắp xếp để lấy $m=arr[(n-1)//2]$, sau đó sắp xếp theo $(-|x-m|,-x)$ và lấy $k$ phần tử đầu tiên.

<!-- thinking:end -->

Trước tiên, chúng ta sắp xếp mảng $\textit{arr}$ rồi tìm median $m$ của mảng.

Tiếp theo, chúng ta sắp xếp mảng theo các quy tắc được mô tả trong đề bài, cuối cùng trả về $k$ phần tử đầu tiên của mảng.

Độ phức tạp thời gian là $O(n \times \log n)$, còn độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng $\textit{arr}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def getStrongest(self, arr: List[int], k: int) -> List[int]:
        arr.sort()
        m = arr[(len(arr) - 1) >> 1]
        arr.sort(key=lambda x: (-abs(x - m), -x))
        return arr[:k]
```

#### Java

```java
class Solution {
    public int[] getStrongest(int[] arr, int k) {
        Arrays.sort(arr);
        int m = arr[(arr.length - 1) >> 1];
        List<Integer> nums = new ArrayList<>();
        for (int v : arr) {
            nums.add(v);
        }
        nums.sort((a, b) -> {
            int x = Math.abs(a - m);
            int y = Math.abs(b - m);
            return x == y ? b - a : y - x;
        });
        int[] ans = new int[k];
        for (int i = 0; i < k; ++i) {
            ans[i] = nums.get(i);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> getStrongest(vector<int>& arr, int k) {
        sort(arr.begin(), arr.end());
        int m = arr[(arr.size() - 1) >> 1];
        sort(arr.begin(), arr.end(), [&](int a, int b) {
            int x = abs(a - m), y = abs(b - m);
            return x == y ? a > b : x > y;
        });
        vector<int> ans(arr.begin(), arr.begin() + k);
        return ans;
    }
};
```

#### Go

```go
func getStrongest(arr []int, k int) []int {
	sort.Ints(arr)
	m := arr[(len(arr)-1)>>1]
	sort.Slice(arr, func(i, j int) bool {
		x, y := abs(arr[i]-m), abs(arr[j]-m)
		if x == y {
			return arr[i] > arr[j]
		}
		return x > y
	})
	return arr[:k]
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function getStrongest(arr: number[], k: number): number[] {
    arr.sort((a, b) => a - b);
    const m = arr[(arr.length - 1) >> 1];
    return arr.sort((a, b) => Math.abs(b - m) - Math.abs(a - m) || b - a).slice(0, k);
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
