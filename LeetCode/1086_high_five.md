# <center>1086 - High Five (M)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Array, Heap
* Difficulty: Medium
* Company: Amazon
* Link: https://leetcode.com/problems/high-five

<br></br>



## Description
----
There are two properties in the node student id and scores, to ensure that each student will have at least 5 points, find the average of 5 highest scores for each person.

<br></br>



## Example
----
Input:  [[1,91],[1,92],[2,93],[2,99],[2,98],[2,97],[1,60],[1,58],[2,100],[1,61]]

Output:
1. 72.40
2. 97.40

<br></br>



## Solution
----
### Python
```python
'''
Definition for a Record
class Record:
    def __init__(self, id, score):
        self.id = id
        self.score = score
'''
class Solution:
    # @param {Record[]} records a list of <student_id, score>
    # @return {dict(id, average)} find the average of 5 highest scores for each person
    # <key, value> (student_id, average_score)
    def highFive(self, records):
        result = {}
        for r in records:
            if r.id not in result:
                result[r.id] = [r.score]
            else:
                scores = result[r.id]
                i, l = 0, len(scores)
                while i < l and r.score < scores[i]:
                    i += 1
                if i == l and l < 5:
                    scores.append(r.score)
                elif i < l:
                    scores = scores[:i] + [r.score] + scores[i:4]
                result[r.id] = scores
        for id in result:
            scores = 0
            for s in result[id]:
                scores += s
            result[id] = scores / 5
        
        return result
```

<br>



### Java
```java
public class Solution {
    /**
     * @param results a list of <student_id, score>
     * @return find the average of 5 highest scores for each person
     * Map<Integer, Double> (student_id, average_score)
     */
    public Map<Integer, Double> highFive(Record[] results) {
        Map<Integer, Double> answer = new HashMap<Integer, Double>();
        Map<Integer, PriorityQueue<Integer>> hash = new HashMap<Integer, PriorityQueue<Integer>>();

        for (Record r : results) {
            if (!hash.containsKey(r.id)){
                hash.put(r.id, new PriorityQueue<Integer>());
            }
            PriorityQueue<Integer> pq=hash.get(r.id);
            if (pq.size() < 5) {
                pq.add(r.score);
            } else {
                if (pq.peek() < r.score){
                    pq.poll();
                    pq.add(r.score);
                }
            }
        }

        for (Map.Entry<Integer, PriorityQueue<Integer>> entry : hash.entrySet()) {
            int id = entry.getKey();
            PriorityQueue<Integer> scores = entry.getValue();
            double average = 0;
            for (int i = 0; i < 5; ++i)
                average += scores.poll();
            average /= 5.0;
            answer.put(id, average);
        }
        return answer;
    }
}
```