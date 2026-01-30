---
title: C++ - Type Erasure
date: 2026-01-30 17:44:39
tags:
  - "What's Cpp"
  - cpp
  - programming
  - language
categories:
  - - cpp
    - idiom
  - - language
    - cpp
---

## Key points

继承中的多态：子类实现（implement）父类中定义的接口。

模板中的多态：不同的类遵守（conform to）相同的接口。

使用基于模板的多态的问题是caller也要是模板，即传染性。总的来说，有需要异质（heterogeneous）容器的需求；而一般的容器都要求元素是相同的类型（homogeneous）。

OO中的interface类比于template中的concept；OO中的interface和implement/instance类比于concept和model。

`std::any`是一个类（不是类模板），以type-safe的方式存`is_copy_constructible`类型的值。即只要类型conform to `is_copy_constructible`接口，它的值就能存在`any`对象里面。`any`擦除了任何`is_copy_constructible`类型的实际类型。

`std::function`是个类模板，它是一个function wrapper，可以存并调用任何`CopyConstructible`且`Callable`的target：function pointer，lambda expression，bind expression，function object/functor，pointer to member function（non-static/static，virtual/non-virtual），pointer to data member。`function`擦除了target的实际类型，而其模板参数表明了target应该conform to的函数原型。

尽管`etl::delegate`看上去和`std::function`很像，但是它没有使用类型擦除技术；是否有类型擦除，主要还是看它是存wrapper还是不是。

### 和CRTP的不同

Type Erasure实现了runtime polymorphism（但不依靠继承），CRTP（Curiously Recurring Template Pattern）实现了compile-time polymorphism（static dispatch或者policy-based design）。Type Erasure以性能换取弹性（trades performance for flexibility），CRTP以弹性换取性能（trades flexibility for performance），更强的type safety。

简单从表相上来看，Type Erasure是模板类继承自普通类，CRTP是普通类继承自模板类。

```cpp
// Type Erasure

class Concept {
public:
    virtual void interface() = 0;
};

template<typename Concrete>
class Model : public Concept {
    Concrete* object; // hold the concrete object
public:
    virtual void interface() override {
        // dispatch to object->implementation()
    }
};

class Concrete {
    void implementation() {} // don't have to be virtual
};
```

```cpp
// CRTP

template<typename Derived>
struct Base {
  void interface() { static_cast<Derived*>(this)->implementation(); }
};

struct Derived : public Base<Derived> {
  void implementation() {}
};
```

## Examples

```cpp
class SeeAndSay {
    class AnimalConcept {
    public:
        virtual const char* see() const = 0;
        virtual const char* say() const = 0;
    };

    template<typename T>
    class AnimalModel : public AnimalConcept {
        const T* m_animal;
    public:
        AnimalModel(const T* animal) : m_animal(animal) {}
        const char* see() const { return m_animal->see(); }
        const char* say() const { return m_animal->say(); }
    };

    vector<AnimalConcept*> m_animals;

public:
    template<typename T>
    void addAnimal(T* animal) {
        m_animals.push_back(new AnimalModel(animal));
    }

    void pullTheString() {
        for (auto animal : m_animals) {
            cout << "The " << animal->see()
                 << " says '" << animal->say()
                 << "'!" << endl;
        }
    }
};

struct Cow {
    const char* see() const { return "cow"; }
    const char* say() const { return "moo"; }
};

struct Pig {
    const char* see() const { return "pig"; }
    const char* say() const { return "oink"; }
};

struct Dog {
    const char* see() const { return "dog"; }
    const char* say() const { return "woof"; }
};

int main() {
    SeeAndSay seeAndSay;
    seeAndSay.addAnimal(new Cow);
    seeAndSay.addAnimal(new Pig);
    seeAndSay.addAnimal(new Dog);
    seeAndSay.pullTheString();

    return 0;
}
```

```cpp
class Object {
    struct ObjectConcept {
        virtual ~ObjectConcept() = default;
    };

    template<typename T>
    struct ObjectModel : ObjectConcept {
        ObjectModel(const T& t) : object(t) {}
        virtual ~ObjectModel() = default;
    private:
        T object;
    };

    shared_ptr<ObjectConcept> object;

public:
    template<typename T>
    Object(const T& t) : object(make_shared<ObjectModel<T>>(t)) {}
};

struct Foo {};
struct Bar {};

int main() {
    vector<Object> objects;

    objects.push_back(Object(1));
    objects.push_back(Object(Foo()));
    objects.push_back(Object(Bar()));

    return 0;
}
```

```cpp
class Greeter {
public:
    template<class T>
    Greeter(T data) : self_(make_shared<Model<T>>(data)) {}

    void greet(const string& name) const {
        self_->greet(name);
    }

private:
    struct Concept {
        virtual ~Concept() = default;
        virtual void greet(const string&) const = 0;
    };

    template<class T>
    class Model : public Concept {
    public:
        Model(T data) : data_(data) {}

        virtual void greet(const string& name) const override {
            data_.greet(name);
        }

    private:
        T data_;
    };

    shared_ptr<const Concept> self_;
};

struct English {
    void greet(const string& name) const {
        cout << "Good day " << name << ". How are you?\n";
    }
};

struct French {
    void greet(const string& name) const {
        cout << "Bonjour " << name << ". Comment ca va?\n";
    }
};

void greet_tom(const Greeter& g) {
    g.greet("Tom");
}

int main() {
    greet_tom(English());
    greet_tom(French());

    return 0;
}
```

## References

- [blog1](https://davekilian.com/cpp-type-erasure.html)
- [cplusplus.com](https://cplusplus.com/articles/oz18T05o/)
- [blog2](https://aherrmann.github.io/programming/2014/10/19/type-erasure-with-merged-concepts/)
- blog3 [part i](https://akrzemi1.wordpress.com/2013/11/18/type-erasure-part-i/), [part ii](https://akrzemi1.wordpress.com/2013/12/06/type-erasure-part-ii/), [part iii](https://akrzemi1.wordpress.com/2013/12/11/type-erasure-part-iii/), [part iv](https://akrzemi1.wordpress.com/2014/01/13/type-erasure-part-iv/)
