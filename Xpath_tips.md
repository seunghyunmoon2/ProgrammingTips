# useful tips when using xpath for crawling
---

> [stackoverflow](https://stackoverflow.com/questions/21455349/xpath-query-get-attribute-href-from-a-tag)

For the following HTML document:

```bash
<html>
  <body>
    <a href="http://www.example.com">Example</a> 
    <a href="http://www.stackoverflow.com">SO</a> 
  </body>
</html>
```

The xpath query /html/body//a/@href (or simply //a/@href) will return:

```bash
    http://www.example.com
    http://www.stackoverflow.com
```
To select a specific instance use /html/body//a[N]/@href,

```bash
    $ /html/body//a[2]/@href
    http://www.stackoverflow.com
```
To test for strings contained in the attribute and return the attribute itself place the check on the tag not on the attribute:

```bash
    $ /html/body//a[contains(@href,'example')]/@href
    http://www.example.com
```
Mixing the two:

```bash
    $ /html/body//a[contains(@href,'com')][2]/@href
    http://www.stackoverflow.com
```
