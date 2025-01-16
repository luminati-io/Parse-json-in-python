# Parsing JSON Data with Python

[![Promo](https://github.com/luminati-io/LinkedIn-Scraper/raw/main/Proxies%20and%20scrapers%20GitHub%20bonus%20banner.png)](https://brightdata.com/) 

This guide covers parsing JSON data with the `json` module in Python and transforming it into a Python dictionary and vice versa.

- [An Introduction to JSON in Python](#an-introduction-to-json-in-python)
- [Parsing JSON Data With Python](#parsing-json-data-with-python-1)
- [Converting a JSON String to a Python Dictionary](#converting-a-json-string-to-a-python-dictionary)
  - [Transforming a JSON API Response Into a Python Dictionary](#transforming-a-json-api-response-into-a-python-dictionary)
  - [Loading a JSON File Into a Python Dictionary](#loading-a-json-file-into-a-python-dictionary)
  - [From JSON Data to Custom Python Object](#from-json-data-to-custom-python-object)
- [Converting Python Data to JSON](#python-data-to-json)
- [Limitations of the `json` Standard Module](#limitations-of-the-json-standard-module)
- [Conclusion](#conclusion)

## An Introduction to JSON in Python

JavaScript Object Notation, or JSON, is a lightweight data-interchange format commonly used for transmitting data between servers and web applications via APIs. JSON data consists of key-value pairs where each key is a string, and each value can be a string, number, boolean, null, array, or object.

Here is an example of JSON:

```json
{
  "name": "Maria Smith",
  "age": 32,
  "isMarried": true,
  "hobbies": ["reading", "jogging"],
  "address": {
    "street": "123 Main St",
    "city": "San Francisco",
    "state": "CA",
    "zip": "12345"
  },
  "phoneNumbers": [
    {
      "type": "home",
      "number": "555-555-1234"
    },
    {
      "type": "work",
      "number": "555-555-5678"
    }
  ],
  "notes": null
}
```

Python natively supports [JSON](https://docs.python.org/3/library/json.html) through the `json` module, which is part of the [Python Standard Library](https://docs.python.org/3/library/index.html). This means that you do not need to install any additional library to work with JSON in Python. You can import `json` as follows:

```python
import json
```

The built-in Python `json` library exposes a complete API to deal with JSON. In particular, it has two key functions: [`loads`](https://docs.python.org/3/library/json.html#json.loads) and [`load`](https://docs.python.org/3/library/json.html#json.load). The `loads` function is for parsing JSON data from a string, while the `load` function is for parsing JSON data into bytes.

Through those two methods, `json` allows you to convert JSON data to equivalent Python objects like [dictionaries](https://docs.python.org/3/tutorial/datastructures.html#dictionaries) and [lists](https://docs.python.org/3/tutorial/datastructures.html#more-on-lists), and vice versa. Plus, the `json` module allows you to create custom encoders and decoders to handle specific data types.

## Parsing JSON Data With Python

## Converting a JSON String to a Python Dictionary

Assume that you have some JSON data stored in a string and you want to convert it to a Python dictionary. This is what the JSON data looks like:

```json
{
  "name": "iPear 23",
  "colors": ["black", "white", "red", "blue"],
  "price": 999.99,
  "inStock": true
}
```

And this is its string representation in Python:

```python
smartphone_json = '{"name": "iPear 23", "colors": ["black", "white", "red", "blue"], "price": 999.99, "inStock": true}'
```

> **Note**\
> Consider using the Python triple quotes convention to store long multi-line JSON strings.

You can verify that `smartphone` contains a valid Python string with the line below:

```python
print(type(smartphone))
```

This will print:

```
<class 'str'>
```

[`str`](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str) stands for “string” and means that the smartphone variable has the text sequence type.

Parse the JSON string contained in smartphone into a Python dictionary with the json.loads() method as follows:

```python
import json

# JSON string
smartphone_json = '{"name": "iPear 23", "colors": ["black", "white", "red", "blue"], "price": 999.99, "inStock": true}'
# from JSON string to Python dict
smartphone_dict = json.loads(smartphone_json)

# verify the type of the resulting variable
print(type(smartphone_dict)) # dict
```

If you run this snippet, you would get:

```
<class 'dict'>
```

Now `smartphone_dict` contains a valid Python dictionary.

Next, pass a valid JSON string to `json.loads()` to convert a JSON string to a Python dictionary.

You can now access the resulting dictionary fields as usual:

```python
product = smartphone_dict['name'] # smartphone
priced = smartphone['price'] # 999.99
colors = smartphone['colors'] # ['black', 'white', 'red', 'blue']
```

The `json.loads()` function will not always return a dictionary. Specifically, the returning data type depends on the input string. For example, if the JSON string contains a flat value, it will be converted to the equivalent Python primitive value:

```python
import json
 
json_string = '15.5'
float_var = json.loads(json_string)

print(type(float_var)) # <class 'float'>
```

Similarly, a JSON string containing an array list will become a Python list:

```python
import json
 
json_string = '[1, 2, 3]'
list_var = json.loads(json_string)
print(json_string) # <class 'list'>
```

The [conversion table](https://docs.python.org/3/library/json.html#json-to-py-table) below explains how JSON values are converted to Python data by `json`:

| **JSON Value** | **Python Data** |
| - | - | - |
| `string` | `str` |
| `number (integer)` | `int` |
| `number (real)` | `float` |
| `true` | `True` |
| `false` | `False` |
| `null` | `None` |
| `array` | `list` |
| `object` | `dict` |

### Transforming a JSON API Response Into a Python Dictionary

Consider that you need to make an API and convert its JSON response to a Python dictionary. In the example below, we will call the following API endpoint from the [{JSON} Placeholder](https://jsonplaceholder.typicode.com/) project to get some fake JSON data:

```
https://jsonplaceholder.typicode.com/todos/1
```

That RESTFul API returns the JSON response below:

```json
{
  "userId": 1,
  "id": 1,
  "title": "delectus aut autem",
  "completed": false
}
```

You can call that API with the [`urllib`](https://docs.python.org/3/library/urllib.html) module from the Standard Library and convert the resulting JSON to a Python dictionary as follows:

```python
import urllib.request
import json

url = "https://jsonplaceholder.typicode.com/todos/1"

with urllib.request.urlopen(url) as response:
     body_json = response.read()

body_dict = json.loads(body_json)
user_id = body_dict['userId'] # 1
```

[`urllib.request.urlopen()`](https://docs.python.org/3/library/urllib.request.html#urllib.request.urlopen) peforms the API call and returns an [`HTTPResponse`](https://docs.python.org/3/library/http.client.html#http.client.HTTPResponse) object. Its [`read()`](https://docs.python.org/3/library/http.client.html#http.client.HTTPResponse.read) method is then used to get the response body body\_json, which contains the API response as a JSON string. Finally, that string can be parsed into a Python dictionary through `json.loads()` as explained earlier.

Similarly, you can achieve the same result with [`requests:`](https://requests.readthedocs.io/en/latest/).

```python
import requests
import json

url = "https://jsonplaceholder.typicode.com/todos/1"
response = requests.get(url)

body_dict = response.json()
user_id = body_dict['userId'] # 1
```

> **Note**\
> The [`.json()`](https://requests.readthedocs.io/en/latest/user/quickstart/?highlight=json#json-response-content) method automatically transforms the response object containing JSON data into the respective Python data structure.

### Loading a JSON File Into a Python Dictionary

Suppose you have some JSON data stored in a `smartphone.json` file as below:

```json
{
  "name": "iPear 23",
  "colors": ["black", "white", "red", "blue"],
  "price": 999.99,
  "inStock": true,
  "dimensions": {
    "width": 2.82,
    "height": 5.78,
    "depth": 0.30
  },
  "features": [
    "5G",
    "HD display",
    "Dual camera"
  ]
}
```

Your goal is to read the JSON file and load it into a Python dictionary. Achieve that with the snippet below:

```python
import json

with open('smartphone.json') as file:
  smartphone_dict = json.load(file)

print(type(smartphone_dict)) # <class 'dict'>
features = smartphone_dict['features'] # ['5G', 'HD display', 'Dual camera']
```

The built-in [`open()`](https://docs.python.org/3/library/functions.html#open) library allows you to load a file and get its corresponding [file object](https://docs.python.org/3/glossary.html#term-file-object). The `json.read()` method then deserializes the [text file](https://docs.python.org/3/glossary.html#term-text-file) or [binary file](https://docs.python.org/3/glossary.html#term-binary-file) containing a JSON document to the equivalent Python object. In this case, `smartphone.json` becomes a Python dictionary.

### From JSON Data to Custom Python Object

Now, let's parse some JSON data into a custom Python class. This is what your custom `Smartphone` Python class looks like:

```python
class Smartphone:
    def __init__(self, name, colors, price, in_stock):
        self.name = name    
        self.colors = colors
        self.price = price
        self.in_stock = in_stock
```

Here, the goal is to convert the following JSON string to a `Smartphone` instance:

```json
{
  "name": "iPear 23 Plus",
  "colors": ["black", "white", "gold"],
  "price": 1299.99,
  "inStock": false
}
```

Create a custom decoder to accomplish this task. To do that, extend the [`JSONDecoder`](https://docs.python.org/3/library/json.html#json.JSONDecoder) class and set the `object_hook` parameter in the `__init__` method. Assign it with the name of the class method containing the custom parsing logic. In that parsing method, you can use the values contained in the standard dictionary returned by `json.read()` to instantiate a `Smartphone` object.

Define a custom `SmartphoneDecoder` as below:

```python
import json
 
class SmartphoneDecoder(json.JSONDecoder):
    def __init__(self, object_hook=None, *args, **kwargs):
        # set the custom object_hook method
        super().__init__(object_hook=self.object_hook, *args, **kwargs)

    # class method containing the 
    # custom parsing logic
    def object_hook(self, json_dict):
        new_smartphone = Smartphone(
            json_dict.get('name'), 
            json_dict.get('colors'), 
            json_dict.get('price'),
            json_dict.get('inStock'),            
        )

        return new_smartphone
```

Use the `get()` method to read the dictionary values within the custom `object_hook()` method. This will ensure that no `KeyError`s are raised if a key is missing from the dictionary. Instead, `None` values will be returned.

Now pass the `SmartphoneDecoder` class to the `cls` parameter in `json.loads()` to convert a JSON string to a `Smartphone` object:

```python
import json

# class Smartphone:
# ...

# class SmartphoneDecoder(json.JSONDecoder): 
# ...

smartphone_json = '{"name": "iPear 23 Plus", "colors": ["black", "white", "gold"], "price": 1299.99, "inStock": false}'

smartphone = json.loads(smartphone_json, cls=SmartphoneDecoder)
print(type(smartphone)) # <class '__main__.Smartphone'>
name = smartphone.name # iPear 23 Plus
```

Similarly, you can use `SmartphoneDecoder` with `json.load()`:

```
smartphone = json.load(smartphone_json_file, cls=SmartphoneDecoder)
```

## Python Data to JSON

You can also go the other way around and convert Python data structures and primitives to JSON. This is possible thanks to the [`json.dump()`](https://docs.python.org/3/library/json.html#json.dump) and [`json.dumps()`](https://docs.python.org/3/library/json.html#json.dumps) functions, which follows the [conversion table](https://docs.python.org/3/library/json.html#py-to-json-table) below:

| **Python Data** | **JSON Value** |
| - | - | - |
| `str` | `string` |
| `int` | `number (integer)` |
| `float` | `number (real)` |
| `True` | `true` |
| `False` | `false` |
| `None` | `null` |
| `list` | `array` |
| `dict` | `object` |
| `Null` | None  |

`json.dump()` allows you to write a JSON string to a file, as in the following example:

```python
import json

user_dict = {
    "name": "John",
    "surname": "Williams",
    "age": 48,
    "city": "New York"
}

# serializing the sample dictionary to a JSON file
with open("user.json", "w") as json_file:
    json.dump(user_dict, json_file)
```

This snippet will serialize the Python `user_dict` variable into the `user.json` file.

Similarly, `json.dumps()` converts a Python variable to its equivalent JSON string:

```python
import json

user_dict = {
    "name": "John",
    "surname": "Williams",
    "age": 48,
    "city": "New York"
}

user_json_string = json.dumps(user_dict)

print(user_json_string)
```
Run this snippet and you will get:

```json
{"name": "John", "surname": "Williams", "age": 48, "city": "New York"}
```

> **Note**\
> Follow the [official documentation](https://docs.python.org/3/library/json.html#json.JSONEncoder) to learn how to specify a custom encoder.

### Limitations of the `json` Standard Module

JSON [data parsing](https://brightdata.com/blog/web-data/what-is-data-parsing) comes with challenges that cannot be overlooked.

Two commons examples are:

- The Python `json` module would fall short in case of invalid, broken, or non-standard JSON.
- Parsing JSON data from untrusted sources is dangerous because a malicious JSON string can cause your parser to break or consume a large amount of resources.

These limitations can be worked around, but it's best to use a commercial tool that makes JSON parsing easier, such as [Web Scraper API](https://brightdata.com/products/web-scraper).

## Conclusion

While natively parsing JSON data through the `json` standard module in Python, you will need reliable proxy servers to bypass restrictions imposed by websites. Try a cutting-edge, fully-featured, commercial solution for data parsing, such as Bright Data's data and [proxy products](https://brightdata.com/proxy-types).
