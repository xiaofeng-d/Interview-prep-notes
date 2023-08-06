

## Inheritance ##

- Object-oriented programming is based on:

 		**encapsulation, abstraction, inheritance, polymorphism**



## Virtual Function ##



1. Virtual function implements **polymorphism**;  it makes sure that derived class function gets called, even when called with base class pointer reference.

Example:  W/ virtual function: "Tuna Swims";  W/o virtual function: "Fish Swims".

```
#include<iostream>
using namespace std;

class Fish
{
public: 
     virtual void Swim () //virtual function
{ cout << "Fish Swims " << endl; }

 };

class Tuna:public Fish
{
public:

 	void Swim( ) 
{ cout << "Tuna Swims " << endl;
}

};

void Fishswim( Fish& inputFish )
{
  inputFish.Swim();
}

int main()
{
	Tuna object;
	Fishswim(object); 
	return 0;
}


```

2. Virtual Destructor ensures that derived class destructor is always invoked if base class destructor is

```
#include <iostream>
using namespace std;

class Vehicle
{
public:
    Vehicle(){ cout << "Vehicle constructor! " << endl; }
    virtual ~Vehicle(){ cout << "Vehicle destructor ! " << endl; } //ADDING VIRTUAL OR NOT MAKES A DIFFERENCE!
};

class Car: public Vehicle
{
public:
    Car(){ cout << "Car constructor! " << endl; }
    ~Car(){ cout << "Car destructor ! " << endl; }
};




int main() {
    std::cout << "Hello, World!" << std::endl;
    Vehicle* pMyRacer = new Car;
    delete pMyRacer;

    return 0;
}
```







