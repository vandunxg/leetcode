---
comments: true
difficulty: Medium
rating: 1471
source: Weekly Contest 142 Q1
tags:
    - Array
    - Math
    - Probability and Statistics
---

<!-- problem:start -->

# [1093. Statistics from a Large Sample](https://leetcode.com/problems/statistics-from-a-large-sample)

[中文文档](/solution/1000-1099/1093.Statistics%20from%20a%20Large%20Sample/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mẫu lớn gồm các số nguyên trong khoảng <code>[0, 255]</code>. Vì mẫu quá lớn, nó được biểu diễn bằng mảng <code>count</code>, trong đó <code>count[k]</code> là <strong>số lần</strong> giá trị <code>k</code> xuất hiện trong mẫu.</p>

<p>Hãy tính các thống kê sau:</p>

<ul>
	<li><code>minimum</code>: Giá trị nhỏ nhất trong mẫu.</li>
	<li><code>maximum</code>: Giá trị lớn nhất trong mẫu.</li>
	<li><code>mean</code>: Giá trị trung bình của mẫu, được tính bằng tổng tất cả phần tử chia cho số lượng phần tử.</li>
	<li><code>median</code>:
	<ul>
		<li>Nếu mẫu có số phần tử lẻ, <code>median</code> là phần tử ở giữa sau khi sắp xếp mẫu.</li>
		<li>Nếu mẫu có số phần tử chẵn, <code>median</code> là trung bình cộng của hai phần tử ở giữa sau khi sắp xếp mẫu.</li>
	</ul>
	</li>
	<li><code>mode</code>: Giá trị xuất hiện nhiều lần nhất trong mẫu. Đảm bảo giá trị này là <strong>duy nhất</strong>.</li>
</ul>

<p>Trả về <em>các thống kê của mẫu dưới dạng mảng số thực </em><code>[minimum, maximum, mean, median, mode]</code><em>. Chấp nhận đáp án sai lệch tối đa </em><code>10<sup>-5</sup></code><em> so với kết quả thực.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> count = [0,1,3,4,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0]
<strong>Output:</strong> [1.00000,3.00000,2.37500,2.50000,3.00000]
<strong>Giải thích:</strong> Mẫu được biểu diễn bởi count là [1,2,2,2,3,3,3,3].
Giá trị nhỏ nhất và lớn nhất lần lượt là 1 và 3.
Giá trị trung bình là (1+2+2+2+3+3+3+3) / 8 = 19 / 8 = 2.375.
Vì mẫu có số phần tử chẵn, median là trung bình cộng của hai phần tử ở giữa là 2 và 3, bằng 2.5.
Mode là 3 vì giá trị này xuất hiện nhiều nhất trong mẫu.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> count = [0,4,3,2,2,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0]
<strong>Output:</strong> [1.00000,4.00000,2.18182,2.00000,1.00000]
<strong>Giải thích:</strong> Mẫu được biểu diễn bởi count là [1,1,1,1,2,2,2,3,3,4,4].
Giá trị nhỏ nhất và lớn nhất lần lượt là 1 và 4.
Giá trị trung bình là (1+1+1+1+2+2+2+3+3+4+4) / 11 = 24 / 11 = 2.18181818... (kết quả hiển thị số đã làm tròn 2.18182).
Vì mẫu có số phần tử lẻ, median là phần tử ở giữa, bằng 2.
Mode là 1 vì giá trị này xuất hiện nhiều nhất trong mẫu.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>count.length == 256</code></li>
	<li><code>0 &lt;= count[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= sum(count) &lt;= 10<sup>9</sup></code></li>
	<li>Mode của mẫu được biểu diễn bởi <code>count</code> là <strong>duy nhất</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mẫu là bảng tần suất trên miền $0..255$ với tối đa $10^9$ phần tử nên không thể bung thành danh sách đầy đủ. Có thể tìm min, max, tổng và mode bằng một lượt duyệt; median là phần tử thứ $k$ theo tổng tần suất tích lũy.
>
> Lượt duyệt cập nhật $mi,mx,s,cnt$ và mode (tần suất lớn nhất). `find(i)` duyệt `count` cho đến khi tổng tích lũy đạt $i$.
>
> Nếu số phần tử lẻ, lấy giá trị ở giữa; nếu chẵn, lấy trung bình cộng của hai giá trị trung tâm. Giá trị trung bình bằng $s/cnt$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def sampleStats(self, count: List[int]) -> List[float]:
        def find(i: int) -> int:
            t = 0
            for k, x in enumerate(count):
                t += x
                if t >= i:
                    return k

        mi, mx = inf, -1
        s = cnt = 0
        mode = 0
        for k, x in enumerate(count):
            if x:
                mi = min(mi, k)
                mx = max(mx, k)
                s += k * x
                cnt += x
                if x > count[mode]:
                    mode = k

        median = (
            find(cnt // 2 + 1) if cnt & 1 else (find(cnt // 2) + find(cnt // 2 + 1)) / 2
        )
        return [mi, mx, s / cnt, median, mode]
```

#### Java

```java
class Solution {
    private int[] count;

    public double[] sampleStats(int[] count) {
        this.count = count;
        int mi = 1 << 30, mx = -1;
        long s = 0;
        int cnt = 0;
        int mode = 0;
        for (int k = 0; k < count.length; ++k) {
            if (count[k] > 0) {
                mi = Math.min(mi, k);
                mx = Math.max(mx, k);
                s += 1L * k * count[k];
                cnt += count[k];
                if (count[k] > count[mode]) {
                    mode = k;
                }
            }
        }
        double median
            = cnt % 2 == 1 ? find(cnt / 2 + 1) : (find(cnt / 2) + find(cnt / 2 + 1)) / 2.0;
        return new double[] {mi, mx, s * 1.0 / cnt, median, mode};
    }

    private int find(int i) {
        for (int k = 0, t = 0;; ++k) {
            t += count[k];
            if (t >= i) {
                return k;
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<double> sampleStats(vector<int>& count) {
        auto find = [&](int i) -> int {
            for (int k = 0, t = 0;; ++k) {
                t += count[k];
                if (t >= i) {
                    return k;
                }
            }
        };
        int mi = 1 << 30, mx = -1;
        long long s = 0;
        int cnt = 0, mode = 0;
        for (int k = 0; k < count.size(); ++k) {
            if (count[k]) {
                mi = min(mi, k);
                mx = max(mx, k);
                s += 1LL * k * count[k];
                cnt += count[k];
                if (count[k] > count[mode]) {
                    mode = k;
                }
            }
        }
        double median = cnt % 2 == 1 ? find(cnt / 2 + 1) : (find(cnt / 2) + find(cnt / 2 + 1)) / 2.0;
        return vector<double>{(double) mi, (double) mx, s * 1.0 / cnt, median, (double) mode};
    }
};
```

#### Go

```go
func sampleStats(count []int) []float64 {
	find := func(i int) int {
		for k, t := 0, 0; ; k++ {
			t += count[k]
			if t >= i {
				return k
			}
		}
	}
	mi, mx := 1<<30, -1
	s, cnt, mode := 0, 0, 0
	for k, x := range count {
		if x > 0 {
			mi = min(mi, k)
			mx = max(mx, k)
			s += k * x
			cnt += x
			if x > count[mode] {
				mode = k
			}
		}
	}
	var median float64
	if cnt&1 == 1 {
		median = float64(find(cnt/2 + 1))
	} else {
		median = float64(find(cnt/2)+find(cnt/2+1)) / 2
	}
	return []float64{float64(mi), float64(mx), float64(s) / float64(cnt), median, float64(mode)}
}
```

#### TypeScript

```ts
function sampleStats(count: number[]): number[] {
    const find = (i: number): number => {
        for (let k = 0, t = 0; ; ++k) {
            t += count[k];
            if (t >= i) {
                return k;
            }
        }
    };
    let mi = 1 << 30;
    let mx = -1;
    let [s, cnt, mode] = [0, 0, 0];
    for (let k = 0; k < count.length; ++k) {
        if (count[k] > 0) {
            mi = Math.min(mi, k);
            mx = Math.max(mx, k);
            s += k * count[k];
            cnt += count[k];
            if (count[k] > count[mode]) {
                mode = k;
            }
        }
    }
    const median =
        cnt % 2 === 1 ? find((cnt >> 1) + 1) : (find(cnt >> 1) + find((cnt >> 1) + 1)) / 2;
    return [mi, mx, s / cnt, median, mode];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
