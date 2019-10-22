# <center>1380 - Log Sorting (E)</center> 


<br></br>

* Tag: Sort, String
* Company: Amazon
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Easy
* Link: https://www.lintcode.com/problem/log-sorting/description

<br></br>



## Description
----
Given a list of string logs, in which each element representing a log. Each log can be separated into two parts by the first space. The first part is the ID of the log, while the second part is the content of the log. (The content may contain spaces as well.) The content is composed of only letters and spaces, or only numbers and spaces.

Now you need to sort logs by following rules:
1. Logs whose content is letter should be ahead of logs whose content is number.
2. Logs whose content is letter should be sorted by their content in lexicographic order. And when two logs have same content, sort them by ID in lexicographic order.
3. Logs whose content is number should be in their input order.

<br></br>



## Example
----
Input:   [2->4->null,null,-1->null]
Output:  -1->2->4->null

<br></br>



## Solution
----
Input:  
```
    logs = [
        "zo4 4 7",
        "a100 Act zoo",
        "a1 9 2 3 1",
        "g9 act car"
    ]
```

Output: 
```
    [
        "a100 Act zoo",
        "g9 act car",
        "zo4 4 7",
        "a1 9 2 3 1"
    ]
```

<br>


### Go
```go
import (
    "strings"
)

func logSort (logs []string) []string {
    l := len(logs)
    logsNum, logsString := make([]string, 0), make([]string, 0)
    for j := 0; j < l; j++ {
        if isContentNum(logs[j]) {
            logsNum = append(logsNum, logs[j])
        } else {
            logsString = append(logsString, logs[j])
        }
    }
    sortByContent(logsString, 0, len(logsString) - 1)
    for j := 0; j < len(logsString); {
        content := getContent(logsString[j])
        k := j + 1
        for ; k < len(logsString) && content == getContent(logsString[k]); k++ {
        }
        sortByID(logsString, j, k - 1)
        j = k
    }
    
    return append(logsString, logsNum...)
}

func sortByID(logs []string, start, end int) {
    if start < 0 || end >= len(logs) || start >= end {
        return
    }
    p := getPartitionByID(logs, start, end)
    sortByID(logs, start, p - 1)
    sortByID(logs, p + 1, end)
}

func getPartitionByID(logs []string, start, end int) int {
    if start < 0 || end >= len(logs) || start >= end {
        return -1
    }
    pivot := getID(logs[end])
    i := start - 1
    for j := start; j < end; j++ {
        if getID(logs[j]) <= pivot {
            i++
            if j != i {
                logs[j], logs[i] = logs[i], logs[j]
            }
        }
    }
    logs[i + 1], logs[end] = logs[end], logs[i + 1]
    
    return i + 1
}

func sortByContent(logs []string, start, end int) {
    if start < 0 || end >= len(logs) || start >= end {
        return
    }
    p := getPartitionByContent(logs, start, end)
    sortByContent(logs, start, p - 1)
    sortByContent(logs, p + 1, end)
}

func getPartitionByContent(logs []string, start, end int) int {
    if start < 0 || end >= len(logs) || start >= end {
        return -1
    }
    pivot := getContent(logs[end])
    i := start - 1
    for j := start; j < end; j++ {
        if getContent(logs[j]) <= pivot {
            i++
            if i != j {
                logs[i], logs[j] = logs[j], logs[i]
            }
        }
    }
    logs[i + 1], logs[end] = logs[end], logs[i + 1]
    
    return i + 1
}

func isContentNum(s string) bool {
    for i := strings.Index(s, " ") + 1; i < len(s); i++ {
        if s[i] == ' ' {
            continue
        }
        if s[i] > '9' || s[i] < '0' {
            return false
        }
    }
    
    return true
}


func getContent(s string) string {
    i := strings.Index(s, " ")
    return s[i + 1:]
}

func getID(s string) string {
    i := strings.Index(s, " ")
    return s[:i]
}
```

<br>
