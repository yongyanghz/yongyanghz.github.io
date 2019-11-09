---
layout: post
little: C++ String Types Summaries
categories: [C++]
---

## Strings in C Standard

- **`char*`**

There's no string type in C language, the basic type is char. We can use `char*`to represent a string.

One `char` takes 1 byte in memory, which can represent `2^8 = 256` different values.

- **`wchar_t`**

`wchar_t` is defined for the C standard library and it is compiler-dependent, therefore not very portable.  



From [http://icu-project.org/docs/papers/unicode_wchar_t.html](http://icu-project.org/docs/papers/unicode_wchar_t.html):



*`wchar_t` is compiler-dependent and therefore not very  portable. Using it for Unicode binds a program to the character model of a compiler. Instead, it is often better to define and use dedicated  data types.*



`wchar_t` takes 2 bytes in memory, it can represent `2*16=65536` different values, which suitable for large character sets.



The [Unicode](https://en.wikipedia.org/wiki/Unicode) standard defins UTF-8, defines [UTF-8](https://en.wikipedia.org/wiki/UTF-8), [UTF-16](https://en.wikipedia.org/wiki/UTF-16), and [UTF-32](https://en.wikipedia.org/wiki/UTF-32), and several other encodings.

UTF-8, uses one byte for the first 128 code points, and up to 4 bytes for other characters.



Non-Unicode is applied in less character set, like English, requires 1 byte per character.



- **`const char *`  and `char * const`**

From book "Effective C++: 55 Specific Ways to Improve Your Programs and Designs By Scott Meyan " Item 3,

For pointers, the `const` keyword can specify whether the pointer is `const` or the data it points to is `const`.

The syntax of this is, if the word `const` is at the left side of the asterisk,  what's  the pointer pointed to is constant; If the word `const` is at the right side of the asterisk, the pointer it self is constant.



`const char *` means the pointer is non-const,  the char  data it points to is const;



`char * const` means the pointer is const, the string data it points to is non-const.



## Strings in std name space

- **`std::string`**

`typedef basic_string<char> string;`

Strings are objects that represent sequences of characters.

- **`std::wstring`**

`typedef basic_string<wchar_t> wstring;`

String class for wide character.



## Strings on Visual C++ compiler

- **`WCHAR`**

A 16-bit Unicode character.

`WCHAR`'s definition is as follows:

```cpp
#if !defined(_NATIVE_WCHAR_T_DEFINED)
    typedef unsigned short WCHAR;
    #else
    typedef wchar_t WCHAR;
    #endif
```

[https://docs.microsoft.com/en-us/windows/win32/extensible-storage-engine/wchar](https://docs.microsoft.com/en-us/windows/win32/extensible-storage-engine/wchar)

- **`CString`**

 A `CString` object supports either the **char** type or the `wchar_t` type, depending on whether the MBCS symbol or the UNICODE symbol is defined at compile time.

[https://docs.microsoft.com/en-us/cpp/atl-mfc-shared/using-cstring?view=vs-2019](https://docs.microsoft.com/en-us/cpp/atl-mfc-shared/using-cstring?view=vs-2019)


- **`CStringA`**

A `CStringA` object contains the **char** type, and supports single-byte and multi-byte (MBCS) strings.


- **`CStringW`**

A `CStringW` object contains the **wchar_t** type and supports Unicode strings. 


- **`LPCSTR`**

An LPCSTR is a 32-bit pointer to a constant null-terminated string of 8-bit Windows ([ANSI](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-dtyp/a66edeb1-52a0-4d64-a93b-2f5c833d7d92#gt_100cd8a6-5cb1-4895-9de6-e4a3c224a583)) characters.

`LPCSTR` is declared as follows:

```cpp
typedef const char* LPCSTR
```

- **`LPCTSTR`**

`LPCSTR` is a pointer to  a `const TCHAR` string. (`TCHAR` being either a wide char or char depending on whether UNICODE is defined in your project)


- **`LPTSTR`**

`LPTST` is a pointer to a non-const `TCHAR` string


- **`_T`**

`_T` stands for “text”. It will turn your literal into a Unicode wide character literal if and only if you are compiling your sources with Unicode support.

#### Generic-Text Data Type Mappings

| Generic-Text Data Type Name | _UNICODE & _MBCS Not Defined        | _MBCS Defined                       | _UNICODE Defined                                             |
| :-------------------------- | :---------------------------------- | :---------------------------------- | :----------------------------------------------------------- |
| `_TCHAR`                    | **char**                            | **char**                            | **wchar_t**                                                  |
| `_TINT`                     | **int**                             | **unsigned int**                    | `wint_t`                                                     |
| `_TSCHAR`                   | **signed char**                     | **signed char**                     | **wchar_t**                                                  |
| `_TUCHAR`                   | **unsigned char**                   | **unsigned char**                   | **wchar_t**                                                  |
| `_TXCHAR`                   | **char**                            | **unsigned char**                   | **wchar_t**                                                  |
| `_T` or `_TEXT`             | No effect (removed by preprocessor) | No effect (removed by preprocessor) | `L` (converts the following character or string to its Unicode counterpart) |

[https://docs.microsoft.com/en-us/cpp/text/generic-text-mappings-in-tchar-h?redirectedfrom=MSDN&view=vs-2019](https://docs.microsoft.com/en-us/cpp/text/generic-text-mappings-in-tchar-h?redirectedfrom=MSDN&view=vs-2019)

## Convert From Each Other

Common string types transform from each other.

- **`char*` to `wchar_t*`**

  From [stackoverflow](https://stackoverflow.com/questions/8032080/how-to-convert-char-to-wchar-t), use `std::wstring` instead of a C99 variable length array.  cause C++ does not support C99 variable length arrays,  the current standard guarantees a contiguous buffer for `std::basic_string`.

  ```cpp
  std::wstring CharPt2WcharPt(const char* ch){
  	const size_t ch_size = strlen(ch) + 1; // strlen does not include the terminating null-character 
  	std::wstring wc(ch_size, L'#'); // L is the prefix for wide character literals
  	mbstowcs(&wc[0], ch, ch_size); // Converts a multibyte character string to wide string, given state
  	return wc;
  }
  
  ```

- **`wchar_t*` to `char*`**

Same as above, cause  C++ does not support C99 variable length arrays, use `std::string` instead.

```cpp
std::string WCharPt2CharPt(const wchar_t* wch){
	const size_t wch_size = wcslen(wch) + 1;// wcslen does not include the terminating null-character 
	std::string ch(wch_size, '#');
	wcstombs(&ch[0], wch, wch_size); // Converts a wide string to narrow multibyte character string
    return ch;
}
```



- **`CString` to `std::string`**

  From [stackoverflow](https://stackoverflow.com/questions/258050/how-to-convert-cstring-and-stdstring-stdwstring-to-each-other)

  On Visual C++ Compiler

  ```cpp
  std::string CString2StdString(CString cstr){
      // Convert a TCHAR string to a LPCSTR
      CT2A pszConvertedAnsiString(cstr);
      // Construct a std::string using the LPCSTR input
      std::string str(pszConvertedAnsiString);
      return str;
  }
  ```

  

- **`std::string` to `CString`**

  On Visual C++ Compiler

  ```cpp
  CString StdString2CString(std::string str){
  	//Construct a CString using const char*
  	CString cstr(str.c_str());
  	return cstr;
  }
  ```

  

- **`const char*`  to `std::string`**

  ```cpp
  const char* cchar = "Hello, World!\n"
  std::string str(cchar);
  ```

  

- **`std::string` to `const char*`**

  ```cpp
  std::string str("Hello, World!\n");
  const char* cchar = str.c_str();
  ```

  

## References 

[https://stackoverflow.com/questions/23136837/in-c-when-to-use-wchar-and-when-to-use-char](https://stackoverflow.com/questions/23136837/in-c-when-to-use-wchar-and-when-to-use-char)

[http://www.cplusplus.com/reference/string/wstring/](http://www.cplusplus.com/reference/string/wstring/)

[http://icu-project.org/docs/papers/unicode_wchar_t.html](http://icu-project.org/docs/papers/unicode_wchar_t.html)

[https://docs.microsoft.com/en-us/windows/win32/extensible-storage-engine/wchar](https://docs.microsoft.com/en-us/windows/win32/extensible-storage-engine/wchar)

[https://docs.microsoft.com/en-us/cpp/text/generic-text-mappings-in-tchar-h?redirectedfrom=MSDN&view=vs-2019](https://docs.microsoft.com/en-us/cpp/text/generic-text-mappings-in-tchar-h?redirectedfrom=MSDN&view=vs-2019)

[https://stackoverflow.com/questions/321413/lpcstr-lpctstr-and-lptstr](https://stackoverflow.com/questions/321413/lpcstr-lpctstr-and-lptst)

[https://irfansworld.wordpress.com/2011/01/25/what-is-unicode-and-non-unicode-data-formats/](https://irfansworld.wordpress.com/2011/01/25/what-is-unicode-and-non-unicode-data-formats/)

[https://stackoverflow.com/questions/2853615/get-length-of-wchar-t-in-c](https://stackoverflow.com/questions/2853615/get-length-of-wchar-t-in-c)

[https://www.geeksforgeeks.org/wide-char-and-library-functions-in-c/](https://www.geeksforgeeks.org/wide-char-and-library-functions-in-c/)