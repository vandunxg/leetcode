---
comments: true
difficulty: Easy
rating: 1259
source: Biweekly Contest 10 Q1
tags:
    - Array
    - Hash Table
    - Binary Search
    - Counting
---

<!-- problem:start -->

# [1213. Intersection of Three Sorted Arrays 🔒](https://leetcode.com/problems/intersection-of-three-sorted-arrays)

[中文文档](/solution/1200-1299/1213.Intersection%20of%20Three%20Sorted%20Arrays/README.md)

## Mô tả

<!-- description:start -->

<p>Cho ba mảng số nguyên <code>arr1</code>, <code>arr2</code> và <code>arr3</code> được <strong>sắp xếp</strong> theo thứ tự <strong>tăng dần nghiêm ngặt</strong>, hãy trả về mảng đã sắp xếp chỉ gồm các số nguyên xuất hiện trong <strong>cả</strong> ba mảng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr1 = [1,2,3,4,5], arr2 = [1,2,5,7,9], arr3 = [1,3,4,5,8]
<strong>Đầu ra:</strong> [1,5]
<strong>Giải thích: </strong>Chỉ có 1 và 5 xuất hiện trong cả ba mảng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> arr1 = [197,418,523,876,1356], arr2 = [501,880,1593,1710,1870], arr3 = [521,682,1337,1395,1764]
<strong>Đầu ra:</strong> []
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= arr1.length, arr2.length, arr3.length &lt;= 1000</code></li>
	<li><code>1 &lt;= arr1[i], arr2[i], arr3[i] &lt;= 2000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm tần suất

<!-- thinking:start -->

> **Tư duy**
>
> Ba mảng đã được sắp xếp, có độ dài tối đa $1000$ và các giá trị nằm trong $[1,2000]$. Ta đếm số lần xuất hiện trong cả ba mảng; giá trị có số đếm bằng $3$ là phần tử chung. Vì mỗi mảng không có phần tử trùng nhau nên một mảng không thể làm tăng số đếm nhiều lần. Duyệt theo thứ tự của $arr1$ giúp kết quả giữ nguyên thứ tự tăng dần.

<!-- thinking:end -->

Duyệt ba mảng để đếm số lần xuất hiện của từng số, sau đó duyệt một trong các mảng. Nếu số đếm của một giá trị bằng $3$, thêm giá trị đó vào mảng kết quả.

Độ phức tạp thời gian là $O(n)$, độ phức tạp không gian là $O(m)$. Trong đó, $n$ là độ dài mảng, còn $m$ là miền giá trị của các số trong mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def arraysIntersection(
        self, arr1: List[int], arr2: List[int], arr3: List[int]
    ) -> List[int]:
        cnt = Counter(arr1 + arr2 + arr3)
        return [x for x in arr1 if cnt[x] == 3]
```

#### Java

```java
class Solution {
    public List<Integer> arraysIntersection(int[] arr1, int[] arr2, int[] arr3) {
        List<Integer> ans = new ArrayList<>();
        int[] cnt = new int[2001];
        for (int x : arr1) {
            ++cnt[x];
        }
        for (int x : arr2) {
            ++cnt[x];
        }
        for (int x : arr3) {
            if (++cnt[x] == 3) {
                ans.add(x);
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
    vector<int> arraysIntersection(vector<int>& arr1, vector<int>& arr2, vector<int>& arr3) {
        vector<int> ans;
        int cnt[2001]{};
        for (int x : arr1) {
            ++cnt[x];
        }
        for (int x : arr2) {
            ++cnt[x];
        }
        for (int x : arr3) {
            if (++cnt[x] == 3) {
                ans.push_back(x);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func arraysIntersection(arr1 []int, arr2 []int, arr3 []int) (ans []int) {
	cnt := [2001]int{}
	for _, x := range arr1 {
		cnt[x]++
	}
	for _, x := range arr2 {
		cnt[x]++
	}
	for _, x := range arr3 {
		cnt[x]++
		if cnt[x] == 3 {
			ans = append(ans, x)
		}
	}
	return
}
```

#### PHP

```php
class Solution {
    /**
     * @param Integer[] $arr1
     * @param Integer[] $arr2
     * @param Integer[] $arr3
     * @return Integer[]
     */
    function arraysIntersection($arr1, $arr2, $arr3) {
        $rs = [];
        $arr = array_merge($arr1, $arr2, $arr3);
        for ($i = 0; $i < count($arr); $i++) {
            $hashtable[$arr[$i]] += 1;
            if ($hashtable[$arr[$i]] === 3) {
                array_push($rs, $arr[$i]);
            }
        }
        return $rs;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Tìm kiếm nhị phân

<!-- thinking:start -->

> **Tư duy**
>
> Cách đếm cần một mảng có kích thước tỷ lệ với miền giá trị. Vì các mảng đã được sắp xếp, ta tìm kiếm nhị phân từng giá trị trong $arr1$ ở $arr2$ và $arr3$. Độ phức tạp không gian bổ sung là hằng số, còn độ phức tạp thời gian là $O(n\log n)$.

<!-- thinking:end -->

Duyệt mảng thứ nhất. Với mỗi số, dùng tìm kiếm nhị phân để tìm số đó trong mảng thứ hai và thứ ba. Nếu tìm thấy trong cả hai mảng, thêm số đó vào mảng kết quả.

Độ phức tạp thời gian là $O(n \times \log n)$, độ phức tạp không gian là $O(1)$. Trong đó, $n$ là độ dài mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def arraysIntersection(
        self, arr1: List[int], arr2: List[int], arr3: List[int]
    ) -> List[int]:
        ans = []
        for x in arr1:
            i = bisect_left(arr2, x)
            j = bisect_left(arr3, x)
            if i < len(arr2) and j < len(arr3) and arr2[i] == x and arr3[j] == x:
                ans.append(x)
        return ans
```

#### Java

```java
class Solution {
    public List<Integer> arraysIntersection(int[] arr1, int[] arr2, int[] arr3) {
        List<Integer> ans = new ArrayList<>();
        for (int x : arr1) {
            int i = Arrays.binarySearch(arr2, x);
            int j = Arrays.binarySearch(arr3, x);
            if (i >= 0 && j >= 0) {
                ans.add(x);
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
    vector<int> arraysIntersection(vector<int>& arr1, vector<int>& arr2, vector<int>& arr3) {
        vector<int> ans;
        for (int x : arr1) {
            auto i = lower_bound(arr2.begin(), arr2.end(), x);
            auto j = lower_bound(arr3.begin(), arr3.end(), x);
            if (*i == x && *j == x) {
                ans.push_back(x);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func arraysIntersection(arr1 []int, arr2 []int, arr3 []int) (ans []int) {
	for _, x := range arr1 {
		i := sort.SearchInts(arr2, x)
		j := sort.SearchInts(arr3, x)
		if i < len(arr2) && j < len(arr3) && arr2[i] == x && arr3[j] == x {
			ans = append(ans, x)
		}
	}
	return
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
