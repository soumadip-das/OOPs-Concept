# OOPs Concept
- **Class**:  It is a user-defined data type that act as a blueprint representing a group of objects.
- **Object**: An Object is an identifiable actual entity with some characteristics and behaviour. In C++, it is an instance of a class.
- **Encapsulation**: Wrapping data and methods into a single unit (e.g., a class).
- **Inheritance**: Mechanism where one class acquires the properties and behaviors of another class.
- **Polymorphism**: Ability to take many forms, such as method overloading and overriding.
- **Abstraction**: Hiding implementation details and showing only the essential features.
---
***
# size of class
- Any structure will take the size equal to the multiple of maximum bytes taken by (bigger) member variable in that structre

## memory block
| Column 1 | Column 2 | Column 3 | Column 4 |
|----------|----------|----------|----------|
| 3        | 9        | 27       | 21       |
| 5        | 10       | 15       | 20       |
| 4        | 8        | 12       | 16       |

- read-write head
    - 4,8,12,16 can cover in one cycle of access.
    - but in case of 5,9,12,21 It is not possible to cover in one rotation.
    - to make faster => any data-type will alawys store its vlue at the address which is multiple of the size of that data-type.

```cpp
#include <bits/stdc++.h>
using namespace std;
class A {
    int a;
    char b;
    double c;
    
    
};

int main() {
	// your code goes here
    A obj;
    cout<<sizeof(obj);
}
```
---
***
# This Pointer
- In C++, ‘this’ pointers is a pointer to the current instance of a class. It is used to refer to the object within its own member functions.E

```cpp
#include <iostream>
using namespace std;

// Class that uses this pointer
class A {
  public:
    int a;
    A(int a) {

        // Assigning a of this object to
        // function argument a
        this->a = a;
    }
    void display() {

        // Accessing a of this object
        cout << "Value: " << this->a;
    }
};

int main() {

    // Checking if this works for the object
    A o(10);
    o.display();

    return 0;
}

```
- To return reference to the calling object
```cpp
// Reference to the calling object can be returned
Test& Test::func () {
    // Some processing
    return *this;
} 
```
---
***
# Constructor
- The name of the constructor is the same as its class name.
- Constructors are mostly declared as public member of the class though they can be declared as private.
- Constructors do not return values, hence they do not have a return type.
- A constructor gets called automatically when we create the object of the class.
- Multiple constructors can be declared in a single class.
- In case of multiple constructors, the one with matching function signature will be called.

**Types**
- **Default Constructor**: Made by compiler if programmer doesnot define any, It has no parameters. 
- **Parameterize Constructor**: Made by programmer, takes any num of argument. In c++ if there is man-made constructor then default constructor does not work.
- **Copy Constructor**: A copy constructor is a member function that initializes an object using another object of the same class.
  - Shallow Copy
  - Deep Copy


# Shallow Copy vs Deep Copy in C++

## Shallow Copy
- Copies the values of data members as-is.
- For pointers, it copies the pointer (both objects point to the same memory).
- Issues: double deletion, unintended side effects.

## Deep Copy
- Allocates separate memory and copies the actual data.
- Each object maintains its own copy.
- Avoids memory issues and provides data safety.

---

## Copy Constructor – Shallow vs Deep Copy (Code Example)

```cpp
#include <iostream>
#include <cstring>
using namespace std;

class Student {
private:
    char* name;

public:
    // Constructor
    Student(const char* n) {
        this->name = new char[strlen(n) + 1];
        strcpy(this->name, n);
    }

    // Deep Copy Constructor using this pointer
    Student(const Student& other) {
        this->name = new char[strlen(other.name) + 1];
        strcpy(this->name, other.name);
        cout << "Deep Copy Constructor Called\n";
    }

    // Destructor
    ~Student() {
        delete[] this->name;
    }

    void setName(const char* newName) {
        strcpy(this->name, newName);
    }

    void display() {
        cout << "Name: " << this->name << endl;
    }
};

int main() {
    Student s1("Ankan");
    Student s2 = s1;  // Deep Copy Constructor is called

    s2.setName("Manna");

    s1.display();  // Name remains unaffected due to deep copy
    s2.display();

    return 0;
}

```

### Shallow Copy Output
```
Shallow Copy Constructor Called
Name: Manna
Name: Manna
```

### Deep Copy Output (if deep constructor is used)
```
Deep Copy Constructor Called
Name: Ankan
Name: Manna
```

