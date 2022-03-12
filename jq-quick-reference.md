#### JQ Quick Reference for version 1.6

JSON data types: string, number, object (dictionary), array, boolean, null

Manual:
   ```
   https://stedolan.github.io/jq/manual/
   https://github.com/stedolan/jq/wiki/jq-Language-Description
   ```

* Identity: .
    * Identity filter that passes the input to its output unchanged
   ```
      jq '.' data3.json
   ```
* suppress output: empty
    * the empty filter "empties" all of the input; it produces no output
    * This may be particularly useful in an if-else filter because the else is required
   ```
      jq 'emtpy' data3.json
   ```
   
* Object Identifer-Index: .\<key\> or .\<key\>.\<key\> 
    * A filter that outputs the value of the key in an object (dictionary)
    * If the key has special characters or starts with a digit it must be in quotes: ."key"
  ```
      jq '.last_name' data3.json
      jq '."last_name"' data3.json
  ```
* Optional Object Identifer-Index: .\<key\>?
  * A filter that outputs the value of the key if the key exists otherwise outputs null
  ```
      jq '.people[] | .first_name?'  data3.json # outputs a list of the first names (outputs a null if the first_name key does not exist)
      jq '.people[] | .middle_name?'  data3.json # outputs a list of nulls because middle_name is not a key
      jq '.people[] | if .first_name then .first_name else empty end'  data3.json # outputs a list of first names with no nulls
      jq '.people[] | if .middle_name then .middle_name else empty end'  data3.json # outputs nothing because middle_name is not a key
      jq '.people[] | if has("first_name") then .first_name else empty end'  data3.json # outputs a list of first names with no nulls
      jq '.people[] | select(has("first_name")) | .first_name'  data3.json # outputs a list of first names with no nulls
   ```
* Array index: .[\<index\>]
  * A filter that outputs the value of a specific element in an array.  Indexes start at 0
  ```
      jq '.people[0]' data5.json
      jq '.people | .[0]' data5.json
  ```
* Array or string slice: .[\<from index\>:\<to index\>]
  ```
      jq '.people[0:2]' data5.json
      jq '.people | .[0:2]' data5.json
  ```
* Array or Object value Iterator: .[]
  * For an array it will iterate over all of the elements and output each one.  The output is a list
  * For an object it will iterate over each key-value pair and output the value of each key.  The output is a list
  ```
      jq '.people | .[]' data5.json
  ```
* Array or Object value Iterator if : .[]?
  * like .[], but if the input is not an array or object there are no errors (there is no output)
  ```
      jq '.people[0].phone_numbers.home | .[]?' data5.json
  ```
* Pipe: |
  * Similar to the Unix pipe, it sends the output of the left filter to the input of the filter on the right
  * If the filter on the left produces multiple results, the the filter on the right will be run once for each result
  ```
      jq '.people[] | .first_name' data5.json      # output a list of first names
      jq '.people | .[] | .first_name' data5.json  # output a list of first names
      jq '[.people[] | .first_name]' data5.json    # output an array of first names
      jq '.people[] | keys' data5.json           # output a list of arrays containing the keys
      jq '.people[0] | keys | .[]' data5.json    # output a list of the top-level keys for the first object
      jq '[.people[0] | keys | .[]]' data5.json    # output an array of the top-level keys for the first object
      jq '[.people[0] | keys | .[]] | sort' data5.json    # output a sorted array of the top-level keys for the first object
      jq '.people[] | length' data5.json         # output a list of the number of top-level keys for each object in the people array
      jq '.people[0] | keys | .[] | length' data5.json  # output a list containing the length of each key for the first object
      jq '.people | length' data5.json           # output the number of objects in the people array
  ```
* Comma: ,
  * Concatenation: The same input is fed into each filter and the output is concatenated
  ```
      jq '.people[] | .first_name, .last_name' data5.json
  ```
* Parenthesis: ()
  * Parenthesis give evaluation precedence to what is enclosed, just like other programming languages
  ```
      jq '.people[] | .first_name, .last_name, (.age+1)' data5.json
  ```
* Object constructor: {}
  * Used to construct objects (dictionaries)
   ```
      jq '.people[] | {fname: .first_name, lname: .last_name}' data5.json # outputs a list of objects where each object has the first and last name
      jq '[.people[] | {fname: .first_name, lname: .last_name}[' data5.json # outputs an array of objects where each object has the first and last name
      jq '.people[] | {fname: .first_name, lname: .last_name, numbers: .phone_numbers}' data5.json # outputs a list of objects with a nested object of phone numbers
      jq '.people[] | {fname: .first_name, lname: .last_name, age_next_year: (.age+1)}' data5.json # outputs a list of objects with the age next year
      jq '[.people[] | {fname: .first_name, lname: .last_name, phnumbers: ([.phone_numbers[] | (if . != null then . else empty end)])}]' data5.json # outputs an array of objects with an array of phone numbers
   ```
