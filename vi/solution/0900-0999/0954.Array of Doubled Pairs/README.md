---
comments: true
difficulty: Medium
tags:
    - Greedy
    - Array
    - Hash Table
    - Sorting
---

<!-- problem:start -->

# [954. Array of Doubled Pairs](https://leetcode.com/problems/array-of-doubled-pairs)

[中文文档](/solution/0900-0999/0954.Array%20of%20Doubled%20Pairs/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mảng số nguyên có độ dài chẵn <code>arr</code>, trả về <code>true</code><em> nếu có thể sắp xếp lại </em><code>arr</code><em> sao cho </em><code>arr[2 * i + 1] = 2 * arr[2 * i]</code><em> với mọi </em><code>0 &lt;= i &lt; len(arr) / 2</code><em>; nếu không thì trả về </em><code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> arr = [3,1,3,6]
<strong>Output:</strong> false
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> arr = [2,1,2,6]
<strong>Output:</strong> false
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> arr = [4,-2,2,-4]
<strong>Output:</strong> true
<strong>Giải thích:</strong> Ta có thể ghép thành hai nhóm [-2,-4] và [2,4], tạo thành [-2,-4,2,4] hoặc [2,4,-2,-4].
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= arr.length &lt;= 3 * 10<sup>4</sup></code></li>
	<li><code>arr.length</code> là số chẵn.</li>
	<li><code>-10<sup>5</sup> &lt;= arr[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Sắp xếp lại mảng sao cho phần tử ở mỗi chỉ số chẵn bằng một nửa phần tử kế tiếp. Số 0 chỉ ghép được với số 0; nếu số lượng 0 lẻ thì không thể. Với các giá trị khác, ghép theo thứ tự trị tuyệt đối tăng dần: mỗi $x$ cần có đủ số lượng $2x$ còn lại, nếu không thì một giá trị lớn hơn có thể đã bị ghép trước. Dùng frequency map để theo dõi số phần tử còn lại.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def canReorderDoubled(self, arr: List[int]) -> bool:
        freq = Counter(arr)
        if freq[0] & 1:
            return False
        for x in sorted(freq, key=abs):
            if freq[x << 1] < freq[x]:
                return False
            freq[x << 1] -= freq[x]
        return True
```

#### Java

```java
class Solution {
    public boolean canReorderDoubled(int[] arr) {
        Map<Integer, Integer> freq = new HashMap<>();
        for (int v : arr) {
            freq.put(v, freq.getOrDefault(v, 0) + 1);
        }
        if ((freq.getOrDefault(0, 0) & 1) != 0) {
            return false;
        }
        List<Integer> keys = new ArrayList<>(freq.keySet());
        keys.sort(Comparator.comparingInt(Math::abs));
        for (int k : keys) {
            if (freq.getOrDefault(k << 1, 0) < freq.get(k)) {
                return false;
            }
            freq.put(k << 1, freq.getOrDefault(k << 1, 0) - freq.get(k));
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool canReorderDoubled(vector<int>& arr) {
        unordered_map<int, int> freq;
        for (int& v : arr) ++freq[v];
        if (freq[0] & 1) return false;
        vector<int> keys;
        for (auto& [k, _] : freq) keys.push_back(k);
        sort(keys.begin(), keys.end(), [](int a, int b) { return abs(a) < abs(b); });
        for (int& k : keys) {
            if (freq[k * 2] < freq[k]) return false;
            freq[k * 2] -= freq[k];
        }
        return true;
    }
};
```

#### Go

```go
func canReorderDoubled(arr []int) bool {
	freq := make(map[int]int)
	for _, v := range arr {
		freq[v]++
	}
	if freq[0]%2 != 0 {
		return false
	}
	var keys []int
	for k := range freq {
		keys = append(keys, k)
	}
	sort.Slice(keys, func(i, j int) bool {
		return abs(keys[i]) < abs(keys[j])
	})
	for _, k := range keys {
		if freq[k*2] < freq[k] {
			return false
		}
		freq[k*2] -= freq[k]
	}
	return true
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
