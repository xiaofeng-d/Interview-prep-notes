

## Inheritance ##

Object-oriented programming based on:

 **encapsulation, abstraction, inheritance, polymorphism**



## Virtual Function ##



1. Virtual function implements **polymorphism**;  Virtual function makes sure that derived class functions gets called, even with base class pointer reference.

Example:

```
class Fish
{
public: 
     ～virtual void Swim () 
{ cout << "Fish Swims " << endl; }

 };

class Tuna:public Fish

{

public:

 void Swim( ) 

{ cout << "Tuna Swims " << endl;}

};


```









