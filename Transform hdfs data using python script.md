Using Transform function to 

Transform() basic constraint, it process all the mirroring columns.

离线数据通过 Hadoop分布式文件系统 (HDFS) 落在 Hive 表当中，我们需要在 Hive 处理数据的过程中，嵌入我们自己编写的特征处理程序。

```plain
ADD FILE hdfs://path/to/fake.py;
SELECT
    TRANSFORM(a, b) USING 'fake.py' AS a, d
FROM test_table;
```
 详情链接：[https://liam.page/2020/05/11/TRANSFORM-in-Hive/](https://liam.page/2020/05/11/TRANSFORM-in-Hive/)
`TRANSFORM` 子句表示 Hive 会将 `column1` 和 `column2` 以制表符 `\t` 分割拼成一个字符串传给用户指定的程序。 

For example:

```python
import re
import sys
import codecs
import logging



_logger = logging.getLogger(__name__)

_filter_patterns = re.compile(
            '(^[0-9]+$)|(^[a-zA-Z][a-zA-Z0-9_-]{7,19}$)')
cjk_codepoints = (u'\u2e80-\u2fd5\u3190-\u319f\u3400-\u4dbf'u'\u4e00-\u9fcc\uf900-\ufaad')
_text_fmt_pattern = re.compile(u'[^a-zA-Z0-9{}]'.format(cjk_codepoints))
_filter_chars = u'丶-'
def transform(text):
  
    text = text.replace(' ', '')
    # remove user mention
    text = re.sub('O\d+', '', text) 
    # remove punctuations and emoji
    text = _text_fmt_pattern.sub('', text.lower()) #lower the case
    # remove some special chars
    text = text.strip(_filter_chars)
    if not text:
        return 'Null'
    match = _filter_patterns.search(text)
    if match:
        return 'Null'
    if not (1 < len(text) < 10): #constraint on the length
        return 'Null'
    return text

if __name__ == "__main__":
    for line in sys.stdin:
        text = line.strip("\n").split("\t") #split two columns 
        if len(text) != 0:
            continue
        else:
            print(text)
```

