# JAVA基础笔记

## 匿名内部类

```java
new Swim(){
    @Override
    public void swim() {
        System.out.println("重写了游泳");
    }
};
```

1. 实现或继承了Swim类
2. 重写了Swim类的方法
3. 创建了这个匿名类的对象









## 多态

所谓多态，就是指一个引用变量在不同的情况下的多种表现状态。也可以理解为，多态是指通过指向父类的引用变量，来调用在不同子类中实现的方法。

多态的使用：

1.一般用于继承体系下，子类重写父类的某个方法，调用时根据具体的子类实现去调用子类的方法

2.向上转型：把子类对象直接赋给父类引用叫向上转型

```java
//		使用多态方法创建对象：
//		能调用父类中的方法,子类重写了父类的方法,但不能调用子类特有的方法
		Pet dog2 = new Dog();	//类型转换，向上转型

```







## Lambda表达式





## 集合进阶

Java集合框架主要包括两种类型的容器，一种是单列集合(Collection)，存储一个元素集合，另一种是双列集合(图，Map)，存储键/值对映射。Collection接口又有三种子类型List，Set和Queue，再下面是一些抽象类，最后是具体实现类

List系列集合：添加的元素是有序(存和取的顺序是一样的)，可重复，有索引

Set系列集合：添加的元素是无序的(存和取的顺序可能是不一样的)，不重复，无索引

Collection：Collection是单列集合的祖宗接口，它的功能是全部单列集合都可以继承使用的

### 

### Collection遍历

#### 迭代器遍历

```java
Collection<String> col = new ArrayList<>();
col.add("aaa");
col.add("bbb");
Iterator<String> it = col.iterator();

//判断当前指向的位置是否有元素
while (it.hasNext()){
    String str = it.next(); //获取当前指向的元素并移动指针
    System.out.println(str);
}
```



#### 增强for遍历

```java
for (String s : coll) {
    System.out.println(s);
}
```

可以使用集合.for的方式快捷完成书写



#### Lambda表达式遍历







### HashMap

HashMap 是一个散列表，它存储的内容是键值对(key-value)映射。

HashMap 实现了 Map 接口，根据键的 HashCode 值存储数据，具有很快的访问速度，最多允许一条记录的键为 null，不支持线程同步。

HashMap 是无序的，即不会记录插入的顺序。

```java
@Test
public void testHashMap(){
    HashMap<String, Integer> m1 = new HashMap<>();  //键值对，前为键后为值
    m1.put("a",1);
    m1.put("b",2);
    System.out.println(m1.get("a"));
}
```





### ArrayList

```java
ArrayList<E> objectName =new ArrayList<>();　 // 初始化，E为泛型，及存储数据的类型
```



## Junit单元测试

步骤：

1.定义一个测试类(测试用例)

2.定义测试方法：可以独立运行

3.给方法加@Test

4.导入Junit依赖环境

一般使用断言(Assert)来处理结果



## 异常处理







## 数据流







## Java正则表达式

![1660566491988](C:\Users\86158\AppData\Roaming\Typora\typora-user-images\1660566491988.png)

注:字符类中如果只匹配单个字符,[]可省略

()表示分组 例如a(bc):表示出现a或出现bc

