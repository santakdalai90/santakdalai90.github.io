---
title: 1. Arrays and Strings
date: 2023-05-09 00:07:00 +0530
categories: [DSA, Cracking The Coding Interview]
tags: [dsa, coding, arrays]
---

# Hashing by Chaining

* Array of size N
* Each element of the array is a linked list
* element with key k gets appended to the linked list at index `hash(k) %N`

# Hashtable using binary search tree

* O(log n) lookup time
* less space consumption, no need to allocate large memory in the beginning

❓ check whether it's an array of binary search trees.

# ArrayList (Dynamin Arrays) / Slice

* Dynamically resizes
* O(1) access
* Doubles everytime it's at the capacity.

# StringBuilder

* Strings are immutable in Golang. So most operations on strings would create a copy and it's inefficient.
* `strings.Builder` can help avoid this problem.

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
	// needs no initialization
	var sb strings.Builder

	for i := 0; i < 100; i++ {
		sb.WriteString("apple")
	}
	fmt.Println(sb.String())
}
```

https://goplay.space/#NUe1vJ4W5Rx

* `strings.Builder` uses a buffer to store the data and converts to string only when necessary.

# Questions
1. Implement an algo to check if a string has all unique characters. What if you cannot use additional data structures?

    ## Solution: 
    
    ### Approach 1: Using Hash

    Create an array of size 26 (assuming the string has only small letters). Iterate over each character of the string and get the ascii value and subtract 61 from it. You get the index and then increment the value at that index.

    If the value at a given index reaches 2 then there is a duplicte and we return false from there.

    ```go
    func CheckUnique(s string) bool {
        b := []byte(s)
        arr := make([]int, 26)

        for _, x := range b {
            arr[x-97]++
            if arr[x-97] > 1 {
                return false
            }
        }
        return true
    }
    ```

    https://goplay.space/#ArKXNJUbvma

    |  |  |
    |---|---|
    | Space Complexity | O(1) |
    | Time complexity | O(n) | 

    ### Approach 2: Using Bit Manipulation

    This approach uses just an integer - int32 to keep track of, if the letter has appeared before.
    
    We can set a bit if we've seen a character.

    ```go
    func CheckUnique(s string) bool {
        if len(s) > 26 {
            return false
        }

        checker := 0
        b := []byte(s)
        for i := 0; i < len(b); i++ {
            mask := 1 << (b[i] - 'a')
            if checker&mask != 0 {
                return false
            }

            checker |= mask
        }

        return true
    }
    ```

    https://goplay.space/#4olyfYEVtyC

    |  |  |
    |---|---|
    | Space Complexity | O(1) |
    | Time complexity | O(n) | 

    Although space complexity is constant across the two approaches but this approach uses just an integer and no additional data structure is required.

1. Implement an function to reverse a string, which takes a pointer and reverses the string.

    ## Solution: 

    Since Golang strings are immutable, the best option is to use an array of runes and to do this.

    ```go
    func reverse(s *string) {
        r := []rune(*s)
        n := len(r)
        for i := 0; i < n/2; i++ {
            r[i], r[n-i-1] = r[n-i-1], r[i]
        }
        *s = string(r)
    }
    ```

1. Given two strings write a method to decide if one is a permutation of other.

    ## Solution: 
    
    ### Approach 1: Using frequency map

    Take first string and make a frequency map for each letter.

    Take second string and for each string check if it exists in the map. If it doeas, then reduce the frequency.

    After all the operations all the frequencies should be zero.

    ```go
    func checkPermutation(a, b string) bool {
        freq := make(map[rune]int)
        if len(a) != len(b) {
            return false
        }
        for _, r := range a {
            freq[r]++
        }

        for _, r := range b {
            freq[r]--
            if freq[r] < 0 {
                return false
            }
        }
        return true
    }
    ```
    
    https://goplay.space/#opPRQcgBpj3

    |  |  |
    |---|---|
    | Space Complexity | O(n) |
    | Time complexity | O(n) |

    ### Approach 2: By sorting the strings

    We can sort the strings and check for equality.
    ```go
    func sortCharacters(word string) string {
        runes := []rune(word)

        sort.Slice(runes, func(i, j int) bool {
            return runes[i] < runes[j]
        })

        return string(runes)
    }

    func checkPermutation(a, b string) bool {
        if len(a) != len(b) {
            return false
        }
        return sortCharacters(a) == sortCharacters(b)
    }
    ```

    https://goplay.space/#HYI985BPICE

    |  |  |  |
    |---|---|---|
    | Space Complexity | O(nlog n) | if sorting has the same complexity |
    | Time complexity | O(n) | as strings are immutable in Golang, additional rune is used |

