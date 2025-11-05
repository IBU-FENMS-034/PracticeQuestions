# Week 5 Practice Questions

## 1. Reverse selection sort on linked lists
```cpp
template <typename Data>
void selection_sort(LinkedList<Data>& list) {
    for (int i = 0; i < list.count(); ++i)  {

        Node<Data>* m = list.head;
        for (int j = 0; j < i; ++j) {
            m = m->next;
        }

        Node<Data>* old_m = m;
        Node<Data>* n = old_m->next;

        for (int j = i + 1; j < list.count(); ++j) {
            if (n->data > m->data) {
                m = n;
            }
            n = n->next;
        }

        Data temp = old_m->data;
        old_m->data = m->data;
        m->data = temp;
    }
}
```

## 2. Count comparisons and swaps in insertion sort
```cpp
template <typename Data>
int* insertion_sort_count(Data *arr, const int len) {
    int* counts = new int[2]{};

    for (int i = 1; i < len; i++) {
        for (int j = i; j > 0; j--) {
            counts[0]++;
            if (arr[j] < arr[j - 1]) {
                std::swap(arr[j], arr[j - 1]);
                counts[1]++;
            } else {
                break;
            }
        }
    }
    
    return counts;
}
```

## 3. Prime number Shell sequence
```cpp
int* generate_sequence(const int n) {
    int* sequence = new int[n]{};
    
    sequence[0] = 1; // first element
    
    for (int i = 1; i < n; i++) {
        int next = sequence[i - 1] * 3;
        
        while (true) {
            bool is_prime = true;
            // check if prime
            for (int j = 2; j < next / 2; j++) {
                if (next % j == 0) {
                    is_prime = false;
                    break;
                }
            }
            
            if (!is_prime) {
                next++;
            } else {
                break;
            }
        }
        
        sequence[i] = next;
    }
    
    return sequence;
}
```