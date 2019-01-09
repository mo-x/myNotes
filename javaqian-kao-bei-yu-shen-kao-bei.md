\#\#\#Java中拷贝分为两种 引用拷贝和对象拷贝

1.引用拷贝，只拷贝引用本身，所指引的对象并不会拷贝,无论修改原有的对象内容或copy后的对象内容则互相影响。\(浅拷贝\)

2.对象拷贝，拷贝引用和对象,堆中分配的对象会复制一份。这时存在两个独立对象，修改原有的或者copy后的对象互相不影响。（深拷贝）

### 浅拷贝

#### 如何实现Cloneable接口 重写clone方法

```java
public class Address implements Cloneable{

    String city;

    public Address() {
    }

    public Address(String city) {
        this.city = city;
    }

    public String getCity() {
        return city;
    }

    public void setCity(String city) {
        this.city = city;
    }

    @Override
    protected Object clone()  {
        Address address = null;
        try {
            address = (Address) super.clone();
        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
        }
        return address;
    }
}
```

#### 2.浅拷贝存在的问题

在Java类型变量分为基本和引用类型，这两种类型在拷贝的时候呈现不一样的结果\(String字符串表现不一样\)。总结下就是clone方法在拷贝对象的时候，是有选择性的拷贝，不会将所有属性全部拷贝过来。规则如下：

&gt; 1.基本类型则拷贝其值

2.String类型 会生成新的String对象，原有对象不变。\(原因：String对象 是在常量池中分配\)

3.引用类型 拷贝其引用，与原有对象公用一个实例对象。

### 深拷贝

1. 在浅拷贝基础上进行修改

```java
protected Object clone()  {
        User user = null;
        try {
            user = (User) super.clone();
            /**
             * 注意这里
             * 来保证修改copy后的对象的修改不会引起原有对象的内容的变化
             */
           /* user.setAddress(new Address());*/
        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
        }
        return user;
    }
}
```

#### last 使用序列化来深拷贝

```java
public class CloneUtils {

    public static <T extends Serializable> T clone(T obj) {

        T cloneObj = null;
        try {
            //写入字节流
            ByteArrayOutputStream out = new ByteArrayOutputStream();
            ObjectOutputStream obs = new ObjectOutputStream(out);
            obs.writeObject(obj);
            obs.close();
            out.close();
            //分配内存，写入原始对象，生成新对象
            ByteArrayInputStream ios = new ByteArrayInputStream(out.toByteArray());
            ObjectInputStream ois = new ObjectInputStream(ios);
            //返回生成的新对象
            cloneObj = (T) ois.readObject();
            ois.close();
            ois.close();
        } catch (Exception e) {
            e.printStackTrace();
        }finally {

        }
        return cloneObj;
    }

}
```

##### 所有代码示例请点击[https://github.com/xps-zzq/HelloJava/tree/master/src/main/Java/copy](https://github.com/xps-zzq/HelloJava/tree/master/src/main/Java/copy)



