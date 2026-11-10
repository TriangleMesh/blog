+++
title = 'PriorityQueue 自定义排序的几种方式'
date = 2024-09-29T16:24:31-07:00
+++

Java 的 `PriorityQueue` 默认采用自然顺序排序，但也可以通过多种方式自定义排序规则。本文介绍三种常见方法，并分析可能出现的错误及其解决方式。

### 1. 实现 `Comparable` 接口

让对象实现 `Comparable` 接口，重写 `compareTo()` 方法，定义对象的自然排序顺序。这适用于对象本身具有“自然顺序”的情况。

```java
class Student implements Comparable<Student> {
    private int age;

    public Student(int age) {
        this.age = age;
    }

    public int getAge() {
        return age;
    }

    @Override
    public int compareTo(Student other) {
        return Integer.compare(this.age, other.age);
    }
}

PriorityQueue<Student> students = new PriorityQueue<>();
students.add(new Student(20));
students.add(new Student(18));
students.add(new Student(22));

while (!students.isEmpty()) {
    System.out.println(students.poll().getAge());  // Output order: 18, 20, 22
}
```

### 2. 使用 `Comparator` 接口

构造 `PriorityQueue` 时传入自定义的 `Comparator` 对象，实现所需的比较逻辑。

```java
PriorityQueue<Student> students = new PriorityQueue<>(new Comparator<Student>() {
    @Override
    public int compare(Student s1, Student s2) {
        return Integer.compare(s1.getAge(), s2.getAge());
    }
});
```

### 3. 使用 Lambda 表达式

在 Java 8 及后续版本中，可以使用 Lambda 表达式简化 **`Comparator`**。注意，`Comparable` 必须在类内部实现，不能用 Lambda 表达式代替。

```java
PriorityQueue<Student> students = new PriorityQueue<>(
    (s1, s2) -> Integer.compare(s1.getAge(), s2.getAge())
);
```

也可以写成：

```java
PriorityQueue<Student> students = new PriorityQueue<>(
    Comparator.comparingInt(Student::getAge)
);
```

### 注意：比较基本类型与包装类型

进行比较时，如果使用的是基本类型（例如 `int`），就不能直接调用 `compareTo()` 等方法，因为基本类型没有方法。需要使用包装类型（例如 `Integer`），或者调用静态比较方法。

#### 常见错误分析

以下代码会导致编译错误：

```java
class Student {
    private int age;

    public Student(int age) {
        this.age = age;
    }

    public int getAge() {
        return age;
    }
}

PriorityQueue<Student> students = new PriorityQueue<>((s1, s2) -> {
    return s1.getAge().compareTo(s2.getAge());
});
```

**错误原因：** `getAge()` 返回的是基本类型 `int`，不能调用 `compareTo()`。编译器会报错：`int cannot be dereferenced`。

#### 解决方法

1. **将 `age` 改为包装类型 `Integer`：**

```java
class Student {
    private Integer age;

    public Student(Integer age) {
        this.age = age;
    }

    public Integer getAge() {
        return age;
    }
}
```

这样，`getAge()` 返回的就是 `Integer`，可以调用 `compareTo()`。

2. **不修改 `age` 的类型，直接使用 `Integer.compare(int x, int y)`：**

```java
PriorityQueue<Student> students = new PriorityQueue<>((s1, s2) -> {
    return Integer.compare(s1.getAge(), s2.getAge());
});
```

或者：

```java
PriorityQueue<Student> students = a new PriorityQueue<>(
    Comparator.comparingInt(Student::getAge)
);
```
