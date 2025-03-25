### Задача 1: Контейнер с первыми n факториалами

```cpp
#include <iostream>
#include <vector>
#include <stdexcept>

std::vector<unsigned long long> generateFactorials(int n) {
    if (n <= 0) {
        throw std::invalid_argument("n должно быть натуральным числом больше 0.");
    }

    std::vector<unsigned long long> factorials;
    factorials.push_back(1); // 0! = 1

    for (int i = 1; i < n; ++i) {
        factorials.push_back(factorials.back() * i);
    }

    return factorials;
}

int main() {
    try {
        int n;
        std::cout << "Введите n: ";
        std::cin >> n;

        auto factorials = generateFactorials(n);
        for (const auto& f : factorials) {
            std::cout << f << " ";
        }
        std::cout << std::endl;
    }
    catch (const std::exception& e) {
        std::cerr << "Ошибка: " << e.what() << std::endl;
    }

    return 0;
}
```
---

### Задача 2: Удаление дубликатов из контейнера

```cpp
#include <iostream>
#include <vector>
#include <unordered_set>

std::vector<int> removeDuplicates(const std::vector<int>& input) {
    std::unordered_set<int> seen;
    std::vector<int> result;

    for (const auto& num : input) {
        if (seen.find(num) == seen.end()) {
            seen.insert(num);
            result.push_back(num);
        }
    }

    return result;
}

int main() {
    std::vector<int> numbers = {1, 2, 2, 3, 4, 4, 5};

    auto uniqueNumbers = removeDuplicates(numbers);

    std::cout << "Уникальные числа: ";
    for (const auto& num : uniqueNumbers) {
        std::cout << num << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

---

### Задача 3: Разворот связного списка с использованием рекурсии

```cpp
#include <iostream>

struct Node {
    int data;
    Node* next;
    Node(int val) : data(val), next(nullptr) {}
};

Node* reverseList(Node* head) {
    if (head == nullptr || head->next == nullptr) {
        return head;
    }

    Node* rest = reverseList(head->next);
    head->next->next = head;
    head->next = nullptr;
    return rest;
}

void printList(Node* head) {
    Node* current = head;
    while (current != nullptr) {
        std::cout << current->data << " ";
        current = current->next;
    }
    std::cout << std::endl;
}

int main() {
    Node* head = new Node(1);
    head->next = new Node(2);
    head->next->next = new Node(3);
    head->next->next->next = new Node(4);

    std::cout << "Исходный список: ";
    printList(head);

    head = reverseList(head);

    std::cout << "Развернутый список: ";
    printList(head);

    return 0;
}
```
---

Ниже приведены примеры юнит-тестов для каждой из задач, использующих библиотеку Google Test (gtest). Эти тесты покрывают основные сценарии использования каждой функции и обрабатывают исключительные ситуации.

### Задача 1: Юнит-тесты для функции `generateFactorials`

```cpp
#include <gtest/gtest.h>
#include <vector>
#include <stdexcept>
#include "factorial.h"
TEST(FactorialTest, Correctness) {
    std::vector<unsigned long long> result = generateFactorials(5);
    
    EXPECT_EQ(result.size(), 5);
    EXPECT_EQ(result[0], 1);
    EXPECT_EQ(result[1], 1);
    EXPECT_EQ(result[2], 2);
    EXPECT_EQ(result[3], 6);
    EXPECT_EQ(result[4], 24);
}


TEST(FactorialTest, Exception) {
    EXPECT_THROW(generateFactorials(0), std::invalid_argument); // n <= 0
    EXPECT_THROW(generateFactorials(-5), std::invalid_argument); // n < 0
}
```

### Задача 2: Юнит-тесты для функции `removeDuplicates`

```cpp
#include <gtest/gtest.h>
#include <vector>
#include "remove_duplicates.h"


TEST(RemoveDuplicatesTest, Correctness) {
    std::vector<int> input = {1, 2, 2, 3, 4, 4, 5};
    std::vector<int> expected = {1, 2, 3, 4, 5};
    
    auto result = removeDuplicates(input);
    
    EXPECT_EQ(result, expected);
}

TEST(RemoveDuplicatesTest, EmptyContainer) {
    std::vector<int> input = {};
    std::vector<int> expected = {};
    
    auto result = removeDuplicates(input);
    
    EXPECT_EQ(result, expected);
}


TEST(RemoveDuplicatesTest, AllDuplicates) {
    std::vector<int> input = {2, 2, 2, 2};
    std::vector<int> expected = {2};
    
    auto result = removeDuplicates(input);
    
    EXPECT_EQ(result, expected);
}
```

### Задача 3: Юнит-тесты для функции `reverseList`

```cpp
#include <gtest/gtest.h>
#include "reverse_list.h"
TEST(ReverseListTest, Correctness) {
    Node* head = new Node(1);
    head->next = new Node(2);
    head->next->next = new Node(3);
    head->next->next->next = new Node(4);

    Node* reversedHead = reverseList(head);
    
    EXPECT_EQ(reversedHead->data, 4);
    EXPECT_EQ(reversedHead->next->data, 3);
    EXPECT_EQ(reversedHead->next->next->data, 2);
    EXPECT_EQ(reversedHead->next->next->next->data, 1);
}

TEST(ReverseListTest, EmptyList) {
    Node* head = nullptr;
    
    Node* reversedHead = reverseList(head);
    
    EXPECT_EQ(reversedHead, nullptr);
}

TEST(ReverseListTest, SingleElementList) {
    Node* head = new Node(5);
    
    Node* reversedHead = reverseList(head);
    
    EXPECT_EQ(reversedHead->data, 5);
    EXPECT_EQ(reversedHead->next, nullptr);
}
```

