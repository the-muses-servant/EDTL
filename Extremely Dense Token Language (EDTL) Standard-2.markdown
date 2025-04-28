# Extremely Dense Token Language (EDTL) Standard

## Introduction
The **Extremely Dense Token Language (EDTL)** is a compact, text-based format designed to represent prompts and requests with minimal whitespace and maximum information density. It uses type indicators and length prefixes to create a structured, parseable format, making it ideal for scenarios where efficiency is critical, such as data transmission in constrained environments or storage in space-limited systems.

EDTL is versatile and supports a wide range of prompt types, from simple instructions to complex requests with associated data. This document outlines the original EDTL standard, its optimized structure for prompts, and an extension specifically for coding-related prompts.

---

## Original EDTL Standard

### Token Types
EDTL supports the following token types, each with a specific type indicator:

- **Primitive Types**:
  - **Integer**: `i<number>`  
    - Example: `i10` (represents 10)
  - **Float**: `f<number>`  
    - Example: `f3.14` (represents 3.14)
  - **Boolean**: `b1` (true) or `b0` (false)  
    - Examples: `b1` (true), `b0` (false)
  - **Null**: `n`  
    - Example: `n` (represents null)
  - **String**: `s<length><string>`  
    - Example: `s5hello` (represents "hello")  
    - **Note**: Strings are assumed to be encoded in UTF-8 to support a wide range of characters while maintaining efficiency.

- **Compound Types**:
  - **Array**: `a<length><token1><token2>...<tokenN>`  
    - Example: `a3i1i2i3` (represents [1, 2, 3])
  - **Object**: `o<length><key1><value1><key2><value2>...<keyN><valueN>`  
    - Example: `o2s4names5Alices3agei30` (represents {"name": "Alice", "age": 30})

---

### Handling Specialized Data Types
For data types not directly supported as primitives, EDTL uses **compound types** (such as objects) to represent them efficiently. This approach maintains the language's simplicity and density while providing flexibility for complex use cases.

- **Complex Numbers**: Represented as an object with "real" and "imag" keys, both containing float values.  
  - Example: `o2s4realf1.5s4imagf2.0` (represents {"real": 1.5, "imag": 2.0})

- **High-Precision Numbers**: Represented as strings or objects to avoid precision loss.  
  - Example: `s510.123` (represents "10.123")  
  - Example: `o1s7decimalss510.123` (represents {"decimals": "10.123"})

- **Strings with Metadata**: Use objects to include encoding, formatting, or other attributes.  
  - Example: `o2s7encodings5UTF-8s5texts5hello` (represents {"encoding": "UTF-8", "text": "hello"})  
  - Example: `o2s4types9multilines5texts14Line1\nLine2` (represents {"type": "multiline", "text": "Line1\nLine2"})

This method ensures that EDTL can handle a wide variety of data types without complicating its core design.

---

### Prompt Representation
In the original EDTL standard, every prompt is represented as an object token with one or two key-value pairs:
- **Instruction ("i")**: A string token that captures the main request or instruction. This field is always present.
- **Data ("d")**: An optional token that holds any associated data relevant to the instruction. This can be any supported token type (primitive, array, object, etc.).

#### Object Structure
- If the prompt contains only an instruction, the object will have one key-value pair (for "i").
- If the prompt includes both an instruction and data, the object will have two key-value pairs (for "i" and "d").

---

## Optimized EDTL for Prompts
To make EDTL more efficient for prompt-based use cases, an optimized structure was introduced using a dedicated prompt token starting with `p`. This token implicitly includes an instruction string and optional data, eliminating the need for explicit key-value pairs in every prompt.

### Prompt Token Syntax
- **Format**: `p<instruction_string_token>[<data_token>]`
  - `p`: Indicates a prompt token.
  - `<instruction_string_token>`: A mandatory string token (`s<length><string>`) representing the instruction.
  - `<data_token>`: An optional token of any type (primitive, array, object, etc.) representing associated data.

### Benefits
- **Reduced Overhead**: The "p" token significantly reduces the character count compared to the original object-based representation.
  - Example: Original format for an instruction-only prompt: `o1s1is12Tell me a joke` (29 characters). Optimized: `ps12Tell me a joke` (16 characters).
- **Simpler Parsing**: The parser can directly expect a string token after `p`, followed by an optional data token, making it faster and more predictable.

---

