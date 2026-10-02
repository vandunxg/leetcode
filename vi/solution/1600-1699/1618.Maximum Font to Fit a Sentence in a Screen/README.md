---
comments: true
difficulty: Medium
tags:
    - Array
    - String
    - Binary Search
    - Interactive
---

<!-- problem:start -->

# [1618. Maximum Font to Fit a Sentence in a Screen 🔒](https://leetcode.com/problems/maximum-font-to-fit-a-sentence-in-a-screen)

[中文文档](/solution/1600-1699/1618.Maximum%20Font%20to%20Fit%20a%20Sentence%20in%20a%20Screen/README.md)

## Mô tả

<!-- description:start -->

<p>Cho chuỗi <code>text</code>. Ta muốn hiển thị <code>text</code> trên màn hình rộng <code>w</code> và cao <code>h</code>. Bạn có thể chọn cỡ chữ trong mảng <code>fonts</code>, chứa các cỡ chữ khả dụng theo <strong>thứ tự tăng dần</strong>.</p>

<p>Bạn có thể dùng interface <code>FontInfo</code> để lấy chiều rộng và chiều cao của mỗi ký tự ở bất kỳ cỡ chữ khả dụng nào.</p>

<p>Interface <code>FontInfo</code> được định nghĩa như sau:</p>

<pre>
interface FontInfo {
  // Returns the width of character ch on the screen using font size fontSize.
  // O(1) per call
  public int getWidth(int fontSize, char ch);

  // Returns the height of any character on the screen using font size fontSize.
  // O(1) per call
  public int getHeight(int fontSize);
}</pre>

<p>Chiều rộng tính được của <code>text</code> với một <code>fontSize</code> là <strong>tổng</strong> mọi lần gọi <code>getWidth(fontSize, text[i])</code> với <code>0 &lt;= i &lt; text.length</code> (<strong>đánh chỉ số từ 0</strong>). Chiều cao là <code>getHeight(fontSize)</code>. Lưu ý rằng <code>text</code> được hiển thị trên <strong>một dòng</strong>.</p>

<p>Đảm bảo <code>FontInfo</code> trả về cùng giá trị khi gọi <code>getHeight</code> hoặc <code>getWidth</code> với cùng tham số.</p>

<p>Ngoài ra, với mọi cỡ chữ <code>fontSize</code> và ký tự <code>ch</code>, đảm bảo rằng:</p>

<ul>
	<li><code>getHeight(fontSize) &lt;= getHeight(fontSize+1)</code></li>
	<li><code>getWidth(fontSize, ch) &lt;= getWidth(fontSize+1, ch)</code></li>
</ul>

