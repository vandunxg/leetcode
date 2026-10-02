---
comments: true
difficulty: Medium
tags:
    - Array
    - Two Pointers
    - Binary Search
    - Sorting
---

<!-- problem:start -->

# [475. Heaters](https://leetcode.com/problems/heaters)

[中文文档](/solution/0400-0499/0475.Heaters/README.md)

## Mô tả

<!-- description:start -->

<p>Mùa đông sắp đến! Trong cuộc thi, nhiệm vụ đầu tiên của bạn là thiết kế một máy sưởi có bán kính tỏa nhiệt cố định để sưởi ấm tất cả ngôi nhà.</p>

<p>Một ngôi nhà được sưởi ấm nếu nằm trong phạm vi bán kính tỏa nhiệt của máy sưởi.</p>

<p>Cho vị trí của <code>houses</code> và <code>heaters</code> trên một đường thẳng ngang, hãy trả về <em>bán kính tỏa nhiệt nhỏ nhất cần đặt cho các máy sưởi để chúng có thể sưởi ấm tất cả ngôi nhà</em>.</p>

<p><strong>Lưu ý</strong> rằng tất cả <code>heaters</code> đều dùng cùng bán kính tỏa nhiệt mà bạn chọn.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> houses = [1,2,3], heaters = [2]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Máy sưởi duy nhất được đặt tại vị trí 2. Nếu bán kính tỏa nhiệt là 1 thì tất cả ngôi nhà đều được sưởi ấm.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> houses = [1,2,3,4], heaters = [1,4]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Hai máy sưởi được đặt tại vị trí 1 và 4. Bán kính tỏa nhiệt 1 là đủ để sưởi ấm tất cả ngôi nhà.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> houses = [1,5], heaters = [2]
<strong>Đầu ra:</strong> 3
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= houses.length, heaters.length &lt;= 3 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= houses[i], heaters[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Máy sưởi phải phủ được mọi ngôi nhà; ta cần tìm bán kính chung nhỏ nhất. Có quá nhiều giá trị bán kính để thử lần lượt, nhưng nếu một bán kính khả thi thì mọi bán kính lớn hơn cũng khả thi.
>
> Sắp xếp cả hai mảng rồi dùng tìm kiếm nhị phân trên $r$. Dùng hai con trỏ để kiểm tra, với mỗi máy sưởi ta xét phạm vi phủ $[h-r,h+r]$: nếu nhà nằm bên trái phạm vi thì không thể thỏa mãn; nếu nằm bên phải thì chuyển sang máy sưởi tiếp theo.
>
> Vì cả hai dãy đã được sắp xếp, các con trỏ chỉ di chuyển về phía trước nên mỗi lần kiểm tra chạy tuyến tính.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findRadius(self, houses: List[int], heaters: List[int]) -> int:
        houses.sort()
        heaters.sort()

        def check(r):
            m, n = len(houses), len(heaters)
            i = j = 0
            while i < m:
                if j >= n:
                    return False
                mi = heaters[j] - r
                mx = heaters[j] + r
                if houses[i] < mi:
                    return False
                if houses[i] > mx:
                    j += 1
                else:
                    i += 1
            return True

        left, right = 0, int(1e9)
        while left < right:
            mid = (left + right) >> 1
            if check(mid):
                right = mid
            else:
                left = mid + 1
        return left
```

#### Java

```java
class Solution {
    public int findRadius(int[] houses, int[] heaters) {
        Arrays.sort(houses);
        Arrays.sort(heaters);
        int left = 0, right = (int) 1e9;
        while (left < right) {
            int mid = (left + right) >> 1;
            if (check(houses, heaters, mid)) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }

    private boolean check(int[] houses, int[] heaters, int r) {
        int m = houses.length, n = heaters.length;
        int i = 0, j = 0;
        while (i < m) {
            if (j >= n) {
                return false;
            }
            int mi = heaters[j] - r;
            int mx = heaters[j] + r;
            if (houses[i] < mi) {
                return false;
            }
            if (houses[i] > mx) {
                ++j;
            } else {
                ++i;
            }
        }
        return true;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findRadius(vector<int>& houses, vector<int>& heaters) {
        sort(houses.begin(), houses.end());
        sort(heaters.begin(), heaters.end());
        int left = 0, right = 1e9;
        while (left < right) {
            int mid = left + right >> 1;
            if (check(houses, heaters, mid))
                right = mid;
            else
                left = mid + 1;
        }
        return left;
    }

    bool check(vector<int>& houses, vector<int>& heaters, int r) {
        int m = houses.size(), n = heaters.size();
        int i = 0, j = 0;
        while (i < m) {
            if (j >= n) return false;
            int mi = heaters[j] - r;
            int mx = heaters[j] + r;
            if (houses[i] < mi) return false;
            if (houses[i] > mx)
                ++j;
            else
                ++i;
        }
        return true;
    }
};
```

#### Go

```go
func findRadius(houses []int, heaters []int) int {
	sort.Ints(houses)
	sort.Ints(heaters)
	m, n := len(houses), len(heaters)

	check := func(r int) bool {
		var i, j int
		for i < m {
			if j >= n {
				return false
			}
			mi, mx := heaters[j]-r, heaters[j]+r
			if houses[i] < mi {
				return false
			}
			if houses[i] > mx {
				j++
			} else {
				i++
			}
		}
		return true
	}
	left, right := 0, int(1e9)
	for left < right {
		mid := (left + right) >> 1
		if check(mid) {
			right = mid
		} else {
			left = mid + 1
		}
	}
	return left
}
```

#### TypeScript

```ts
function findRadius(houses: number[], heaters: number[]): number {
    houses.sort((a, b) => a - b);
    heaters.sort((a, b) => a - b);
    const check = (r: number): boolean => {
        const m = houses.length;
        const n = heaters.length;
        let i = 0;
        let j = 0;
        while (i < m) {
            if (j >= n) {
                return false;
            }
            const mi = heaters[j] - r;
            const mx = heaters[j] + r;
            if (houses[i] < mi) {
                return false;
            }
            if (houses[i] > mx) {
                ++j;
            } else {
                ++i;
            }
        }
        return true;
    };
    let left = 0;
    let right = 1e9;
    while (left < right) {
        const mid = (left + right) >> 1;
        if (check(mid)) {
            right = mid;
        } else {
            left = mid + 1;
        }
    }
    return left;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
