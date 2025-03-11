# Week 1 Practice Questions

## 1. Complex numbers
Implement a simple `Complex` number class that overloads the **addition** (`operator+`) and **subtraction** (`operator-`) operator (both should return a new `Complex` number). Addition / subtraction of complex numbers is done by adding / subtracting individual real and imaginary components.

The `Complex` class should have **two constructors**:
- a *default constructor*, that will initialize the *real and imaginary (i)* components of the number to 0.
- a constructor with *two parameters* to initialize the *real and imaginary (i)* components.

Example:
- `Complex c1;` `-->` `_real` = 0, `_imaginary` = 0
- `Complex c2(3, -2);` `-->` `_real` = 3, `_imaginary` = -2
- `Complex c3(4, 1);` `-->` `_real` = 4, `_imaginary` = 1
- `Complex c4 = c2 + c3;` `-->` `_real` = 7, `_imaginary` = -1
- `Complex c5 = c2 - c3;` `-->` `_real` = -1, `_imaginary` = -3

### Solution:
```cpp
class Complex {
private:
    double _real;
    double _imaginary;
public:
    Complex();
    Complex(double r, double i);
    Complex operator+(const Complex& other) const;
    Complex operator-(const Complex& other) const;
    double real() const;
    double imaginary() const;
};

Complex::Complex(): _real(0), _imaginary(0) {}

Complex::Complex(double r, double i): _real(r), _imaginary(i) {}

double Complex::real() const {
    return _real;
}

double Complex::imaginary() const {
    return _imaginary;
}

Complex Complex::operator+(const Complex& other) const {
    return Complex(_real + other._real, _imaginary + other._imaginary);
}

Complex Complex::operator-(const Complex& other) const {
    return Complex(_real - other._real, _imaginary - other._imaginary);
}
```

## 2. Pair-and-swap
Create a **templated** class called `Pair` that has two public members: `T first` and `T second` (T being the *template type*). 
Moreover, the class should have a *constructor* through which you can assign "first" and "second" values.

Afterwards, implement *two methods* to swap the values of `first` and `second` members: one via a **pointer**, and another one via a **reference**.

Example:
- `Pair<int> p1(3, 4)` `-->` `first` = 3, `second` = 4
- `swap_pair_values(p1)` `-->` (swap by *reference*) `first` = 4, `second` = 3
- `swap_pair_values(&p1)` `-->` (swap by *pointer*) `first` = 3, `second` = 4

### Solution:
```cpp
template <typename T>
class Pair {
public:
    T first{0};
    T second{0};
    
    Pair(T f, T s): first(f), second(s) {}
};

template <typename T>
void swap_pair_values(Pair<T>* pair) {
    if (pair != nullptr) {
        T temp = pair->first;
        pair->first = pair->second;
        pair->second = temp;
    }
}

template <typename T>
void swap_pair_values(Pair<T>& pair) {
    T temp = pair.first;
    pair.first = pair.second;
    pair.second = temp;
}
```

## 3. Rule of 5
You need to implement a **templated class** `ResizingArray` that represents a *dynamic array capable of resizing*. Your task is to implement all the member functions declared in the class header.

Implement the following aspects:

1. **Constructor** (`ResizingArray(int s)`): allocate a *dynamic array* of size `s`.

2. **Destructor** (`~ResizingArray()`): properly *deallocate* any dynamic memory.

3. **Copy and move semantics (constructors + operators)**: properly handle all copy and move operations

4. **Size Accessor** (`int size() const`): return the current size of the array.

5. **Data Accessor** (`T* data() const`): return a pointer to the underlying array data.

6. **Resize Function** (void resize(int new_size)): allocate a new dynamic array of size new_size, and copy over all old elements into it.

### Solution:
```cpp
template <typename T>
class ResizingArray {
private:
    int _size;
    T* _data;
public:
    ResizingArray(int s);
    ~ResizingArray();
    ResizingArray(const ResizingArray& other);
    ResizingArray& operator=(const ResizingArray& other);
    ResizingArray(ResizingArray&& other) noexcept;
    ResizingArray& operator=(ResizingArray&& other) noexcept;
    int size() const;
    T* data() const;
    void resize(int new_size);
};

template <typename T>
ResizingArray<T>::ResizingArray(int s): _size(s) {
    _data = new T[s];
}

template <typename T>
ResizingArray<T>::~ResizingArray() {
    delete[] _data;
}

template <typename T>
int ResizingArray<T>::size() const { return _size; }

template <typename T>
T* ResizingArray<T>::data() const { return _data; }

template <typename T>
ResizingArray<T>::ResizingArray(const ResizingArray& other) {
    _size = other._size;
    _data = new T[other._size];
    
    for (int i = 0; i < _size; i++) {
        _data[i] = other._data[i];
    }
}

template <typename T>
ResizingArray<T>& ResizingArray<T>::operator=(const ResizingArray<T>& other) {
    if (this != &other) {
        delete [] _data;
        _size = other._size;
        _data = new T[other._size];
        for (int i = 0; i < _size; i++) {
            _data[i] = other._data[i];
        }
    }
    return *this;
}

template <typename T>
ResizingArray<T>::ResizingArray(ResizingArray&& other) noexcept {
    _size = other._size;
    _data = other._data;
    other._size = 0;
    other._data = nullptr;
}

template <typename T>
ResizingArray<T>& ResizingArray<T>::operator=(ResizingArray<T>&& other) noexcept {
    if (this != &other) {
        delete [] _data;
        _size = other._size;
        _data = other._data;
        other._size = 0;
        other._data = nullptr;
    }
    return *this;
}

template <typename T>
void ResizingArray<T>::resize(int new_size) {
    T* new_data = new T[new_size]{};
    for (int i = 0; i < _size; i++) {
        new_data[i] = _data[i];
    }
    delete[] _data;
    _data = new_data;
    _size = new_size;
}
```