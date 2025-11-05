# Week 4 Practice Questions

## 1. Binary search on linked lists
```cpp
template <typename Data>
int binary_search(const LinkedList<Data>& list, const Data& key) {
    int low = 0;
    int high = list.count() - 1;
    
    while (low <= high) {
        int mid = low + (high - low) / 2;
        
        Node<Data>* current = list.head;
        for (int i = 0; i < mid; ++i) {
            current = current->next;
        }
        
        if (key == current->data) {
            return mid;
        } else if (key < current->data) {
            high = mid - 1;
        } else {
            low = mid + 1;
        }
    }
    
    return -1;
}
```

## 2. Bubble sort on linked lists
```cpp
template <typename Data>
void bubble_sort(LinkedList<Data>& list) {
    for (int i = 0; i < list.count(); ++i) {
        bool swapped = false;
        
        Node<Data>* n1 = list.head;
        Node<Data>* n2 = n1->next;
        
        for (int j = 1; j < list.count() - i; ++j) {
            if (n2->data < n1->data) {
                Data temp = n1->data;
                n1->data = n2->data;
                n2->data = temp;
                swapped = true;
            }
            n1 = n2;
            n2 = n2->next;
        }
        
        if (!swapped) {
            break;
        }
    }
}
```

## 3. Semantic versioning
```cpp
struct Version {
    int major;
    int minor;
    int patch;
    
    Version(int _ma, int _mi, int _pa): major(_ma), minor(_mi), patch(_pa) {}

    friend bool operator<(const Version &v1, const Version &v2) {
        if (v1.major < v2.major) {
            return true;
        } else if (v1.major > v2.major) {
            return false;
        } else { // equal majors
            if (v1.minor < v2.minor) {
                return true;
            } else if (v1.minor > v2.minor) {
                return false;
            } else { // equal minors
                if (v1.patch < v2.patch) {
                    return true;
                } else {
                    return false;
                }
            }
        }
    }
    
    friend bool operator>(const Version &v1, const Version &v2) {
        return operator<(v2, v1);
    }
    
    friend bool operator==(const Version &v1, const Version &v2) {
        return v1.major == v2.major && v1.minor == v2.minor && v1.patch == v2.patch;
    }
    
    friend bool operator!=(const Version &v1, const Version &v2) {
        return !operator==(v1, v2);
    }
    
    friend bool operator<=(const Version &v1, const Version &v2) {
        return !operator>(v1, v2);
    }
    
    friend bool operator>=(const Version &v1, const Version &v2) {
        return !operator<(v1, v2);
    }
};
```