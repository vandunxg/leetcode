---
comments: true
difficulty: Medium
rating: 1373
source: Weekly Contest 167 Q2
tags:
    - Enumeration
---

<!-- problem:start -->

# [1291. Sequential Digits](https://leetcode.com/problems/sequential-digits)

[中文文档](/solution/1200-1299/1291.Sequential%20Digits/README.md)

## Mô tả

<!-- description:start -->

<p>Một số nguyên có <em>các chữ số liên tiếp</em> khi và chỉ khi mỗi chữ số lớn hơn chữ số đứng trước nó đúng một đơn vị.</p>

<p>Hãy trả về danh sách <strong>đã sắp xếp</strong> gồm tất cả số nguyên trong đoạn <code>[low, high]</code> (bao gồm cả hai đầu mút) có các chữ số liên tiếp.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<pre><strong>Đầu vào:</strong> low = 100, high = 300
<strong>Đầu ra:</strong> [123,234]
</pre><p><strong class="example">Ví dụ 2:</strong></p>
<pre><strong>Đầu vào:</strong> low = 1000, high = 13000
<strong>Đầu ra:</strong> [1234,2345,3456,4567,5678,6789,12345]
</pre>
<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>10 &lt;= low &lt;= high &lt;= 10^9</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các chữ số liên tiếp tạo thành một dãy chữ số tăng liền nhau; chúng chỉ là các chuỗi con của $123456789$. $low$ và $high$ có thể lên tới $10^9$, nên duyệt từng giá trị sẽ lãng phí. Ta duyệt vị trí bắt đầu $i$ và kết thúc $j$, tạo số từ $i\ldots j$, giữ lại các số nằm trong đoạn yêu cầu rồi sắp xếp.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sequentialDigits(self, low: int, high: int) -> List[int]:
        ans = []
        for i in range(1, 9):
            x = i
            for j in range(i + 1, 10):
                x = x * 10 + j
                if low <= x <= high:
                    ans.append(x)
        return sorted(ans)
```

#### Java

```java
class Solution {
    public List<Integer> sequentialDigits(int low, int high) {
        List<Integer> ans = new ArrayList<>();
        for (int i = 1; i < 9; ++i) {
            int x = i;
            for (int j = i + 1; j < 10; ++j) {
                x = x * 10 + j;
                if (x >= low && x <= high) {
                    ans.add(x);
                }
            }
        }
        Collections.sort(ans);
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> sequentialDigits(int low, int high) {
        vector<int> ans;
        for (int i = 1; i < 9; ++i) {
            int x = i;
            for (int j = i + 1; j < 10; ++j) {
                x = x * 10 + j;
                if (x >= low && x <= high) {
                    ans.push_back(x);
                }
            }
        }
        sort(ans.begin(), ans.end());
        return ans;
    }
};
```

#### Go

```go
func sequentialDigits(low int, high int) (ans []int) {
	for i := 1; i < 9; i++ {
		x := i
		for j := i + 1; j < 10; j++ {
			x = x*10 + j
			if low <= x && x <= high {
				ans = append(ans, x)
			}
		}
	}
	sort.Ints(ans)
	return
}
```

#### TypeScript

```ts
function sequentialDigits(low: number, high: number): number[] {
    const ans: number[] = [];
    for (let i = 1; i < 9; ++i) {
        let x = i;
        for (let j = i + 1; j < 10; ++j) {
            x = x * 10 + j;
            if (x >= low && x <= high) {
                ans.push(x);
            }
        }
    }
    ans.sort((a, b) => a - b);
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
