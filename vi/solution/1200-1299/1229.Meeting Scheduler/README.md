---
comments: true
difficulty: Medium
rating: 1541
source: Biweekly Contest 11 Q2
tags:
    - Array
    - Two Pointers
    - Sorting
---

<!-- problem:start -->

# [1229. Meeting Scheduler 🔒](https://leetcode.com/problems/meeting-scheduler)

[中文文档](/solution/1200-1299/1229.Meeting%20Scheduler/README.md)

## Mô tả

<!-- description:start -->

<p>Cho hai mảng khung giờ rảnh <code>slots1</code> và <code>slots2</code> của hai người, cùng thời lượng cuộc họp <code>duration</code>. Hãy trả về <strong>khung giờ sớm nhất</strong> phù hợp với cả hai người và có thời lượng ít nhất <code>duration</code>.</p>

<p>Nếu không có khung giờ chung nào thỏa mãn yêu cầu, trả về <strong>mảng rỗng</strong>.</p>

<p>Khung giờ có dạng mảng gồm hai phần tử <code>[start, end]</code>, biểu diễn khoảng thời gian bao gồm cả <code>start</code> và <code>end</code>.</p>

<p>Đảm bảo không có hai khung giờ rảnh nào của cùng một người giao nhau. Cụ thể, với hai khung giờ <code>[start1, end1]</code> và <code>[start2, end2]</code> của cùng một người, hoặc <code>start1 &gt; end2</code>, hoặc <code>start2 &gt; end1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> slots1 = [[10,50],[60,120],[140,210]], slots2 = [[0,15],[60,70]], duration = 8
<strong>Output:</strong> [60,68]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> slots1 = [[10,50],[60,120],[140,210]], slots2 = [[0,15],[60,70]], duration = 12
<strong>Output:</strong> []
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= slots1.length, slots2.length &lt;= 10<sup>4</sup></code></li>
	<li><code>slots1[i].length, slots2[i].length == 2</code></li>
	<li><code>slots1[i][0] &lt; slots1[i][1]</code></li>
	<li><code>slots2[i][0] &lt; slots2[i][1]</code></li>
	<li><code>0 &lt;= slots1[i][j], slots2[i][j] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= duration &lt;= 10<sup>6</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Sắp xếp + Two Pointers

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi người có tối đa $10^4$ khung giờ; kiểm tra mọi cặp sẽ tốn $O(mn)$. Sau khi sắp xếp theo thời điểm bắt đầu, phần giao chỉ cần xét hai khung giờ hiện tại: thời điểm bắt đầu là giá trị lớn hơn của hai thời điểm bắt đầu, còn thời điểm kết thúc là giá trị nhỏ hơn của hai thời điểm kết thúc.
>
> Nếu khoảng giao đủ dài, trả về khoảng sớm nhất như vậy. Nếu không, khung giờ kết thúc trước không thể tạo ra một khoảng giao hợp lệ sớm hơn với các khung giờ tiếp theo của người kia, nên ta tiến con trỏ tương ứng.
>
> Sắp xếp giúp xét các khả năng theo thứ tự thời gian; mỗi bước, two pointers loại bỏ một khung giờ không thể tạo đáp án, nên số lần so sánh là tuyến tính.

<!-- thinking:end -->

Ta sắp xếp các khung giờ rảnh của cả hai người, rồi dùng two pointers duyệt hai mảng để tìm phần giao giữa các khung giờ. Nếu độ dài phần giao lớn hơn hoặc bằng `duration`, trả về thời điểm bắt đầu phần giao và thời điểm bắt đầu cộng `duration`. Nếu không, khi khung giờ của người thứ nhất kết thúc sớm hơn khung giờ của người thứ hai, tiến con trỏ thứ nhất; nếu không thì tiến con trỏ thứ hai. Tiếp tục cho đến khi tìm được khung giờ phù hợp hoặc duyệt hết.

