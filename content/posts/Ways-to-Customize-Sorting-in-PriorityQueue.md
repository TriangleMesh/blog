+++
title = 'Ways to Customize Sorting in PriorityQueue'
date = 2024-09-29T16:24:31-07:00
+++

By default, Java's `PriorityQueue` uses natural ordering, but we can customize the sorting rules in several ways. This article will introduce three common methods and analyze possible errors and their solutions.

### 1. Implementing the `Comparable` Interface

Allow the object to implement the `Comparable` interface and override the `compareTo()` method to define the natural sorting order of the object. This is suitable for cases where a "natural order" exists.

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

### 2. Using the `Comparator` Interface

Pass a custom `Comparator` object when constructing the `PriorityQueue` to implement custom sorting logic.

```java
PriorityQueue<Student> students = new PriorityQueue<>(new Comparator<Student>() {
    @Override
    public int compare(Student s1, Student s2) {
        return Integer.compare(s1.getAge(), s2.getAge());
    }
});
```

### 3. Using Lambda Expressions

In Java 8 and later versions, you can simplify the **`Comparator`** using lambda expressions.  Note that `Comparable` must be implemented inside the class and cannot be expressed using lambda expressions.

```java
PriorityQueue<Student> students = new PriorityQueue<>(
    (s1, s2) -> Integer.compare(s1.getAge(), s2.getAge())
);
```

Or:

```java
PriorityQueue<Student> students = new PriorityQueue<>(
    Comparator.comparingInt(Student::getAge)
);
```

### Note: Handling Primitive Types and Wrapper Types in Comparisons

When performing comparisons, if you are using primitive types (e.g., `int`), you cannot directly call methods like `compareTo()` because primitive types do not have methods. You need to use wrapper types (e.g., `Integer`) or static comparison methods to handle this.

#### Common Error Analysis

The following code will lead to a compilation error:

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

**Error Reason:** The `getAge()` method returns an `int`, which is a primitive type and cannot call the `compareTo()` method. The compiler will throw the error: "`int cannot be dereferenced`."

#### Solutions:

1. **Change `age` to the wrapper type `Integer`:**

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

This way, `getAge()` returns an `Integer`, and you can call the `compareTo()` method.

2. **Use the `Integer.compare(int x, int y)` method without modifying the `age` type:**

```java
PriorityQueue<Student> students = new PriorityQueue<>((s1, s2) -> {
    return Integer.compare(s1.getAge(), s2.getAge());
});
```

Or:

```java
PriorityQueue<Student> students = a new PriorityQueue<>(
    Comparator.comparingInt(Student::getAge)
);
```
