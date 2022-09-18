<https://digestcpp.com/designpattern/creational/factory/>

```ruby
#include <iostream>

using std;

class Pizza
{
    public :
    virtual void BakePizza() = 0;
    virtual void PackPizza() = 0;
};

class DominoPanerrPizza : public Pizza
{
    public:
    void BakePizza() { cout << "DomioPanerrPizza is ready" << endl;}
    void packPizza() { cout << "DomioPanerrPizza is  packed" << endl;}
}

class DominoCheesePizza : public Pizza
{
    public:
    void BakePizza() { cout << "DomioCheesePizza is ready" << endl;}
    void packPizza() { cout << "DomioCheesePizza is  packed" << endl;}
}

class PizzaFactory
{
    protected:
    std::unique_ptr<Pizza>_mPizza;
    public:
    Pizza *GetPizza(std::string type) {
        _mPizza.reset(CreatePizza(type));
        return _mPizza.release();
    }
    private:
    virtual Pizza* CreatePizza(std::string type) = 0;
};

class DominoPizzaFactory:public PizzaFactory
{
    private:
        Pizza *createPizza(std::string type) {
            pizza *pz = nullptr;
            if (type == "Paneer")
                pz = new DominoPanerrPizza;
            else if (type == "Cheese")
                pz = new DominoCheesePizza;
            return pz;             
        }
};

//client
int main()
{
    std::cout << "In Main" << std::endl;
    //Select Factory
    std::unique_ptr<PizzaFactory> ptr{nullptr};
    ptr.reset(new DominoPizzaFactory);
    //Order the Pizza with Type
    std::cout << "Ordering Paneer Pizza from Domino" << std::endl;
    std::unique_ptr<Pizza> upPz{nullptr};
    upPz.reset(ptr->GetPizza("Paneer"));
    upPz->BakePizza();
    upPz->PackPizza();
    std::cout << "!!!!Got the Pizza!!!!!!!"<<std::endl;
    return 0;
}
}
```