---

## Copy Constructor vs Default Assignment Operator

### Copy Constructor
- Called when a new object is initialized from another.
- Syntax: `ClassName obj2 = obj1;`

### Default Assignment Operator
- Called when assigning values between already existing objects.
- Syntax: `obj2 = obj1;`
- Performs shallow copy by default.

### Comparison Table
| Feature                     | Copy Constructor         | Assignment Operator            |
|----------------------------|--------------------------|--------------------------------|
| When Called                | Object creation          | After object exists            |
| Default behavior           | Shallow copy             | Shallow copy                   |
| Customizable               | Yes                      | Yes                            |

---

## Assignment Operator Overloading Example

```cpp
#include <iostream>
#include <cstring>
using namespace std;

class Student {
private:
    char* name;

public:
    // Constructor
    Student(const char* n) {
        this->name = new char[strlen(n) + 1];
        strcpy(this->name, n);
    }

    // Deep Copy Constructor
    Student(const Student& other) {
        this->name = new char[strlen(other.name) + 1];
        strcpy(this->name, other.name);
        cout << "Copy Constructor\n";
    }

    // Overloaded Assignment Operator
    Student& operator=(const Student& other) {
        if (this != &other) {
            delete[] this->name;
            this->name = new char[strlen(other.name) + 1];
            strcpy(this->name, other.name);
            cout << "Assignment Operator\n";
        }
        return *this;
    }

    // Destructor
    ~Student() {
        delete[] this->name;
    }

    void display() const {
        cout << "Name: " << this->name << endl;
    }
};

int main() {
    Student s1("Ankan");
    Student s2 = s1;   // Copy Constructor
    Student s3("Dummy");
    s3 = s1;           // Assignment Operator

    s1.display();
    s2.display();
    s3.display();

    return 0;
}

```

---

## Summary Table

| Concept               | Description                                                       |
|----------------------|-------------------------------------------------------------------|
| Shallow Copy          | Pointer copied, shared memory                                     |
| Deep Copy             | Data copied to new memory                                         |
| Copy Constructor      | Used during initialization (can be deep or shallow)               |
| Assignment Operator   | Used when assigning values to an existing object                  |
| Destructor            | Should handle memory cleanup if dynamic memory is used            |

---
***

# Destructor
- A destructor is also a special member function like a constructor. Destructor destroys the class objects created by the constructor. 
- Destructor has the same name as their class name preceded by a tilde (~) symbol.
- It is not possible to define more than one destructor.
- The destructor is only one way to destroy the object created by the constructor. Hence, destructor cannot be overloaded.
- It cannot be declared static or const.
- Destructor neither requires any argument nor returns any value.
- It is automatically called when an object goes out of scope. 
- Destructor release memory space occupied by the objects created by the constructor.
- In destructor, objects are destroyed in the reverse of an object creation.

**What is the use of private destructor?**

Whenever we want to control the destruction of objects of a class, we make the destructor private. For dynamically created objects, it may happen that you pass a pointer to the object to a function and the function deletes the object. If the object is referred after the function call, the reference will become dangling.

```cpp
// CPP program to illustrate
// Private Destructor
#include <iostream>
using namespace std;

class Test {
private:
	~Test() {}
};
int main() {}

```
***
---
# Static keyword
- It use when we need a kind of data member/function which is not depends upon object, It depends upon class.

## static data member:
- Only one copy of that member is created for the entire class and is shared by all the objects of that class, no matter how many objects are created.
- It is initialized before any object of this class is created, even before the main starts outside the class itself.
- It is visible can be controlled with the class access specifiers.
- Its lifetime is the entire program.

## static member function
- A static member function is independent of any object of the class. 
- A static member function can be called even if no objects of the class exist.
- A static member function can also be accessed using the class name through the scope resolution operator.
- A static member function can access static data members and static member functions inside or outside of the class.
- Static member functions have a scope inside the class and cannot access the **This** pointer .
- **why it need**-You can also use a static member function to determine how many objects of the class have been created. 

