# **Exceptions**

**1. ConcurrentModificationException**

Sample Bug:

```
List<String> list = new ArrayList<String>();
list.add("1");
list.add("2");
for (String item : list) {
    list.add("3");
}
```

Runtime Exception:

> Exception in thread "main" java.util.ConcurrentModificationException
> 	at java.util.ArrayList$Itr.checkForComodification(ArrayList.java:909)
> 	at java.util.ArrayList$Itr.next(ArrayList.java:859)
> 	at Solution.main(Solution.java:41)

Positive Example:

```
Iterator<String> iterator = list.iterator(); while (iterator.hasNext()) {
    String item = iterator.next(); 
    if (删除元素的条件) {
              iterator.remove();          
    }
}
```

Explanation:

1. foreach 为java*语法糖*，实际使用的是集合累的迭代器Iterator。

```
List<String> userNames = Arrays.asList(new String[] {"小明", "小紅", "小张"});
for (String name : userNames) {
			System.out.println(name);
}
```

反编译后：

```
List list = Arrays.asList(new String[] {
        "\u5C0F\u660E", "\u5C0F\u7D05", "\u5C0F\u5F20"
});
String s;
for(Iterator iterator = list.iterator(); iterator.hasNext(); System.out.println(s))
    s = (String)iterator.next();
```

2. fail-fast检查会抛出ConcurrentModificationException，本用以多线程对集合操作的检测机制。

```
final void checkForComodification() {
    if (modCount != expectedModCount)
        throw new ConcurrentModificationException();
}
```

3. Iterator.next()内调用fail-fast检查，集合类自身的remove()/add()只修改modCount，而不会修改expectedModCount。使用iterator.remove()/add() 两者都会修改。

*语法糖：不改变功能，方便开发人员使用。*