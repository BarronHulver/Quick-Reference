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
