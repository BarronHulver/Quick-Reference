# YAML Quick Reference

#### General notes
- YAML file names use the .yml or .yaml extension
- The YAML file optionally starts with three hyphens (---)
- The basic construct is a key-value pair
- A key-value pair has a colon between them: first-name: "John"
- The key is on the left
  - Best practice is to have the key be letters and underscores (no spaces)
- The value is on the right and is one of these 6 simple data types
  - strings - must be in double quotes
  - numeric - not quoted
    - integers (e.g. 238)
    - real numbers (e.g. 572.352)
    - hexadecimal (e.g. 0xf31a)
    - octal (e.g. 026)
    - some other types
  - objects/dictionaries
    - single line:  phone_numbers: { home: "440-229-7392", cell: "440-737-3983", work: "216-225-7352" }
     - multiple lines:
       -  phone_numbers: 
        home: "440-229-7392",
        cell: "440-737-3983",
        work: "216-225-7352"

  - arrays
    - can be specified on a single line: items: [ "Sun", "Mon", "Tues", "Wed", "Thurs", "Fri", "Sat" ]
    - can be specified on multiple lines
      - "Sun"
      - "Mon"
      - "Tues"
      - "Wed"
      - "Thurs"
      - "Fri"
      - "Sat"
  - boolean (True, Yes, or On; False, No, Off) - not quoted
  - null
  - timestamp
- key-value pairs are separated by commas.  Note that the last key-value pair will not have a comma
- Indentation does not matter.  Best practice is to make it readable so use 2-4 spaces
- A nested JSON object just means that for a key-value pair, the value is a JSON object
  - If a JSON object is not nested, it is also called a "flat" JSON object
- An array has just values (no keys).  An array is enclosed in square brackets []
