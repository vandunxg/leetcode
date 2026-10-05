---
comments: true
difficulty: Medium
rating: 1347
source: Weekly Contest 489 Q2
tags:
    - Array
    - Hash Table
    - Counting
---

<!-- problem:start -->

# [3843. First Element with Unique Frequency](https://leetcode.com/problems/first-element-with-unique-frequency)

[中文文档](/solution/3800-3899/3843.First%20Element%20with%20Unique%20Frequency/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng số nguyên <code>nums</code>.</p>

<p>Hãy trả về một số nguyên biểu thị phần tử <strong>đầu tiên</strong> (quét từ trái sang phải) trong <code>nums</code> có <strong>tần suất</strong> là <strong>duy nhất</strong>. Nghĩa là không có số nguyên nào khác xuất hiện cùng số lần trong <code>nums</code>. Nếu không có phần tử như vậy, hãy trả về -1.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [20,10,30,30]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">30</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>20 xuất hiện một lần.</li>
	<li>10 xuất hiện một lần.</li>
	<li>30 xuất hiện hai lần.</li>
	<li>Tần suất của 30 là duy nhất vì không có số nguyên nào khác xuất hiện đúng hai lần.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [20,20,10,30,30,30]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">20</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>20 xuất hiện hai lần.</li>
	<li>10 xuất hiện một lần.</li>
	<li>30 xuất hiện 3 lần.</li>
	<li>Tần suất của 20, 10 và 30 đều là duy nhất. Phần tử đầu tiên có tần suất duy nhất là 20.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [10,10,20,20]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>10 xuất hiện hai lần.</li>
	<li>20 xuất hiện hai lần.</li>
	<li>Không có phần tử nào có tần suất duy nhất.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hash Table

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm giá trị ngoài cùng bên trái có tần suất duy nhất trong tất cả các tần suất. Vì $n \le 10^5$, cần một lời giải tuyến tính.
>
> Trước tiên, đếm số lần xuất hiện của mỗi giá trị, sau đó đếm xem có bao nhiêu giá trị cùng một tần suất.
>
> Chỉ cần hai bộ đếm: giá trị $\to$ tần suất và tần suất $\to$ số lượng giá trị có tần suất đó. Duyệt theo thứ tự ban đầu và trả về giá trị đầu tiên có số lượng bằng $1$.
>
> Nếu không có giá trị nào như vậy, trả về $-1$.

<!-- thinking:end -->

Ta dùng một hash table $\textit{cnt}$ để đếm số lần xuất hiện của mỗi phần tử, sau đó dùng một hash table khác $\textit{freq}$ để đếm số lượng tần suất xuất hiện. Cuối cùng, ta duyệt lại mảng $\textit{nums}$. Với mỗi phần tử $x$, nếu giá trị của $\textit{freq}[\textit{cnt}[x]]$ bằng 1, điều đó có nghĩa là tần suất xuất hiện của $x$ là duy nhất, nên ta trả về $x$. Nếu duyệt hết mảng mà không tìm thấy phần tử nào như vậy, ta trả về -1.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là độ dài của mảng $\textit{nums}$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def firstUniqueFreq(self, nums: List[int]) -> int:
        cnt = Counter(nums)
        freq = Counter(cnt.values())
        for x in nums:
            if freq[cnt[x]] == 1:
                return x
        return -1
```

#### Java

```java
class Solution {
    public int firstUniqueFreq(int[] nums) {
        Map<Integer, Integer> cnt = new HashMap<>();
        for (int x : nums) {
            cnt.merge(x, 1, Integer::sum);
        }
        Map<Integer, Integer> freq = new HashMap<>();
        for (int v : cnt.values()) {
            freq.merge(v, 1, Integer::sum);
        }
        for (int x : nums) {
            if (freq.get(cnt.get(x)) == 1) {
                return x;
            }
        }
        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int firstUniqueFreq(vector<int>& nums) {
        unordered_map<int, int> cnt;
        for (int x : nums) {
            ++cnt[x];
        }

        unordered_map<int, int> freq;
        for (auto& [_, v] : cnt) {
            ++freq[v];
        }

        for (int x : nums) {
            if (freq[cnt[x]] == 1) {
                return x;
            }
        }

        return -1;
    }
};
```

#### Go

```go
func firstUniqueFreq(nums []int) int {
	cnt := make(map[int]int)
	for _, x := range nums {
		cnt[x]++
	}

	freq := make(map[int]int)
	for _, v := range cnt {
		freq[v]++
	}

	for _, x := range nums {
		if freq[cnt[x]] == 1 {
			return x
		}
	}

	return -1
}
```

#### TypeScript

```ts
function firstUniqueFreq(nums: number[]): number {
    const cnt = new Map<number, number>();
    for (const x of nums) {
        cnt.set(x, (cnt.get(x) ?? 0) + 1);
    }

    const freq = new Map<number, number>();
    for (const v of cnt.values()) {
        freq.set(v, (freq.get(v) ?? 0) + 1);
    }

    for (const x of nums) {
        if (freq.get(cnt.get(x)!) === 1) {
            return x;
        }
    }

    return -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
