# Perl使用技巧

- [Perl使用技巧](#perl使用技巧)
  - [Perl哈希嵌套数组](#perl哈希嵌套数组)


## Perl哈希嵌套数组
> https://blog.csdn.net/weixin_56198196/article/details/120978024


1)访问哈希中的数组的元素：

`$hash{$key}[position];` 就可以访问哈希里嵌套的数组的元素了，这里的position就是指要访问第几个元素(从0开始的)

2)把数组直接赋值给哈希中的数组：

`$hash{$key}=[@array];` 就可以把数组传递给哈希了

3)`push()`:

向数组中添加元素

`push(@{$hash{$key}},element);`

如果是添加一组数

`push(@{$hash{$key}},@array);`

注：

1）`$hash{$key}[0]=2`，这里的`$key`假设为“apple”，如果要`print`的话，apple不要带上双引号，类似这样 `print"$hash{"apple"}[0]"`,这样是会报错的，写为 `print"$hash{apple}[0]" `就可以了

2）`print($hash{$array});` 得到的是array的地址，不是其中的元素，想要看其中有什么元素要这样写 `print(@{$hash{$array}});`


