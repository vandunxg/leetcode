---
comments: true
difficulty: Medium
rating: 1896
source: Weekly Contest 383 Q3
tags:
    - Array
    - Matrix
---

<!-- problem:start -->

# [3030. Find the Grid of Region Average](https://leetcode.com/problems/find-the-grid-of-region-average)

[中文文档](/solution/3000-3099/3030.Find%20the%20Grid%20of%20Region%20Average/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một lưới <code>m x n</code> <code>image</code> biểu diễn một ảnh grayscale, trong đó <code>image[i][j]</code> là một pixel có cường độ nằm trong khoảng <code>[0..255]</code>. Bạn cũng được cho một số nguyên <strong>không âm</strong> <code>threshold</code>.</p>

<p>Hai pixel <strong>kề nhau</strong> nếu chúng có chung một cạnh.</p>

<p>Một <strong>vùng</strong> là một lưới con <code>3 x 3</code> trong đó <strong>độ chênh lệch tuyệt đối</strong> về cường độ giữa mọi cặp pixel <strong>kề nhau</strong> đều <strong>nhỏ hơn hoặc bằng</strong> <code>threshold</code>.</p>

<p>Tất cả các pixel trong một vùng đều thuộc về vùng đó. Lưu ý rằng một pixel có thể thuộc về <strong>nhiều</strong> vùng.</p>

<p>Bạn cần tính một lưới <code>m x n</code> <code>result</code>, trong đó <code>result[i][j]</code> là <strong>giá trị trung bình</strong> cường độ của các vùng mà <code>image[i][j]</code> thuộc về, được <strong>làm tròn xuống</strong> đến số nguyên gần nhất. Nếu <code>image[i][j]</code> thuộc về nhiều vùng, <code>result[i][j]</code> là <strong>giá trị trung bình</strong> của các <strong>cường độ trung bình đã làm tròn xuống</strong> của những vùng đó, rồi được <strong>làm tròn xuống</strong> đến số nguyên gần nhất. Nếu <code>image[i][j]</code> <strong>không</strong> thuộc về vùng nào, <code>result[i][j]</code> sẽ <strong>bằng</strong> <code>image[i][j]</code>.</p>

<p>Hãy trả về lưới <code>result</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">image = [[5,6,7,10],[8,9,10,10],[11,12,13,10]], threshold = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[[9,9,9,9],[9,9,9,9],[9,9,9,9]]</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3000-3099/3030.Find%20the%20Grid%20of%20Region%20Average/images/example0corrected.png" style="width: 832px; height: 275px;" /></p>

<p>Có hai vùng như minh họa ở trên. Cường độ trung bình của vùng đầu tiên là 9, còn cường độ trung bình của vùng thứ hai là 9.67, được làm tròn xuống thành 9. Cường độ trung bình của cả hai vùng là (9 + 9) / 2 = 9. Vì tất cả các pixel đều thuộc vùng 1, vùng 2 hoặc cả hai vùng, cường độ của mọi pixel trong result đều là 9.</p>

<p>Lưu ý rằng các giá trị đã làm tròn xuống được sử dụng khi tính giá trị trung bình của nhiều vùng. Vì vậy, phép tính sử dụng 9 làm cường độ trung bình của vùng 2, không phải 9.67.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">image = [[10,20,30],[15,25,35],[20,30,40],[25,35,45]], threshold = 12</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[[25,25,25],[27,27,27],[27,27,27],[30,30,30]]</span></p>

<p><strong>Giải thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3000-3099/3030.Find%20the%20Grid%20of%20Region%20Average/images/example1corrected.png" /></p>

<p>Có hai vùng như minh họa ở trên. Cường độ trung bình của vùng đầu tiên là 25, còn cường độ trung bình của vùng thứ hai là 30. Cường độ trung bình của cả hai vùng là (25 + 30) / 2 = 27.5, được làm tròn xuống thành 27.</p>

<p>Tất cả các pixel ở hàng 0 của ảnh đều thuộc vùng 1, nên tất cả các pixel ở hàng 0 trong result đều có giá trị 25. Tương tự, tất cả các pixel ở hàng 3 trong result đều có giá trị 30. Các pixel ở hàng 1 và 2 của ảnh thuộc vùng 1 và vùng 2, nên giá trị tương ứng trong result là 27.</p>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">image = [[5,6,7],[8,9,10],[11,12,13]], threshold = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[[5,6,7],[8,9,10],[11,12,13]]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Chỉ có một lưới con <code>3 x 3</code>, nhưng lưới này không thỏa điều kiện về độ chênh lệch giữa các pixel kề nhau. Chẳng hạn, độ chênh lệch giữa <code>image[0][0]</code> và <code>image[1][0]</code> là <code>|5 - 8| = 3 &gt; threshold = 1</code>. Không pixel nào thuộc về một vùng hợp lệ, nên <code>result</code> phải giống với <code>image</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= n, m &lt;= 500</code></li>
	<li><code>0 &lt;= image[i][j] &lt;= 255</code></li>
	<li><code>0 &lt;= threshold &lt;= 255</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Một khối $3 \times 3$ là một vùng khi mọi cặp pixel kề nhau có độ chênh lệch không vượt quá ngưỡng; một ô nhận giá trị trung bình của các vùng bao phủ nó. Vì $n,m \le 500$, ta có thể duyệt các khối trong $O(nm)$.
>
> Tính hợp lệ phụ thuộc vào mười hai cạnh bên trong. Mỗi khối hợp lệ sẽ cộng giá trị trung bình của nó vào mọi ô và tăng bộ đếm số vùng bao phủ.
>
> Ô có bộ đếm bằng 0 giữ nguyên giá trị ban đầu; ngược lại, ta chia tổng đã tích lũy cho bộ đếm.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def resultGrid(self, image: List[List[int]], threshold: int) -> List[List[int]]:
        n, m = len(image), len(image[0])
        ans = [[0] * m for _ in range(n)]
        ct = [[0] * m for _ in range(n)]
        for i in range(n - 2):
            for j in range(m - 2):
                region = True
                for k in range(3):
                    for l in range(2):
                        region &= (
                            abs(image[i + k][j + l] - image[i + k][j + l + 1])
                            <= threshold
                        )
                for k in range(2):
                    for l in range(3):
                        region &= (
                            abs(image[i + k][j + l] - image[i + k + 1][j + l])
                            <= threshold
                        )

                if region:
                    tot = 0
                    for k in range(3):
                        for l in range(3):
                            tot += image[i + k][j + l]
                    for k in range(3):
                        for l in range(3):
                            ct[i + k][j + l] += 1
                            ans[i + k][j + l] += tot // 9

        for i in range(n):
            for j in range(m):
                if ct[i][j] == 0:
                    ans[i][j] = image[i][j]
                else:
                    ans[i][j] //= ct[i][j]

        return ans
```

#### Java

```java
class Solution {
    public int[][] resultGrid(int[][] image, int threshold) {
        int n = image.length;
        int m = image[0].length;
        int[][] ans = new int[n][m];
        int[][] ct = new int[n][m];
        for (int i = 0; i + 2 < n; ++i) {
            for (int j = 0; j + 2 < m; ++j) {
                boolean region = true;
                for (int k = 0; k < 3; ++k) {
                    for (int l = 0; l < 2; ++l) {
                        region
                            &= Math.abs(image[i + k][j + l] - image[i + k][j + l + 1]) <= threshold;
                    }
                }
                for (int k = 0; k < 2; ++k) {
                    for (int l = 0; l < 3; ++l) {
                        region
                            &= Math.abs(image[i + k][j + l] - image[i + k + 1][j + l]) <= threshold;
                    }
                }
                if (region) {
                    int tot = 0;
                    for (int k = 0; k < 3; ++k) {
                        for (int l = 0; l < 3; ++l) {
                            tot += image[i + k][j + l];
                        }
                    }
                    for (int k = 0; k < 3; ++k) {
                        for (int l = 0; l < 3; ++l) {
                            ct[i + k][j + l]++;
                            ans[i + k][j + l] += tot / 9;
                        }
                    }
                }
            }
        }
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < m; ++j) {
                if (ct[i][j] == 0) {
                    ans[i][j] = image[i][j];
                } else {
                    ans[i][j] /= ct[i][j];
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
    vector<vector<int>> resultGrid(vector<vector<int>>& image, int threshold) {
        int n = image.size(), m = image[0].size();
        vector<vector<int>> ans(n, vector<int>(m));
        vector<vector<int>> ct(n, vector<int>(m));
        for (int i = 0; i + 2 < n; ++i) {
            for (int j = 0; j + 2 < m; ++j) {
                bool region = true;
                for (int k = 0; k < 3; ++k) {
                    for (int l = 0; l < 2; ++l) {
                        region &= abs(image[i + k][j + l] - image[i + k][j + l + 1]) <= threshold;
                    }
                }
                for (int k = 0; k < 2; ++k) {
                    for (int l = 0; l < 3; ++l) {
                        region &= abs(image[i + k][j + l] - image[i + k + 1][j + l]) <= threshold;
                    }
                }
                if (region) {
                    int tot = 0;
                    for (int k = 0; k < 3; ++k) {
                        for (int l = 0; l < 3; ++l) {
                            tot += image[i + k][j + l];
                        }
                    }
                    for (int k = 0; k < 3; ++k) {
                        for (int l = 0; l < 3; ++l) {
                            ct[i + k][j + l]++;
                            ans[i + k][j + l] += tot / 9;
                        }
                    }
                }
            }
        }
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < m; ++j) {
                if (ct[i][j] == 0) {
                    ans[i][j] = image[i][j];
                } else {
                    ans[i][j] /= ct[i][j];
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func resultGrid(image [][]int, threshold int) [][]int {
	n := len(image)
	m := len(image[0])
	ans := make([][]int, n)
	ct := make([][]int, n)
	for i := range ans {
		ans[i] = make([]int, m)
		ct[i] = make([]int, m)
	}
	for i := 0; i+2 < n; i++ {
		for j := 0; j+2 < m; j++ {
			region := true
			for k := 0; k < 3; k++ {
				for l := 0; l < 2; l++ {
					region = region && abs(image[i+k][j+l]-image[i+k][j+l+1]) <= threshold
				}
			}
			for k := 0; k < 2; k++ {
				for l := 0; l < 3; l++ {
					region = region && abs(image[i+k][j+l]-image[i+k+1][j+l]) <= threshold
				}
			}
			if region {
				tot := 0
				for k := 0; k < 3; k++ {
					for l := 0; l < 3; l++ {
						tot += image[i+k][j+l]
					}
				}
				for k := 0; k < 3; k++ {
					for l := 0; l < 3; l++ {
						ct[i+k][j+l]++
						ans[i+k][j+l] += tot / 9
					}
				}
			}
		}
	}
	for i := 0; i < n; i++ {
		for j := 0; j < m; j++ {
			if ct[i][j] == 0 {
				ans[i][j] = image[i][j]
			} else {
				ans[i][j] /= ct[i][j]
			}
		}
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
function resultGrid(image: number[][], threshold: number): number[][] {
    const n: number = image.length;
    const m: number = image[0].length;
    const ans: number[][] = new Array(n).fill(0).map(() => new Array(m).fill(0));
    const ct: number[][] = new Array(n).fill(0).map(() => new Array(m).fill(0));
    for (let i = 0; i + 2 < n; ++i) {
        for (let j = 0; j + 2 < m; ++j) {
            let region: boolean = true;
            for (let k = 0; k < 3; ++k) {
                for (let l = 0; l < 2; ++l) {
                    region &&= Math.abs(image[i + k][j + l] - image[i + k][j + l + 1]) <= threshold;
                }
            }
            for (let k = 0; k < 2; ++k) {
                for (let l = 0; l < 3; ++l) {
                    region &&= Math.abs(image[i + k][j + l] - image[i + k + 1][j + l]) <= threshold;
                }
            }
            if (region) {
                let tot: number = 0;

                for (let k = 0; k < 3; ++k) {
                    for (let l = 0; l < 3; ++l) {
                        tot += image[i + k][j + l];
                    }
                }
                for (let k = 0; k < 3; ++k) {
                    for (let l = 0; l < 3; ++l) {
                        ct[i + k][j + l]++;
                        ans[i + k][j + l] += Math.floor(tot / 9);
                    }
                }
            }
        }
    }
    for (let i = 0; i < n; ++i) {
        for (let j = 0; j < m; ++j) {
            if (ct[i][j] === 0) {
                ans[i][j] = image[i][j];
            } else {
                ans[i][j] = Math.floor(ans[i][j] / ct[i][j]);
            }
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
