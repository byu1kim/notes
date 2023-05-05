+++
title = "Theory"
weight = 1
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

## Brute force

A method of accomplishing something primarily by means of strength, without the use of great skill

---

## Big O

Big O, also known as Big O notation, represents an algorithm's worst-case complexity. It uses algebraic terms to describe the complexity of an algorithm.

Big O defines the runtime required to execute an algorithm by identifying how the performance of your algorithm will change as the input size grows. But it does not tell you how fast your algorithm's runtime is.

Big O notation measures the efficiency and performance of your algorithm using time and space complexity.

`Time & Space complexity relationsthip with input size`

> ### Time Complexity

An algorithm's time complexity specifies `how long it will take` to execute an algorithm as a function of its input size.

{{< hbox orange >}}
<b>As a function of its input size</b><br>
How many statement executed based on the input size

{{< /hbox >}}

#### Types of complexities

- Constant: O(1), Excellent/Best
- Logarithmic time: O(log n), Good
- Linear time: O(n), Fair
- Logarithmic time: O(n log n), Bad
- Quadratic time: O(n^2), Horrible/Worst
- Exponential time: O(2^n), Horrible/Worst
- Factorial time: O(n!), Horrible/Worsk

#### Constant Time : O(1)

When your algorithm is not dependent on the input size n, it is said to have a constant time complexity with order O(1). This means that the run time will always be the same regardless of the input size.

```
// no matter how big the size of input is, there's only one execution.

const firstElement = (array) => {
  return array[0];
};
```

#### Linear Time: O(n)

You get linear time complexity when the running time of an algorithm increases linearly with the size of the input. This means that when a function has an iteration that iterates over an input size of n, it is said to have a time complexity of order O(n).

```
# Runtime depends on the input size.
const calcFactorial = (n) => {
  let factorial = 1;
  for (let i = 2; i <= n; i++) {
    factorial = factorial * i;
  }
  return factorial;
};
```

#### Logarithm Time: O(log n)

This is similar to linear time complexity, except that the runtime does not depend on the input size but rather on `half the input size`. When the input size decreases on each iteration or step, an algorithm is said to have logarithmic time complexity.

```
const binarySearch = (array, target) => {
  let firstIndex = 0;
  let lastIndex = array.length - 1;
  while (firstIndex <= lastIndex) {
    let middleIndex = Math.floor((firstIndex + lastIndex) / 2);

    if (array[middleIndex] === target) {
      return middleIndex;
    }

    if (array[middleIndex] > target) {
      lastIndex = middleIndex - 1;
    } else {
      firstIndex = middleIndex + 1;
    }
  }
  return -1;
};
```

#### Quadratic Time: O(n^2)

When you perform nested iteration, meaning having a loop in a loop, the time complexity is quadratic, which is horrible.

```
const matchElements = (array) => {
  for (let i = 0; i < array.length; i++) {
    for (let j = 0; j < array.length; j++) {
      if (i !== j && array[i] === array[j]) {
        return `Match found at ${i} and ${j}`;
      }
    }
  }
  return "No matches found 😞";
};
```

#### Exponential Time: O(2^n)

```
const recursiveFibonacci = (n) => {
  if (n < 2) {
    return n;
  }
  return recursiveFibonacci(n - 1) + recursiveFibonacci(n - 2);
};
```

> ### Space Complexity

An algorithm's space complexity specifies `the total amount of space or memory required` to execute an algorithm as a function of the size of the input. (If space depend on the size of the input array)

But often, people confuse Space-complexity with Auxiliary space. Auxiliary space is just a temporary or extra space and it is not the same as space-complexity. In simpler terms,

Space Complexity = Auxiliary space + Space use by input values

[Reference](https://www.freecodecamp.org/news/big-o-cheat-sheet-time-complexity-chart/#:~:text=An%20algorithm's%20time%20complexity%20specifies,the%20size%20of%20the%20input.)

#### Constant Space: O(1)

Constant Space Complexity occurs when the program doesn’t contain any loops, recursive functions or call to any other functions.

```java
int main()
{
  int a = 5, b = 5, c;
  c = a + b;
  printf("%d", c);
}
```

#### Linear Space : O(n)

Linear space complexity occurs when the program contains any loops.

```java
int main()
{
  int n, i, sum = 0;
  scanf("%d", &n);
  int arr[n];
  for(i = 0; i < n; i++)
  {
    scanf("%d", &arr[i]);
    sum = sum + arr[i];
  }
  printf("%d", sum);
}
```

---

## Hash Table

A hash table is a data structure that is used to store keys/value pairs. It uses a hash function to compute an index into an array in which an element will be inserted or searched. By using a good hash function, hashing can work well.

{{< hbox green >}}
<b>Hasing</b><br>
Hashing is a technique that is used to uniquely identify a specific object from a group of similar objects.

{{< /hbox >}}

---
