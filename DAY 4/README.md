# DAY 4 SAMPLE WORK
1. Question 1:
Task

Given a year, determine whether it is a leap year. If it is a leap year, return the Boolean True, otherwise return False.

Note that the code stub provided reads from STDIN and passes arguments to the is_leap function. It is only necessary to complete the is_leap function.

Input Format

Read year, the year to test.

Constraints


Output Format

The function must return a Boolean value (True/False). Output is handled by the provided code stub.

code:

def is_leap(year):
    leap = False
    if year % 400 == 0:
        leap = True
    elif year % 100 == 0:
        leap = False
    elif year % 4 == 0:
        leap = True
    else:
        leap = False    
    return leap

year = int(input())
print(is_leap(year))

1.A) Leap Year Rules Used
The code follows the standard Gregorian calendar leap year rules:
Divisible by 400 → Leap year (overrides other rules)
Divisible by 100 → NOT leap year (exception to the divisible-by-4 rule)
Divisible by 4 → Leap year (basic rule)
Otherwise → NOT leap year

2.A) Conditional Logic Flow
The if-elif-elif-else structure ensures:
Order matters: Check divisibility by 400 first (highest priority)
Mutual exclusion: Once a condition matches, others aren't checked
Clear hierarchy: 400 > 100 > 4 > default

3.c)  Boolean Variable Pattern
leap = False sets default value
Each condition updates this variable
Final return leap gives the result



2. Input Format

The first line contains a single integer , the number of test cases.
For each test case, there are  lines.
The first line of each test case contains , the number of cubes.
The second line contains  space separated integers, denoting the sideLengths of each cube in that order.

Output Format
For each test case, output a single line containing either Yes or No.

SAMPLE INPUT:

STDIN        Function
-----        --------
2            T = 2
6            blocks[] size n = 6
4 3 2 1 3 4  blocks = [4, 3, 2, 1, 3, 4]
3            blocks[] size n = 3
1 3 2        blocks = [1, 3, 2, 4]

Sample Output

Yes
No

SOLUTION:

T = int(input())

for _ in range(T):
    n = int(input())
    cubes = list(map(int, input().split()))

    i = 0             
    j = n - 1          
    prev = 10**15     
    possible = True

    while i <= j:
        left = cubes[i]
        right = cubes[j]

       
        if left <= prev and right <= prev:
            if left >= right:
                prev = left
                i += 1
            else:
                prev = right
                j -= 1

      
        elif left <= prev:
            prev = left
            i += 1

       
        elif right <= prev:
            prev = right
            j -= 1

        
        else:
            possible = False
            break

    if possible:
        print("Yes")
    else:
        print("No")

Time Complexity
O(n) for each test case (single pass through the cubes)
O(T × n) total time
O(1) extra space
Key Points to Remember
Greedy approach works: Always pick the largest available end that fits
Two-pointer technique: Efficiently processes from both ends
Edge cases:
Single cube → Always "Yes"
Already sorted list → Works fine
List with all equal elements → Works fine
List where largest is in middle → May fail

