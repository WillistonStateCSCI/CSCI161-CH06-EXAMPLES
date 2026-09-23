# CSCI161-CH06-EXAMPLES

Examples from Data Structures and Algorithms in Java (Goodrich, Tamassia, Goldwasser, 6th ed.), Chapter 6: Stacks, Queues, and Deques.

## Stacks (Section 6.1)

A stack is last-in, first-out (LIFO): the last item pushed is the first item popped.

### EX6_01 – The Stack Interface

Code Fragment 6.1, Section 6.1.1. File: `Stack.java`

This interface lists the five stack operations: `size`, `isEmpty`, `push`, `top`, and `pop`. It contains no working code. It only says what a stack must be able to do. Notice that `top` and `pop` return `null` on an empty stack instead of throwing an exception. This is not the same as Java's built-in `java.util.Stack`.

### EX6_02_03 – Array-Based Stack

Code Fragments 6.2 and 6.3, Section 6.1.2. File: `ArrayStack.java`

`ArrayStack` stores the stack in a fixed-size array (1000 by default). The variable `t` holds the index of the top element and starts at `-1` when the stack is empty. Every operation runs in constant time. `push` throws an `IllegalStateException` if the array is full.

The `main` method is Code Fragment 6.3. It runs a series of pushes and pops, and the comment on each line shows what the stack holds at that point. When you run it, it prints:

```
2
3
false
5
true
null
9
3
4
8
```

Try this: cover the output and predict each printed value from the comments before you run it.

### EX6_04 – Linked-List Stack

Code Fragment 6.4, Section 6.1.3. File: `LinkedStack.java`
Uses: `OldFiles/SinglyLinkedList.java`

`LinkedStack` builds a stack out of the `SinglyLinkedList` from Chapter 3. This is an example of the adapter pattern. Every stack method is one line that calls a list method: `push` calls `addFirst`, `top` calls `first`, and `pop` calls `removeFirst`. The top of the stack is the head of the list, so every operation runs in constant time, and the stack never fills up. This file has no `main` method. It is used by the matching examples in `EX6_07`.

Try this: explain why the top of the stack goes at the head of the list and not at the tail.

### EX6_06 – Reversing an Array with a Stack

Code Fragments 6.5 and 6.6, Section 6.1.4. File: `ReverseWithStack.java`

The generic method `reverse` (Fragment 6.5) pushes every element of an array onto an `ArrayStack`, then pops them back into the array. Because a stack is LIFO, the elements come back in reverse order. The `main` method (Fragment 6.6) tests it on an `Integer` array and a `String` array:

```
a = [4, 8, 15, 16, 23, 42]
s = [Jack, Kate, Hurley, Jin, Michael]
Reversing...
a = [42, 23, 16, 15, 8, 4]
s = [Michael, Jin, Hurley, Kate, Jack]
```

Try this: the method works for both arrays even though it is written only once. Find the part of the method header that makes this possible.

### EX6_07 – Matching Delimiters and HTML Tags

Code Fragments 6.7 and 6.8, Section 6.1.5. Files: `MatchDelimiters.java`, `MatchHTML.java`

Both programs use a `LinkedStack` to check that every opening symbol has a matching closing symbol in the correct order.

- `MatchDelimiters` (Fragment 6.7) reads an expression one character at a time. It pushes `(`, `{`, and `[`. When it sees a closing symbol, it pops and checks that the pair matches. The expression is valid only if the stack is empty at the end. `main` tests five valid and three invalid strings and prints something only if a string is judged wrong. A correct run prints nothing.
- `MatchHTML` (Fragment 6.8) does the same thing with HTML tags such as `<body>` and `</body>`. It pushes each opening tag's name and pops when it finds a closing tag.
  - Run it with any command-line argument (for example `java -cp out MatchHTML book`) to test the book's "Little Boat" example. A correct run prints nothing.
  - Run it with no argument and it reads HTML from standard input. For example, `echo "<b><i>hi</i></b>" | java -cp out MatchHTML` prints `The input file is a matched HTML document.`, and swapping the closing tags to `</b></i>` prints `The input file is not a matched HTML document.`

