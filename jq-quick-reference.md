#### JQ Quick Reference

JSON data types: string, number, object (dictionary), array, boolean, null

Manual:
   ```
   https://stedolan.github.io/jq/manual/
   https://github.com/stedolan/jq/wiki/jq-Language-Description
   ```

* Identity: .
    * Identity filter that passes input to its output unchanged
   ```
      jq '.'
   ```
  
* Object Identifer-Index: .\<key\> or .\<key\>.\<key\> 
    * If the key has special characters or starts with a digit: ."key"
  ```
      jq '.last_name' data3.json
      jq '."last_name"' data3.json
  ```
* Optional Object Identifer-Index: .\<key\>?
  ```
      jq '.middle_name?'  data3.json
   ```
* Array index: .[\<index\>]
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
  * For an array it will return all elements of an array as a list
  * For an object it will return all the values as a list
  ```
      jq '.people | .[]' data5.json
  ```
* Array or Object value Iterator if : .[]?
  * like .[], but if the input is not an array or object there are no errors
  ```
      jq '.people[0].phone_numbers.home | .[]?' data5.json
  ```
* Pipe: |
  * Similar to the Unix pipe, it sends the output of the left filter to the input on the right
  ```
      jq '.people[] | .first_name' data5.json    # print a list of first names
      jq '.people[] | keys' data5.json           # print an array of the keys
      jq '.people[0] | keys | .[]' data5.json    # print a list of the top-level keys for the first object
      jq '.people[] | length' data5.json         # get the number of top-level keys for each object in the people array
      jq '.people[0] | keys | .[] | length' data5.json  # show the length of each key for the first object
      jq '.people | length' data5.json           # get the number of objects in the people array
  ```
* Comma: ,
  * Concatenation: The same input is fed into each filter and the output is concatenated
  ```
      jq '.people[] | .first_name, .last_name' data5.json
  ```
* Parenthesis: ()
  * Parenthesis give evaluation precedence to what is enclosed, just like other programming languages
  ```
      jq '.people[] | .first_name, (.age+1)' data5.json
  ```
* Object constructor: {}
  * Used to construct objects (dictionaries)
   ```
      jq '.people[] | {fname: .first_name, lname: .last_name, agenextyear: (.age+1)}' data5.json
   ```
* Array constructor: []
  * Used to construct arrays
   ```
      jq '.people | [.[].first_name]' data5.json # produces an array of first names
      jq '.people | [.[].age] | sort' data5.json # produces an array of sorted ages
      jq '.people | [.[].age] | sort | reverse' data5.json # produces an array of sorted ages oldest to youngest
      jq '.people | [.[].city] | unique' data5.json # produces an array of unique cities sorted
   ```
* Recursive Descent: ..
  * Descends through the structure producing every value
   ```
      jq '.. | .first_name?' data5.json # produces a list of first names (not a very good example)
      jq 'path(..)' data5.json # may be helpful in seeing the path
   ```
* Some built-in operators and functions
  * +, -, *, /, and %
  * length # get the length of the value
  * keys, keys_sorted # produce all the keys in an object
  * reverse # reverses the order of an array
  * has(key) # If an object has the key then returns true else false.  If an array has an element at that index return true
  * in(object|array) # if the input key is in the object then returns true else false.  If the input index has an element in the array return true.
  * map(filter) # Applies the filter to each element of the input array and return a new array.  Map(filter) is equivalent to [.[] | filter]
    ```
      jq '.people | map(.first_name)' data5.json # produces an array of first names
      jq '.people | [.[] | .first_name]' data5.json # produces an array of first names
      jq '.people | .[] | [.first_name, .last_name, .age]' data5.json # produces a list of arrays
      jq -r '.people | .[] | [.first_name, .last_name, .age] | @csv' data5.json # produces a comma-separated values list with one record per line
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
  * if A then B else C end # produces its input unchanged if the expression is true
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
    
