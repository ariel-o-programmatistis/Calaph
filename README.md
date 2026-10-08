# Calaph

**Calaph** is a numerical language and symbolic encoding system implemented in **C**. It represents information through numerical values, primitive symbols, and arithmetic composition, providing a structured way to work with text and other encoding systems from the terminal.

Calaph is designed as a lightweight, portable, and dependency-free command-line application. Its primary implementation is written in C and is intended to compile across Linux, FreeBSD, macOS, and Windows environments.

> **Note:** Calaph is a numerical language and symbolic representation system, not a cryptographically secure encryption algorithm.

## Features

* Native C implementation
* Lightweight terminal application
* No external runtime libraries
* Text-to-Calaph conversion
* Calaph-to-text conversion
* Calaph-to-binary conversion
* Binary-to-Calaph conversion
* Calaph-to-hexadecimal conversion
* Hexadecimal-to-Calaph conversion
* Calaph-to-Morse conversion
* Morse-to-Calaph conversion
* Latin alphabet support
* Spanish `Ñ` support
* Greek alphabet support
* Cyrillic alphabet support
* Numerical expressions
* Addition and multiplication
* Decimal notation
* ANSI terminal interface
* Input validation
* Portable source code

## Calaph Numerical System

Calaph is built around a small set of primitive symbols and operations:

| Symbol | Meaning                    |
| ------ | -------------------------- |
| `.`    | 1                          |
| `,`    | 3                          |
| `<>`   | 0                          |
| `'`    | Multiplication             |
| Space  | Addition                   |
| `\|`   | Decimal separator          |
| `\`    | Fractional digit separator |

The primitive symbols can be combined to construct larger numerical values.

### Primitive Values

```text
.       = 1
.,      = 4
,.      = 4
.,,     = 5
,,      = 6
,,.     = 7
.,,,    = 8
,,,     = 9
,,,.    = 10
<>      = 0
```

Different expressions may represent the same numerical value.

For example:

```text
.,      = 4
,.      = 4
....    = 4
```

### Addition

A space separates additive terms.

```text
,, ,,. = 6 + 7 = 13
```

### Multiplication

The apostrophe character represents multiplication.

```text
,', = 3 × 3 = 9
```

This allows Calaph to construct larger numerical values through combinations of primitive values, addition, and multiplication.

The current system defines the following preferred representation:

```text
.,',,. = 50
```

## Decimal Notation

Calaph uses `|` to separate the integer portion from the fractional portion and `\` to separate individual fractional digits.

For example:

```text
,|.\,.
```

represents:

```text
3.14
```

Zero is represented by:

```text
<>
```

## Character Mapping

Calaph associates supported characters with numerical values.

### Latin Alphabet

The Latin alphabet occupies values `1–27`, including Spanish `Ñ`.

```text
A  = 1
B  = 2
C  = 3
D  = 4
E  = 5
F  = 6
G  = 7
H  = 8
I  = 9
J  = 10
K  = 11
L  = 12
M  = 13
N  = 14
Ñ  = 15
O  = 16
P  = 17
Q  = 18
R  = 19
S  = 20
T  = 21
U  = 22
V  = 23
W  = 24
X  = 25
Y  = 26
Z  = 27
```

### Greek Alphabet

The Greek alphabet occupies values `28–51`.

```text
Α = 28
Β = 29
Γ = 30
Δ = 31
Ε = 32
Ζ = 33
Η = 34
Θ = 35
Ι = 36
Κ = 37
Λ = 38
Μ = 39
Ν = 40
Ξ = 41
Ο = 42
Π = 43
Ρ = 44
Σ = 45
Τ = 46
Υ = 47
Φ = 48
Χ = 49
Ψ = 50
Ω = 51
```

### Cyrillic Alphabet

The Russian Cyrillic alphabet occupies values `52–84`.

```text
А = 52
Б = 53
В = 54
Г = 55
Д = 56
Е = 57
Ё = 58
Ж = 59
З = 60
И = 61
Й = 62
К = 63
Л = 64
М = 65
Н = 66
О = 67
П = 68
Р = 69
С = 70
Т = 71
У = 72
Ф = 73
Х = 74
Ц = 75
Ч = 76
Ш = 77
Щ = 78
Ъ = 79
Ы = 80
Ь = 81
Э = 82
Ю = 83
Я = 84
```

## Text Mode

Text Mode provides a direct interface for working with ordinary text and Calaph.

```text
TEXT
  |
  v
CHARACTER MAPPING
  |
  v
NUMERICAL VALUE
  |
  v
CALAPH
```

The reverse operation evaluates Calaph expressions and converts the resulting numerical values back into characters.

```text
CALAPH
  |
  v
NUMERICAL PARSER
  |
  v
NUMERICAL VALUE
  |
  v
CHARACTER MAPPING
  |
  v
