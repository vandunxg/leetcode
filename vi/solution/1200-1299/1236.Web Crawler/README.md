---
comments: true
difficulty: Medium
tags:
    - Depth-First Search
    - Breadth-First Search
    - String
    - Interactive
---

<!-- problem:start -->

# [1236. Web Crawler 🔒](https://leetcode.com/problems/web-crawler)

[中文文档](/solution/1200-1299/1236.Web%20Crawler/README.md)

## Mô tả

<!-- description:start -->

<p>Cho URL <code>startUrl</code> và interface <code>HtmlParser</code>, hãy triển khai web crawler để thu thập mọi liên kết có <strong>cùng hostname</strong> với <code>startUrl</code>.</p>

<p>Trả về tất cả URL mà web crawler tìm được theo <strong>bất kỳ</strong> thứ tự nào.</p>

<p>Crawler cần thực hiện các việc sau:</p>

<ul>
	<li>Bắt đầu từ trang: <code>startUrl</code></li>
	<li>Gọi <code>HtmlParser.getUrls(url)</code> để lấy tất cả URL từ trang web có URL đã cho.</li>
	<li>Không crawl cùng một liên kết hai lần.</li>
	<li>Chỉ duyệt các liên kết có <strong>cùng hostname</strong> với <code>startUrl</code>.</li>
</ul>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1236.Web%20Crawler/images/urlhostname.png" style="width: 600px; height: 164px;" /></p>

<p>Như ví dụ URL ở trên, hostname là <code>example.org</code>. Để đơn giản, có thể giả sử mọi URL đều dùng <strong>giao thức http</strong> và không chỉ định <strong>port</strong>. Ví dụ, <code>http://leetcode.com/problems</code> và <code>http://leetcode.com/contest</code> có cùng hostname, còn <code>http://example.org/test</code> và <code>http://example.com/abc</code> thì không.</p>

<p>Interface <code>HtmlParser</code> được định nghĩa như sau:</p>

<pre>
interface HtmlParser {
  // Return a list of all urls from a webpage of given <em>url</em>.
  public List&lt;String&gt; getUrls(String url);
}</pre>

<p>Dưới đây là hai ví dụ minh họa chức năng của bài toán. Khi custom test, bạn sẽ có ba biến <code data-stringify-type="code">urls</code>, <code data-stringify-type="code">edges</code> và <code data-stringify-type="code">startUrl</code>. Lưu ý, trong code bạn chỉ truy cập được <code data-stringify-type="code">startUrl</code>; <code data-stringify-type="code">urls</code> và <code data-stringify-type="code">edges</code> không thể được truy cập trực tiếp.</p>

<p>Lưu ý: URL có dấu gạch chéo &quot;/&quot; ở cuối được xem là URL khác. Ví dụ, &quot;http://news.yahoo.com&quot; và &quot;http://news.yahoo.com/&quot; là hai URL khác nhau.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1236.Web%20Crawler/images/sample_2_1497.png" style="width: 610px; height: 300px;" /></p>

<pre>
<strong>Đầu vào:
</strong>urls = [
&nbsp; &quot;http://news.yahoo.com&quot;,
&nbsp; &quot;http://news.yahoo.com/news&quot;,
&nbsp; &quot;http://news.yahoo.com/news/topics/&quot;,
&nbsp; &quot;http://news.google.com&quot;,
&nbsp; &quot;http://news.yahoo.com/us&quot;
]
edges = [[2,0],[2,1],[3,2],[3,1],[0,4]]
startUrl = &quot;http://news.yahoo.com/news/topics/&quot;
<strong>Đầu ra:</strong> [
&nbsp; &quot;http://news.yahoo.com&quot;,
&nbsp; &quot;http://news.yahoo.com/news&quot;,
&nbsp; &quot;http://news.yahoo.com/news/topics/&quot;,
&nbsp; &quot;http://news.yahoo.com/us&quot;
]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1236.Web%20Crawler/images/sample_3_1497.png" style="width: 540px; height: 270px;" /></strong></p>