Độ phức tạp thời gian là $O(m \times \log m + n \times \log n)$ và độ phức tạp không gian là $O(\log m + \log n)$. Trong đó, $m$ và $n$ lần lượt là độ dài của hai mảng.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minAvailableDuration(
        self, slots1: List[List[int]], slots2: List[List[int]], duration: int
    ) -> List[int]:
        slots1.sort()
        slots2.sort()
        m, n = len(slots1), len(slots2)
        i = j = 0
        while i < m and j < n:
            start = max(slots1[i][0], slots2[j][0])
            end = min(slots1[i][1], slots2[j][1])
            if end - start >= duration:
                return [start, start + duration]
            if slots1[i][1] < slots2[j][1]:
                i += 1
            else:
                j += 1
        return []
```

#### Java

```java
class Solution {
    public List<Integer> minAvailableDuration(int[][] slots1, int[][] slots2, int duration) {
        Arrays.sort(slots1, (a, b) -> a[0] - b[0]);
        Arrays.sort(slots2, (a, b) -> a[0] - b[0]);
        int m = slots1.length, n = slots2.length;
        int i = 0, j = 0;
        while (i < m && j < n) {
            int start = Math.max(slots1[i][0], slots2[j][0]);
            int end = Math.min(slots1[i][1], slots2[j][1]);
            if (end - start >= duration) {
                return Arrays.asList(start, start + duration);
            }
            if (slots1[i][1] < slots2[j][1]) {
                ++i;
            } else {
                ++j;
            }
        }
        return Collections.emptyList();
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> minAvailableDuration(vector<vector<int>>& slots1, vector<vector<int>>& slots2, int duration) {
        sort(slots1.begin(), slots1.end());
        sort(slots2.begin(), slots2.end());
        int m = slots1.size(), n = slots2.size();
        int i = 0, j = 0;
        while (i < m && j < n) {
            int start = max(slots1[i][0], slots2[j][0]);
            int end = min(slots1[i][1], slots2[j][1]);
            if (end - start >= duration) {
                return {start, start + duration};
            }
            if (slots1[i][1] < slots2[j][1]) {
                ++i;
            } else {
                ++j;
            }
        }
        return {};
    }
};
```

#### Go

```go
func minAvailableDuration(slots1 [][]int, slots2 [][]int, duration int) []int {
	sort.Slice(slots1, func(i, j int) bool { return slots1[i][0] < slots1[j][0] })
	sort.Slice(slots2, func(i, j int) bool { return slots2[i][0] < slots2[j][0] })
	i, j, m, n := 0, 0, len(slots1), len(slots2)
	for i < m && j < n {
		start := max(slots1[i][0], slots2[j][0])
		end := min(slots1[i][1], slots2[j][1])
		if end-start >= duration {
			return []int{start, start + duration}
		}
		if slots1[i][1] < slots2[j][1] {
			i++
		} else {
			j++
		}
	}
	return []int{}
}
```

#### TypeScript

```ts
function minAvailableDuration(slots1: number[][], slots2: number[][], duration: number): number[] {
    slots1.sort((a, b) => a[0] - b[0]);
    slots2.sort((a, b) => a[0] - b[0]);
    const [m, n] = [slots1.length, slots2.length];
    let [i, j] = [0, 0];
    while (i < m && j < n) {
        const [start1, end1] = slots1[i];
        const [start2, end2] = slots2[j];
        const start = Math.max(start1, start2);
        const end = Math.min(end1, end2);
        if (end - start >= duration) {
            return [start, start + duration];
        }
        if (end1 < end2) {
            i++;
        } else {
            j++;
        }
    }
    return [];
}
```

#### Rust

```rust
impl Solution {
    pub fn min_available_duration(mut slots1: Vec<Vec<i32>>, mut slots2: Vec<Vec<i32>>, duration: i32) -> Vec<i32> {
        slots1.sort_by_key(|slot| slot[0]);
        slots2.sort_by_key(|slot| slot[0]);

        let (mut i, mut j) = (0, 0);
        let (m, n) = (slots1.len(), slots2.len());

        while i < m && j < n {
            let (start1, end1) = (slots1[i][0], slots1[i][1]);
            let (start2, end2) = (slots2[j][0], slots2[j][1]);

            let start = start1.max(start2);
            let end = end1.min(end2);

            if end - start >= duration {
                return vec![start, start + duration];
            }

            if end1 < end2 {
                i += 1;
            } else {
                j += 1;
            }
        }

        vec![]
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