TEXT
```

### Available Operations

```text
[ 1 ] Encrypt text to Calaph
[ 2 ] Decrypt Calaph to text
[ 3 ] Return to main menu
```

## Code Mode

Code Mode provides bidirectional conversion between Calaph and several conventional encoding systems.

```text
                    CALAPH
                      |
          +-----------+-----------+
          |           |           |
          v           v           v
       BINARY       HEXADECIMAL  MORSE
          ^           ^           ^
          |           |           |
          +-----------+-----------+
                      |
                    CALAPH
```

### Available Operations

```text
[ 1 ] CALAPH → BINARY
[ 2 ] BINARY → CALAPH

[ 3 ] CALAPH → HEXADECIMAL
[ 4 ] HEXADECIMAL → CALAPH

[ 5 ] CALAPH → MORSE
[ 6 ] MORSE → CALAPH

[ 7 ] Return to main menu
```

## Binary

Calaph numerical values can be represented as 8-bit binary values.

Example:

```text
5 = 00000101
```

Binary input is validated before conversion.

Text spaces are represented by the byte:

```text
00100000
```

## Hexadecimal

Calaph numerical values can be represented using hexadecimal notation.

Examples:

```text
5  = 05
10 = 0A
15 = 0F
```

Text spaces are represented by:

```text
20
```

Hexadecimal input is validated before conversion to Calaph.

## Morse

Calaph supports conversion to and from conventional Morse code for supported Latin letters and decimal digits.

Examples:

```text
A = .-
B = -...
C = -.-.
D = -..
E = .
```

Digits are represented using standard Morse notation:

```text
0 = -----
1 = .----
2 = ..---
3 = ...--
4 = ....-
5 = .....
6 = -....
7 = --...
8 = ---..
9 = ----.
```

Characters outside the supported Morse character set are reported as unsupported.

## Architecture

The C implementation is organized around several functional components:

```text
Calaph
|
+-- Terminal Utilities
|
+-- Visual Interface
|   +-- Headers
|   +-- Borders
|   +-- Menus
|   +-- Output Formatting
|
+-- Numerical System
|   +-- Primitive Encoding
|   +-- Primitive Parsing
|   +-- Expression Evaluation
|   +-- Integer Encoding
|
+-- Character System
|   +-- Latin
|   +-- Greek
|   +-- Cyrillic
|
+-- Text Conversion
|   +-- Text → Calaph
|   +-- Calaph → Text
|
+-- Code Conversion
    +-- Calaph ↔ Binary
    +-- Calaph ↔ Hexadecimal
    +-- Calaph ↔ Morse
