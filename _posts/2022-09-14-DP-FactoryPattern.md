AbstractFactory 패턴 <https://m.blog.naver.com/hblee4119/221961302794>


<https://digestcpp.com/designpattern/creational/factory/>

```ruby
class Pizza
{
public:
    virtual void BakePizza()=0; 
    virtual void PackPizza()=0;
};

class DominoPanerrPiza : public Pizza
{
public:
    void BakePizza()  {  std::cout<<"DominoPanerrPiza is ready"<<std::endl;  }
    void PackPizza()  {  std::cout<<"DominoPanerrPiza is packed"<<std::endl;  }
};

class DominoCheesePiza : public Pizza {
public:
    void BakePizza() { std::cout<<"DominoCheesePiza is ready"<<std::endl; }
    void PackPizza() { std::cout<<"DominoCheesePiza is packed"<<std::endl; }
};

class PizzaFactory
{
protected:
    std::unique_ptr<Pizza>_mPizza;
public:
    Pizza* GetPizza(std::string type)
        {  
        _mPizza.reset(CreatePizza(type));
            return _mPizza.release();     
    }
private:
    virtual Pizza* CreatePizza(std::string type)=0;
};

//DominoPizzaFactory
class DominoPizzaFactory:public PizzaFactory
{
private:
    Pizza* CreatePizza(std::string type)
    {
            Pizza* pz = nullptr;
            if(type == "Paneer")
                pz = new DominoPanerrPiza;
            else if (type == "Cheese")
                 pz = new DominoCheesePiza;
            else
                nullptr;
            return  pz;
    }
};

//Client

int main()
{
    std::cout<<"In Main"<<std::endl;
    //Select Factory
    std::unique_ptr<PizzaFactory>ptr{nullptr};
    ptr.reset(new DominoPizzaFactory);

    //Order the Pizza with type
    std::cout<<"Ordering Paneer Pizza from Domino"<<std::endl;
    std::unique_ptr<Pizza>upPz{nullptr};
    upPz.reset(ptr->GetPizza("Paneer"));
    upPz->BakePizza();
    upPz->PackPizza();

    std::cout<<"!!!!!!!!!!Got the Pizza!!!!!!!!!"<<std::endl;
    return 0;
}

```
