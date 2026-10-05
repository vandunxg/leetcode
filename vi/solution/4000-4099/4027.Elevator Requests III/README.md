---
comments: true
difficulty: Hard
rating: 2200
source: Weekly Contest 515 Q4
tags:
    - Bit Manipulation
    - Array
    - Dynamic Programming
    - Bitmask
    - Sorting
---

<!-- problem:start -->

# [4027. Elevator Requests III](https://leetcode.com/problems/elevator-requests-iii)

[中文文档](/solution/4000-4099/4027.Elevator%20Requests%20III/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên <code>n</code> biểu thị số tầng trong một tòa nhà, các tầng được đánh số từ 0 đến <code>n - 1</code>.</p>

<p>Đồng thời, cho một số nguyên <code>start</code> và một mảng số nguyên 2 chiều <code>requests</code>, trong đó <code>requests[i] = [arrival<sub>i</sub>, floor<sub>i</sub>]</code> cho biết một yêu cầu đến <code>floor<sub>i</sub></code> được tạo tại thời điểm <code>arrival<sub>i</sub></code>.</p>

<p>Tại thời điểm 0, thang máy đang ở tầng <code>start</code>.</p>

<p>Mỗi giây, thang máy có thể đi <strong>lên</strong> 1 tầng, đi <strong>xuống</strong> 1 tầng hoặc <strong>đứng yên</strong> tại tầng hiện tại.</p>

<p>Một yêu cầu chỉ có thể được thực hiện <strong>tại hoặc sau</strong> thời điểm đến; yêu cầu được thực hiện <strong>ngay lập tức</strong> khi thang máy ở tầng được yêu cầu vào bất kỳ thời điểm nào từ thời điểm đến trở đi.</p>

<p>Trả về thời gian <strong>nhỏ nhất</strong> cần để thực hiện tất cả yêu cầu.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 9, start = 0, requests = [[0,8],[6,5]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">9</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Di chuyển từ tầng 0 (<code>start</code>) đến tầng 5 (<code>requests[1][1]</code>) trong 5 giây, đến nơi tại thời điểm 5. Vì <code>requests[1][0] = 6</code>, chờ đến thời điểm 6 để thực hiện yêu cầu.</li>
	<li>Di chuyển từ tầng 5 đến tầng 8 (<code>requests[0][1]</code>) trong 3 giây, thực hiện yêu cầu tại thời điểm 9.</li>
</ul>

<p>Vậy tất cả yêu cầu được thực hiện trước hoặc tại thời điểm 9.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 8, start = 5, requests = [[1,7],[7,3]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">7</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Di chuyển từ tầng 5 (<code>start</code>) đến tầng 7 (<code>requests[0][1]</code>) trong 2 giây, đến nơi tại thời điểm 2. Vì <code>requests[0][0] = 1</code> đã trôi qua, yêu cầu ở tầng 7 được thực hiện tại thời điểm 2.</li>
	<li>Di chuyển từ tầng 7 đến tầng 3 (<code>requests[1][1]</code>) trong 4 giây, đến nơi tại thời điểm 6. Vì <code>requests[1][0] = 7</code>, chờ đến thời điểm 7.</li>
</ul>

<p>Vậy tất cả yêu cầu được thực hiện trước hoặc tại thời điểm 7.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 7, start = 3, requests = [[0,5],[0,1],[6,3]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">8</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Di chuyển từ tầng 3 (<code>start</code>) đến tầng 5 (<code>requests[0][1]</code>) trong 2 giây, thực hiện yêu cầu tại thời điểm 2.</li>
	<li>Di chuyển từ tầng 5 đến tầng 1 (<code>requests[1][1]</code>) trong 4 giây, thực hiện yêu cầu tại thời điểm 6.</li>
	<li>Di chuyển từ tầng 1 đến tầng 3 (<code>requests[2][1]</code>) trong 2 giây, đến nơi tại thời điểm 8. Yêu cầu của tầng này đến tại <code>requests[2][0] = 6</code>, vì vậy tầng 3 được thực hiện tại thời điểm 8.</li>
</ul>

<p>Vậy tất cả yêu cầu được thực hiện trước hoặc tại thời điểm 8.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= requests.length &lt;= 16</code></li>
	<li><code>requests[i] == [arrival<sub>i</sub>, floor<sub>i</sub>]</code></li>
	<li><code>0 &lt;= arrival<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= start, floor<sub>i</sub> &lt;= n - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DP nén trạng thái

<!-- thinking:start -->

> **Tư duy**
>
> Chỉ số tầng có thể lên tới $10^9$, nhưng chỉ có $m\le 16$ yêu cầu, đúng bằng quy mô của bài toán TSP trên các tập con. Thời điểm đến là các cận dưới: nếu đến nơi sớm thì phải chờ.
>
> $f[S][j]$ là thời điểm sớm nhất để thực hiện tập $S$ và kết thúc tại yêu cầu $j$. Từ tập rỗng, chi phí là $\max(|\textit{start}-\textit{floor}_j|,\textit{arrival}_j)$; ngược lại, ta duyệt yêu cầu trước đó $j_0$ và lấy $\max(f[S\setminus\{j\}][j_0]+\text{distance},\textit{arrival}_j)$.
>
> Lấy giá trị nhỏ nhất theo chỉ số cuối trên toàn bộ tập là thời điểm hoàn thành mọi yêu cầu.

<!-- thinking:end -->

Số tầng $n$ có thể lớn tới $10^9$, nhưng có nhiều nhất $m \le 16$ yêu cầu, nên ta chỉ cần lập kế hoạch cho một đường đi qua nhiều nhất $m$ tầng đích.

Đây là bài toán người du lịch với các ràng buộc về thời điểm đến. Gọi $f[i][j]$ là thời gian nhỏ nhất để thực hiện tập yêu cầu được biểu diễn bởi bitmask $i$, trong đó yêu cầu $j$ được thực hiện cuối cùng.

Với mỗi trạng thái $i$ chứa yêu cầu $j$, gọi $i_0 = i \oplus 2^j$:

- Nếu $i_0 = 0$, ta bắt đầu từ $\textit{start}$, thời gian là $\max(|\textit{start} - \textit{floor}_j|, \textit{arrival}_j)$;
- Ngược lại, ta duyệt yêu cầu trước đó $j_0$, thời gian là $\max(f[i_0][j_0] + |\textit{floor}_{j_0} - \textit{floor}_j|, \textit{arrival}_j)$.

Đáp án là giá trị nhỏ nhất của $f[2^m-1][j]$ với mọi $j$.

Độ phức tạp thời gian là $O(m^2 \times 2^m)$, độ phức tạp không gian là $O(m \times 2^m)$, trong đó $m$ là số lượng yêu cầu.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def elevatorRequests(self, n: int, start: int, requests: list[list[int]]) -> int:
        m = len(requests)
        f = [[0] * m for _ in range(1 << m)]
        for i in range(1 << m):
            for j in range(m):
                if i >> j & 1:
                    f[i][j] = inf
                    i0 = i ^ (1 << j)
                    if i0 == 0:
                        d = abs(start - requests[j][1])
                        f[i][j] = min(f[i][j], max(d, requests[j][0]))
                    else:
                        for j0 in range(m):
                            if j0 != j and (i >> j0 & 1):
                                d = abs(requests[j0][1] - requests[j][1])
                                f[i][j] = min(
                                    f[i][j], max(f[i0][j0] + d, requests[j][0])
                                )
        return min(f[(1 << m) - 1][j] for j in range(m))
```

#### Java

```java
class Solution {
    public long elevatorRequests(int n, int start, int[][] requests) {
        int m = requests.length;
        long[][] f = new long[1 << m][m];

        for (int i = 0; i < (1 << m); i++) {
            for (int j = 0; j < m; j++) {
                if (((i >> j) & 1) == 1) {
                    f[i][j] = Long.MAX_VALUE;
                    int i0 = i ^ (1 << j);

                    if (i0 == 0) {
                        long d = Math.abs(start - requests[j][1]);
                        f[i][j] = Math.min(f[i][j], Math.max(d, requests[j][0]));
                    } else {
                        for (int j0 = 0; j0 < m; j0++) {
                            if (j0 != j && ((i >> j0) & 1) == 1) {
                                long d = Math.abs(requests[j0][1] - requests[j][1]);

                                f[i][j]
                                    = Math.min(f[i][j], Math.max(f[i0][j0] + d, requests[j][0]));
                            }
                        }
                    }
                }
            }
        }

        long ans = Long.MAX_VALUE;

        for (int j = 0; j < m; j++) {
            ans = Math.min(ans, f[(1 << m) - 1][j]);
        }

        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long elevatorRequests(int n, int start, vector<vector<int>>& requests) {
        int m = requests.size();

        vector<vector<long long>> f(1 << m, vector<long long>(m, 0));

        for (int i = 0; i < (1 << m); i++) {
            for (int j = 0; j < m; j++) {
                if ((i >> j) & 1) {
                    f[i][j] = LLONG_MAX;
                    int i0 = i ^ (1 << j);

                    if (i0 == 0) {
                        long long d = abs(start - requests[j][1]);

                        f[i][j] = min(
                            f[i][j],
                            max(d, (long long) requests[j][0]));
                    } else {
                        for (int j0 = 0; j0 < m; j0++) {
                            if (j0 != j && ((i >> j0) & 1)) {
                                long long d = abs(
                                    requests[j0][1] - requests[j][1]);

                                f[i][j] = min(
                                    f[i][j],
                                    max(
                                        f[i0][j0] + d,
                                        (long long) requests[j][0]));
                            }
                        }
                    }
                }
            }
        }

        long long ans = LLONG_MAX;
        for (int j = 0; j < m; j++) {
            ans = min(ans, f[(1 << m) - 1][j]);
        }

        return ans;
    }
};
```

#### Go

```go
func elevatorRequests(n int, start int, requests [][]int) int64 {
	m := len(requests)
	f := make([][]int64, 1<<m)

	for i := range f {
		f[i] = make([]int64, m)
	}

	const INF int64 = 1 << 60

	for i := 0; i < 1<<m; i++ {
		for j := 0; j < m; j++ {
			if (i>>j)&1 == 1 {
				f[i][j] = INF
				i0 := i ^ (1 << j)

				if i0 == 0 {
					d := int64(abs(start - requests[j][1]))
					f[i][j] = min(
						f[i][j],
						max(d, int64(requests[j][0])),
					)
				} else {
					for j0 := 0; j0 < m; j0++ {
						if j0 != j && (i>>j0)&1 == 1 {
							d := int64(abs(
								requests[j0][1] - requests[j][1],
							))

							f[i][j] = min(
								f[i][j],
								max(
									f[i0][j0]+d,
									int64(requests[j][0]),
								),
							)
						}
					}
				}
			}
		}
	}

	full := (1 << m) - 1
	ans := INF

	for j := 0; j < m; j++ {
		ans = min(ans, f[full][j])
	}

	return ans
}

func abs(x int) int {
	if x < 0 {
		return -x
	}
	return x
}
```

#### TypeScript

```ts
function elevatorRequests(n: number, start: number, requests: number[][]): number {
    const m = requests.length;
    const f: number[][] = Array.from({ length: 1 << m }, () => Array(m).fill(0));

    for (let i = 0; i < 1 << m; i++) {
        for (let j = 0; j < m; j++) {
            if (((i >> j) & 1) === 1) {
                f[i][j] = Infinity;

                const i0 = i ^ (1 << j);

                if (i0 === 0) {
                    const d = Math.abs(start - requests[j][1]);

                    f[i][j] = Math.min(f[i][j], Math.max(d, requests[j][0]));
                } else {
                    for (let j0 = 0; j0 < m; j0++) {
                        if (j0 !== j && ((i >> j0) & 1) === 1) {
                            const d = Math.abs(requests[j0][1] - requests[j][1]);

                            f[i][j] = Math.min(f[i][j], Math.max(f[i0][j0] + d, requests[j][0]));
                        }
                    }
                }
            }
        }
    }

    const full = (1 << m) - 1;
    let ans = Infinity;

    for (let j = 0; j < m; j++) {
        ans = Math.min(ans, f[full][j]);
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
