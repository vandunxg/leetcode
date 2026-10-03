---
comments: true
difficulty: Medium
rating: 1860
source: Weekly Contest 257 Q2
tags:
    - Stack
    - Greedy
    - Array
    - Sorting
    - Monotonic Stack
---

<!-- problem:start -->

# [1996. The Number of Weak Characters in the Game](https://leetcode.com/problems/the-number-of-weak-characters-in-the-game)

[中文文档](/solution/1900-1999/1996.The%20Number%20of%20Weak%20Characters%20in%20the%20Game/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn đang chơi một trò chơi có nhiều nhân vật, mỗi nhân vật có <strong>hai</strong> thuộc tính chính: <strong>tấn công</strong> và <strong>phòng thủ</strong>. Cho một mảng số nguyên 2D <code>properties</code>, trong đó <code>properties[i] = [attack<sub>i</sub>, defense<sub>i</sub>]</code> biểu thị các thuộc tính của nhân vật thứ <code>i<sup>th</sup></code> trong trò chơi.</p>

<p>Một nhân vật được gọi là <strong>yếu</strong> nếu có một nhân vật khác có <strong>cả</strong> chỉ số tấn công và phòng thủ đều <strong>lớn hơn nghiêm ngặt</strong> chỉ số tấn công và phòng thủ của nhân vật này. Cụ thể hơn, nhân vật <code>i</code> được gọi là <strong>yếu</strong> nếu tồn tại một nhân vật khác <code>j</code> sao cho <code>attack<sub>j</sub> &gt; attack<sub>i</sub></code> và <code>defense<sub>j</sub> &gt; defense<sub>i</sub></code>.</p>

<p>Trả về <em>số lượng nhân vật <strong>yếu</strong></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> properties = [[5,5],[6,3],[3,6]]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Không có nhân vật nào có cả chỉ số tấn công và phòng thủ đều lớn hơn nghiêm ngặt nhân vật khác.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> properties = [[2,2],[3,3]]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Nhân vật đầu tiên yếu vì nhân vật thứ hai có cả chỉ số tấn công và phòng thủ đều lớn hơn nghiêm ngặt.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> properties = [[1,5],[10,4],[4,3]]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Nhân vật thứ ba yếu vì nhân vật thứ hai có cả chỉ số tấn công và phòng thủ đều lớn hơn nghiêm ngặt.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= properties.length &lt;= 10<sup>5</sup></code></li>
	<li><code>properties[i].length == 2</code></li>
	<li><code>1 &lt;= attack<sub>i</sub>, defense<sub>i</sub> &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Duyệt

<!-- thinking:start -->

> **Tư duy**
>
> Một nhân vật yếu nếu có nhân vật khác vượt trội hơn ở cả tấn công và phòng thủ. Kiểm tra từng cặp có độ phức tạp bậc hai. Sắp xếp theo thứ tự tấn công giảm dần và phòng thủ tăng dần để các nhân vật đứng trước có chỉ số tấn công lớn hơn nghiêm ngặt, hoặc có chỉ số tấn công bằng nhau nhưng không thể thắng nghiêm ngặt.
>
> Theo dõi chỉ số phòng thủ lớn nhất đã gặp; nếu chỉ số phòng thủ hiện tại nhỏ hơn giá trị đó thì nhân vật hiện tại là nhân vật yếu.

<!-- thinking:end -->

Ta có thể sắp xếp tất cả nhân vật theo thứ tự giảm dần của chỉ số tấn công và tăng dần của chỉ số phòng thủ.

Sau đó, duyệt qua tất cả nhân vật. Với nhân vật hiện tại, nếu chỉ số phòng thủ của nhân vật đó nhỏ hơn chỉ số phòng thủ lớn nhất trước đó, đây là một nhân vật yếu và ta tăng đáp án lên một. Nếu không, ta cập nhật chỉ số phòng thủ lớn nhất.

Sau khi duyệt xong, ta thu được đáp án.

Độ phức tạp thời gian là $O(n \times \log n)$, còn độ phức tạp không gian là $O(\log n)$. Ở đây, $n$ là số lượng nhân vật.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def numberOfWeakCharacters(self, properties: List[List[int]]) -> int:
        properties.sort(key=lambda x: (-x[0], x[1]))
        ans = mx = 0
        for _, x in properties:
            ans += x < mx
            mx = max(mx, x)
        return ans
```

#### Java

```java
class Solution {
    public int numberOfWeakCharacters(int[][] properties) {
        Arrays.sort(properties, (a, b) -> b[0] - a[0] == 0 ? a[1] - b[1] : b[0] - a[0]);
        int ans = 0, mx = 0;
        for (var x : properties) {
            if (x[1] < mx) {
                ++ans;
            }
            mx = Math.max(mx, x[1]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numberOfWeakCharacters(vector<vector<int>>& properties) {
        sort(properties.begin(), properties.end(), [&](auto& a, auto& b) { return a[0] == b[0] ? a[1] < b[1] : a[0] > b[0]; });
        int ans = 0, mx = 0;
        for (auto& x : properties) {
            ans += x[1] < mx;
            mx = max(mx, x[1]);
        }
        return ans;
    }
};
```

#### Go

```go
func numberOfWeakCharacters(properties [][]int) (ans int) {
	sort.Slice(properties, func(i, j int) bool {
		a, b := properties[i], properties[j]
		if a[0] == b[0] {
			return a[1] < b[1]
		}
		return a[0] > b[0]
	})
	mx := 0
	for _, x := range properties {
		if x[1] < mx {
			ans++
		} else {
			mx = x[1]
		}
	}
	return
}
```

#### TypeScript

```ts
function numberOfWeakCharacters(properties: number[][]): number {
    properties.sort((a, b) => (a[0] == b[0] ? a[1] - b[1] : b[0] - a[0]));
    let ans = 0;
    let mx = 0;
    for (const [, x] of properties) {
        if (x < mx) {
            ans++;
        } else {
            mx = x;
        }
    }
    return ans;
}
```

#### JavaScript

```js
/**
 * @param {number[][]} properties
 * @return {number}
 */
var numberOfWeakCharacters = function (properties) {
    properties.sort((a, b) => (a[0] == b[0] ? a[1] - b[1] : b[0] - a[0]));
    let ans = 0;
    let mx = 0;
    for (const [, x] of properties) {
        if (x < mx) {
            ans++;
        } else {
            mx = x;
        }
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
