# Week 2 Practice Questions

## 1. add() and remove() operations
```cpp
template<typename Data>
struct Node {
    Data data;
    Node *next{};
};

template <typename Data>
class LinkedList {
private:
    Node<Data>* head{};
    int size{0};
public:
    LinkedList() = default;
    ~LinkedList();
    void add(int index, const Data& data); // insert data at position index
    void remove(int index); // delete a node from position index
    
    Data& get(int index);
    int count() const;
};
    
// Your implementation of add() and remove():
template <typename Data>
void LinkedList<Data>::add(int index, const Data& data) {
    if (index < 0 || index > size) {
        throw std::out_of_range("Index is out of range");
    }
    
    Node<Data>* new_node = new Node<Data>();
    new_node->data = data;
    
    if (head == nullptr) {
        head = new_node;
    } else {
        Node<Data>* current = head;
        for (int i = 0; i < index - 1; ++i) {
            current = current->next;
        }
        new_node->next = current->next;
        current->next = new_node;
    }
    ++size;
}

template <typename Data>
void LinkedList<Data>::remove(int index) {
    if (index < 0 || index >= size) {
        throw std::out_of_range("Index is out of range");
    }
    
    if (head == nullptr) {
        throw std::out_of_range("List is empty");
    } else if (head->next == nullptr) {
        delete head;
        head = nullptr;
    } else if (index == 0) {
        Node<Data>* temp = head;
        head = head->next;
        delete temp;
    } else {
        Node<Data>* current = head;
        for (int i = 0; i < index - 1; i++) {
            current = current->next;
        }
        Node<Data>* temp = current->next;
        current->next = current->next->next;
        delete temp;
    }
    --size;
}


// Do not edit the methods below:
template<typename Data>
Data& LinkedList<Data>::get(int index) {
    if (index < 0 || index >= size) {
        throw std::out_of_range("Index is out of range");
    }

    Node<Data>* current = head;
    for (int i = 0; i < index; ++i) {
        current = current->next;
    }
    return current->data;
}

template<typename Data>
int LinkedList<Data>::count() const {
    return size;
}

template<typename Data>
LinkedList<Data>::~LinkedList() {
    while (head != nullptr) {
        const Node<Data>* current = head;
        head = head->next;
        delete current;
    }
}
```

## 2. Concatenate lists
```cpp
template <typename Data>
void concatenate(LinkedList<Data>& l1, LinkedList<Data>& l2) {
    for (const int i: l2) {
        l1.add_to_back(i);
    }
}

template <typename Data>
void concatenate_and_steal(LinkedList<Data>& l1, LinkedList<Data>& l2) {
    Node<Data>* current = l1.head;
    l1.size += l2.size;
    
    if (current != nullptr) {
        while (current->next != nullptr) {
            current = current->next;
        }
        current->next = l2.head;
    }
    
    l2.head = nullptr;
    l2.size = 0;
}
```

