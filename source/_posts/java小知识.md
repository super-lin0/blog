---
title: Java小知识
date: 2016-10-25 11:49:41
updateed: 2016-10-25 11:49:41
tags: [Java]
---

<center>
  What？Java中1000 != 1000，100 == 100，3 + 3 = 12?
<center>
</br>
</center>
  标签：#Java
</center>

<!-- more -->

不得不说，这是一个很有趣的知识点。如果运行下面的代码：

```
		Integer a = 1000, b = 1000;
        System.out.println(a == b);

        Integer c = 100, d = 100;
        System.out.println(c == d);
```

你会得到一个非常有趣的结果：
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTYxMDI1MTE1NTU0MjIy)

结果出乎意料吧，我们知道，Java 在比较两个对象的时候是比较他们的引用，就是说如果两个对象的引用相同，那他们的==结果才是 true，按照这个思路想，上面的代码都应该是 false 才对。

这就是它有趣的地方了。如果你看去看 Integer.Java 类，你会发现有一个内部私有类，IntegerCache.java，它缓存了从-128 到 127 之间的所有的整数对象。
![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTYxMDI1MTIwMDE3MjEy)

当我们创建 Integer 对象的时候

```
Integer a = 100;
```

其实内部的操作是下面这样的：

```
Integer a = Integer.valueOf(100);
```

然后再看 valueOf()方法：

```
 /**
     * Returns an {@code Integer} instance representing the specified
     * {@code int} value.  If a new {@code Integer} instance is not
     * required, this method should generally be used in preference to
     * the constructor {@link #Integer(int)}, as this method is likely
     * to yield significantly better space and time performance by
     * caching frequently requested values.
     *
     * This method will always cache values in the range -128 to 127,
     * inclusive, and may cache other values outside of this range.
     *
     * @param  i an {@code int} value.
     * @return an {@code Integer} instance representing {@code i}.
     * @since  1.5
     */
    public static Integer valueOf(int i) {
        assert IntegerCache.high >= 127;
        if (i >= IntegerCache.low && i <= IntegerCache.high)
            return IntegerCache.cache[i + (-IntegerCache.low)];
        return new Integer(i);
    }
```

看看注释大致就明白了。如果值在-128 到 127 之间，就从缓存里面取值，这样两次得到的是同一个实例，因此上面的 100 == 100.

现在你可能会问，为什么这里需要缓存？

合乎逻辑的理由是，在此范围内的“小”整数使用率比大整数要高，因此，使用相同的底层对象是有价值的，可以减少潜在的内存占用。

下面，我们通过反射去修改这个内部缓存类中的数组，会得到题目中说的 3 + 3 = 12。

```
public static void main(String[] args) throws NoSuchFieldException,IllegalAccessException  {

        Class cache = Integer.class.getDeclaredClasses()[0]; //得到IntegerCache类
        Field myCache = cache.getDeclaredField("cache");    //     得到cache数组对象
        myCache.setAccessible(true);                        //将数组对象的可访问性设置为false
        Integer[] newCache = (Integer[]) myCache.get(cache); //返回IntegerCache对象上"myCache"表示的字段的值([-128~127])

        newCache[134] = newCache[140]; //6 = 12

        Integer s = 6;
        System.out.println(s);

        Integer a = 3;
        Integer b = a + a;
        System.out.println(b);

        int c = 3;
        int d = c + c;
        System.out.printf("%d + %d = %d", c, c, d);
    }
```

下面就是我们要的结果：

![这里写图片描述](https://imgconvert.csdnimg.cn/aHR0cDovL2ltZy5ibG9nLmNzZG4ubmV0LzIwMTYxMDI1MTIxMDU5MTAx)

相信注释已经写的很明确了，快去自己的环境上试试吧！

<p style="text-align: center;"><span style="font-size:18px;"><strong><span style="color:#ff00;"><span style="color:#ff0000;">友情提示：</span></span>请尊重作者劳动成果，如需转载本博客文章请注明出处！谢谢合作！</strong></span></p>

<p align="center"><strong><span style="font-size:18px;">【作者：吴林&nbsp;&nbsp;</span></strong><a target="_blank" href="https://super-lin0.github.io/"><strong><span style="font-size:18px;">https://super-lin0.github.io/</span></strong></a><strong>】</span></strong></p>
