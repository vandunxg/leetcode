---
comments: true
difficulty: Easy
rating: 1188
source: Weekly Contest 145 Q1
tags:
    - Array
    - Hash Table
    - Bubble Sort
    - Counting Sort
    - Sorting
    - Quick Sort
---

<!-- problem:start -->

# [1122. Relative Sort Array](https://leetcode.com/problems/relative-sort-array)

[中文文档](/solution/1100-1199/1122.Relative%20Sort%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng <code>arr1</code> và <code>arr2</code>. Các phần tử trong <code>arr2</code> đôi một khác nhau và đều xuất hiện trong <code>arr1</code>.</p>

<p>Hãy sắp xếp các phần tử của <code>arr1</code> sao cho thứ tự tương đối của chúng giống với thứ tự trong <code>arr2</code>. Các phần tử không xuất hiện trong <code>arr2</code> phải được đặt ở cuối <code>arr1</code> theo thứ tự <strong>tăng dần</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr1 = [2,3,1,3,2,4,6,7,9,2,19], arr2 = [2,1,4,3,9,6]
<strong>Đầu ra:</strong> [2,2,2,1,4,3,3,9,6,7,19]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr1 = [28,6,22,8,44,17], arr2 = [22,28,8,6]
<strong>Đầu ra:</strong> [22,28,8,6,17,44]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr1.length, arr2.length &lt;= 1000</code></li>
	<li><code>0 &lt;= arr1[i], arr2[i] &lt;= 1000</code></li>
	<li>Các phần tử trong <code>arr2</code> đôi một <strong>khác nhau</strong>.</li>
	<li>Mỗi <code>arr2[i]</code> đều có trong <code>arr1</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp tùy chỉnh

<!-- thinking:start -->

> **Tư duy**
>
> Khóa sắp xếp là thứ tự tương đối trong $arr2$; các giá trị không có trong $arr2$ xếp sau và được sắp theo chính giá trị của chúng. Một map lưu chỉ số của từng phần tử trong $arr2$; comparator dùng chỉ số đó nếu có, nếu không thì dùng $1000+x$, nhờ vậy chỉ cần sắp xếp một lần cho cả hai nhóm. Các giá trị trong $arr2$ là duy nhất nên thứ tự chỉ số không bị trùng.

<!-- thinking:end -->

Trước tiên, dùng hash table $pos$ để lưu vị trí của mỗi phần tử trong mảng $arr2$. Sau đó, ánh xạ mỗi phần tử trong $arr1$ thành tuple $(pos.get(x, 1000 + x), x)$ rồi sắp xếp các tuple này. Cuối cùng, lấy phần tử thứ hai của mỗi tuple và trả về mảng kết quả.

Độ phức tạp thời gian là $O(n \times \log n + m)$ và độ phức tạp không gian là $O(n + m)$, trong đó $n$ và $m$ lần lượt là độ dài của $arr1$ và $arr2$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def relativeSortArray(self, arr1: List[int], arr2: List[int]) -> List[int]:
        pos = {x: i for i, x in enumerate(arr2)}
        return sorted(arr1, key=lambda x: pos.get(x, 1000 + x))
```

#### Java

```java
class Solution {
    public int[] relativeSortArray(int[] arr1, int[] arr2) {
        Map<Integer, Integer> pos = new HashMap<>(arr2.length);
        for (int i = 0; i < arr2.length; ++i) {
            pos.put(arr2[i], i);
        }
        int[][] arr = new int[arr1.length][0];
        for (int i = 0; i < arr.length; ++i) {
            arr[i] = new int[] {arr1[i], pos.getOrDefault(arr1[i], arr2.length + arr1[i])};
        }
        Arrays.sort(arr, (a, b) -> a[1] - b[1]);
        for (int i = 0; i < arr.length; ++i) {
            arr1[i] = arr[i][0];
        }
        return arr1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> relativeSortArray(vector<int>& arr1, vector<int>& arr2) {
        unordered_map<int, int> pos;
        for (int i = 0; i < arr2.size(); ++i) {
            pos[arr2[i]] = i;
        }
        vector<pair<int, int>> arr;
        for (int i = 0; i < arr1.size(); ++i) {
            int j = pos.count(arr1[i]) ? pos[arr1[i]] : arr2.size();
            arr.emplace_back(j, arr1[i]);
        }
        sort(arr.begin(), arr.end());
        for (int i = 0; i < arr1.size(); ++i) {
            arr1[i] = arr[i].second;
        }
        return arr1;
    }
};
```

#### Go

```go
func relativeSortArray(arr1 []int, arr2 []int) []int {
	pos := map[int]int{}
	for i, x := range arr2 {
		pos[x] = i
	}
	arr := make([][2]int, len(arr1))
	for i, x := range arr1 {
		if p, ok := pos[x]; ok {
			arr[i] = [2]int{p, x}
		} else {
			arr[i] = [2]int{len(arr2), x}
		}
	}
	sort.Slice(arr, func(i, j int) bool {
		return arr[i][0] < arr[j][0] || arr[i][0] == arr[j][0] && arr[i][1] < arr[j][1]
	})
	for i, x := range arr {
		arr1[i] = x[1]
	}
	return arr1
}
```

#### TypeScript

```ts
function relativeSortArray(arr1: number[], arr2: number[]): number[] {
    const pos: Map<number, number> = new Map();
    for (let i = 0; i < arr2.length; ++i) {
        pos.set(arr2[i], i);
    }
    const arr: number[][] = [];
    for (const x of arr1) {
        const j = pos.get(x) ?? arr2.length;
        arr.push([j, x]);
    }
    arr.sort((a, b) => a[0] - b[0] || a[1] - b[1]);
    return arr.map(a => a[1]);
}
```

#### Swift

```swift
class Solution {
    func relativeSortArray(_ arr1: [Int], _ arr2: [Int]) -> [Int] {
        var pos = [Int: Int]()
        for (i, x) in arr2.enumerated() {
            pos[x] = i
        }
        var arr = [(Int, Int)]()
        for x in arr1 {
            let j = pos[x] ?? arr2.count
            arr.append((j, x))
        }
        arr.sort { $0.0 < $1.0 || ($0.0 == $1.0 && $0.1 < $1.1) }
        return arr.map { $0.1 }
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Counting sort

<!-- thinking:start -->

> **Tư duy**
>
> Phép comparison sort ở Lời giải 1 có độ phức tạp $O(n\log n)$. Khi miền giá trị nhỏ, có thể đếm tần suất trong $arr1$, xuất các giá trị theo thứ tự của $arr2$, rồi duyệt các giá trị còn lại theo thứ tự tăng dần; cách này có độ phức tạp tuyến tính.

<!-- thinking:end -->

Ta có thể dùng counting sort. Trước tiên, đếm số lần xuất hiện của mỗi phần tử trong mảng $arr1$. Sau đó, theo thứ tự trong $arr2$, đưa các phần tử vào mảng kết quả $ans$ theo đúng số lần xuất hiện. Cuối cùng, duyệt các giá trị trong $arr1$ và thêm vào cuối $ans$ theo thứ tự tăng dần những phần tử không xuất hiện trong $arr2$.

Độ phức tạp thời gian là $O(n + m)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ và $m$ lần lượt là độ dài của $arr1$ và $arr2$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def relativeSortArray(self, arr1: List[int], arr2: List[int]) -> List[int]:
        cnt = Counter(arr1)
        ans = []
        for x in arr2:
            ans.extend([x] * cnt[x])
            cnt.pop(x)
        mi, mx = min(arr1), max(arr1)
        for x in range(mi, mx + 1):
            ans.extend([x] * cnt[x])
        return ans
```

#### Java

```java
class Solution {
    public int[] relativeSortArray(int[] arr1, int[] arr2) {
        int[] cnt = new int[1001];
        int mi = 1001, mx = 0;
        for (int x : arr1) {
            ++cnt[x];
            mi = Math.min(mi, x);
            mx = Math.max(mx, x);
        }
        int m = arr1.length;
        int[] ans = new int[m];
        int i = 0;
        for (int x : arr2) {
            while (cnt[x] > 0) {
                --cnt[x];
                ans[i++] = x;
            }
        }
        for (int x = mi; x <= mx; ++x) {
            while (cnt[x] > 0) {
                --cnt[x];
                ans[i++] = x;
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
    vector<int> relativeSortArray(vector<int>& arr1, vector<int>& arr2) {
        vector<int> cnt(1001);
        for (int x : arr1) {
            ++cnt[x];
        }
        auto [mi, mx] = minmax_element(arr1.begin(), arr1.end());
        vector<int> ans;
        for (int x : arr2) {
            while (cnt[x]) {
                ans.push_back(x);
                --cnt[x];
            }
        }
        for (int x = *mi; x <= *mx; ++x) {
            while (cnt[x]) {
                ans.push_back(x);
                --cnt[x];
            }
        }
        return ans;
    }
};
```

#### Go

```go
func relativeSortArray(arr1 []int, arr2 []int) []int {
	cnt := make([]int, 1001)
	mi, mx := 1001, 0
	for _, x := range arr1 {
		cnt[x]++
		mi = min(mi, x)
		mx = max(mx, x)
	}
	ans := make([]int, 0, len(arr1))
	for _, x := range arr2 {
		for cnt[x] > 0 {
			ans = append(ans, x)
			cnt[x]--
		}
	}
	for x := mi; x <= mx; x++ {
		for cnt[x] > 0 {
			ans = append(ans, x)
			cnt[x]--
		}
	}
	return ans
}
```

#### TypeScript

```ts
function relativeSortArray(arr1: number[], arr2: number[]): number[] {
    const cnt = Array(1001).fill(0);
    let mi = Number.POSITIVE_INFINITY;
    let mx = Number.NEGATIVE_INFINITY;

    for (const x of arr1) {
        cnt[x]++;
        mi = Math.min(mi, x);
        mx = Math.max(mx, x);
    }

    const ans: number[] = [];
    for (const x of arr2) {
        while (cnt[x]) {
            cnt[x]--;
            ans.push(x);
        }
    }

    for (let i = mi; i <= mx; i++) {
        while (cnt[i]) {
            cnt[i]--;
            ans.push(i);
        }
    }

    return ans;
}
```

#### Swift

```swift
class Solution {
    func relativeSortArray(_ arr1: [Int], _ arr2: [Int]) -> [Int] {
        var cnt = [Int](repeating: 0, count: 1001)
        for x in arr1 {
            cnt[x] += 1
        }

        guard let mi = arr1.min(), let mx = arr1.max() else {
            return []
        }

        var ans = [Int]()
        for x in arr2 {
            while cnt[x] > 0 {
                ans.append(x)
                cnt[x] -= 1
            }
        }

        for x in mi...mx {
            while cnt[x] > 0 {
                ans.append(x)
                cnt[x] -= 1
            }
        }

        return ans
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
