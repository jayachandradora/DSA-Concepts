# Reversal, Cycle Detection

### Linked Lists: Reversal and Cycle Detection

Linked lists are a fundamental data structure that consists of nodes, where each node contains data and a reference (or link) to the next node in the sequence. Let's dive into two core problems related to linked lists:

1. **Reversing a Linked List**
2. **Detecting a Cycle in a Linked List**

I'll provide a brief overview, use cases, and Java solutions for each problem.

---

### 1. **Reversing a Linked List**

#### **Problem Statement:**
Given the head of a singly linked list, reverse the linked list and return its head.

#### **Use Cases:**
- **Reversing a data stream**: Sometimes, in algorithms, reversing the order of elements is necessary.
- **Reversing history**: In undo operations, where a series of actions needs to be reversed (like browser history).
- **Iterative processing**: When data needs to be processed from the end back to the beginning.

#### **Java Solution (Iterative):**

```java
class ListNode {
    int val;
    ListNode next;
    ListNode(int x) { val = x; }
}

public class LinkedListReversal {
    public static ListNode reverseList(ListNode head) {
        ListNode prev = null;
        ListNode current = head;
        
        while (current != null) {
            ListNode nextTemp = current.next;  // Store next node
            current.next = prev;               // Reverse the current node's pointer
            prev = current;                    // Move prev and current one step forward
            current = nextTemp;
        }
        return prev;  // prev will be the new head of the reversed list
    }

    // Helper method to print the list
    public static void printList(ListNode head) {
        ListNode temp = head;
        while (temp != null) {
            System.out.print(temp.val + " ");
            temp = temp.next;
        }
        System.out.println();
    }

    public static void main(String[] args) {
        // Creating a linked list: 1 -> 2 -> 3 -> 4 -> 5
        ListNode head = new ListNode(1);
        head.next = new ListNode(2);
        head.next.next = new ListNode(3);
        head.next.next.next = new ListNode(4);
        head.next.next.next.next = new ListNode(5);

        System.out.print("Original List: ");
        printList(head);

        head = reverseList(head);

        System.out.print("Reversed List: ");
        printList(head);
    }
}
```

#### **Explanation:**
- The algorithm iterates through the linked list, adjusting the `next` pointers to reverse the direction.
- The time complexity is **O(n)**, where **n** is the number of nodes in the list, as we only need to traverse the list once.
- The space complexity is **O(1)**, as we are using only a few extra pointers.

---

### 2. **Cycle Detection in a Linked List**

#### **Problem Statement:**
Given the head of a linked list, determine if the list contains a cycle. A cycle occurs when a node’s `next` pointer points back to one of the previous nodes, creating an infinite loop.

#### **Use Cases:**
- **Memory management**: Detecting cycles is critical to prevent memory leaks in systems that have circular references (e.g., in garbage collection).
- **Detecting infinite loops**: In scenarios where linked structures may loop back to earlier nodes, it’s essential to detect these cycles to avoid infinite processing.
- **Networking protocols**: Cycle detection can help detect network loops or circular dependencies in graph-like structures.

#### **Java Solution (Floyd’s Tortoise and Hare Algorithm):**

```java
class ListNode {
    int val;
    ListNode next;
    ListNode(int x) { val = x; }
}

public class LinkedListCycleDetection {
    public static boolean hasCycle(ListNode head) {
        if (head == null || head.next == null) {
            return false;  // No cycle if list is empty or has only one node
        }

        ListNode slow = head;  // Slow pointer (tortoise)
        ListNode fast = head;  // Fast pointer (hare)

        while (fast != null && fast.next != null) {
            slow = slow.next;        // Move slow pointer by 1 step
            fast = fast.next.next;   // Move fast pointer by 2 steps

            if (slow == fast) {      // If they meet, there's a cycle
                return true;
            }
        }
        return false;  // No cycle found
    }

    public static void main(String[] args) {
        // Creating a linked list: 1 -> 2 -> 3 -> 4 -> 5
        ListNode head = new ListNode(1);
        head.next = new ListNode(2);
        head.next.next = new ListNode(3);
        head.next.next.next = new ListNode(4);
        head.next.next.next.next = new ListNode(5);

        // Introducing a cycle: 5 -> 2 (creates a loop)
        head.next.next.next.next.next = head.next; 

        if (hasCycle(head)) {
            System.out.println("The list has a cycle.");
        } else {
            System.out.println("The list has no cycle.");
        }
    }
}
```

#### **Explanation:**
- Floyd's Cycle-Finding Algorithm uses two pointers:
  - **Slow pointer** moves one step at a time.
  - **Fast pointer** moves two steps at a time.
- If the linked list contains a cycle, the fast pointer will eventually "catch up" with the slow pointer, indicating a cycle.
- The time complexity is **O(n)**, where **n** is the number of nodes in the list, because both pointers traverse the list at most once.
- The space complexity is **O(1)**, as we are using only a fixed amount of extra space for the two pointers.

---

### Summary of Solutions

#### **Reversing a Linked List:**
- **Core idea**: Reverse the direction of the links in the list.
- **Use cases**: Undo operations, data processing in reverse order.
- **Complexity**: Time complexity is **O(n)**, space complexity is **O(1)**.

#### **Cycle Detection in a Linked List:**
- **Core idea**: Use two pointers (slow and fast) to detect if the list has a cycle.
- **Use cases**: Detect infinite loops, network protocol cycles, circular dependencies.
- **Complexity**: Time complexity is **O(n)**, space complexity is **O(1)**.

These problems and solutions are widely applicable in various domains, particularly in data structure manipulation and algorithm design.
