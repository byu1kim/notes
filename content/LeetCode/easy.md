+++
title = "Easy"
weight = 2
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

## Two Sum

#### Solution

{{< tabs >}}
{{% tab name="Python" %}}
Solution #1 : Brute Force

```python
# Runtime: 9ms
class Solution(object):
    def twoSum(self, nums, target):
        for i in range(len(nums)):
            for j in range(i+1, len(nums)):
                if nums[j] == target - nums[i]:
                    return [i, j]
```

Solution #2 : Two-pass Hash Table

```python
# Runtime: 20ms
class Solution(object):
    def twoSum(self, nums, target):
        map = {}
        for (i, num) in enumerate(nums):
            map[num]=i
        for (i, num) in enumerate(nums):
            x = target - num
            if(x in map and map[x]!=i):
                return [i, map[x]]
```

Solution #3 : One-pass Hash Table

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        hashmap = {}
        for i in range(len(nums)):
            complement = target - nums[i]
            if complement in hashmap:
                return [i, hashmap[complement]]
            hashmap[nums[i]] = i
```

{{% /tab %}}
{{% tab name="C#" %}}

```c#
 static int[] twoSum(List<int> nums, int target)
{
    Dictionary<int, int> map = new Dictionary<int, int>();

    int x;
    for(int i =0; i < nums.Count; i++)
    {
        x = target - nums[i];
        if (map.ContainsKey(x))
        {
            Console.WriteLine(x);
            return new int[] { i, map[x] };
        }
        map.Add(nums[i], i);
    }
    return new int[] { 0,0 };
}

```

{{% /tab %}}
{{< /tabs >}}

#### Key Concepts

- Consider Big O (Time complexity, Space complexity)
- Using hash tables

## Roman to Integer

#### Solutions

{{< tabs >}}
{{% tab name="Python" %}}
Solution #1

```python
def romanToInt(s):
    s = s.replace('IV', 'IIII').replace('IX', 'VIIII').replace('XL', 'XXXX').replace('XC', 'LXXXX').replace('CD', 'CCCC').replace('CM', 'DCCCC')
    roman_to_integer = {
        'I': 1,
        'V': 5,
        'X': 10,
        'L': 50,
        'C': 100,
        'D': 500,
        'M': 1000,
    }
    to_int = sum(map(lambda x: roman_to_integer[x], s))
    print("aa", to_int)
    return to_int
```

Solution #2

```python
def romanToInt(s):
    # 12ms
    roman = {
        'I': 1,
        'V': 5,
        'X': 10,
        'L': 50,
        'C': 100,
        'D': 500,
        'M': 1000,
    }
    count = 0
    for i in range(len(s) -1 ):
        if roman[s[i]] < roman[s[(i+1)] ]:
            count-= roman[s[i]]
        else:
            count+= roman[s[i]]
    return count + roman[s[-1]]
```

{{% /tab %}}
{{% tab name="C#" %}}

```c#
int RomanToInt(string s)
{
    var map = new Dictionary<Char, int> {
        { 'I' , 1 }, { 'V' , 5 }, { 'X' , 10 },
        { 'L' , 50 }, { 'C' , 100 }, { 'D' , 500 }, { 'M' , 1000 }
    };

    int sum = 0;
    for (int i = 0; i < s.Length -1; i++)
    {
        if (map[s[i]] < map[s[i]])
        {
            sum -= map[s[i + 1]];
        } else
        {
            sum += map[s[i]];
        }
    }
    return sum + map[s.LastOrDefault()];
}
```

{{% /tab %}}
{{< /tabs >}}

#### Key concepts

- Hashtable

---

## Longest Common Prefix

#### Solutions

{{< tabs >}}
{{% tab name="Python" %}}

```python
def longestCommonPrefix(self, strs):
    pre = strs[0]

    for i in strs:
        while not i.startswith(pre):
            pre = pre[:-1]
    return pre
```

{{% /tab %}}

{{% tab name="C#" %}}

```c#
public string LongestCommonPrefix(string[] strs)
{
    string prefix = strs[0];
    foreach (string item in strs)
    {
        while (!item.StartsWith(prefix))
        {
            prefix = prefix.Remove(prefix.Length - 1, 1);
        }
    }
    return prefix;
}
```

{{% /tab %}}
{{< /tabs >}}

#### Algorithm

- Horizantal
- Vertical

---

## Valid Parentheses

{{< tabs >}}
{{% tab name="Python" %}}

```python
def isValid(s):
    stack = []
    pairs = {'(': ')', '{': '}', '[': ']'}
    for item in s:
        if item in pairs:
            stack.append(item)
        elif stack != [] and pairs[stack[-1]] == item:
            stack.pop()
        else:
            return False

    return len(stack) == 0
```

{{% /tab %}}

{{% tab name="C#" %}}

```c#
bool IsValid(string s)
{
    List<char> stack = new List<char>();
    Dictionary<char, char> pairs = new Dictionary<char, char>()
    {
        { '(', ')'},{ '{', '}'},{ '[', ']'},
    };

    foreach(char item in s)
    {
        if (pairs.ContainsKey(item))
        {
            stack.Add(item);
        } else if (stack.Count !=0 && pairs[stack[stack.Count-1]] == item) {
            stack.RemoveAt(stack.Count-1);
        } else
        {
            return false;
        }
    }

    return stack.Count == 0;
}
```

{{% /tab %}}
{{< /tabs >}}

---

## Merge Two Sorted Lists

When you assign to `list.next`, it becomes the next value

#### Solutions

{{< tabs >}}
{{% tab name="Python" %}}

```python
list1.next = value

```

{{% /tab %}}

{{% tab name="C#" %}}

```c#

```

{{% /tab %}}
{{< /tabs >}}

#### Algorithm

- Linked list
- Recursive Approach
- Iterative Approach
- Time complexity : O(n+m) time
- Space complexity : O(n+m) / O(1)

---

## Search Insert Position

{{< tabs >}}
{{% tab name="Python" %}}

```python
def searchInsert(self, nums, target):
    nums.append(target)
    nums.sort()
    return nums.index(target)
```

```python
def searchInsert(self, nums, target):
    return len([x for x in nums if x < target])
    # Add item if target is less than the item
    # Return the length of list, it will be the index the target should be

```

{{% /tab %}}

{{% tab name="C#" %}}

```c#

```

{{% /tab %}}
{{< /tabs >}}

#### Algorithm

## Climbing Staris

#### Solution

{{< tabs >}}
{{% tab name="Python" %}}

```python
def searchInsert(self, nums, target):
```

{{% /tab %}}

{{% tab name="C#" %}}

```c#

```

{{% /tab %}}
{{< /tabs >}}

#### Algorithm

- The Fibonacci Sequence
- Recursive / Dynamic Programming / Back Tracking

# TAB

{{< tabs >}}
{{% tab name="Python" %}}

```python


```

{{% /tab %}}

{{% tab name="C#" %}}

```c#

```

{{% /tab %}}
{{< /tabs >}}