* Array constructor: []
  * Used to construct arrays
  * Especially useful if the output of a filter is a list but you need an array for input to the next filter
   ```
      jq '.people | [.[].first_name]' data5.json # produces an array of first names
      jq '.people | [.[].age] | sort' data5.json # produces an array of sorted ages
      jq '.people | [.[].age] | sort | reverse' data5.json # produces an array of sorted ages oldest to youngest
      jq '.people | [.[].city] | unique' data5.json # produces an array of unique cities sorted
   ```
* Recursive Descent: ..
  * Descends through the structure producing every value
   ```
      jq '..' data5.json
      jq 'path(..)' data5.json # may be helpful in seeing the path
      jq '.. | .first_name?' data5.json # outputs a list of first names (not a very good example because the output has some null values)
      jq '.. | if (.first_name?) then .first_name else empty end' data5.json # outputs a list of first names
      jq 'map(.. | if (.first_name?) then .first_name else empty end)' data5.json # outputs an array of first names
      jq 'map(.. | if (.first_name?) then .first_name, .last_name else empty end)' data5.json # outputs an array of first and last names
      jq 'map(.. | if (.first_name?) then {fname: .first_name, lname: .last_name} else empty end)' data5.json # outputs an array of objects where each object contains the first and last name
   ```
* Some built-in operators and functions
  * Addition: +
    * The same input is fed into each filter and the output is "added" together depending on the type
      * numbers are added together
      * strings are concatenated
      * arrays are joined together
      * objects are merged
    ```
      jq '3 + 5' data5.json # ignores the input and outputs the number 8
      jq '"Hello " + "World!"' data5.json # ignores the input and outputs the string "Hello World!"
      jq '[1,2,3] + [3,4,5]' data5.json # ignores the input and outputs an array of six elements [1,2,3,3,4,5]
      jq '[1,2,3] + [1,2,3] | unique' data5.json # ignores the input and outputs an array of three elements (a union of the two input arrays)
      jq '{"fname": "John"} + {"lname": "Doe"}' data5.json # ignores the input and outputs one object containing the two keys "fname" and "lname"
    ```
  *  -, *, /, and %
  * length # get the length of the value
  * keys, keys_sorted # produce all the keys in an object
  * reverse # reverses the order of an array
  * has(key) # If an object has the key then returns true else false.  If an array has an element at that index return true
  * in(object|array) # if the input key is in the object then returns true else false.  If the input index has an element in the array return true.
  * map(filter) # Applies the filter to each element of the input array and return a new array.
    * map(filter) is equivalent to [.[] | filter]
    ```
      jq '.people | map(.first_name)' data5.json # produces an array of first names
      jq '.people | [.[] | .first_name]' data5.json # produces an array of first names
      jq '.people | .[] | [.first_name, .last_name, .age]' data5.json # produces a list of arrays
      jq -r '.people | .[] | [.first_name, .last_name, .age] | @csv' data5.json # produces a comma-separated values list with one record per line
      jq -r '.people | .[] | " \(.first_name),\(.last_name),\(.age)"' data5.json # produces a comma-separated values list with one record per line
      jq -r '.people[] | " \(.first_name),\(.last_name),\(.age)"' data5.json # produces a comma-separated values list with one record per line
      jq '.people | map(.first_name, .last_name)'  data5.json # produces an array of first names and last names, in order
      jq '.people | map({fname: .first_name, lname: .last_name})' data5.json # produces an array of objects.  Each object has two keys: "fname" and "lname".
      jq '.people | [.[] | {fname: .first_name, lname: .last_name}]' data5.json # produces an array of objects.  Each object has two keys: "fname" and "lname".
    ```
  * map_values(filter) # applies the filter to each elemnt but will return an object
  * path(path_expression) # produces an array of the path of each element
  * select(expression) # produces its input unchanged if the expression is true
    ```
      jq '.people[] | select(.first_name == "John")' data5.json # selects all objects where the first_name is John
      jq '[.people[] | select(.last_name == "Doe")]' data5.json # produces an array of all objects where the last_name is Doe
      jq '.people | map(select(.last_name == "Doe"))' data5.json # produces an array of all objects where the last_name is Doe
    ```
  * if A then B else C end # if A is true then applies the filter B otherwise applies the filter C
    * "else" is required.  The filter "empty" can be useful if you do not need an else.  emtpy outputs nothing
  * if A then B elif C else D end
    ```
      jq '.people[] | if (.first_name | length) > 3 then .first_name else empty  end' data5.json # produce a list of first names that are more than 3 characters
    ```
  * and/or/not # normal Boolean operators
      ```
      jq '.people[] | if (.first_name | length) > 3 and .last_name == "Doe" then .first_name else empty  end' data5.json # produce a list of first names that are more than 3 characters and whose last name is Doe
    ```
 
  * builtins
    * list the built-in fuctions
    ```
      jq 'builtins | length' data5.json # get the number of builtin functions
      jq 'builtins' data5.json # produce an array of builtin functions
      jq 'builtins | sort' data5.json # produce a sorted array of builtin functions
      jq 'builtins | sort[0:20]' data5.json # produce a sorted array of the first 20 builtin functions (after the sort)
    ```
    
