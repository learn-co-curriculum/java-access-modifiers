# Access Modifiers

## Learning Goals

- Explain what an access modifier is in Java.
- Explain when certain access modifiers should be used.

## Introduction

So far, we have seen quite a few keywords. For example, we have seen the `main`
method pop up quite frequently in the code that we write:

`public static void main(String[] args)`

In this lesson and the next few lessons, let us investigate this line some more.

For purposes of this lesson, we will focus more on the keyword `public` and what
that means and how it is an **access modifier**.  We will discuss the keyword `static`
in another lesson.

## What is an Access Modifier?

All of our classes, fields, and methods so far have either been `public` or have not
specified their level of visibility.

**Access modifiers** in Java let us specify the level of access that classes,
fields, methods and constructors should have.

The access levels are as follows:

1. `public` - Classes, fields, methods and constructors defined with this
   keyword are accessible by all classes everywhere.
2. `default` - This is the access modifier that is applied when an access modifier
   is not specified. It allows the fields, methods and constructors in a class
   to be visible to any other classes in the same package. More to come on what a
   package is later, but you can think of it like a folder or directory in a file system.
3. `protected` - It allows the fields, methods and constructors in a class
   to be visible to any other classes in the same package or to any subclass in another package.
   We'll cover what a subclass is when we get to inheritance.
4. `private` - Any fields, methods and constructors defined with this keyword
   are only visible to the class where they are defined.  They can't be accessed
   by any other class.

When specified, the access modifier must be the first part of the definition it
applies to. Let us look at a `Bicycle` class:

```java
public class Bicycle {
    String color;
    int height;
}
```

`public class Bicycle` defines a public class, meaning anyone can access this
class.

Notice that the fields `color` and `height` do not have an access modifier
attached to them. This means they have the default access modifier attached to
them, which makes them visible to any class in the same package.

Let's look at each access modifier in more detail, from least restrictive to
most restrictive.

## Public

The `public` access modifier is the least restrictive access modifier. Classes,
methods, fields and constructors defined with this access modifier can be
accessed from any other class.

Since it is the most permissive access modifier, it should only be used for
fields and methods that are meant to be used by a wide range of other
classes. As a general principle, most fields should not be `public` and
should instead be made available through "accessor methods", so that additional
logic can be added to their handling without having to modify the code that
accesses them.

When we look at our `main` method, we see that it has the `public` access
modifier. Therefore, it is accessible to another other class, including the JVM
itself:

`public static main(String[] args)`

## Default (No Access Modifier)

The default access modifier is when no access modifiers are specified.

Classes, fields, methods and constructors defined without an access modifier
are available to all other classes in the same package.

If we go back to our `Bicycle` class example, the variables `color` and `height`
both use the default access level since the access modifier is not specified
prior to the data type. This means that we can make use of those variables even
outside the `Bicycle` class as long as it is within the same package of the
`Bicycle` class. Again, we will discuss packages more in depth in a later
lesson.

## Private

`private` is the most restrictive access modifier. Methods, fields and
constructors defined with this modifier can only be accessed within the class
where they are defined.

The `private` modifier is very useful in hiding implementation details of a
specific class from other classes that might use it. A very common pattern for
doing this is to define `private` instance variables and make their values accessible
through `public` accessor methods, which when used in this way are usually called
"accessor methods".

We'll be learning about accessor methods shortly, but for now, let us apply this knowledge
to the variables in the `Bicycle` class as an example.  We'll update the
fields to add the `private` keyword at the beginning of each variable declaration:

```java
public class Bicycle {
   private String color;
   private int height;
}
```

The modification we made here is that we made the instance variables `color` and `height`
private - meaning that they can only be accessed within the `Bicycle` class.

So now what happens if we have the following `main` method that attempts to assign
values to the private instance variables?

```java
public static void main(String[] args) {}
    Bicycle johnsBike = new Bicycle();
    johnsBike.color = "blue";
    johnsBike.height = 36;
}
```

If the `main` method is contained **within** the `Bicycle` class, then
the method will compile without error because the private
instance variables are visible to all methods in the same class.  For example,
the code below compiles without an error:


```java
public class Bicycle {

    private String color;
    private int height;

    public static void main(String[] args) {
        Bicycle johnsBike = new Bicycle();
        johnsBike.color = "blue";
        johnsBike.height = 36;
    }
}
```

However, if the `main` method is  contained in a separate class, for example `TestBicycle`, we will
see a compiler error because `TestBicycle` can't access the private members of `Bike`:

<table>

<tr>

<td>
<pre>
<code>

public class Bicycle {
  private String color;
  private int height;
}

</code>
</pre>
</td>

<td>
<pre>
<code>

public class TestBicycle {
  public static void main(String[] args) {
    Bicycle johnsBike = new Bicycle();
    johnsBike.color = "blue";
    johnsBike.height = 36;
  }
}

</code>
</pre>
</td>

</tr>

</table>


![private access compiler error](https://curriculum-content.s3.amazonaws.com/6676/java-mod2-oop-fundamentals/compiler_error_private_access.png)


Now that we have the `private` access modifier attached to the properties, we
would no longer be able to access `color` or `height` outside of the `Bicycle` class. We
would have to find another way to access these attributes in `TestBicycle`.


## Coding Convention

The initial lessons in this module omitted the access modifier
when declaring instance variables, since the concept had not
yet been introduced. However, from this point on we will use the following coding convention:

**_Instance variables should always be declared as private!_**

So consider the `Dog` class we worked with in the first few lessons.
We should update it to make the instance variables private, while keeping the
methods public:

```java
public class Dog {

    private String name;
    private String breed;
    private int age;
    private boolean waggingTail;


    @Override
    public String toString() {
        return "Dog{" +
                "name='" + name + '\'' +
                ", breed='" + breed + '\'' +
                ", age=" + age +
                ", waggingTail=" + waggingTail +
                '}';
    }

    public static void main(String[] args) {

        // create 2 Dog instances
        Dog bigDog = new Dog();
        Dog smallDog = new Dog();

        // assign new values to the fields of the first dog
        bigDog.name = "Bruno";
        bigDog.breed = "Great Dane";
        bigDog.age = 8;

        // assign new values to the fields of the second dog
        smallDog.name = "Penny";
        smallDog.breed = "Pug";
        smallDog.age = 5;
        smallDog.waggingTail = true;

        //print the state of each Dog instance
        System.out.println(bigDog);
        System.out.println(smallDog);

    }
}
```


## Conclusion

An access modifier controls the visibility of a class, field, constructor, or method.

| Modifier  | Class | Package  | Subclass In<br>Another Package  | World |
|-----------|-------|----------|---------------------------------|-------|
| public    | Y     | Y        | Y                               | Y     | 
| protected | Y     | Y        | Y                               | N     | 
| default   | Y     | Y        | N                               | N     | 
| private   | Y     | N        | N                               | N     | 


## Resources

[Controlling access to a member of a class](https://docs.oracle.com/javase/tutorial/java/javaOO/accesscontrol.html)