<pre>
<strong>Đầu vào:</strong> 
urls = [
&nbsp; &quot;http://news.yahoo.com&quot;,
&nbsp; &quot;http://news.yahoo.com/news&quot;,
&nbsp; &quot;http://news.yahoo.com/news/topics/&quot;,
&nbsp; &quot;http://news.google.com&quot;
]
edges = [[0,2],[2,1],[3,2],[3,1],[3,0]]
startUrl = &quot;http://news.google.com&quot;
<strong>Đầu ra:</strong> [&quot;http://news.google.com&quot;]
<strong>Giải thích: </strong>startUrl liên kết đến tất cả các trang khác không cùng hostname.</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= urls.length &lt;= 1000</code></li>
	<li><code>1 &lt;= urls[i].length &lt;= 300</code></li>
	<li><code>startUrl</code> là một trong các URL trong <code>urls</code>.</li>
	<li>Nhãn hostname dài từ 1 đến 63 ký tự, tính cả dấu chấm; chỉ được chứa chữ cái ASCII từ &#39;a&#39; đến &#39;z&#39;, chữ số từ &#39;0&#39; đến &#39;9&#39; và dấu gạch nối (&#39;-&#39;).</li>
	<li>Hostname không được bắt đầu hoặc kết thúc bằng dấu gạch nối (&#39;-&#39;).</li>
	<li>Xem thêm:&nbsp;&nbsp;<a href="https://en.wikipedia.org/wiki/Hostname#Restrictions_on_valid_hostnames">https://en.wikipedia.org/wiki/Hostname#Restrictions_on_valid_hostnames</a></li>
	<li>Có thể giả sử thư viện URL không chứa URL trùng lặp.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta chỉ được truy cập URL thuộc cùng host với URL bắt đầu; có tối đa $1000$ trang. Mở rộng từ trang bắt đầu bằng $getUrls$ tương đương duyệt đồ thị, và mỗi URL chỉ được truy cập một lần.
>
> Một set ghi lại các URL đã thăm. DFS đi vào URL chưa thăm và chỉ theo các cạnh có cùng host. Host là phần nằm sau $http://$ đến dấu gạch chéo tiếp theo. Set loại bỏ trùng lặp, còn bước kiểm tra host giới hạn phạm vi tìm kiếm.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
# """
# This is HtmlParser's API interface.
# You should not implement it, or speculate about its implementation
# """
# class HtmlParser(object):
#    def getUrls(self, url):
#        """
#        :type url: str
#        :rtype List[str]
#        """


class Solution:
    def crawl(self, startUrl: str, htmlParser: 'HtmlParser') -> List[str]:
        def host(url):
            url = url[7:]
            return url.split('/')[0]

        def dfs(url):
            if url in ans:
                return
            ans.add(url)
            for next in htmlParser.getUrls(url):
                if host(url) == host(next):
                    dfs(next)

        ans = set()
        dfs(startUrl)
        return list(ans)
```

#### Java

```java
/**
 * // This is the HtmlParser's API interface.
 * // You should not implement it, or speculate about its implementation
 * interface HtmlParser {
 *     public List<String> getUrls(String url) {}
 * }
 */

class Solution {
    private Set<String> ans;

    public List<String> crawl(String startUrl, HtmlParser htmlParser) {
        ans = new HashSet<>();
        dfs(startUrl, htmlParser);
        return new ArrayList<>(ans);
    }

    private void dfs(String url, HtmlParser htmlParser) {
        if (ans.contains(url)) {
            return;
        }
        ans.add(url);
        for (String next : htmlParser.getUrls(url)) {
            if (host(next).equals(host(url))) {
                dfs(next, htmlParser);
            }
        }
    }

    private String host(String url) {
        url = url.substring(7);
        return url.split("/")[0];
    }
}
```

#### C++

```cpp
/**
 * // This is the HtmlParser's API interface.
 * // You should not implement it, or speculate about its implementation
 * class HtmlParser {
 *   public:
 *     vector<string> getUrls(string url);
 * };
 */

class Solution {
public:
    vector<string> ans;
    unordered_set<string> vis;

    vector<string> crawl(string startUrl, HtmlParser htmlParser) {
        dfs(startUrl, htmlParser);
        return ans;
    }

    void dfs(string& url, HtmlParser& htmlParser) {
        if (vis.count(url)) return;
        vis.insert(url);
        ans.push_back(url);
        for (string next : htmlParser.getUrls(url))
            if (host(url) == host(next))
                dfs(next, htmlParser);
    }

    string host(string url) {
        int i = 7;
        string res;
        for (; i < url.size(); ++i) {
            if (url[i] == '/') break;
            res += url[i];
        }
        return res;
    }
};
```

#### Go

```go
/**
 * // This is HtmlParser's API interface.
 * // You should not implement it, or speculate about its implementation
 * type HtmlParser struct {
 *     func GetUrls(url string) []string {}
 * }
 */

func crawl(startUrl string, htmlParser HtmlParser) []string {
	var ans []string
	vis := make(map[string]bool)
	var dfs func(url string)
	host := func(url string) string {
		return strings.Split(url[7:], "/")[0]
	}
	dfs = func(url string) {
		if vis[url] {
			return
		}
		vis[url] = true
		ans = append(ans, url)
		for _, next := range htmlParser.GetUrls(url) {
			if host(next) == host(url) {
				dfs(next)
			}
		}
	}
	dfs(startUrl)
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