```

The core implementation is maintained as a single C source file to keep the language easy to inspect, compile, modify, and extend.

## Requirements

Calaph requires:

* A C compiler supporting C99 or later
* The standard C library
* A terminal capable of displaying ANSI escape sequences
* UTF-8 locale support for extended character sets

Recommended compilers include:

* GCC
* Clang
* MinGW-w64 GCC

No external libraries are required.

## Installation

### Debian / Ubuntu / Linux Mint

Install the required tools:

```bash
sudo apt update
sudo apt install build-essential git
```

Clone the repository:

```bash
git clone https://github.com/ariel-o-programmatistis/Calaph.git
cd Calaph
```

Compile:

```bash
gcc -x c -Wall -Wextra -O2 "calaph c" -o calaph
```

Run:

```bash
./calaph
```

To install Calaph system-wide:

```bash
sudo install -m 755 calaph /usr/local/bin/calaph
```

Then run:

```bash
calaph
```

### Fedora

Install GCC and Git:

```bash
sudo dnf install gcc git
```

Clone and compile:

```bash
git clone https://github.com/ariel-o-programmatistis/Calaph.git
cd Calaph
gcc -x c -Wall -Wextra -O2 "calaph c" -o calaph
```

Run:

```bash
./calaph
```

Optional system installation:

```bash
sudo install -m 755 calaph /usr/local/bin/calaph
```

### Arch Linux / Manjaro

Install the development tools:

```bash
sudo pacman -S base-devel git
```

Clone and compile:

```bash
git clone https://github.com/ariel-o-programmatistis/Calaph.git
cd Calaph
gcc -x c -Wall -Wextra -O2 "calaph c" -o calaph
```

Run:

```bash
./calaph
```

Optional system installation:

```bash
sudo install -m 755 calaph /usr/local/bin/calaph
```

### openSUSE

Install GCC and Git:

```bash
sudo zypper install gcc git
```

Clone and compile:

```bash
git clone https://github.com/ariel-o-programmatistis/Calaph.git
cd Calaph
gcc -x c -Wall -Wextra -O2 "calaph c" -o calaph
```

Run:

```bash
./calaph
```

### Alpine Linux

Install the development tools:

```bash
sudo apk add build-base git
```

Clone and compile:

```bash
git clone https://github.com/ariel-o-programmatistis/Calaph.git
cd Calaph
gcc -x c -Wall -Wextra -O2 "calaph c" -o calaph
```

Run:

```bash
./calaph
```

### Gentoo

Install GCC and Git:

```bash
sudo emerge --ask sys-devel/gcc dev-vcs/git
```

Clone and compile:

```bash
git clone https://github.com/ariel-o-programmatistis/Calaph.git
cd Calaph
gcc -x c -Wall -Wextra -O2 "calaph c" -o calaph
```

Run:

```bash
./calaph
```

## FreeBSD

Install Git and GCC:

```bash
sudo pkg install git gcc
```

Clone the repository:

```bash
git clone https://github.com/ariel-o-programmatistis/Calaph.git
cd Calaph
```

Compile:

```bash
gcc -x c -Wall -Wextra -O2 "calaph c" -o calaph
```

Run:

```bash
./calaph
```

Alternatively, FreeBSD's system compiler can be used:

```bash
cc -x c -Wall -Wextra -O2 "calaph c" -o calaph
```

Install system-wide:

```bash
sudo install -m 755 calaph /usr/local/bin/calaph
```

## macOS

Install Apple's Command Line Tools:

```bash
xcode-select --install
```

Clone the repository:

```bash
git clone https://github.com/ariel-o-programmatistis/Calaph.git
cd Calaph
```

Compile using Clang:

```bash
clang -x c -Wall -Wextra -O2 "calaph c" -o calaph
```

Run:

```bash
./calaph
```

Optional installation:

```bash
sudo install -m 755 calaph /usr/local/bin/calaph
```

Then:

```bash
calaph
```

## Windows

Calaph can be compiled on Windows using a GCC-compatible environment such as **MSYS2** or **MinGW-w64**.

### MSYS2

Install MSYS2 and open the **UCRT64** terminal.

Update the system:

```bash
pacman -Syu
```

Install GCC and Git:

```bash
pacman -S --needed mingw-w64-ucrt-x86_64-gcc git
```

Clone the repository:

```bash
git clone https://github.com/ariel-o-programmatistis/Calaph.git
cd Calaph
```

Compile:

```bash
gcc -Wall -Wextra -O2 "calaph c" -o calaph.exe
```

Run:

```bash
./calaph.exe
```

### MinGW-w64

With MinGW-w64 configured:

```bash
git clone https://github.com/ariel-o-programmatistis/Calaph.git
cd Calaph
gcc -Wall -Wextra -O2 "calaph c" -o calaph.exe
```

Run:

```bash
calaph.exe
```

The resulting executable can be placed in a directory included in the Windows `PATH`.

## Generic Compilation

If GCC is already installed:

```bash
gcc -x c -Wall -Wextra -O2 "calaph c" -o calaph
```

With Clang:

```bash
clang -x c -Wall -Wextra -O2 "calaph c" -o calaph
```

For strict compiler diagnostics:

```bash
gcc -x c -Wall -Wextra -Wpedantic -O2 "calaph c" -o calaph
```

For debugging:

```bash
gcc -x c -Wall -Wextra -Wpedantic -O0 -g "calaph c" -o calaph
```

## Usage

After launching Calaph, the main menu provides two working modes:

```text
[ 1 ] WORK WITH TEXT
[ 2 ] WORK WITH CODES
[ 3 ] EXIT
```

### Text Mode

```text
[ 1 ] Encrypt text to Calaph
[ 2 ] Decrypt Calaph to text
[ 3 ] Return to main menu
```

### Code Mode

```text
[ 1 ] CALAPH → BINARY
[ 2 ] BINARY → CALAPH

[ 3 ] CALAPH → HEXADECIMAL
[ 4 ] HEXADECIMAL → CALAPH

[ 5 ] CALAPH → MORSE
[ 6 ] MORSE → CALAPH

[ 7 ] Return to main menu
```

## Development

The recommended development build is:

```bash
gcc -x c -Wall -Wextra -Wpedantic -O2 "calaph c" -o calaph
```

For debugging:

```bash
gcc -x c -Wall -Wextra -Wpedantic -O0 -g "calaph c" -o calaph
```

The project is intentionally maintained as a compact C implementation so that the numerical system remains accessible to developers and can be extended without introducing unnecessary dependencies.

## Project Structure

```text
Calaph/
├── calaph c
├── calaph bash
└── README.md
```

The C implementation is the primary native implementation of Calaph.

## Portability

Calaph is designed to operate across multiple platforms using a standard C toolchain.

Supported environments include:

* Linux
* FreeBSD
* macOS
* Windows through MSYS2 or MinGW-w64

The core implementation does not require external libraries or a language runtime.

## Project Status

Calaph is under active development. The C implementation is the primary foundation of the project, with development focused on numerical consistency, parser reliability, portability, terminal usability, and expansion of the Calaph language.

## Repository

[Calaph on GitHub](https://github.com/ariel-o-programmatistis/Calaph)


