+++
title = "Easy"
weight = 2
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

## Two Sum

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
This is R

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

## Roman to Integer

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
