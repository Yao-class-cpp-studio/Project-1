# Project 1: JSON

<sup><sub>This project is based on [jsoncpp](https://github.com/open-source-parsers/jsoncpp) [1.9.5](https://github.com/open-source-parsers/jsoncpp/releases/tag/1.9.5) licensed under [MIT](https://spdx.org/licenses/MIT.html).</sub></sup>![codecov](https://codecov.io/gh/Yao-class-cpp-studio/project-1-huang-wj/branch/main/graph/badge.svg?token=WCxeJow4Ow)

## Introduction

> You can skip this section if you are familiar with JSON.

[JSON](https://www.json.org/) is probably the most popular data-interchange format in the world.
To see why it is needed, lets imagine a scenario.

You are developing an instant message app, OurChat, which can send and receive messages.
Alice sends a message "Hello World!" to Bob.
The message sent by the program would look like:

```
Hello World!
```

Now Bob wants to know when the message was sent.
Alice can add a timestamp to the message:

```
Hello World! (sent at 2019-01-01 12:00:00)
```

A new user, Charlie, starts using OurChat.
If Charlie also sends a message to Bob,
Bob want to know who sent the message.
You would need to add the sender's name to the message:

```
Hello World! (sent at 2019-01-01 12:00:00) - Charlie
```

The message is getting longer and longer.
The more information is added to the message,
the harder it is to read.
The receiver of the message would need to parse the message
to get the information without ambiguity.
This is where JSON comes in.

JSON is a simple formal language for representing structured data.
It defines six types of values:

- `null`: `null` represents the absence of a value.
- `boolean`: Either `true` or `false`.
- `number`: A signed decimal number. Examples: `2022`, `-1.5`, `1e9`.
- `string`: A sequence of characters. Examples: `"Hello World!"`.
- `array`: An ordered sequence of values (of any type). Examples: `[1, 2, "Hello", null]`.
- `object`: An unordered collection of key-value pairs.
  The keys have to be strings, but the values can be any type.
  Examples: `{"name": "Alice", "age": 20}`.

You might have noticed that JSON is not type-safe:
any value can appear in any place.
This is a natural consequence of the fact that JSON is a data-interchange format.
It is suitable for dynamic languages like JavaScript and python,
though it's a little bit tricky to use in static languages like C++.
In this project, we work on a C++ library for JSON
to explore how to design a type-safe dynamic library in C++.

## Tasks (max 100pts)

- [x] Maintenance (40%pts off if not done): Setup [Codecov](https://about.codecov.io/) for your repository to track the code coverage.
    - Check below for instructions.
- [x] C++ API (15pts): Define JSON directly in C++ with [User-defined literals](https://en.cppreference.com/w/cpp/language/user_literal).
    - Example: `R"([1,2,3])"_json`.
    - Use a string followed by `_json` to define JSON directly.
    - Don't forget to add `using Json::operator"" _json;` in order to use it, although I have written `JSON_API`.
- [x] C++ API (15pts): Support [user-defined conversion](https://en.cppreference.com/w/cpp/language/cast_operator) to primitive types.
    - Example: `static_cast<double>("1.23"_json)`.
    - Support conversion from `Value` with single data to `Int,UInt,Int64,UInt64,LargestInt,LargestUInt,Float,Double,String,CString,Bool`.
    - Conversion for `Array,Object` are `std::vector<Json::Value>,std::map<std::string,Json::Value>`, respectively. 
    - Support explicit conversion for value with single data for safety, and support implicit conversion for `Array,Object` value.
- [ ] Utility (15pts): Convert from and to [CSV Files](https://datatracker.ietf.org/doc/html/rfc4180).
    - An implementation as simple as comma-separated values suffices.
- [ ] Feature (30pts): Allow zero or one [trailing commas](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Trailing_commas) in JSON arrays and objects.
- [x] Feature (30pts): Support string concatenation.
    - Example: `{"na"+"me":"San"+"Zh"+"ang",["H"+"W",1]}` is equivalent to `{"name":"SanZhang",["HW",1]}`.
    - Concatenation in keys in objects, values in objects, values in arrays and single string are all supported.
    - You can use more than one `+` to concate strings into one.
    - Any spaces and endlines before and after each `+` are automatically ignored and skipped.
    - Support concatenation for string with both single quote (when `allowSingleQuotes_=true`) and double quote.
    - Support string concatenation when parsing both in new way (`CharReader`) and in old way (`Reader`).
- [ ] Feature (30pts): Add a new type `binary` to represent binary data.
- [ ] Feature (30pts): Support pointers to share JSON values.
- [ ] Utility (30pts): [Schema](https://json-schema.org/learn/) validation.
    - Only need to support the keys used in the [examples collection](https://json-schema.org/learn/).
    - URL-related keywords are not required, such as `$ref`.
- [ ] Utility (20pts + 10pts): Support [XPath](https://developer.mozilla.org/en-US/docs/Web/XPath)-like query to JSON.
    - Required path selector: `.` (current node), `..` (parent node), `*` (all children), `name` (child with name `name`), array index (e.g. `[0]`).
    - Overload `operator / ()` to support path selector. No need to parse the query string.
    - Extra 10pts if you can support assignment via the query.
- [ ] Utility (30pts): Support bundling and unbundling all the contents of a folder into a JSON file.
    - You might want to combine this task with the `binary` type.
    - Metadata of the files do not need to be preserved.
    - You might want to enable C++ 17 to use `std::filesystem` by `set(CMAKE_CXX_STANDARD 17)` in `CMakeLists.txt`.
- [ ] Performance (50pts): JSON minifier using pointers.
    - Minify the output size of JSON by removing unnecessary characters.
    - You probably want to combine this task with the task of pointer support and string concatenation.
- [x] Utility (50pts): Make a terminal UI for easy browsing & editing JSON.
    - [See README]
- [ ] Performance (50pts): Add indexing support for JSON to support random read on disk without loading the full file in memory.
    - The index should be stored in the same file.
    - The JSON file should still be able to be parsed correctly with your program.
- [ ] Other (50pts): Anything else you can think of.
    - The score is judged by usefulness, difficulty, and implementation quality.

As seen from the first task,
tests consist of 40% of the total score in this project.
Each implemented feature should contain **reasonable** tests in order to get full points.
The specific test requirements for each task are **not** revealed
since it is part of the project to design the API and the tests.

### Report

Please tick the checkbox here in `README.md` after finishing a task,
and briefly summarize what you did in the list above.

A clear and well-organized summary helps TAs to understand your work better.
If you don't think a detail is worth mentioning,
neither does the reader.
Sometimes a screenshot is worth a thousand words.

A bad example:

> - [x] Feature (10pts): Add a new type `date` to represent date and time.
>     - A C++ class `Date` is added to represent date information.
>     - Added a new switch branch in `json_reader` to parse date strings.
>     - Added a serialization function to `json_writer` to convert `Date` to string.
>     - Test: Input data with "data" field can be parsed correctly.
>     - Test: Converting JSON with "date" field to string outputs expected results.
>     - Test: Invalid date strings are rejected.
>     - Other tests are still passed after the changing the code.

A (too) good example:

> - [x] Feature (10pts): Add a new type `date` to represent date and time.
>     - Support date in ISO 8601 format.
>     - Distinguish date from string with quotes "`"`".
>     - Distinguish date from numbers using `isNumber()`.
>     - Test: Valid date strings are not parsed as numbers or strings.
>     - Test: Use substrings of valid date strings to generate erroneous date strings.

## Guides

### Project Structure

The core library is written in [`src/lib_json/`](src/lib_json)
and its header files are put in [`include/`](include/).
Users of the library only need to include the header files from [`include/`](include/).

The major part of the library consists of a JSON parser
(see [`src/lib_json/json_reader.cpp`](src/lib_json/json_reader.cpp))
and a JSON writer [`src/lib_json/json_writer.cpp`](src/lib_json/json_writer.cpp)
The JSON parser reads from a string (or a file) and generates a JSON value type in C++,
named `Value` in [`src/lib_json/json_reader.cpp`](src/lib_json/json_reader.cpp).
A few examples of using the reader and the parser are given in [`example/`](example/).

This project is much larger than previous assignments in terms of the code you need to read.
There are techniques to jump right in the library without reading the whole thing.
Some common techniques include:

* Don't read the whole file. Read the header files in [`include/`](include/) first.
* Only look at the relevant code of a variable / function.
Use the IDE's "Find Usages" feature, or simply do a global search.
* Analyze which parts of the project are relevant to the task you are working on,
  and change them accordingly.
  For example, the task `date` might involve:
    * Adding a new enum type in `include/value.h`
    * Adding corresponding token and `read*()` parser in `src/lib_json/json_reader.cpp`
    * Adding a `switch` case and `write*()` writer in `src/lib_json/json_writer.cpp`
* You might find that good encapsulation is important
since it allows you to modify a part of the code without even knowing
how the rest of the code works.
Therefore, try to keep encapsulation in mind when you are writing your part of the code.

For more usage of existing source code, see the original [Wiki](https://github.com/open-source-parsers/jsoncpp/wiki)
and [API document](http://open-source-parsers.github.io/jsoncpp-docs/doxygen/index.html).

### Running the Program

As always, you need a working C++ compiler and CMake installed on your system
to compile and run the source code.

We would like to clarify that this project is a *library*, but not an *executable*.
Therefore, there is no `main()` function in the core library,
and nothing for you to execute.
Instead, the project has a bunch of [tests](test/), [example programs](example/), and benchmarks
(which this project does not contain).

To write a program that uses this JSON library,
the headers must be included:

```cpp
#include <json/json.h>
int main() {
    // Some json operations
}
```

and a new entry needs to be added to `CMakeLists.txt`:

```cmake
add_executable(my_executable path/to/source/code.cpp)
target_link_libraries(my_executable jsoncpp_static)
```

in order to compile and link it with the JSON library.
Now a new target has been added to the project,
and you can find it in the list of executables,
as what we have done in the previous assignments (`*_test`).

### Testing

To do testing, Python3 is required.

Run test from "Testing"(测试) section on the top of Visual Studio,
or in the terminal:

```bash
cd build/ # Go to your build directory
ctest
```

### Adding a Reader/Writer Test

Tests are stored in `test/data`.
To add a test, you need to create two files:

* a `TESTNAME.json` file, that contains the input document in JSON format.
* a `TESTNAME.expected` file, that contains a flatened representation of the
  input document.

The `TESTNAME.expected` file format is as follows:

* Each line represents a JSON element of the element tree represented by the
  input document.
* Each line has two parts: the path to access the element separated from the
  element value by `=`. Array and object values are always empty (i.e.
  represented by either `[]` or `{}`).
* Element path `.` represents the root element, and is used to separate object
  members. `[N]` is used to specify the value of an array element at index `N`.

See the examples `test_complex_01.json` and `test_complex_01.expected` to better understand element paths.

When a test is run, output files are generated beside the input test files. Below is a short description of the content of each file:

* `test_complex_01.json`: input JSON document.
* `test_complex_01.expected`: flattened JSON element tree used to check if
  parsing was corrected.
* `test_complex_01.actual`: flattened JSON element tree produced by `jsontest`
  from reading `test_complex_01.json`.
* `test_complex_01.rewrite`: JSON document written by `jsontest` using the
  `Json::Value` parsed from `test_complex_01.json` and serialized using
  `Json::StyledWritter`.
* `test_complex_01.actual-rewrite`: flattened JSON element tree produced by
  `jsontest` from reading `test_complex_01.rewrite`.
* `test_complex_01.process-output`: `jsontest` output, typically useful for
  understanding parsing errors.

### Adding a Test for Custom Data Types

Under `src/jsontestrunner/main.cpp`,
the `printValueTree()` function is used to serialize the values in JSON,
in order to compare line-by-line in the `.expected` files.

### Setting-up Codecov

Codecov requires admin access from the organization so TA will open up access for you
**after you have accepted the homework in github classroom**.
This might take 2-3 days depending on the workload of the TA.

After the setup is done,
you can find your own badge in the [homework document page](https://yaoclasscpp.studio/assignments/project-1/).
Add the badge to your `README`!
Right-click the badge and select "Open image in new tab" to get the URL.
The markdown source should look like:

```markdown
![codecov](https://codecov.io/gh/Yao-class-cpp-studio/Project-1/branch/main/graph/badge.svg?token=JS6LK1XNFY)
```

If you want to view the coverage reports yourself,
fork this repository and play with Codecov.
You could also run locally with `gcov` after adding the following lines to `CMakeLists.txt`:

```cmake
add_compile_options(--coverage)
add_link_options(--coverage)
```

See [`.github/workflows/`](.github/workflows/) for reference.
Remember, **never show others your code or it will be considered plagiarism**.

### About the Parser

The parser in `src/lib_json/json_reader.cpp` is a recursive descent parser.
It starts from `Reader::readValue()`, and scans the string for input.
The `readToken()` function fetches a new token from the input string,
similar to `char c; cin >> c;`.
The difference is that tokens can be something longer than a single character,
such as the number token,
which reads in a whole number from the input string.

Depending on the first character encountered,
we can distinguish which types of values we are going to encounter in the stream.
For example,
if `{` is found, we know that it's going to be an object;
if `"` is found, we know that it's going to be a string.
Then it calls `readObject()` or `readString()` to continue reading the whole value.

Something interesting about `readObject()` and `readArray()` is that
it's a recursive structure.
For each element in the array,
it calls `readValue()` recursively to read the value inside,
no matter what type it is.
It only stops when it encounters a `]`.

To be more specific about the implementation,
`readValue()` consumes a few characters from the input string,
and store the value in the `Value` object
which the stack top of `OurReader::node_` points to.
Therefore, to use `readValue()` functions:

```cpp
Value val;
nodes_.push(&val);
bool successful = readValue();
nodes_.pop();
if (successful) {
    // val is correctly parsed!
}
```

And to write a `read*()` function:
```cpp
bool readSomething() {
    Value val;
    if (*current_ == 'a') {
        // this is the current character
    }
    if (*(current + 1) == 'b') {
        // this is the next character
    }
    current += 1; // Only do this if you decide that (*current) is part of this value!
    // Do something more to get val
    currentValue().swapPayload(val);
}
```

Finally, add it to a branch in `Reader::readValue()` and `Reader::readToken()`.

### About the Writer

Writers are much simpler.
Add a new case in `Writer::writeValue()`, write a corresponding function,
and you are set.
The only caveat is that there are two writers,
so you might or might not need to implement both of them.
