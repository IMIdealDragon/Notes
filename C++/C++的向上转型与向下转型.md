# C++的向上转型与向下转型

类其实也是一种数据类型，也可以发生数据类型转换，不过这种转换只有在基类和派生类之间才有意义，并且只能将派生类赋值给基类，包括将**派生类对象赋值给基类对象、将派生类指针赋值给基类指针、将派生类引用赋值给基类引用**，因为派生类内容多于基类，因此切割赋值即可，但是如果是反过来，我无从可赋，就不行了。这在 C++ 中称为向上转型（Upcasting）。相应地，将基类赋值给派生类称为向下转型（Downcasting）。
向上转型非常安全，可以由编译器**自动完成**；也就是隐式转换。

 向下转型有风险，需要程序员手动干预（有时编译器认为只是错误） 

 赋值的**本质**是将现有的数据写入已分配好的内存中，对象的内存只包含了成员变量，所以对象之间的赋值是成员变量的赋值，成员函数不存在赋值问题。 

将派生类对象赋值给基类对象时，会**舍弃派生类新增的成员**，所以**不存在安全问题**。

即使将派生类对象赋值给基类对象，基类对象也不会包含派生类的成员，所以依然不能通过基类对象来访问派生类的成员

 这种转换关系是不可逆的，只能用派生类对象给基类对象赋值，而不能用基类对象给派生类对象赋值。理由很简单，**基类不包含派生类的成员变量，无法对派生类的成员变量赋值。同理，同一基类的不同派生类对象之间也不能赋值。** 

 要理解这个问题，还得从赋值的本质入手。赋值实际上是向内存填充数据，当数据较多时很好处理，舍弃即可，所以不会发生赋值错误。但当数据较少时，问题就很棘手，编译器不知道如何填充剩下的内存；如果本例中有b= a;这样的语句，编译器就不知道该如何给变量 m_b 赋值，所以会发生错误。 

派生类指针赋值给基类指针
除了可以将派生类对象赋值给基类对象（对象变量之间的赋值），还可以将派生类指针赋值给基类指针（对象指针之间的赋值）
下面看一段示例代码：

```cpp
#include<iostream>
using namespace std;
class A{
public:
    A(int a);
public:
    void display();
protected:
    int m_a;
};
A::A(int a): m_a(a){ }
void A::display(){
    cout<<"Class A: m_a="<<m_a<<endl;
}
//中间派生类B
class B: public A{
public:
    B(int a, int b);
public:
    void display();
protected:
    int m_b;
};
B::B(int a, int b): A(a), m_b(b){ }
void B::display(){
    cout<<"Class B: m_a="<<m_a<<", m_b="<<m_b<<endl;
}
//基类C
class C{
public:
    C(int c);
public:
    void display();
protected:
    int m_c;
};
C::C(int c): m_c(c){ }
void C::display(){
    cout<<"Class C: m_c="<<m_c<<endl;
}
//最终派生类D
class D: public B, public C{
public:
 int temp;
    D(int a, int b, int c, int d);
public:
    void display();
private:
    int m_d;
};
D::D(int a, int b, int c, int d): B(a, b), C(c), m_d(d){temp = 0; }
void D::display(){
    cout<<"Class D: m_a="<<m_a<<", m_b="<<m_b<<", m_c="<<m_c<<", m_d="<<m_d<<endl;
}
int main(){
    A *pa = new A(1);
    B *pb = new B(2, 20);
    C *pc = new C(3);
    D *pd = new D(4, 40, 400, 4000);
    pa = pd;
    pa -> display();
    pb = pd;
    pb -> display();
    pc = pd;
    pc -> display();
    cout<<"-----------------------"<<endl;
    cout<<"pa="<<pa<<endl;
    cout<<"pb="<<pb<<endl;
    cout<<"pc="<<pc<<endl;
    cout<<"pd="<<pd<<endl;
 A a(1);                        //以下代码可用来验证最后的结论
 D d(4,40,400,4000);
    A *pa1 = &a;
 D *pd1 = &d;
 pa1 = pd1;
 pa1->display();
 a.display();

    return 0;

}
```