<p>Trả về <em>cỡ chữ lớn nhất có thể dùng để hiển thị </em><code>text</code><em> trên màn hình</em>. Nếu <code>text</code> không vừa với bất kỳ cỡ chữ nào, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> text = &quot;helloworld&quot;, w = 80, h = 20, fonts = [6,8,10,12,14,16,18,24,36]
<strong>Output:</strong> 6
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> text = &quot;leetcode&quot;, w = 1000, h = 50, fonts = [1,2,4]
<strong>Output:</strong> 4
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> text = &quot;easyquestion&quot;, w = 100, h = 100, fonts = [10,15,20,25]
<strong>Output:</strong> -1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= text.length &lt;= 50000</code></li>
	<li><code>text</code> contains only lowercase English letters.</li>
	<li><code>1 &lt;= w &lt;= 10<sup>7</sup></code></li>
	<li><code>1 &lt;= h &lt;= 10<sup>4</sup></code></li>
	<li><code>1 &lt;= fonts.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= fonts[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>fonts</code> is sorted in ascending order and does not contain duplicates.</li>
</ul>

<!-- description:end -->

## Lời giải

Các tham số chính là <code>text</code> và <code>fontSize</code>.

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Các cỡ chữ đã được sắp xếp và cỡ lớn hơn khó vừa hơn, nên tính khả thi đơn điệu theo chỉ số và ta có thể tìm nhị phân cỡ lớn nhất còn vừa.
>
> Một cỡ chữ vừa khi chiều cao không quá $h$ và tổng chiều rộng các ký tự không quá $w$, đều được tính qua $\texttt{FontInfo}$.
>
> Tìm nhị phân trên khoảng chỉ số, $\texttt{check}$ phần tử giữa và tăng đầu trái khi thành công. Cuối cùng kiểm tra $\textit{fonts}[\textit{left}]$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
# """
# This is FontInfo's API interface.
# You should not implement it, or speculate about its implementation
# """
# class FontInfo(object):
#    Return the width of char ch when fontSize is used.
#    def getWidth(self, fontSize, ch):
#        """
#        :type fontSize: int
#        :type ch: char
#        :rtype int
#        """
#
#    def getHeight(self, fontSize):
#        """
#        :type fontSize: int
#        :rtype int
#        """
class Solution:
    def maxFont(
        self, text: str, w: int, h: int, fonts: List[int], fontInfo: 'FontInfo'
    ) -> int:
        def check(size):
            if fontInfo.getHeight(size) > h:
                return False
            return sum(fontInfo.getWidth(size, c) for c in text) <= w

        left, right = 0, len(fonts) - 1
        ans = -1
        while left < right:
            mid = (left + right + 1) >> 1
            if check(fonts[mid]):
                left = mid
            else:
                right = mid - 1
        return fonts[left] if check(fonts[left]) else -1
```

#### Java

```java
/**
 * // This is the FontInfo's API interface.
 * // You should not implement it, or speculate about its implementation
 * interface FontInfo {
 *     // Return the width of char ch when fontSize is used.
 *     public int getWidth(int fontSize, char ch) {}
 *     // Return Height of any char when fontSize is used.
 *     public int getHeight(int fontSize)
 * }
 */
class Solution {
    public int maxFont(String text, int w, int h, int[] fonts, FontInfo fontInfo) {
        int left = 0, right = fonts.length - 1;
        while (left < right) {
            int mid = (left + right + 1) >> 1;
            if (check(text, fonts[mid], w, h, fontInfo)) {
                left = mid;
            } else {
                right = mid - 1;
            }
        }
        return check(text, fonts[left], w, h, fontInfo) ? fonts[left] : -1;
    }

    private boolean check(String text, int size, int w, int h, FontInfo fontInfo) {
        if (fontInfo.getHeight(size) > h) {
            return false;
        }
        int width = 0;
        for (char c : text.toCharArray()) {
            width += fontInfo.getWidth(size, c);
        }
        return width <= w;
    }
}
```

#### C++

```cpp
/**
 * // This is the FontInfo's API interface.
 * // You should not implement it, or speculate about its implementation
 * class FontInfo {
 *   public:
 *     // Return the width of char ch when fontSize is used.
 *     int getWidth(int fontSize, char ch);
 *
 *     // Return Height of any char when fontSize is used.
 *     int getHeight(int fontSize)
 * };
 */
class Solution {
public:
    int maxFont(string text, int w, int h, vector<int>& fonts, FontInfo fontInfo) {
        auto check = [&](int size) {
            if (fontInfo.getHeight(size) > h) return false;
            int width = 0;
            for (char& c : text) {
                width += fontInfo.getWidth(size, c);
            }
            return width <= w;
        };
        int left = 0, right = fonts.size() - 1;
        while (left < right) {
            int mid = (left + right + 1) >> 1;
            if (check(fonts[mid])) {
                left = mid;
            } else {
                right = mid - 1;
            }
        }
        return check(fonts[left]) ? fonts[left] : -1;
    }
};
```

#### JavaScript

```js
/**
 * // This is the FontInfo's API interface.
 * // You should not implement it, or speculate about its implementation
 * function FontInfo() {
 *
 *		@param {number} fontSize
 *		@param {char} ch
 *     	@return {number}
 *     	this.getWidth = function(fontSize, ch) {
 *      	...
 *     	};
 *
 *		@param {number} fontSize
 *     	@return {number}
 *     	this.getHeight = function(fontSize) {
 *      	...
 *     	};
 * };
 */
/**
 * @param {string} text
 * @param {number} w
 * @param {number} h
 * @param {number[]} fonts
 * @param {FontInfo} fontInfo
 * @return {number}
 */
var maxFont = function (text, w, h, fonts, fontInfo) {
    const check = function (size) {
        if (fontInfo.getHeight(size) > h) {
            return false;
        }
        let width = 0;
        for (const c of text) {
            width += fontInfo.getWidth(size, c);
        }
        return width <= w;
    };
    let left = 0;
    let right = fonts.length - 1;
    while (left < right) {
        const mid = (left + right + 1) >> 1;
        if (check(fonts[mid])) {
            left = mid;
        } else {
            right = mid - 1;
        }
    }
    return check(fonts[left]) ? fonts[left] : -1;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
