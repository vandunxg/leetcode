---
comments: true
difficulty: Easy
rating: 1176
source: Biweekly Contest 25 Q1
tags:
    - Array
---

<!-- problem:start -->

# [1431. Kids With the Greatest Number of Candies](https://leetcode.com/problems/kids-with-the-greatest-number-of-candies)

[中文文档](/solution/1400-1499/1431.Kids%20With%20the%20Greatest%20Number%20of%20Candies/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> đứa trẻ với số kẹo khác nhau. Cho một mảng số nguyên <code>candies</code>, trong đó mỗi <code>candies[i]</code> biểu thị số kẹo mà đứa trẻ thứ <code>i<sup>th</sup></code> có, và một số nguyên <code>extraCandies</code> biểu thị số kẹo thêm mà bạn có.</p>

<p>Hãy trả về <em>một mảng boolean </em><code>result</code><em> có độ dài </em><code>n</code><em>, trong đó </em><code>result[i]</code><em> là </em><code>true</code><em> nếu sau khi đưa toàn bộ </em><code>extraCandies</code><em> cho đứa trẻ thứ </em><code>i<sup>th</sup></code><em>, nó sẽ có số kẹo <strong>lớn nhất</strong> trong tất cả các đứa trẻ</em><em>, ngược lại là </em><code>false</code><em>.</em></p>

<p>Lưu ý rằng có thể có <strong>nhiều</strong> đứa trẻ cùng có số kẹo <strong>lớn nhất</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> candies = [2,3,5,1,3], extraCandies = 3
<strong>Đầu ra:</strong> [true,true,true,false,true]
<strong>Giải thích:</strong> Nếu đưa toàn bộ extraCandies cho:
- Đứa trẻ 1, nó sẽ có 2 + 3 = 5 viên kẹo, là số lớn nhất trong các đứa trẻ.
- Đứa trẻ 2, nó sẽ có 3 + 3 = 6 viên kẹo, là số lớn nhất trong các đứa trẻ.
- Đứa trẻ 3, nó sẽ có 5 + 3 = 8 viên kẹo, là số lớn nhất trong các đứa trẻ.
- Đứa trẻ 4, nó sẽ có 1 + 3 = 4 viên kẹo, không phải là số lớn nhất trong các đứa trẻ.
- Đứa trẻ 5, nó sẽ có 3 + 3 = 6 viên kẹo, là số lớn nhất trong các đứa trẻ.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> candies = [4,2,1,1,2], extraCandies = 1
<strong>Đầu ra:</strong> [true,false,false,false,false]
<strong>Giải thích:</strong> Chỉ có 1 viên kẹo thêm.
Đứa trẻ 1 luôn có số kẹo lớn nhất, ngay cả khi đưa viên kẹo thêm cho một đứa trẻ khác.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> candies = [12,1,12], extraCandies = 10
<strong>Đầu ra:</strong> [true,false,true]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == candies.length</code></li>
	<li><code>2 &lt;= n &lt;= 100</code></li>
	<li><code>1 &lt;= candies[i] &lt;= 100</code></li>
	<li><code>1 &lt;= extraCandies &lt;= 50</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> $n\le 100$. Tính giá trị lớn nhất hiện tại $mx$, sau đó kiểm tra xem số kẹo của từng đứa trẻ cộng với số kẹo thêm có lớn hơn hoặc bằng $mx$ hay không. Không cần mô phỏng việc phân phát kẹo.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def kidsWithCandies(self, candies: List[int], extraCandies: int) -> List[bool]:
        mx = max(candies)
        return [candy + extraCandies >= mx for candy in candies]
```

#### Java

```java
class Solution {
    public List<Boolean> kidsWithCandies(int[] candies, int extraCandies) {
        int mx = 0;
        for (int candy : candies) {
            mx = Math.max(mx, candy);
        }
        List<Boolean> res = new ArrayList<>();
        for (int candy : candies) {
            res.add(candy + extraCandies >= mx);
        }
        return res;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<bool> kidsWithCandies(vector<int>& candies, int extraCandies) {
        int mx = *max_element(candies.begin(), candies.end());
        vector<bool> res;
        for (int candy : candies) {
            res.push_back(candy + extraCandies >= mx);
        }
        return res;
    }
};
```

#### Go

```go
func kidsWithCandies(candies []int, extraCandies int) (ans []bool) {
	mx := slices.Max(candies)
	for _, candy := range candies {
		ans = append(ans, candy+extraCandies >= mx)
	}
	return
}
```

#### TypeScript

```ts
function kidsWithCandies(candies: number[], extraCandies: number): boolean[] {
    const max = candies.reduce((r, v) => Math.max(r, v));
    return candies.map(v => v + extraCandies >= max);
}
```

#### Rust

```rust
impl Solution {
    pub fn kids_with_candies(candies: Vec<i32>, extra_candies: i32) -> Vec<bool> {
        let max = *candies.iter().max().unwrap();
        candies.iter().map(|v| v + extra_candies >= max).collect()
    }
}
```

#### PHP

```php
class Solution {
    /**
     * @param Integer[] $candies
     * @param Integer $extraCandies
     * @return Boolean[]
     */
    function kidsWithCandies($candies, $extraCandies) {
        $max = max($candies);
        $rs = [];
        for ($i = 0; $i < count($candies); $i++) {
            array_push($rs, $candies[$i] + $extraCandies >= $max);
        }
        return $rs;
    }
}
```

#### C

```c
#define max(a, b) (((a) > (b)) ? (a) : (b))
/**
 * Note: The returned array must be malloced, assume caller calls free().
 */
bool* kidsWithCandies(int* candies, int candiesSize, int extraCandies, int* returnSize) {
    int mx = 0;
    for (int i = 0; i < candiesSize; i++) {
        mx = max(mx, candies[i]);
    }
    bool* ans = malloc(candiesSize * sizeof(bool));
    for (int i = 0; i < candiesSize; i++) {
        ans[i] = candies[i] + extraCandies >= mx;
    }
    *returnSize = candiesSize;
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