解释：
我们将派生类指针 pd 赋值给了基类指针 pa，从运行结果可以看出，调用 display() 函数时虽然使用了派生类的成员变量，但是 display() 函数本身却是基类的。也就是说，将派生类指针赋值给基类指针时，通过基类指针只能使用派生类的成员变量，但不能使用派生类的成员函数，这看起来有点不伦不类，究竟是为什么呢？  
pa 本来是基类 A 的指针，现在指向了派生类 D 的对象，这使得隐式指针 this 发生了变化，也指向了 D 类的对象，所以最终在 display() 内部使用的是 D 类对象的成员变量，相信这一点不难理解。
编译器虽然通过指针的指向来访问成员变量，但是却不通过指针的指向来访问成员函数：**编译器通过指针的类型来访问成员函数。对于 pa，它的类型是 A，不管它指向哪个对象，使用的都是 A 类的成员函数**。 
    概括起来说就是：编译器通过指针来访问成员变量，指针指向哪个对象就使用哪个对象的数据；编译器通过指针的类型来访问成员函数，指针属于哪个类的类型就使用哪个类的函数。

本例中我们将最终派生类的指针 pd 分别赋值给了基类指针 pa、pb、pc，按理说它们的值应该相等，都指向同一块内存，但是运行结果却有力地反驳了这种推论，只有 pa、pb、pd 三个指针的值相等，pc 的值比它们都大。也就是说，执行pc = pd;语句后，pc 和 pd 的值并不相等。

## 相应的转型函数

因为之前说了，向上是安全的，就是隐式就可以完成

```cpp
class Base{ };
class Derived : public base{ };
Base* Bptr;
Derived* Dptr;
Bptr = Dptr; //编译正确，允许隐式向上类型转换
Dptr = Bptr；//编译错误，C++不允许隐式的向下转型；
```

 正如上面所述，类层次间的向下转型是不能通过隐式转换完成的。此时要向达到这种转换，可以借助static_cast 或者dynamic_cast。 

```cpp
class Base{ };
class Derived : public base{ };
Base* B;
Derived* D;
D = static_cast<Drived*>(B); //正确，通过使用static_cast向下转型
```

 需要注意的是：static_cast的使用，当且仅当类型之间可隐式转化时，static_cast的转化才是合法的。有一个例外，那就是类层次间的向下转型，static_cast可以完成类层次间的向下转型，当时向下转型无法通过隐式转换完成！ 

和static_cast不同，dynamic_cast涉及运行时的类型检查。如果向下转型是安全的（也就是说，如果基类指针或者引用确实指向一个派生类的对象），这个运算符会传回转型过的指针。如果downcast不安全（即基类指针或者引用没有指向一个派生类的对象），这个运算符会传回空指针。
ps：要使用dynamic_cast类中必须定义虚函数

```cpp
class Base{
public：
	virtual void fun(){} 
 };
class Drived : public base{
public:
	int i;
 };
Base *Bptr = new Drived()；//语句0
Derived *Dptr1 = static_cast<Derived *>(Bptr); //语句1；
Derived *Dptr2 = dynamic_cast<Derived *>(Bptr); //语句2；
```

 此时语句1和语句2都是安全的，因为此时Bptr确实是指向的派生类，虽然其类型被声明为Base*，但是其实际指向的内容确确实实是Drived对象，所以语句1和2都是安全的，Dptr1和Dptr2可以尽情访问Drived类中的成员，绝对不会出问题。
但是此时如果将语句0改为这样： 

```cpp
Base *Bptr = new Base()；
```

 那语句1就不安全了，例如访问Drived类的成员变量i的值时，将得到一个垃圾值。（延后了错误的发现）
语句2使得Dptr2得到的是一个空指针，对空指针进行操作，将发生异常，从而能够尽早的发现错误，这也是为什么说dynamic_cast更安全的原因。 