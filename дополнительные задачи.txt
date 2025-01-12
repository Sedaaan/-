#1
def analyze_text(text):
    words = text.split()
    word_count = {}
    longest_word = ""

    for word in words:
        word_lower = word.lower()
        word_count[word_lower] = word_count.get(word_lower, 0) + 1
        
        if len(word_lower) > len(longest_word):
            longest_word = word_lower
            
    unique_word_count = len(word_count)
    longest_word_length = len(longest_word)

    print(f"Количество уникальных слов: {unique_word_count}")
    print("Частота встречаемости каждого слова:")
    for word, count in word_count.items():
        print(f"{word}: {count}")
    print(f"Самое длинное слово: '{longest_word}' (длина: {longest_word_length})")

text_input = "Пример текста с различными словами. Слова могут повторяться, и некоторые слова могут быть длиннее других."
analyze_text(text_input)

from collections import Counter

#2
def analyze_matrix(matrix):
    row_sums = []
    max_indices = []
    is_symmetric = True

    for i in range(len(matrix)):
        row_sum = sum(matrix[i])
        row_sums.append(row_sum)

        max_value = max(matrix[i])
        max_index = (i, matrix[i].index(max_value))
        max_indices.append(max_index)

        if len(matrix) != len(matrix[i]):
            is_symmetric = False
        else:
            for j in range(len(matrix)):
                if matrix[i][j] != matrix[j][i]:
                    is_symmetric = False

    return row_sums, max_indices, is_symmetric

#3
import random
import string

def generate_password(length, requirements):
    all_chars = ""
    if 'letters' in requirements:
        all_chars += string.ascii_letters
    if 'digits' in requirements:
        all_chars += string.digits
    if 'special' in requirements:
        all_chars += string.punctuation

    if length > len(all_chars):
        raise ValueError("Длина пароля превышает количество доступных уникальных символов.")

    password = ''.join(random.sample(all_chars, length))
    return password

length = int(input("Введите количество символов в пароле: "))
requirements = input("Введите требования (letters, digits, special): ").split(", ")

password = generate_password(length, requirements)
print(f"Сгенерированный пароль: {password}")

#4
import calendar

days_of_week = {
    0: 'Пн', 1: 'Вт', 2: 'Ср', 3: 'Чт', 4: 'Пт', 5: 'Сб', 6: 'Вс'
}

def print_calendar(year, month):
    month_days = calendar.monthcalendar(year, month)
    print(' '.join(days_of_week[i] for i in range(7)))
    for week in month_days:
        print(' '.join(f'{day if day != 0 else " ":2}' for day in week))

year = int(input("Введите год: "))
month = int(input("Введите месяц: "))

print_calendar(year, month)

#5
def caesar_cipher(text, shift):
    alphabet_lower = 'abcdefghijklmnopqrstuvwxyz'
    alphabet_upper = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'
    
    result = []
    
    for char in text:
        if char.islower():
            new_char = alphabet_lower[(alphabet_lower.index(char) + shift) % 26]
        elif char.isupper():
            new_char = alphabet_upper[(alphabet_upper.index(char) + shift) % 26]
        else:
            new_char = char
        
        result.append(new_char)
    
    return ''.join(result)

text = input("Введите текст: ")
shift = int(input("Введите сдвиг: "))
print("Зашифрованный текст:", caesar_cipher(text, shift))

#6
def find_leaders(arr):
    leaders = []
    max_right = float('-inf')
    
    for i in range(len(arr) - 1, -1, -1):
        if arr[i] > max_right:
            leaders.append(arr[i])
            max_right = arr[i]
    
    return leaders[::-1]

arr = [16, 17, 4, 3, 5, 2]
print("Лидеры:", find_leaders(arr))

#7
import random

class BullsAndCows:
    def __init__(self):
        self.secret = ''.join(random.sample('0123456789', 4))

    def check_guess(self, guess):
        bulls = sum(1 for i in range(4) if guess[i] == self.secret[i])
        cows = sum(1 for i in range(4) if guess[i] in self.secret and guess[i] != self.secret[i])
        return bulls, cows

    def play(self):
        print("Добро пожаловать в игру 'Быки и коровы'!")
        while True:
            guess = input("Введите 4-значное число (с уникальными цифрами): ")
            if len(guess) != 4 or len(set(guess)) != 4 or not guess.isdigit():
                print("Некорректный ввод. Попробуйте снова.")
                continue
            
            bulls, cows = self.check_guess(guess)
            print(f"Быков: {bulls}, Коров: {cows}")
            
            if bulls == 4:
                print("Поздравляю! Вы угадали число!")
                break

game = BullsAndCows()
game.play()

#8
def add(x, y):
    return x + y

def subtract(x, y):
    return x - y

def multiply(x, y):
    return x * y

def divide(x, y):
    if y != 0:
        return x / y
    else:
        raise ValueError("Деление на ноль!")

def apply_operator(operators, values):
    operator = operators.pop()
    right = values.pop()
    left = values.pop()
    if operator == '+':
        values.append(add(left, right))
    elif operator == '-':
        values.append(subtract(left, right))
    elif operator == '*':
        values.append(multiply(left, right))
    elif operator == '/':
        values.append(divide(left, right))

def precedence(op):
    if op == '+' or op == '-':
        return 1
    if op == '*' or op == '/':
        return 2
    return 0

def evaluate(expression):
    operators = []
    values = []
    i = 0
    while i < len(expression):
        if expression[i] == ' ':
            i += 1
            continue
        if expression[i] == '(':
            operators.append(expression[i])
        elif expression[i] == ')':
            while operators and operators[-1] != '(':
                apply_operator(operators, values)
            operators.pop()
        elif expression[i].isdigit():
            num = 0
            while i < len(expression) and expression[i].isdigit():
                num = num * 10 + int(expression[i])
                i += 1
            values.append(num)
            continue
        else:
            while (operators and precedence(operators[-1]) >= precedence(expression[i])):
                apply_operator(operators, values)
            operators.append(expression[i])
        i += 1
    
    while operators:
        apply_operator(operators, values)
    
    return values[-1]

expression = input("Введите арифметическое выражение: ")
print("Результат:", evaluate(expression))

#9
def analyze_expenses():
    expenses = {}
    total_expenses = 0
    
    while True:
        entry = input("Введите расходы в формате категория: сумма (или 'стоп' для завершения): ")
        if entry.lower() == 'стоп':
            break
        try:
            category, amount = entry.split(':')
            category = category.strip()
            amount = float(amount.strip())
            total_expenses += amount
            if category in expenses:
                expenses[category] += amount
            else:
                expenses[category] = amount
        except ValueError:
            print("Неверный формат. Попробуйте снова.")
    
    max_category = max(expenses, key=expenses.get)
    print(f"Общая сумма расходов: {total_expenses:.2f}")
    print("Расходы по категориям:")
    for category, amount in expenses.items():
        print(f"{category}: {amount:.2f}")
    print(f"Категория с максимальными расходами: {max_category} ({expenses[max_category]:.2f})")

analyze_expenses()

#10
def group_anagrams(words):
    anagrams = {}
    
    for word in words:
        sorted_word = ''.join(sorted(word))
        if sorted_word not in anagrams:
            anagrams[sorted_word] = []
        anagrams[sorted_word].append(word)
    
    return anagrams

words = ["eat", "tea", "tan", "ate", "nat", "bat"]
result = group_anagrams(words)
print(result)


