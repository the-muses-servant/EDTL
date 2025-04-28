# Extremely Dense Token Language (EDTL)

The **Extremely Dense Token Language (EDTL)** is a compact, text-based format designed to represent prompts and requests with minimal whitespace and maximum information density. It uses type indicators and length prefixes to create a structured, parseable format, making it ideal for scenarios where efficiency is critical, such as data transmission in constrained environments or storage in space-limited systems.

EDTL is versatile and supports a wide range of prompt types, from simple instructions to complex requests with associated data. This repository provides the standard specification, examples, and tools for working with EDTL.

## Features

- **Compact Representation**: Minimizes whitespace and maximizes information density for efficient storage and transmission.
- **Structured Format**: Uses type indicators and length prefixes for reliable parsing and interpretation.
- **Versatile**: Supports diverse prompt types, from basic commands to complex data-rich requests.
- **Coding-Friendly**: Easy to generate and parse programmatically, with straightforward syntax.
- **Compatibility**: Works with AI models like **ChatGPT 4o** and **Grok** in preliminary testing, making it suitable for prompt engineering and AI-driven applications.

## Use Cases
- **API Requests**: Efficiently serialize and transmit requests in bandwidth-constrained environments.
- **Data Serialization**: Store structured data in a compact, human-readable format.
- **Prompt Engineering**: Represent prompts with optional data for AI models, optimizing for both simplicity and detail.
- **Embedded Systems**: Use in systems with limited storage or processing power due to its lightweight design.

## Getting Started
To start using EDTL, refer to the [EDTL Standard Specification](link-to-spec.md) for a detailed breakdown of the token types, prompt structures, and parsing guidelines.

### Example: Simple Instruction-Only Prompt
```
ps12Tell me a joke
```
- **Breakdown**:
  - `p`: Prompt token.
  - `s12Tell me a joke`: Instruction string "Tell me a joke".

### Example: Coding Prompt with Language and Requirements
```
ps28Write a function to sort an arrayo2s1ls10JavaScripts1ro2s1is5arrays1cs16O(n log n) time
```
- **Breakdown**:
  - `p`: Prompt token.
  - `s28Write a function to sort an array`: Instruction string.
  - `o2s1ls10JavaScripts1ro2s1is5arrays1cs16O(n log n) time`: Data object with:
    - `"l"`: "JavaScript" (language)
    - `"r"`: Requirements object with:
      - `"i"`: "array" (input)
      - `"c"`: "O(n log n) time" (constraint)

## Installation
Currently, EDTL is a specification. To use it in your projects, you can implement a parser based on the standard or use one of the community-contributed libraries (coming soon).

## Contributing
Contributions are welcome! Whether it's improving the specification, adding examples, or building tools and libraries, feel free to open issues or submit pull requests.


## License
This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
