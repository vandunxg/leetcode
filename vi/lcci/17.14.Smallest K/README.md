---
comments: true
difficulty: Medium
---

<!-- problem:start -->

# [17.14. Smallest K](https://leetcode.cn/problems/smallest-k-lcci)

[中文文档](/lcci/17.14.Smallest%20K/README.md)

## Mô tả

<!-- description:start -->

<p>Hãy thiết kế một thuật toán để tìm k số nhỏ nhất trong một mảng.</p>
<p><strong>Ví dụ: </strong></p>
<pre>

<strong>Đầu vào: </strong> arr = [1,3,5,7,2,4,6,8], k = 4

<strong>Đầu ra: </strong> [1,2,3,4]

</pre>
<p><strong>Lưu ý: </strong></p>
<ul>
	<li><code>0 &lt;= len(arr) &lt;= 100000</code></li>
	<li><code>0 &lt;= k &lt;= min(100000, len(arr))</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Chọn $k$ giá trị nhỏ nhất, theo bất kỳ thứ tự nào. Sắp xếp toàn bộ rồi lấy prefix là tối ưu khi $k$ gần với $n$.
>
> `sorted(arr)[:k]` ngắn gọn và có constant tốt trong Python.
>
> Đề bài chấp nhận mọi thứ tự, vì vậy ở đây không cần thêm heap.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def smallestK(self, arr: List[int], k: int) -> List[int]:
        return sorted(arr)[:k]
```

#### Java

```java
class Solution {
    public int[] smallestK(int[] arr, int k) {
        Arrays.sort(arr);
        int[] ans = new int[k];
        for (int i = 0; i < k; ++i) {
            ans[i] = arr[i];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> smallestK(vector<int>& arr, int k) {
        sort(arr.begin(), arr.end());
        vector<int> ans(k);
        for (int i = 0; i < k; ++i) {
            ans[i] = arr[i];
        }
        return ans;
    }
};
```

#### Go

```go
func smallestK(arr []int, k int) []int {
	sort.Ints(arr)
	ans := make([]int, k)
	for i, v := range arr[:k] {
		ans[i] = v
	}
	return ans
}
```

#### Swift

```swift
class Solution {
    func smallestK(_ arr: [Int], _ k: Int) -> [Int] {
        guard k > 0 else { return [] }
        let sortedArray = arr.sorted()
        return Array(sortedArray.prefix(k))
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Sắp xếp toàn bộ sẽ lãng phí công sức khi $k\ll n$.
>
> Một max-heap có kích thước $k$ duy trì $k$ phần tử nhỏ nhất hiện tại trong khi duyệt phần còn lại, với độ phức tạp $O(n\log k)$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def smallestK(self, arr: List[int], k: int) -> List[int]:
        h = []
        for v in arr:
            heappush(h, -v)
            if len(h) > k:
                heappop(h)
        return [-v for v in h]
```

#### Java

```java
class Solution {
    public int[] smallestK(int[] arr, int k) {
        PriorityQueue<Integer> q = new PriorityQueue<>((a, b) -> b - a);
        for (int v : arr) {
            q.offer(v);
            if (q.size() > k) {
                q.poll();
            }
        }
        int[] ans = new int[k];
        int i = 0;
        while (!q.isEmpty()) {
            ans[i++] = q.poll();
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> smallestK(vector<int>& arr, int k) {
        priority_queue<int> q;
        for (int& v : arr) {
            q.push(v);
            if (q.size() > k) {
                q.pop();
            }
        }
        vector<int> ans;
        while (q.size()) {
            ans.push_back(q.top());
            q.pop();
        }
        return ans;
    }
};
```

#### Go

```go
func smallestK(arr []int, k int) []int {
	q := hp{}
	for _, v := range arr {
		heap.Push(&q, v)
		if q.Len() > k {
			heap.Pop(&q)
		}
	}
	ans := make([]int, k)
	for i := range ans {
		ans[i] = heap.Pop(&q).(int)
	}
	return ans
}

type hp struct{ sort.IntSlice }

func (h hp) Less(i, j int) bool { return h.IntSlice[i] > h.IntSlice[j] }
func (h *hp) Push(v any)        { h.IntSlice = append(h.IntSlice, v.(int)) }
func (h *hp) Pop() any {
	a := h.IntSlice
	v := a[len(a)-1]
	h.IntSlice = a[:len(a)-1]
	return v
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
