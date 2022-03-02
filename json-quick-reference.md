# JSON Quick Reference

#### General notes
- JSON file names use the .json extension
- A JSON object starts with a left brace { and ends with a right brace }
- JSON is a key-value data format
- A key-value pair has a colon between them: "first-name" : "John"
- The key is on the left and is in double quotes
  - Best practice is to have the key be letters and underscores (no spaces)
- The value is on the right and is one of these 6 simple data types
  - strings - must be in double quotes
  - numbers - not quoted
  - objects (e.g. another JSON object)
  - arrays
  - boolean (true or false) - not quoted
  - null
- key-value pairs are separated by commas.  Note that the last key-value pair will not have a comma
- Indentation does not matter.  Best practice is to make it readable so use 2-4 spaces
- A nested JSON object just means that for a key-value pair, the value is a JSON object
  - If a JSON object is not nested, it is also called a "flat" JSON object
- An array has just values (no keys).  An array is enclosed in square brackets []
- Tip: A JSON file can easily be converted to YAML and vice-versa.  Try https://www.json2yaml.com

#### Example of an empty JSON object
```
{}
```

#### Example of a simple JSON object (flat)

```
{
  "first_name" : "John",
  "last_name" : "Doe",
  "gender" : "male",
  "age" : 48,
  "street_address" : "125 main street",
  "city" : "Cleveland",
  "state" : "OH",
  "zip_code" : "44106",
  "home_phone" : "440-229-7392",
  "cell_phone" : "440-737-3983",
  "work_phone" :  "216-225-7352",
  "retired" : false
}
```

#### Example of a nested JSON object
```
{
    "first_name": "John",
    "last_name": "Doe",
    "gender": "male",
    "age": 48,
    "street_address": "125 main street",
    "city": "Cleveland",
    "state": "OH",
    "zip_code": "44106",
    "phone_numbers": {
        "home": "440-229-7392",
        "cell": "440-737-3983",
        "work": "216-225-7352"
     },
     "retired" : false
}
```

#### Example of an array
```
{
    "first_name": "John",
    "last_name": "Doe",
    "gender": "male",
    "age": 48,
    "street_address": "125 main street",
    "city": "Cleveland",
    "state": "OH",
    "zip_code": "44106",
    "phone_numbers": [
        "440-229-7392",
        "440-737-3983",
        "216-225-7352"
     ],
     "retired" : false
}
```

