# 原码、反码和补码
计算机中保存的是 0/1——最原始的数字，无（正负）符号数字。为了区分正负，选择最高位代表符号：0->正，1->负，此为**原码**。

+1 原码为 0001（假设四位），-1 原码为 1001。但简单通过原码相加得到的结果为 1010，即 -2 并非正确答案。

为解决此问题，引入**反码**。正数的反码等于原码，而负数的反码等于原码取反（符号位置不变，其余位置相反），反码和反码相加结果也为反码。

所以 +1 反码为 0001，-1 反码为 1110，相加为 1111（反码），转换为原码为 1000（即-0）。

1000 代表 0（-0），但 0000 也代表 0（+0）。这样 0 值存在两种表示。

为解决此问题，引入**补码**，正数的补码等于原码，而负数的补码等于反码加一，补码和补码相加的结果也为补码，需转换为原码。

所以 +1 补码为 0001，-1 补码等于 1110 + 1 = 1111，相加的结果为 10000，此时需要丢掉最高位，即结果为 0000（反码），转换为原码为 0000。

更多例子，
- +3 和 -3 相加，即 0011 + 1101 = 10000，丢掉最高位，结果为 0000（补码），转换为原码则为 0000；
- +2 和 -4 相加，即 0010 + 1100 = 1110（补码），再转回原码则为 1010；
- +3 和 -1 相加，即 0011 + 1111 = 10010，丢掉最高位，结果为 0010（补码），转换为原码为 0010；

以上可以按这么理解，但实际出现顺序不一定这样。

正数：原码、反码、补码都相同
负数：反码等于原码取反，补码等于反码加一

# xxx