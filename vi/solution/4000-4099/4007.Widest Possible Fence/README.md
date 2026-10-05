---
comments: true
difficulty: Hard
rating: 1908
source: Biweekly Contest 188 Q2
tags:
    - Array
    - Hash Table
    - Counting
    - Enumeration
---

<!-- problem:start -->

# [4007. Widest Possible Fence](https://leetcode.com/problems/widest-possible-fence)

[中文文档](/solution/4000-4099/4007.Widest%20Possible%20Fence/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một mảng số nguyên <code>planks</code>, trong đó <code>planks[i]</code> là chiều cao của thanh gỗ thứ <code>i<sup>th</sup></code>. Mỗi thanh gỗ có chiều rộng 1 đơn vị.</p>

<p>Bạn muốn xây một hàng rào gồm các thanh gỗ có cùng <strong>chiều cao</strong>.</p>

<p>Bạn có thể giữ nguyên một thanh gỗ hoặc kết hợp <strong>chính xác</strong> hai thanh gỗ ban đầu khác nhau thành một thanh duy nhất có chiều cao <strong>bằng</strong> tổng chiều cao của chúng. Mỗi thanh gỗ ban đầu được dùng <strong>nhiều nhất</strong> một lần và không nhất thiết phải dùng tất cả các thanh gỗ ban đầu.</p>

<p>Trả về <strong>chiều rộng lớn nhất</strong> của hàng rào có thể xây dựng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">planks = [1,3,2,5,7,5,4,2,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p>Ta có thể tạo bốn thanh gỗ có chiều cao 5.</p>

<ul>
\t<li><code>planks[3] = 5</code></li>
\t<li><code>planks[5] = 5</code></li>
\t<li><code>planks[0] + planks[6] = 1 + 4 = 5</code></li>
\t<li><code>planks[1] + planks[2] = 3 + 2 = 5</code></li>
</ul>

<p>Vậy chiều rộng lớn nhất là 4.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">planks = [2,3,7]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
\t<li>Không thể tạo hai thanh gỗ có cùng chiều cao, kể cả sau khi kết hợp hai thanh gỗ ban đầu khác nhau.</li>
\t<li>Vì không cần dùng tất cả các thanh gỗ ban đầu, ta có thể chọn bất kỳ một thanh gỗ nào làm hàng rào.</li>
\t<li>Do đó, chiều rộng lớn nhất có thể là 1.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
\t<li><code>1 &lt;= planks.length &lt;= 1000</code></li>
\t<li><code>1 &lt;= planks[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đếm + Liệt kê

<!-- thinking:start -->

> **Tư duy**
>
> Mọi cột của hàng rào phải có cùng chiều cao. Một thanh gỗ có thể được dùng riêng, hai thanh gỗ bằng nhau có thể được xếp cùng nhau, hoặc hai thanh gỗ có chiều cao khác nhau có thể được xếp cùng nhau. Việc tìm kiếm từng tổ hợp thanh gỗ sẽ tăng quá nhanh theo $n$.
>
> Sau khi đếm các chiều cao, mỗi chiều cao đích có thể đến từ chính chiều cao $h$, hai thanh gỗ có chiều cao $h/2$, hoặc một cặp $x+y=h$. Một thanh gỗ không thể tham gia hai cặp cùng lúc, nên ta có thể cộng trực tiếp số cột của các cặp chiều cao.
>
> Với $n\le 1000$, việc liệt kê các cặp chiều cao có độ phức tạp $O(m^2)$. Đáp án là số cột lớn nhất trong các trường hợp đó.

<!-- thinking:end -->

Trước hết, ta dùng hash table $\textit{cnt}$ để đếm số thanh gỗ ở mỗi chiều cao.

Với chiều cao đích $h$, số thanh gỗ có chiều cao $h$ mà ta có thể tạo gồm ba phần:

- Dùng trực tiếp các thanh gỗ có chiều cao $h$, tạo ra $\textit{cnt}[h]$ thanh gỗ;
- Nếu $h$ chẵn, hai thanh gỗ có chiều cao $h/2$ có thể được kết hợp thành một thanh, tạo ra $\lfloor \textit{cnt}[h/2] / 2 \rfloor$ thanh;
- Với mỗi cặp chiều cao $x + y = h$ và $x < y$, ta có thể kết hợp $\min(\textit{cnt}[x], \textit{cnt}[y])$ thanh.

Với một $h$ cố định, ba phần này và các cặp chiều cao khác nhau $(x, h - x)$ sử dụng những tập thanh gỗ ban đầu không trùng nhau, nên có thể cộng trực tiếp.

Ta duyệt từng chiều cao $x$ trong $\textit{cnt}$ và cộng đóng góp vào một hash table khác $t$:

- $t[x] \mathrel{+}= \textit{cnt}[x]$, dùng trực tiếp các thanh gỗ có chiều cao $x$;
- $t[2x] \mathrel{+}= \lfloor \textit{cnt}[x] / 2 \rfloor$, ghép hai thanh gỗ có chiều cao $x$;
- Với mỗi chiều cao $y > x$, $t[x + y] \mathrel{+}= \min(\textit{cnt}[x], \textit{cnt}[y])$, kết hợp các thanh gỗ có chiều cao $x$ và $y$.

Đáp án là giá trị lớn nhất trong $t$.

Độ phức tạp thời gian là $O(n + m^2)$, và độ phức tạp không gian là $O(m)$, trong đó $n$ là số thanh gỗ và $m$ là số chiều cao khác nhau, với $m \leq n$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximumWidth(self, planks: list[int]) -> int:
        cnt = Counter(planks)
        ans = 0
        t = defaultdict(int)
        for x, v1 in cnt.items():
            t[x] += v1
            t[x * 2] += v1 // 2
            for y, v2 in cnt.items():
                if y > x:
                    t[x + y] += min(v1, v2)
        return max(t.values())
```

#### Java

```java
class Solution {
    public int maximumWidth(int[] planks) {
        Map<Integer, Integer> cnt = new HashMap<>();
        for (int x : planks) {
            cnt.merge(x, 1, Integer::sum);
        }

        Map<Integer, Integer> t = new HashMap<>();
        int ans = 0;

        for (var e1 : cnt.entrySet()) {
            int x = e1.getKey();
            int v1 = e1.getValue();

            t.merge(x, v1, Integer::sum);
            ans = Math.max(ans, t.get(x));

            t.merge(x * 2, v1 / 2, Integer::sum);
            ans = Math.max(ans, t.get(x * 2));

            for (var e2 : cnt.entrySet()) {
                int y = e2.getKey();
                int v2 = e2.getValue();
                if (y > x) {
                    int key = x + y;
                    t.merge(key, Math.min(v1, v2), Integer::sum);
                    ans = Math.max(ans, t.get(key));
                }
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
    int maximumWidth(vector<int>& planks) {
        unordered_map<int, int> cnt;
        for (int x : planks) {
            cnt[x]++;
        }

        unordered_map<int, int> t;
        int ans = 0;

        for (auto& [x, v1] : cnt) {
            t[x] += v1;
            ans = max(ans, t[x]);

            t[x * 2] += v1 / 2;
            ans = max(ans, t[x * 2]);

            for (auto& [y, v2] : cnt) {
                if (y > x) {
                    t[x + y] += min(v1, v2);
                    ans = max(ans, t[x + y]);
                }
            }
        }

        return ans;
    }
};
```

#### Go

```go
func maximumWidth(planks []int) int {
	cnt := make(map[int]int)
	for _, x := range planks {
		cnt[x]++
	}

	t := make(map[int]int)
	ans := 0

	for x, v1 := range cnt {
		t[x] += v1
		if t[x] > ans {
			ans = t[x]
		}

		t[x*2] += v1 / 2
		if t[x*2] > ans {
			ans = t[x*2]
		}

		for y, v2 := range cnt {
			if y > x {
				key := x + y
				if v1 < v2 {
					t[key] += v1
				} else {
					t[key] += v2
				}
				if t[key] > ans {
					ans = t[key]
				}
			}
		}
	}

	return ans
}
```

#### TypeScript

```ts
function maximumWidth(planks: number[]): number {
    const cnt = new Map<number, number>();
    for (const x of planks) {
        cnt.set(x, (cnt.get(x) ?? 0) + 1);
    }

    const t = new Map<number, number>();
    let ans = 0;

    for (const [x, v1] of cnt) {
        t.set(x, (t.get(x) ?? 0) + v1);
        ans = Math.max(ans, t.get(x)!);

        t.set(x * 2, (t.get(x * 2) ?? 0) + Math.floor(v1 / 2));
        ans = Math.max(ans, t.get(x * 2)!);

        for (const [y, v2] of cnt) {
            if (y > x) {
                const key = x + y;
                t.set(key, (t.get(key) ?? 0) + Math.min(v1, v2));
                ans = Math.max(ans, t.get(key)!);
            }
        }
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
