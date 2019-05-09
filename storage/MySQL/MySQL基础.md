## BigInt(20) 和 Int(20) 辨别

括号里的数字只用于填充0（ZEROFILL），而跟能存储数字的大小没有区别，所以基本没用，因为一般不会打开ZEROFILL选项。

BigInt存储8字节有符号整数，Int存储4字节有符号整数。

https://stackoverflow.com/questions/3135804/types-in-mysql-bigint20-vs-int20

