# Week 7 Discussion  


## C++ Const

Write your text answers in the shared slides for discussion.

We've seen that the `const` keyword can be used in several places, which can be confusing.

Below is an incomplete, but compilable, snippet of code that represents a chemical reaction. This consists of `Molecule`s
that participate in the reaction and their stoichiometric coefficients.

```C++

class Reaction
{
    private:
    
      std::vector<Molecule> molecules_;
      std::vector<double> coefficients_;

    public:

      void add_component(Molecule mol, double coefficient)
      {
        molecules_.push_back(mol);
        coefficients_.push_back(coefficient);
      }
      
      const Molecule & get_component(int index) const
      {
        return this->molecules_.at(index);
      }

      Molecule & get_component(int index)
      {
        return molecules_.at(index);
      }

      ~Molecule()
      {
          coords_.clear();
          num_atoms_ = 0;
      }
};

Reaction r(..);
const Molecule & m = r.get_component(0);
m.atoms.clear()

```

We see `const` used in 3 places:

1. What are the three different places, and what do they each mean? That is, what is the `const` referring to (what is being kept constant)?
2. Could you rewrite the `add_component` function to use `const`? Why or why not?
3. You can rewrite the first `get_component` function and get rid of one `const`. Which one? Is there a benefit to that?
4. Is the second `get_component` function a good idea?
5. How does the compiler know which `get_component` function to use when it is called?



## C++ Class Design

Write your text answers on the shared class slides, and modify the code in `main.cpp` as necessary.

1. What are the main differences between classes in C++ and classes in Python?
2. What are some possible mistakes or bad design decisions in the following code?
3. How could chemical bonds be represented in this class? Remember that bonds have an 'order' to them (single, double, triple, etc)
4. What are some advantages and disadvantages to storing coordinates as a single (flattened) list?
5. If you were to need to use a molecule class for a calculation, what other features would you like to have?
6. What else could be done to improve this class? Modify the code in `main.cpp` to improve this class!

```C++
#include <vector>

class Molecule
{
    protected:
      int num_atoms_;

      // Coordinates are stored "flattened" as a single array
      std::vector<double> coords_;

    public:
      Molecule()
        : num_atoms_(0)
      {}

      Molecule(std::vector<double> coords)
        : coords_(coords)
      {
        num_atoms_ = coords.size() / 3;
      }

      const std::vector<double> & get_coords(void) const { return coords_; }

      void set_coords(std::vector<double> coords)
      {
        coords_ = coords;
      }
};
```


## C++ Templates

C++ templates are often used to write functions and classes that are generic with respect to types (for example,
types of functions arguments or types of objects stored in classes). However, templates can also be used
to define compile-time integer variables. For example, the following code will run the `print_integer` function
with `I` = 3.

```C++
template<int I>
void print_integer()
{
  std::cout << "Integer is " << I << std::endl;
}

int main(void)
{
  print_integer<3>();
}
```


### Questions

1. What is the difference between this and just passing the argument in? When can you/can't you use this?
2. Can you think of an example where you have seen this already with classes? (Hint: It was a class that is part of the Standard Library).
3. In `main.cpp`, add a function that calculates a factorial of a template argument. What would be the advantage/disadvantage of this function?
4. Can you write code that calculates a factorial using no `if` statements or `for` loops using templates? Hint: You need a template specialization of a function.
