# YAML Quick Reference

#### General notes
- YAML file names use the .yml or .yaml extension
- Comments begin with a pound sign.  Anything after a pound sign is a comment
- The YAML file optionally starts with three hyphens (---)
- no tabs are allowed.  Indentation must be done using spaces
- The basic construct is a key-value pair
- A key-value pair has a colon and space between them
  ```
  first-name: "John"
  ```
- The key is on the left and is not in quotes
  - Best practice is to have the key be letters and underscores (no spaces)
- The value is on the right and is one of these 6 simple data types
  - strings e.g. first_name: "John"
    - Strings do not have to be enclosed in quotes, or they can be in single or double quotes
    - If a string looks like another data type then it must be in quotes.  e.g. zip_code: '44106'
  - numeric - not quoted
    - integers e.g. age: 14
    - floating point (real) numbers e.g. price: 572.35
    - hexadecimal - begins with 0x e.g. mac_address: 0x3c8b7f0231a4
    - octal - begins with a zero e.g. age: 014
    - some other numeric types
  - dictionaries
      - a dictionary can be on a single line
        ```
        phone_numbers: { home: "440-229-7392", cell: "440-737-3983", work: "216-225-7352" }
        ```
      
      - or a dictionary can be on multiple lines
      
        ```
        phone_numbers: 
          home: "440-229-7392",
          cell: "440-737-3983",
          work: "216-225-7352"
        ```

  - arrays
    - arrays can be specified on a single line
      ```
      days: [ "Sun", "Mon", "Tues", "Wed", "Thurs", "Fri", "Sat" ]
      ```
    - or arrays can be specified on multiple lines
      ```
      days:
        - "Sun"
        - "Mon"
        - "Tues"
        - "Wed"
        - "Thurs"
        - "Fri"
        - "Sat"
    ```
  - boolean (True, Yes, or On; False, No, Off) - not quoted
  - null
  - timestamp
- key-value pairs are separated by commas.  Note that the last key-value pair will not have a comma
- Indentation does not matter.  Best practice is to make it readable so use 2-4 spaces
- A nested YAML dictionary just means that for a key-value pair, the value is a dictionary
- An array has just values (no keys).  An array is enclosed in square brackets [] if on a single line, or listed with hypens
- Tip: A YAML file can easily be converted to JSON and vice-versa.  Try https://www.json2yaml.com

#### Example of a simple YAML file
```
---
first_name: John
last_name: Doe
gender: male
age: 48
street_address: 125 main street
city: Cleveland
state: OH
zip_code: '44106'
home_phone: 440-229-7392
cell_phone: 440-737-3983
work_phone: 216-225-7352
retired: false
```

#### Example of a nested YAML file
```
---
first_name: John
last_name: Doe
gender: male
age: 48
street_address: 125 main street
city: Cleveland
state: OH
zip_code: '44106'
phone_numbers:
  home: 440-229-7392
  cell: 440-737-3983
  work: 216-225-7352
retired: false


```

#### Example of an array
```
---
first_name: John
last_name: Doe
gender: male
age: 48
street_address: 125 main street
city: Cleveland
state: OH
zip_code: '44106'
phone_numbers:
  - 440-229-7392
  - 440-737-3983
  - 216-225-7352
retired: false
```
