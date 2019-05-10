---
layout: post
title: Leetcode Problem – Merge Intervals
---

Question:

Given a collection of intervals, merge all overlapping intervals.

~~~
Example 1:

Input: [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlaps, merge them into [1,6].
Example 2:

Input: [[1,4],[4,5]]
Output: [[1,5]]
Explanation: Intervals [1,4] and [4,5] are considered overlapping.
NOTE: input types have been changed on April 15, 2019. Please reset to default code definition to get new method signature.
~~~

Answer:

So my initial instinct for this was to sort by the start of each interval and then you would only have to compare one interval to its adjacent interval. Mostly due to having done some pseudocode for the similar Meeting Rooms II problem. However I didn't think about a lot of edge cases.  The biggest error was assuming the adjacent interval will always have a higher end value.  I fixed this with the whatToAdd variable.


```javascript
/**
 * @param {number[][]} intervals
 * @return {number[][]}
 */
var merge = function(intervals) {
    intervals.sort(Comparator);
    var mergedIntervals = [];
    var justMerged = false;
    intervals.forEach(function(interval, index, intervals){
        if (intervals.length == 1) {
            mergedIntervals = intervals;
        } else {
            if (index == 0) {
                mergedIntervals.push(interval);
            } else {
                if (mergedIntervals[mergedIntervals.length - 1][1] >= interval[0]) {
                    var whatToAdd;
                    if (interval[1] > mergedIntervals[mergedIntervals.length - 1][1]) {
                        whatToAdd = interval[1];
                    } else {
                        whatToAdd = mergedIntervals[mergedIntervals.length - 1][1];
                    }
                    mergedIntervals[mergedIntervals.length - 1].splice(1,1);
                    mergedIntervals[mergedIntervals.length - 1].push(whatToAdd);
                } else {
                    mergedIntervals.push(interval);
                }
            }
        }
    });

    return mergedIntervals;
};

function Comparator(a, b) {
    if (a[0] < b[0]) return -1;
    if (a[0] > b[0]) return 1;
    return 0;
};
```