```cpp
#include <iostream>
using namespace std;

class Demo {
private:
    int value;                     // Normal variable
    static int objectCount;        // Static variable

public:
    // Constructor
    Demo(int v) {
        this->value = v;           // Using 'this' pointer
        objectCount++;
    }

    // Normal member function
    void showValue() const {
        cout << "Value (normal): " << this->value << endl;
    }

    // Static member function
    static void showCount() {
        cout << "Object Count (static): " << objectCount << endl;
    }
};

// Definition of static variable (outside the class using scope resolution)
int Demo::objectCount = 0;

// Normal function outside the class
void greet() {
    cout << "Hello from normal function!\n";
}

// Global variable
int globalVar = 100;

int main() {
    ::greet();                // Scope resolution to call global function
    cout << "Global Variable using ::globalVar = " << ::globalVar << endl;

    Demo d1(10);
    Demo d2(20);

    d1.showValue();
    d2.showValue();

    Demo::showCount();        // Calling static function using class name

    return 0;
}
```
***
---
# Friend class & function
## friend class:
- A friend class can access private and protected members of other classes in which it is declared as a friend. It is sometimes useful to allow a particular class to access private and protected members of other classes.

## friend function:
- Like a friend class, a friend function can be granted special access to private and protected members of a class in C++. They are not the member functions of the class but can access and manipulate the private and protected members of that class for they are declared as friends.

```cpp
#include <iostream>
using namespace std;

class B;  // Forward declaration

class A {
private:
    int secret;

public:
    A(int s) : secret(s) {}

    // Friend Class
    friend class B;

    // Global Friend Function
    friend void showSecret(const A& obj);

    // Member function of another class
    friend void B::revealASecret(const A& a);
};

// 1️⃣ Friend Global Function
void showSecret(const A& obj) {
    cout << "Global Friend Function - Secret of A: " << obj.secret << endl;
}

// 2️⃣ Friend Class
class B {
public:
    void accessA(const A& a) {
        cout << "Friend Class B - Accessing A's secret: " << a.secret << endl;
    }

    // 3️⃣ Member Function of Another Class (also marked as friend in A)
    void revealASecret(const A& a) {
        cout << "B's Member Function (Friend of A) - Secret is: " << a.secret << endl;
    }
};

int main() {
    A a(42);
    B b;

    showSecret(a);         // Global friend function
    b.accessA(a);          // Friend class accessing private data
    b.revealASecret(a);    // Member function of B (declared as friend)

    return 0;
}
```
***
---
# Encapsulation
- wraping up data & methods in a single unit. 
- Think of it as hiding the data inside a capsule and providing controlled access using getters and setters.
```cpp
    #include <iostream>
using namespace std;

class BankAccount {
private:
    double balance; // Private data

public:
    // Constructor
    BankAccount(double initialBalance) {
        balance = initialBalance;
    }

    // Getter
    double getBalance() {
        return balance;
    }

    // Setter
    void deposit(double amount) {
        if (amount > 0)
            balance += amount;
    }

    void withdraw(double amount) {
        if (amount > 0 && amount <= balance)
            balance -= amount;
    }
};

int main() {
    BankAccount account(1000);

    account.deposit(500);
    account.withdraw(300);

    cout << "Final Balance: " << account.getBalance() << endl;

    return 0;
}

```
# Access Modifires

| Parent     | Child       | Result      |
|------------|-------------|-------------|
| public     | public      | public      |
| public     | private     | private     |
| public     | protected   | protected   |
|------------|-------------|-------------|
| protected  | public      | protected   |
| protected  | protected   | protected   |
| protected  | private     | private     |
|------------|-------------|-------------|
| private    | public      | private     |
| private    | protected   | private     |
| private    | private     | private     |


