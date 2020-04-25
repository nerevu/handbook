# Nerevu Coding Style

The following examples will help you understand how you will be expected to code at Nerevu. Read through them and ask questions if you don't understand.

## Python/Flask

- Avoid complexity in your code. If it's hard to understand what's going on in a function, break it out. It shouldn't take a lot of explaining to show what code is doing. Commenting will help with this.

- Most Nerevu projects have a `manage.py` file that has CLI commands that can be run for a project using the following CLI call - `manage {function_name}`. If the project you are working on has a `manage.py` file with a`prettify` function in it, you should run `manage prettify` on your code before your PR is merged. This keeps the formatting consistent.

- Practice `DRY` coding (Don't Repeat Yourself). If you find you are writing similar code over and over, try abstracting it out into a function (if it makes sense to do so). The below example is trivial, but should get the point across.

```python
# INCORRECT
user1 = 'doug peterson'
user1 = " ".join(name.capitalize() for name in user1.split(" "))
user2 = 'george clooney'
user2 = " ".join(name.capitalize() for name in user2.split(" "))

# CORRECT
def format_name(name):
    return " ".join(name.capitalize() for name in user1.split(" "))

user1 = 'doug peterson'
user1 = format_name(user1)
user2 = 'george clooney'
user2 = format_name(user2)
```

- Start thinking like a functional programmer. Don't mutate objects. Return new objects instead. See [the docs](https://docs.python.org/3/library/functional.html) for more info on functional programming in python.

```python
users = [{"name": "brian"}, {"name": "jenna"}]

# INCORRECT
def lowercase_name(user):
    user["name"] = user["name"].lower()
    return user

list_of_users = map(lowercase_name, users)

# CORRECT
def lowercase_name(user):
    return {**user, "name": user["name"].lower()}

list_of_users = map(lowercase_name, users)
```

- use `pos` instead of `i` when using enumerate (with `range()` you don't need to use this)

```python
names = ['bob', 'jack', 'bill', 'jessica']

# INCORRECT
for i, name in enumerate(names):
    print(f"{i}: {name}")

# CORRECT
for pos, name in enumerate(names):
    print(f"{pos}: {name}")
```

- Generators

Name Generators `gen_{function_name}`
Use generators instead of appending to empty lists

```python
# INCORRECT
letters = []

for i in range(10):
    if i % 2:
        letters.append(chr(i))

# CORRECT - notice the naming of the function as well as the (gen_)

def gen_letters(nums):
    for i in nums:
        if i % 2:
            yield chr(i)

letters = list(gen_letters(range(10)))
```

- use list comprehensions instead of list and map

```python
names = ['Phillip', 'Annette']
lowercase_names = lambda n: n.lower()

# INCORRECT
new_names = list(map(lowercase_names, names))

# CORRECT
new_names = [lowercase_names(n) for n in names]
```

- use `dict.items()` instead of `key in dict`

```python
obj = {"name": "brian"}

# INCORRECT
for key in obj:
    print(obj[key])

# CORRECT
for key, val in obj.items()
    print(val)
```

- make variable names meaningful unless they are in a list comprehension or lambda function, etc..

```python
users = [{"name": "brian"}, {"name": "jenna"}]

# INCORRECT
def gen_users(n):
    yield n['name']

list_of_users = list(gen_users(users))

# CORRECT
def gen_users(user):
    yield user['name']

list_of_users = list(gen_users(users))

# ALSO CORRECT
list_of_users = [u["name"] for u in users]
list_of_users = map(lambda u: u["name"], users)
```

- If you're not sure if a key will be present in a dictionary, use `dict.get("key")` so your code doesn't break.

```python
user = {"name": "brian"}

# INCORRECT
age = user["age"] # Throws a KeyError

# CORRECT
age = user.get("age") # returns None
```

- Add breaks to short circuit for loops once the correct field is found

```python
numbers = [1,2,3,4,5,6,7,8,9,10]

# CORRECT EXAMPLE
contains_number_four = False

for number in numbers:
    if number is 4:
        contains_number_four = True
        break # prevent the loop from continuing!
```

- Nested loops in general are code smell really - `Set` datatypes are indexed, so doing `if x in Set` is faster than doing a double for loop

```python
names = ["Bob", "Jesse", "Alyssa", "Frank"]
whitelist = {"Bill", "Jack", "Frank", "Frank"}

# These functions yield names that are present in a whitelist

# INCORRECT
def gen_names():
    for name in names:
        for other_name in whitelist:
            if name == other_name:
                yield name

# CORRECT
def gen_names():
    for name in names:
        if name in whitelist:
            yield name

# MORE CORRECT
def gen_names():
    for name in set(names).intersection(whitelist):
        yield name
```

- Don't use `types` (e.g. str, list, dict, etc.) in your variable names

```python
# INCORRECT VARIABLE NAMES
numbers_array = [1,2,3,4,5,6,7,8,9,10]
user_dict = {'name': 'bob', 'age': 16}
names_str_list = ['bill', 'george', 'katie', 'geoffrey', 'jessica']

# CORRECT VARIABLE NAMES
numbers = [1,2,3,4,5,6,7,8,9,10]
user = {'name': 'bob', 'age': 16}
names = ['bill', 'george', 'katie', 'geoffrey', 'jessica']
```

- Don't commit commented out code.

```python
name = 'bob'

# DON'T COMMIT THIS
# name += "cat"
# print(name)
```

- Most Nerevu projects have a `manage.py` file that has CLI commands that can be run for a project using the following CLI call - `manage {function_name}`. If the project you are working on has a `manage.py` file with a`prettify` function in it, you should run `manage prettify` on your code before your PR is merged. This keeps the formatting consistent.

## Coffeescript/Mithril

- prefer `is` and `isnt` over `==` and `!=`
- prefer `unless ctrl.page()` to `if ctrl.page() is undefined`
- coffeescript vars should be pascalCase
- Loop through objects like this: `Object.entries(statsByRep).map ([rep, repStats]) =>`
