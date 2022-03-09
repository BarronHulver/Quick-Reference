#### JQ Quick Reference

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
  * Parenthesis give evaluation priority to what is enclosed, just like other programming languages
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
      jq '.people | [.[].first_name]' data5.json. # produces an array of first names
   ```
