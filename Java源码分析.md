# Java8源码分析



## String类分析

### fields分析

```java
/** The value is used for character storage. */
//数值使用character数组的形式存储
/*当然,虽然定义成了final,不能将value指向其他地址,但是数组存储在堆中还是可以修改的*/
private final char value[];
/** Cache the hash code for the string */
//这个string实例的hash值,在hashCode()方法中生成
/*
  计算方法:
  s[0]*31^(n-1) + s[1]*31^(n-2) + ... + s[n-1]
*/
private int hash; // Default to 0

/** use serialVersionUID from JDK 1.0.2 for interoperability(互用性) */
/**
     * Class String is special cased within the Serialization Stream Protocol.
     *
     * A String instance is written into an ObjectOutputStream according to
     * <a href="{@docRoot}/../platform/serialization/spec/output.html">
     * Object Serialization Specification, Section 6.2, "Stream Elements"</a>
     */
private static final ObjectStreamField[] serialPersistentFields =
    new ObjectStreamField[0];
```

###  部分重要方法

1. equals(Object object)

```java
/*
 先判断引用是否相同
 再判断是否是string类,长度是否一致
 再逐字进行比较
*/
public boolean equals(Object anObject) {
    if (this == anObject) {
        return true;
    }
    if (anObject instanceof String) {
        String anotherString = (String)anObject;
        int n = value.length;
        if (n == anotherString.value.length) {
            char v1[] = value;
            char v2[] = anotherString.value;
            int i = 0;
            while (n-- != 0) {
                if (v1[i] != v2[i])
                    return false;
                i++;
            }
            return true;
        }
    }
    return false;
}
```



### 相关面试题

1. String类为什么要设计成final?

   答:为了安全,不被其他类继承破环,线程安全