## 3. Circular linked list
```cpp
template<typename Data>
struct Node {
    Data data;
    Node *next{};
};

template <typename Data>
class CircularLinkedList {
private:
    Node<Data>* head{};
    int size{0};
public:
    void add_to_front(const Data& data); // to be implemented
    void add_to_back(const Data& data); // to be implemented
    void remove_from_front(); // to be implemented
    void remove_from_back(); // to be implemented
    
    // already implemented; do not edit
    CircularLinkedList() = default;    
    ~CircularLinkedList();
    
    Data& get(int index);
    int count() const;
    Node<Data>* get_head() const;
};

template<typename Data>
void CircularLinkedList<Data>::add_to_front(const Data& data) {
    Node<Data>* new_node = new Node<Data>();
    new_node->data = data;
    new_node->next = head;
    Node<Data>* old_head = head;
    head = new_node;
    
    if (old_head == nullptr) {
        head->next = head;
    } else {
        Node<Data>* current = old_head;
        while (current->next != old_head) {
            current = current->next;
        }
        current->next = head;  
    }
    
    ++size;
}

template<typename Data>
void CircularLinkedList<Data>::remove_from_front() {
    if (head == nullptr) {
        throw std::out_of_range("List is empty");
    }
    
    if (head->next == head) {
        delete head;
        head = nullptr;
    } else {
        Node<Data>* old_head = head;
        head = head->next;
        
        Node<Data>* current = old_head;
        while (current->next != old_head) {
            current = current->next;
        }
        current->next = head;  
        
        delete old_head;
    }
    --size;
}

template<typename Data>
void CircularLinkedList<Data>::add_to_back(const Data& data) {
    Node<Data>* new_node = new Node<Data>();
    new_node->data = data;

    if (head == nullptr) {
        head = new_node;
        head->next = head;
    } else {
        new_node->next = head;
        Node<Data>* current = head;
        while (current->next != head) {
            current = current->next;
        }
        current->next = new_node;
    }
    ++size;
}

template<typename Data>
void CircularLinkedList<Data>::remove_from_back() {
    if (head == nullptr) {
        throw std::out_of_range("List is empty");
    } else if (head->next == head) {
        delete head;
        head = nullptr;
    } else {
        Node<Data>* temp = head;
        while (temp->next->next != head) {
            temp = temp->next;
        }
        delete temp->next;
        temp->next = head;
    }
    --size;
}

template<typename Data>
Data& CircularLinkedList<Data>::get(int index) {
    if (index < 0 || index >= size) {
        throw std::out_of_range("Index is out of range");
    }

    Node<Data>* temp = head;
    for (int i = 0; i < index; ++i) {
        temp = temp->next;
    }
    return temp->data;
}

// The methods below are already implemented; do not edit them.

template<typename Data>
int CircularLinkedList<Data>::count() const {
    return size;
}

template<typename Data>
Node<Data>* CircularLinkedList<Data>::get_head() const {
    return head;
}

template<typename Data>
CircularLinkedList<Data>::~CircularLinkedList() {
    while (head != nullptr) {
        remove_from_front();
    }
}

```

## 4. Polynomial
```cpp
struct Term {
    int coefficient;
    int exponent;
    Term* next{};
    
    Term() = default;
    explicit Term(int c, int e) : coefficient(c), exponent(e) {}
};

class Polynomial {
private:
    Term* head{};
    int size{0};
public:
    Polynomial() = default;
    
    ~Polynomial();
    Polynomial(const Polynomial& other);
    Polynomial& operator=(const Polynomial& other);
    Polynomial(Polynomial&& other) noexcept;
    Polynomial& operator=(Polynomial&& other) noexcept;
    
    void add_term(int coefficient, int exponent);
    int count() const;
    Polynomial operator+(const Polynomial& other) const;
    
    friend std::ostream &operator<<(std::ostream &os, const Polynomial &p);
};

void Polynomial::add_term(int coefficient, int exponent) {
    if (head == nullptr) {
        Term* new_term = new Term(coefficient, exponent);
        head = new_term;
    } else {
        Term* current = head;
        bool sameExponent = false;
        while (current->next != nullptr) {
            if (current->exponent == exponent) {
                current->coefficient += coefficient;
                sameExponent = true;
                break;
            }
            current = current->next;
        }
        
        if (!sameExponent) {
            Term* new_term = new Term(coefficient, exponent);
            current->next = new_term;
        }
    }
    ++size;
}

Polynomial Polynomial::operator+(const Polynomial& other) const {
    Polynomial new_poly(*this);
    
    Term* current = other.head;
    while (current != nullptr) {
        new_poly.add_term(current->coefficient, current->exponent);
        current = current->next;
    }
    return new_poly;
}

Polynomial::~Polynomial() {
    while (head != nullptr) {
        Term* current = head;
        head = head->next;
        delete current;
    }
}

Polynomial::Polynomial(const Polynomial &other) {
    Term* current = other.head;
    while (current != nullptr) {
        add_term(current->coefficient, current->exponent);
        current = current->next;
    }
}

Polynomial &Polynomial::operator=(const Polynomial &other) {
    if (this != &other) {
        while (head != nullptr) {
            Term* current = head;
            head = head->next;
            delete current;
        }
        size = 0;
        
        Term* current = other.head;
        while (current != nullptr) {
            add_term(current->coefficient, current->exponent);
            current = current->next;
        }
    }
    return *this;
}

Polynomial::Polynomial(Polynomial &&other) noexcept {
    head = other.head;
    size = other.size;

    other.head = nullptr;
    other.size = 0;
}

Polynomial &Polynomial::operator=(Polynomial &&other) noexcept {
    if (this != &other) {
        while (head != nullptr) {
            Term* current = head;
            head = head->next;
            delete current;
        }

        head = other.head;
        size = other.size;

        other.head = nullptr;
        other.size = 0;
    }
    return *this;
}

int Polynomial::count() const {
    return size;
}
```