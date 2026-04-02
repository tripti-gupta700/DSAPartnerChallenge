# Day 1 – Pseudocode & Flowcharts

## 1. Leap Year Check

START
Input the year
IF ((year % 4==0 and year % 100 !=0) or (year %400==0))
    PRINT "Leap Year"
ELSE
    PRINT "Not Leap Year"
END

### Flowchart -

Start
  ↓
Input Year
  ↓
Check: Year %4 == 0 AND (Year %100 != 0 OR Year %400 == 0)?
  ↓
Yes → Print "Leap Year"
No  → Print "Not Leap Year"
  ↓
End

----


## 2. Sum of Two Numbers
START
INPUT num1
INPUT num2
sum = num1 + num2
PRINT sum
END

### Flowchart

Start
  ↓
Input num1, num2
  ↓
sum = num1 + num2
  ↓
Print sum
  ↓
End

-----

## 3. Multiplication Table
### Pseudocode

START
INPUT number
FOR i = 1 TO 10
    result = number * i
    PRINT number x i = result
END FOR
END

### Flowchart
Start
  ↓
Input number
  ↓
i = 1
  ↓
Check i ≤ 10 ?
  ↓
Yes → Print number × i
       i = i + 1
       Repeat
No
  ↓
End

-----

## 4. HCF and LCM
### Pseudocode

START

INPUT a
INPUT b
temp1 = a
temp2 = b
WHILE b ≠ 0
    remainder = a % b
    a = b
    b = remainder
END WHILE
HCF = a
LCM = (temp1 * temp2) / HCF
PRINT HCF
PRINT LCM
END

### Flowchart
Start
  ↓
Input a, b
  ↓
Check b ≠ 0 ?
  ↓
Yes → r = a % b
       a = b
       b = r
       Repeat
No
  ↓
HCF = a
  ↓
LCM = (temp1 × temp2) / HCF
  ↓
Print HCF and LCM
  ↓
End

------

## 5. Sum Until 'x'
### Pseudocode
START
sum = 0
REPEAT
    INPUT value
    IF value == 'x'
        BREAK
    END IF
    sum = sum + value
UNTIL value == 'x'
PRINT sum
END

### Flowchart
Start
  ↓
sum = 0
  ↓
Input value
  ↓
Check value = 'x' ?
  ↓
Yes → Print sum → End
No
  ↓
sum = sum + value
  ↓
Repeat

-----