# Abstraction
-  Abstraction is the concept of hiding  implementation details and showing only essential features to the user.
- It's like using a car – you know how to drive it (interface), but not necessarily how the engine works (implementation).
```cpp
#include <iostream>
using namespace std;

// Abstract class
class Shape {
public:
    virtual void draw() = 0; // Pure virtual function
};

class Circle : public Shape {
public:
    void draw() override {
        cout << "Drawing a Circle" << endl;
    }
};

class Rectangle : public Shape {
public:
    void draw() override {
        cout << "Drawing a Rectangle" << endl;
    }
};

int main() {
    Shape* shape1 = new Circle();
    Shape* shape2 = new Rectangle();

    shape1->draw();  // Output: Drawing a Circle
    shape2->draw();  // Output: Drawing a Rectangle

    delete shape1;
    delete shape2;

    return 0;
}

```
# Abstruct class
- An abstract class is a class that contains at least one pure virtual function. It cannot be instantiated (you can't create objects of it directly). It's meant to be a base class.
```cpp
class Shape {
public:
    virtual void draw() = 0; // Pure virtual function → makes Shape an abstract class
};
```
# virtual
- Used to tell the compiler to enable runtime polymorphism. It ensures that the right function is called based on the object type (not pointer type).

## virtal function 
- A virtual function allows function overriding in derived classes.
```cpp
#include <iostream>
using namespace std;

class Animal {
public:
    virtual void sound() { // virtual function
        cout << "Animal sound" << endl;
    }
};

class Dog : public Animal {
public:
    void sound() override {
        cout << "Dog barks" << endl;
    }
};

int main() {
    Animal* a = new Dog();  // Base pointer, derived object
    a->sound();  // Calls Dog's sound() due to virtual function

    delete a;
    return 0;
}
```
## pure virtual function
- Used when a base class only wants to declare a function, but the implementation must be in the derived class.
```cpp
class Shape {
public:
    virtual void draw() = 0;  // Pure virtual function
};

class Circle : public Shape {
public:
    void draw() override {
        cout << "Drawing Circle" << endl;
    }
};
```
# Inharitance
- The capability of a class to derive properties and characteristics from another class is called Inheritance.

| **Mode**                 | **Public Members**                                         | **Protected Members**                                       | **Notes**                                      |
|--------------------------|------------------------------------------------------------|--------------------------------------------------------------|------------------------------------------------|
| **Public Inheritance**   | Become **public** in the derived class                     | Remain **protected** in the derived class                    | Commonly used, preserves access levels         |
| **Protected Inheritance**| Become **protected** in the derived class                  | Remain **protected** in the derived class                    | Less common, restricts access to derived only  |
| **Private Inheritance**  | Become **private** in the derived class                    | Become **private** in the derived class                      | Default mode if not specified                  |


## Types of inharitance
- Single inheritance
- Multilevel inheritance
- Multiple inheritance
- Hierarchical inheritance
- Hybrid inheritance

  


## 1️⃣ Single Inheritance
One derived class inherits from one base class.

```
   [Base]
      |
   [Derived]
```

---

## 2️⃣ Multilevel Inheritance
A class is derived from a class which is also derived from another class.

```
   [Base]
      |
   [Intermediate]
      |
   [Derived]
```

---

## 3️⃣ Multiple Inheritance
A class is derived from more than one base class.

```
 [Base1]   [Base2]
     \     /
     [Derived]
```
### Problem inharitance ambiguity/ Daimond problem
```
       [A]         ← Base class
      /   \
   [B]     [C]     ← Both inherit from A
      \   /
       [D]          ← Inherits from both B and C


```
---

## 4️⃣ Hierarchical Inheritance
Multiple classes inherit from a single base class.

```
       [Base]
        /   \
 [Derived1] [Derived2]
```

---

## 5️⃣ Hybrid Inheritance
Combination of two or more types of inheritance (e.g., Multiple + Hierarchical).

```
       [Base1]
        /   \
 [Derived1] [Base2]
        \   /
        [Derived2]
```
### Inharitance cannot effect Static, Friend, Constructor & Destructor
---
# Polimorphism
- Polymorphism means "many forms". It allows a single function or method to behave differently based on the object that invokes it.

### Imagine a function called draw() — it behaves differently depending on the object:

- For Circle, it draws a circle.

- For Rectangle, it draws a rectangle.

### But you call the same function: shape.draw(); — that’s polymorphism!
# Types
- ## Compile time/ Early Binding ##
    - ### Function overloading ###
        - same name but changing the number of arguments or changing the type of arguments.
```cpp
#include <iostream>
using namespace std;

class Calculator {
public:
    int add(int a, int b) {
        return a + b;
    }

    float add(float a, float b) {
        return a + b;
    }

    int add(int a, int b, int c) {
        return a + b + c;
    }
};

int main() {
    Calculator calc;
    cout << "add(2, 3): " << calc.add(2, 3) << endl;
    cout << "add(2.5f, 3.1f): " << calc.add(2.5f, 3.1f) << endl;
    cout << "add(1, 2, 3): " << calc.add(1, 2, 3) << endl;
    return 0;
}
```
- ## Compile time/ Early Binding ##
    - ### operator overloading ###
        - In C++, operator overloading allows you to redefine the behavior of operators (like +, -, *, etc.) for user-defined types (classes), enabling intuitive manipulation of objects using familiar syntax. 
```cpp
#include <iostream>
using namespace std;

class Complex {
    float real, imag;

public:
    Complex(float r = 0, float i = 0) : real(r), imag(i) {}

    // Overload + operator
    Complex operator + (const Complex& other) {
        return Complex(real + other.real, imag + other.imag);
    }

    void display() {
        cout << real << " + " << imag << "i" << endl;
    }
};

int main() {
    Complex c1(2, 3), c2(1, 4);
    Complex c3 = c1 + c2;  // Calls overloaded + operator
    c3.display();          // Output: 3 + 7i
    return 0;
}

```

- Run time/ Late Binding
    - Function overridding
        - Function overriding is when a derived class provides a specific implementation of a function already defined in its base class.
```cpp
#include <iostream>
using namespace std;

class Animal {
public:
    virtual void speak() {
        cout << "Animal speaks" << endl;
    }
};

class Dog : public Animal {
public:
    void speak() override {
        cout << "Dog barks" << endl;
    }
};

int main() {
    Animal* a;
    Dog d;
    a = &d;

    a->speak(); // Output: Dog barks
    return 0;
}
```

# Ways to Achieve Polymorphism in C++

Polymorphism in C++ is of two main types:

## 🔷 1. Compile-Time Polymorphism (Early Binding)
Achieved at compile time. No `virtual` required.

| Method              | Description                                | Example                          |
|---------------------|--------------------------------------------|----------------------------------|
| Function Overloading| Same function name, different params       | `add(int, int)` vs `add(float, float)` |
| Operator Overloading| Redefine operators for user-defined types  | `Complex + Complex`              |
| Templates           | Enables generic code across data types     | `template <typename T>`          |

## 🔶 2. Run-Time Polymorphism (Late Binding)
Achieved at runtime using virtual functions and inheritance.

| Method               | Description                                            |
|----------------------|--------------------------------------------------------|
| Function Overriding  | Redefine base class function in derived class          |
| Virtual Functions    | Tells compiler to bind function at runtime             |
| Pure Virtual Functions | Forces derived classes to implement the function (abstract class) |
| Abstract Classes     | Base classes with at least one pure virtual function   |

## 🧪 Examples

### ✅ Function Overloading (Compile-time)
```cpp
int add(int a, int b);
float add(float a, float b);
```

### ✅ Operator Overloading (Compile-time)
```cpp
Complex operator+(const Complex& other);
```

### ✅ Function Overriding (Runtime)
```cpp
class A {
public:
    virtual void show() { cout << "A" << endl; }
};

class B : public A {
public:
    void show() override { cout << "B" << endl; }
};
```

### ✅ Pure Virtual Function & Abstract Class (Runtime)
```cpp
class Shape {
public:
    virtual void draw() = 0; // pure virtual
};

class Circle : public Shape {
public:
    void draw() override { cout << "Drawing Circle" << endl; }
};
```

## 📘 Summary Table

| Technique            | Type         | Keyword Used           | Notes                              |
|----------------------|--------------|-------------------------|------------------------------------|
| Function Overloading | Compile Time | None                    | Same name, different params        |
| Operator Overloading | Compile Time | `operator`              | Redefine how operators behave      |
| Function Overriding  | Run Time     | `virtual`, `override`   | Must use base pointer/reference    |
| Pure Virtual Functions| Run Time    | `= 0`                   | Makes a class abstract             |
| Virtual Functions    | Run Time     | `virtual`               | Enables dynamic dispatch           |
| Templates            | Compile Time | `template`              | Used for generic programming       |

# Dynaic Method Dispatch
- Dynamic Method Dispatch is a mechanism in object-oriented programming (especially in C++) where a call to an overridden function is resolved at runtime, not at compile-time.
```cpp
#include <iostream>
using namespace std;

class Animal {
public:
    virtual void sound() { // virtual enables dynamic dispatch
        cout << "Animal sound" << endl;
    }
};

class Dog : public Animal {
public:
    void sound() override {
        cout << "Dog barks" << endl;
    }
};

int main() {
    Animal* a;  // base class pointer
    Dog d;

    a = &d;
    a->sound(); // Calls Dog's version => dynamic dispatch

    return 0;
}
```
- Common in scenarios like:
    - GUI event handling
    - Game character behavior
    - Plugins and frameworks
### with out  virtual  it always call parent. ###





