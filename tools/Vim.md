vim粘贴`:set paste`，关闭粘贴`:set nopaste`

### `:s` 替换字符串
- `:s/{old}/{new}/g` 将光标所在行所有 old 字符串替换为 new 字符串
- `50,100s/{old}/{new}/g` 将第50行到第100行所有 old 字符串替换为 new 字符串
- `1,$s/{old}/{new}/g` `%s/{old}/{new}/g` 将全局所有 old 字符串替换为 new 字符串

不加g则替换每行第一个匹配的到 old 字符串。