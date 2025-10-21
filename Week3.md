# Week 3 Practice Questions

## 1. Interleave queue
```cpp
template <typename Data>
void interleave_halves(Queue<Data>& queue) {
    Node<Data>* start = queue.head;
    Node<Data>* mid = queue.head;
    int half = queue.size() / 2;

    for (int i = 0; i < half; ++i) {
        mid = mid->next;
    }
    
    for (int i = 0; i < half - 1; ++i) {
        Node<Data>* old_start_next = start->next;
        Node<Data>* old_mid_next = mid->next;
        start->next = mid;
        start = old_start_next;
        mid->next = start;
        mid = old_mid_next;
    }
    
    if (start) {
        start->next = mid;
    }
}
```

## 2. Priority queue
```cpp
template <typename Data>
class PriorityQueue {
private:
    Node<Data>* head{};
    Node<Data>* tail{};
    int length{0};
    bool is_max_queue{};
public:
    // constructors
    PriorityQueue() = default;
    explicit PriorityQueue(bool is_max);
    // copy semantics
    PriorityQueue(const PriorityQueue& src);
    PriorityQueue& operator=(const PriorityQueue& src);
    // move semantics
    PriorityQueue(PriorityQueue&& src) noexcept;
    PriorityQueue& operator=(PriorityQueue&& src) noexcept;
    // destructor
    ~PriorityQueue();

    void enqueue(const Data& data);
    Data dequeue();
    bool is_empty() const;
    int size() const;
    const Data& peek() const;
};

template <typename Data>
PriorityQueue<Data>::PriorityQueue(bool is_max): is_max_queue(is_max) {}

template<typename Data>
void PriorityQueue<Data>::enqueue(const Data& data) {
    Node<Data> *new_node = new Node<Data>();
    new_node->data = data;

    if (head == nullptr) {
        head = tail = new_node;
    } else {
        if ((is_max_queue && data > head->data) || (!is_max_queue && data < head->data)) {
            new_node->next = head;
            head = new_node;
        } else {
            tail->next = new_node;
            tail = new_node;   
        }
    }
    ++length;
}

template<typename Data>
Data PriorityQueue<Data>::dequeue() {
    if (head == nullptr) {
        throw std::out_of_range("Priority queue is empty");
    }

    Node<Data> *temp = head;
    Data data = temp->data;
    head = head->next;

    if (head == nullptr) {
        tail = nullptr;
    }

    delete temp;
    --length;
    
    // Rearrange head
    Node<Data>* max = head;
    Node<Data>* prev = nullptr;
    Node<Data>* prev_max = nullptr;
    Node<Data>* current = head ? head->next : nullptr;
    
    while (current != nullptr) {
        if ((is_max_queue && current->data > max->data) || (!is_max_queue && current->data < max->data)) {
            max = current;
            if (prev == nullptr) {
                prev = head;
            }
            prev_max = prev;
        }
        prev = current;
        current = current->next;
    }
    
    if (prev_max) {
        prev_max->next = max->next;
        max->next = head;
        head = max;
    }
    
    if (max == tail) {
        tail = prev_max;
    }
    
    return data;
}

template<typename Data>
bool PriorityQueue<Data>::is_empty() const {
    return head == nullptr;
};

template<typename Data>
int PriorityQueue<Data>::size() const {
    return length;
};

template<typename Data>
const Data& PriorityQueue<Data>::peek() const {
    if (head == nullptr) {
        throw std::out_of_range("Priority queue is empty");
    }
    
    return head->data;
};

template<typename Data>
PriorityQueue<Data>::~PriorityQueue() {
    while (head != nullptr) {
        const Node<Data> *temp = head;
        head = head->next;
        delete temp;
    }
    tail = nullptr;
}

template<typename Data>
PriorityQueue<Data>::PriorityQueue(const PriorityQueue &src) {
    Node<Data> *current = src.head;
    is_max_queue = src.is_max_queue;

    while (current != nullptr) {
        enqueue(current->data);
        current = current->next;
    }
}

template<typename Data>
PriorityQueue<Data>& PriorityQueue<Data>::operator=(const PriorityQueue &src) {
    if (this != &src) {
        is_max_queue = src.is_max_queue;

        while (head != nullptr) {
            dequeue();
        }

        Node<Data> *current = src.head;

        while (current != nullptr) {
            enqueue(current->data);
            current = current->next;
        }
    }
    return *this;
}

template<typename Data>
PriorityQueue<Data>::PriorityQueue(PriorityQueue &&src) noexcept {
    head = src.head;
    tail = src.tail;
    length = src.length;
    is_max_queue = src.is_max_queue;

    src.head = nullptr;
    src.tail = nullptr;
    src.length = 0;
    src.is_max_queue = false;
}

template<typename Data>
PriorityQueue<Data>& PriorityQueue<Data>::operator=(PriorityQueue &&src) noexcept {
    if (this != &src) {
        while (head != nullptr) {
            dequeue();
        }

        head = src.head;
        tail = src.tail;
        length = src.length;
        is_max_queue = src.is_max_queue;

        src.head = nullptr;
        src.tail = nullptr;
        src.length = 0;
        src.is_max_queue = false;
    }
    return *this;
}
```

## 3. String reversal
```cpp
std::string reverse_string(std::string input) {
    Stack<char> stack;
    std::string reversed{};
    
    for (const char c: input) {
        stack.push(c);
    }
    
    while (!stack.is_empty()) {
        reversed += stack.pop();
    }
    
    return reversed;
}
```

## 4. Two-stack algorithm
```cpp
double two_stack_algorithm(std::string expression) {
    Stack<double> values;
    Stack<char> operators;
    
    std::string token;
    std::stringstream ss(expression);
    
    while (getline(ss, token, ' ')) {
        if (token == "(") {
            continue;    
        } else if (token == ")") {
            double num = values.pop();
            char op = operators.pop();

            switch (op) {
                case '+':
                    values.push(num + values.pop());
                    break;
                case '-':
                    values.push(values.pop() - num);
                    break;
                case '*':
                    values.push(num * values.pop());
                    break;
                case '/':
                    values.push(values.pop() / num);
                    break;
                case '^':
                    values.push(std::pow(values.pop(), num));
                    break;
                case '%':
                    values.push(std::fmod(values.pop(), num));
                    break;
                default: ;
            }
        } else if (token == "+" || token == "-" || token == "/" || token == "*"
            || token == "^" || token == "%") {
                operators.push(token[0]);
        } else {
            values.push(stod(token));
        }
    }
    return values.pop();
}
```