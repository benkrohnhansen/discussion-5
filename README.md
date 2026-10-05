# Week 7 Discussion  


[Link to slides](https://docs.google.com/presentation/d/1OapdyKlvJpard0NzeoOSCW_bMbiccTxX2wCC1yJX2Zc/edit?slide=id.g3f9c62c1b5f_0_0#slide=id.g3f9c62c1b5f_0_0).
Please make a new slide titled with your Group # and answer the questions below.


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
6. Why is `&` used in both versions of `get_component`? How would the functionality change if that was left out?



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


## RDKit Applications

### Questions
1. **Read-only vs. editable molecules.** RDKit has two molecule classes: ROMol (a "read-only" molecule) and RWMol(a "read-write" molecule that allows adding and removing atoms and bonds). Most analysis functions accept a read-only molecule. Computing ring information is slow, so the library computes it once and stores the result inside the molecule. Why is it safer to store results like this in a molecule that can't be changed?

2. **Bonds**. A molecule created in RDKit, for downstream modeling purposes, has to know which bonds are single, double, or aromatic. Why might a library use a named set of choices (SINGLE, DOUBLE, TRIPLE, AROMATIC) rather than storing the bond order as a number? (Hint: "aromatic" isn't really a number, it's a separate category. )
 
3. **One molecule, many shapes**. Our `Molecule` class keeps the atoms and the coordinates together. So to store 60 conformers of ibuprofen, you'd need 60 `Molecule` objects, each with its own copy of the same atoms and bonds. RDKit stores the atoms and bonds once, and each conformer holds only its coordinates. Why is RDKit's way better?
