此接口,需要判断出相等的情况,否则有可能会报错,至于什么时候报错,并不确定,但是肯定会出现报错的情况

之后再找为什么会报错吧以及报错的案例吧

---

这个在jdk7之后需要满足三点:

- 自反性： x ， y 的比较结果和 y ， x 的比较结果相反。
- 传递性： x > y , y > z ,则 x > z 。
- 对称性： x = y ,则 x , z 比较结果和 y ， z 比较结果相同。

查得的博客说十几条不会出错,但是百万条会出粗,我本地测试了1000万条数据,并没有报错,但是在生产环境确实看到了报错,目前并不知道如何复现

---



**报错内容**

```
java.lang.IllegalArgumentException: Comparison method violates its general contract!
  at java.util.TimSort.mergeLo(Unknown Source)
  at java.util.TimSort.mergeAt(Unknown Source)
  at java.util.TimSort.mergeCollapse(Unknown Source)
  at java.util.TimSort.sort(Unknown Source)
  at java.util.Arrays.sort(Unknown Source)
  at java.util.ArrayList.sort(Unknown Source)
  at java.util.Collections.sort(Unknown Source)
  at movieDemo.Demo4.main(Demo4.java:46)
```

**解决方法:**

需要将相等的情况返回0

---

# 一个案例

## 背景

报告需要对查得的三张表内容,根据不同字段进行排序,之前是写在`sql`中使用了`order by case when then end`这种方式来进行排序的,对三个字段分别依次进行了指定规则的排序



实际情况稍微复杂一些,所以提取出关键点复现

```java
    public static List<MyInfo> order(List<MyInfo> srcList){
        // 每个字段给一个排序因素值,十位数区分开来,以区别优先排序的列
        Map<String,Integer> idMap = new HashMap<>();
        idMap.put("1",1000);
        idMap.put("2",2000);
        idMap.put("3",3000);
        Map<Integer,Integer> ageMap = new HashMap<>();
        ageMap.put(1,100);
        ageMap.put(2,300);
        ageMap.put(3,200);
        ageMap.put(4,500);
        ageMap.put(5,400);
        Map<Integer,Integer> verMap = new HashMap<>();
        verMap.put(10,-1);
        verMap.put(11,-2);
        verMap.put(12,-3);
        verMap.put(13,-4);
        verMap.put(14,-5);

        Collections.sort(srcList,(s1,s2) ->{
            int num1 = idMap.getOrDefault(s1.getId(), 4000) + ageMap.getOrDefault(s1.getAge(), 500) + verMap.getOrDefault(s1.getVersion(), 0);
            int num2 = idMap.getOrDefault(s2.getId(), 4000) + ageMap.getOrDefault(s2.getAge(), 500) + verMap.getOrDefault(s2.getVersion(), 0);
            if(num1-num2>0){
                return 1;
            }else{
                return -1;
            }
        });
        return srcList;
    }
}
```



此接口,从前往后对比,所以第一个形参在对比前在第二个形参前,接口默认是升序排序,所以自定义排序规则返回负数则不调换位置,返回正数调换位置