# Perl使用技巧

- [Perl使用技巧](#perl使用技巧)
  - [Perl哈希嵌套数组](#perl哈希嵌套数组)
  - [Perl哈希嵌套哈希](#perl哈希嵌套哈希)
  - [Perl数组排序](#perl数组排序)
    - [基于ASCII码排序](#基于ascii码排序)
    - [按字母顺序排列（忽略大小写）](#按字母顺序排列忽略大小写)
    - [对数值排序（使用`<=>`）](#对数值排序使用)
    - [Hash 按 keys 排序](#hash-按-keys-排序)
    - [Hash 按 value 排序](#hash-按-value-排序)
  - [Perl数组去重](#perl数组去重)
    - [利用hash去重](#利用hash去重)
    - [利用uniq函数去重](#利用uniq函数去重)
  - [Perl函数参数传递与返回值](#perl函数参数传递与返回值)
    - [参数传递](#参数传递)
      - [普通模式：参数中没有数组和哈希](#普通模式参数中没有数组和哈希)
      - [文艺模式：参数中包含数组](#文艺模式参数中包含数组)
  - [The Arrow Operators (`->`)](#the-arrow-operators--)


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

3）需要**初始化**：`$hash{$key} = [()];`


## Perl哈希嵌套哈希

与哈希嵌套数组类似，例：

定义：`$hash{$key1}->{$key2} = $a`

取用值：`$hash{$key1}->{$key2}`

取用键：`keys %{$hash{$key1}}`


## Perl数组排序
> https://www.jb51.net/article/67894.htm  
> https://blog.csdn.net/zxianyong/article/details/5931487


Perl有个内置函数叫做`sort`，毫无疑问的可以排序一个数组。其最简单的形式是传递一个数组，它会返回排序后的元素组成的数组：`@sorted = sort @original`。

### 基于ASCII码排序

```perl
my @words = qw(foo bar zorg moo);
my @sorted_words = sort @words;
# bar' 'foo' 'moo' 'zorg'

my @words = qw(foo bar Zorg moo);
my @sorted_words = sort @words;
# 'Zorg' 'bar' 'foo' 'moo'
```

Perl的`sort`的工作方式是这样的，它遍历原始数组的每两个元素；每次把左边的值放入变量`$a`，把右边的值放入变量`$b`。然后调用比较函数。如果`$a`的内容应该在左边的话，比较函数会返回`1`；如果`$b`应该在左边的话，返回`-1`，两者一样的话，返回`0`。

通常你看不到比较函数，`sort`会根据ASCII码表对值进行比较，不过如果你想的话，你可以显式的写出来：`sort { $a cmp $b } @words;`

这段代码会跟没有使用块的`sort @words`达到同样的效果。

这里你可以看到，默认Perl使用`cmp`作为比较函数。这是因为正是`cmp`可以做这里边我们需要的工作。 它比较两边的字符串的值，如果左边参数“小于”右边参数，就返回1；如果左边参数“大于”右边参数，就返回-1；如果相等，就返回0。

### 按字母顺序排列（忽略大小写）

```perl
my @words = qw(foo bar Zorg moo);
my @sorted_words = sort { lc($a) cmp lc($b) } @words;  # 调用lc函数返回小写
# bar' 'foo' 'moo' 'Zorg'
```

### 对数值排序（使用`<=>`）

```perl
my @numbers = (14, 3, 12, 2, 23);
my @sorted_numbers = sort { $a <=> $b } @numbers;
# 2 3 12 14 23
```

### Hash 按 keys 排序

```perl
## 按ASCII码排序
my %hash = ( 'a' => 'd', 'b' => 'e', 'c' => 'f' );
foreach my $key ( sort keys %hash ) {
    print $key, "=>", $hash{$key}, "\n";
}

## 按数值排序
my %hash = ( 'a' => 2, 'b' => 3, 'c' => 1 );
foreach my $key ( sort { $a <=> $b } keys %hash ) {
    print $key, "=>", $hash{$key}, "\n";
}
```

### Hash 按 value 排序

```perl
# 按ASCII码排序
my %hash = ( 'a' => 'd', 'b' => 'e', 'c' => 'f' );
foreach my $key ( sort { $hash{$a} cmp $hash{$b} } keys %hash ) {
    my $value = $hash{$key};
}

# 按数值从小到大排序
my %hash = ( 'a' => 2, 'b' => 3, 'c' => 1 );
foreach my $key ( sort { $hash{$a} <=> $hash{$b} } keys %hash ) {
    my $value = $hash{$key};
}

# 按数值从大到小排序（a和b互换位置）
my %hash = ( 'a' => 2, 'b' => 3, 'c' => 1 );
my @keysd = sort { $hash{$b} <=> $hash{$a} } keys %hash;
foreach my $key ( sort { $hash{$b} <=> $hash{$a} } keys %hash ) {
    my $value = $hash{$key};
}
```

## Perl数组去重
> https://www.cnblogs.com/mmtinfo/p/11970736.html

### 利用hash去重

```perl
my @list = qw /1 2 3 2 1 4 aa a bb c b bb d/;
foreach (@list) { print "$_ "; }
# 1 2 3 2 1 4 aa a bb c b bb d

my %hash;
my @uniq = grep { ++$hash{$_} < 2 } @list;
foreach (@uniq) { print "$_ "; }
# 1 2 3 4 aa a bb c b d
```

基本原理是将原数组元素作为`hash`的`key`，遍历计数，`grep`函数筛选出只出现一次的`key`，放入新的数组`@uniq`中。

### 利用uniq函数去重

这个函数所在的模块：[`List::MoreUtils`](https://metacpan.org/pod/List::MoreUtils)

```perl
use List::MoreUtils qw(uniq);
#use List::MoreUtils ':all';

my @list = qw /1 2 3 2 1 4 aa a bb c b bb d/;
foreach (@list) { print "$_ "; }
# 1 2 3 2 1 4 aa a bb c b bb d

my @uniq = uniq(@list);
foreach (@uniq) { print "$_ "; }
# 1 2 3 4 aa a bb c b d
```


## Perl函数参数传递与返回值
> https://www.cnblogs.com/tobecrazy/archive/2013/06/11/3131887.html

### 参数传递

#### 普通模式：参数中没有数组和哈希

```perl
#!/usr/bin/perl -w
use strict;
sub getparameter
{
    my $i;
    for($i=0;$i<=$#_;$i++)
    {
        print "It's the ";
        print $i+1;
        print "parameter: $_[$i] \n";
    }
}

# 调用函数
&getparameter($first,$second .. $end)
```

无论参数有多少个，均能正常传递。

#### 文艺模式：参数中包含数组

还是这个函数，只不过我们传递的参数里包括一个数组

```perl
use strict;
sub getparameter
{
    my $i;
    for($i=0;$i<=$#_;$i++)
    {
        print "It's the ";
        print $i+1;
        print "parameter: $_[$i] \n";
    }
}
my @array=("this","is","a","test");
my $variable="this is another test";
&getparameter($variable,@array);
```

当我们只传入2个参数，一个数组，一个变量，结果是这样，变成了5个参数。无论数组在前还是在后，都是显示5个参数。 由此`@_`会把数组每一个值当做一个参数储存。那我的疑问是perl能否正确的把传递的数组还原成数组而不是单个变量？？？

那我们换一种方式接受参数：

```perl
use strict;
sub getparameter
{
    (my @arr,my $var)=@_;
    print "It's the 1st parameter: @arr \n";
    print "It's the 2nd parameter: $var \n";

}
my @array=("this","is","a","test");
my $variable="this is another test";
&getparameter(@array,$variable);
```

结果令人意外，`$variable`传递的参数丢失，同时数组却取得所有参数，相当于把变量归为数组的一个元素。perl接受传递来的数组，会贪婪的把变量变成数组的元素。所以在接受参数传递赋值时，不要把数组放前面。

改成这样就好了：

```perl
use strict;
sub getparameter
{
    (my $var,my @arr)=@_;
    print "It's the 1st parameter: $var \n";
    print "It's the 2nd parameter: @arr \n";

}
my @array=("this","is","a","test");
my $variable="this is another test";
&getparameter($variable,@array);
```

如果要传递2个数组怎么办？？？可以采用引用的方式：

```perl
use strict;
sub getparameter
{
    (my $arr1,my $arr2)=@_;
    print "It's the 1st parameter: @$arr1 \n";
    print "It's the 2nd parameter: @$arr2 \n";

}
my @array1=("this","is","a","test");
my @array2=qw/this is another test/;
&getparameter(\@array1,\@array2);
```

perl使用引用是在变量或数组前加`\`，相当于地址传递


## The Arrow Operators (`->`)
> https://www.shlomifish.org/lecture/Perl/Newbies/lecture2/references/arrow.html

An arrow (`->`) followed by a square (`[]`) or curly brackets (`{}`) can be used to directly access the elements of an array or a hash referenced by a certain hash. For instance: `$array_ref->[5]` will retrieve the 5th element of the array pointed to by `$array_ref`.

An example:

```perl
do "lol.pl";         # Load the other file (foo)
my $cont = get_contents();

print $cont->{'title'}, "\n";
print $cont->{'subs'}->[0]->{'url'}, "\n";
print $cont->{'subs'}->[0]->{'subs'}->[1]->{'title'}, "\n";
```

Note that the arrows following the first arrow are optional as perl sees that the programmer wishes to access the subseqeunt sub-items. However, the first one is mandatory because the expression `$array_ref{'elem'}` looks for the hash `%array_ref`.


