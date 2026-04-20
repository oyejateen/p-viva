# p-viva

https://chatgpt.com/share/69e59690-7ca4-83e8-a9dc-5ff1a6fa6d3c

1. Hello World:

print("Hello World")

2. Sum of Two Numbers

a = int(input("Enter first number: "))
b = int(input("Enter second number: "))
print("Sum =", a + b)

3. Even or Odd

num = int(input("Enter a number: "))
if num % 2 == 0:
    print("Even")
else:
    print("Odd")
4. Factorial
num = int(input("Enter a number: "))
fact = 1

for i in range(1, num + 1):
    fact *= i

print("Factorial =", fact)
5. GCD of Two Numbers
a = int(input("Enter first number: "))
b = int(input("Enter second number: "))

while b:
    a, b = b, a % b

print("GCD =", a)
6. Fibonacci Series
n = int(input("Enter number of terms: "))
a, b = 0, 1

for i in range(n):
    print(a, end=" ")
    a, b = b, a + b
7. Largest of Three Numbers
a = int(input("Enter first: "))
b = int(input("Enter second: "))
c = int(input("Enter third: "))

largest = max(a, b, c)
print("Largest =", largest)
8. Armstrong Number
num = int(input("Enter a number: "))
temp = num
sum = 0

while temp > 0:
    digit = temp % 10
    sum += digit ** 3
    temp //= 10

if num == sum:
    print("Armstrong")
else:
    print("Not Armstrong")
9. Sum of First N Natural Numbers
n = int(input("Enter n: "))
sum = n * (n + 1) // 2
print("Sum =", sum)
10. Power of a Number
base = int(input("Enter base: "))
exp = int(input("Enter exponent: "))

result = base ** exp
print("Result =", result)
11. Command Line Arguments
import sys

print("Arguments:", sys.argv)

Run like:

python file.py hello 123
12. Reverse String (Function)
def reverse_string(s):
    return s[::-1]

text = input("Enter string: ")
print("Reversed:", reverse_string(text))
13. Count Vowels
text = input("Enter string: ")
count = 0

for ch in text.lower():
    if ch in "aeiou":
        count += 1

print("Vowels:", count)
14. List Operations
lst = [5, 2, 9]

lst.append(7)
print("After append:", lst)

lst.remove(2)
print("After remove:", lst)

lst.sort()
print("After sort:", lst)
15. Tuple and Dictionary
# Tuple
t = (1, 2, 3)
print("Tuple:", t)

# Dictionary
d = {"name": "Jatin", "age": 20}
print("Dictionary:", d)
16. File Count (Words, Lines, Characters)
file = open("test.txt", "r")

lines = file.readlines()
line_count = len(lines)
word_count = sum(len(line.split()) for line in lines)
char_count = sum(len(line) for line in lines)

print("Lines:", line_count)
print("Words:", word_count)
print("Characters:", char_count)

file.close()
17. Class and Object
class Student:
    def __init__(self, name):
        self.name = name

    def display(self):
        print("Name:", self.name)

s1 = Student("Jatin")
s1.display()
18. Inheritance
class Parent:
    def show(self):
        print("This is parent")

class Child(Parent):
    def display(self):
        print("This is child")

c = Child()
c.show()
c.display()
19. Read File and Display
file = open("test.txt", "r")
data = file.read()
print(data)
file.close()
20. Raspberry Pi LED Blink (Logic Only)
import RPi.GPIO as GPIO
import time

GPIO.setmode(GPIO.BCM)
pin = 18
GPIO.setup(pin, GPIO.OUT)

while True:
    GPIO.output(pin, True)
    time.sleep(1)
    GPIO.output(pin, False)
    time.sleep(1)