## Extension for Coding-Related Prompts
For coding-related prompts, the optional data token in the prompt structure is an object that includes specific keys to denote coding requirements, such as the programming language and project details.

### Structure for Coding Prompts
- **Prompt Token**: `p<instruction_string_token><data_object_token>`
  - `<instruction_string_token>`: The coding task instruction (e.g., "Write a function to sort an array").
  - `<data_object_token>`: An object token (`o<length><key1><value1>...<keyN><valueN>`) containing coding-specific details.

### Keys in the Data Object
The data object for coding prompts includes the following keys:
- **"l" (Language)**: A string token specifying the desired programming language.
  - Example: `s6Python` for Python.
- **"r" (Requirements)**: A token (typically an object or array) specifying additional requirements for the coding task.
  - Can include sub-keys such as:
    - **"i" (Input)**: Specifies the input format or data.
    - **"o" (Output)**: Specifies the expected output format.
    - **"c" (Constraints)**: Lists any constraints or conditions (e.g., performance requirements).
- **Additional Keys**: The data object is flexible and can include other keys as needed for specific use cases.

---

## Examples

### 1. Instruction-Only Prompt (General)
- **Prompt**: "Tell me a joke"
- **EDTL Token**: `ps12Tell me a joke`
- **Breakdown**:
  - `p`: Prompt token.
  - `s12Tell me a joke`: Instruction string "Tell me a joke".

### 2. Prompt with Data (Array)
- **Prompt**: "Calculate the sum of [1, 2, 3]"
- **EDTL Token**: `ps15Calculate the suma3i1i2i3`
- **Breakdown**:
  - `p`: Prompt token.
  - `s15Calculate the sum`: Instruction string.
  - `a3i1i2i3`: Data, an array of integers [1, 2, 3].

### 3. Coding Prompt with Language and Requirements
- **Prompt**: "Write a function to sort an array in JavaScript with time complexity O(n log n)"
- **EDTL Token**: `ps28Write a function to sort an arrayo2s1ls10JavaScripts1ro2s1is5arrays1cs16O(n log n) time`
- **Breakdown**:
  - `p`: Prompt token.
  - `s28Write a function to sort an array`: Instruction string.
  - `o2s1ls10JavaScripts1ro2s1is5arrays1cs16O(n log n) time`: Data object with:
    - `"l"`: "JavaScript"
    - `"r"`: Requirements object with:
      - `"i"`: "array" (input)
      - `"c"`: "O(n log n) time" (constraint)

### 4. Complex Coding Prompt with Multiple Requirements
- **Prompt**: "Develop a REST API in Python that handles user authentication and data encryption"
- **EDTL Token**: `ps45Develop a REST API that handles user authentication and data encryptiono2s1ls6Pythons1ra2s19user authentications14data encryption`
- **Breakdown**:
  - `p`: Prompt token.
  - `s45Develop a REST API that handles user authentication and data encryption`: Instruction string.
  - `o2s1ls6Pythons1ra2s19user authentications14data encryption`: Data object with:
    - `"l"`: "Python"
    - `"r"`: Array of requirements ["user authentication", "data encryption"]

---

## Parsing Guidelines
To parse an EDTL token correctly:

1. **Identify the Token Type**:
   - Read the first character to determine the token type (e.g., `p` for prompt, `o` for object, `a` for array, etc.).

2. **For Prompt Tokens (`p`)**:
   - Read the next token as a string (`s<length><string>`), which is the instruction.
   - If more characters follow, parse the next token as the data (which could be any type, including an object for coding prompts).

3. **For Objects (`o`)**:
   - Read the length (number of key-value pairs).
   - Parse each key (string token) and its corresponding value token.

4. **For Arrays (`a`)**:
   - Read the length (number of elements).
   - Parse each element token sequentially.

5. **For Strings (`s`)**:
   - Read the length and then consume exactly that many characters as the string value.

6. **For Primitives**:
   - Read the value directly following the type indicator (e.g., `i10`, `f3.14`, `b1`, `n`).

The use of length prefixes ensures that the parser knows exactly how much data to read for each token, making the format unambiguous and efficient.

---

## Conclusion
The **Extremely Dense Token Language (EDTL)** provides a highly efficient and structured way to represent both general and coding-related prompts. Its compactness, flexibility, and ease of parsing make it ideal for use cases such as API requests, data serialization, or embedded systems. By leveraging compound types for specialized data, EDTL maintains its core simplicity while supporting a wide range of applications.