Try this: trace the stack by hand for `({[])}` and find the exact step where the match fails.

## Queues (Section 6.2)

A queue is first-in, first-out (FIFO): items leave in the same order they arrived, like a line at a store.

### EX6_09 – The Queue Interface

Code Fragment 6.9, Section 6.2.1. File: `Queue.java`

This interface lists the queue operations: `size`, `isEmpty`, `enqueue` (add at the back), `first` (look at the front), and `dequeue` (remove from the front). As with `Stack`, empty-queue calls return `null`. This is a simpler version of Java's `java.util.Queue`.

### EX6_10 – Array-Based Queue

Code Fragment 6.10, Section 6.2.2. File: `ArrayQueue.java`

`ArrayQueue` stores the queue in a fixed-size array and treats the array as a circle. It keeps `f`, the index of the front element, and `sz`, the number of elements. The next open spot is found with `(f + sz) % data.length`. After a `dequeue`, `f` moves forward with `(f + 1) % data.length`. This modular arithmetic lets the queue wrap around to the start of the array, so no elements ever need to be shifted. All operations are constant time. This file has no `main` method.

Try this: with a capacity of 5, draw the array after enqueue A, B, C, D, then dequeue twice, then enqueue E and F. Where do E and F end up?

### EX6_11 – Linked-List Queue

Code Fragment 6.11, Section 6.2.3. File: `LinkedQueue.java`
Uses: `OldFiles/SinglyLinkedList.java`

This is another adapter built on `SinglyLinkedList`. `enqueue` calls `addLast` and `dequeue` calls `removeFirst`, so the front of the queue is the head of the list and the back is the tail. The list keeps a tail reference, so both ends run in constant time. This file has no `main` method.

Try this: compare with `LinkedStack` in `EX6_04`. Which one method call is different, and why does that one change turn LIFO into FIFO?

### EX6_12 – The CircularQueue Interface

Code Fragment 6.12, Section 6.2.4. File: `CircularQueue.java`

`CircularQueue` extends `Queue` and adds a single method, `rotate()`, which moves the front element to the back. It does the same thing as `enqueue(dequeue())` but can be done more efficiently. This is useful when you need to cycle through items over and over, like players taking turns.

### EX6_13 – The Josephus Problem

Code Fragment 6.13, Section 6.2.4. Files: `Josephus.java`, `LinkedCircularQueue.java`
Uses: `OldFiles/CircularlyLinkedList.java`

`LinkedCircularQueue` implements `CircularQueue` using the `CircularlyLinkedList` from Chapter 3. Its `rotate` simply moves the list's tail forward one node, so no nodes are created or removed.

`Josephus` (Fragment 6.13) uses it to solve the classic Josephus problem, also known as "hot potato." People sit in a circle, the potato is passed `k - 1` times, and whoever is holding it on the kth count is out. The last person left wins. The program plays three games:

```
    Cindy is out
    Fred is out
    Doug is out
    Bob is out
    Ed is out
First winner is Alice
    Jack is out
    Irene is out
    Lance is out
    Gene is out
    Kim is out
Second winner is Hope
    Mike is out
Third winner is Roberto
```

Try this: work out the first game by hand (6 players, `k = 3`) and check that Cindy is out first and Alice wins.

## Double-Ended Queues (Section 6.3)

A deque (pronounced "deck") lets you add and remove items at both the front and the back.

### EX6_14 – The Deque Interface and Linked Deque

Code Fragment 6.14, Sections 6.3.1–6.3.2. Files: `Deque.java`, `LinkedDeque.java`
Uses: `OldFiles/DoublyLinkedList.java`

`Deque.java` (Fragment 6.14) defines `first`, `last`, `addFirst`, `addLast`, `removeFirst`, and `removeLast`, plus `size` and `isEmpty`. It is a simpler version of Java's `java.util.Deque`.

`LinkedDeque` implements it as an adapter on the `DoublyLinkedList` from Chapter 3. Each node links to both its neighbors, and the list uses header and trailer sentinels, so removing from either end is constant time. A singly linked list cannot remove from the tail in constant time. This file has no `main` method.

Try this: show how a deque can act as a stack, and then as a queue, using only its methods.