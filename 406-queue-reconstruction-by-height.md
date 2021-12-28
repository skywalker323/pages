# 406 Queue Reconstruction by Height

### Problem:

Suppose you have a random list of people standing in a queue. Each person is described by a pair of integers \(h, k\), where h is the height of the person and k is the number of people in front of this person who have a height greater than or equal to h. Write an algorithm to reconstruct the queue.

Note:  
The number of people is less than 1,100.

Example

```
Input:
[[7,0], [4,4], [7,1], [5,0], [6,1], [5,2]]

Output:
[[5,0], [7,0], [5,2], [6,1], [4,4], [7,1]]
```

### Solutions:

```cpp
imagine your are standing in a queue in ascending format, and you get a card,
telling you how many people taller / equal than you should stand in front of you.

what will you do ?
because the queue is sorted according to height, i know for sure that people behind me are taller than me,
so i will just move k value back in queue,
but theres a problem, consider value {7, 0}, {4, 4}, {7, 1}, {5, 0}, {6, 1}, {5, 2}
after sorting we get (4, 4), (5, 0), (5,2), (6, 1), (7, 0), (7, 1)

now our logic above will work fine till we reach (5, 0), because k value is 0, we wont move it,
but if we check the our result till now,
we get wrong answer for (5, 2), (5, 0), (6, 1), (7, 0), (5, 2) -> now (5, 2) comes before 3 elements >= then itself,

how to fix this ?
by simply putting the values of  same height but greater k before than the lesser one, ex ->
if we do normal sorting our result will be 
  normal sorting -> (5, 0), (5, 2)
  but after applying our logic
  modified sorting -> (5, 2), (5, 0)
now if we use modified sorting we will get correct answers
ex , after modifies sorting we get -> (4, 4), (5, 2), (5,0), (6, 1), (7, 0), (7, 1)

and after applying the logic we get
(5, 0), (7, 0), (5, 2), (6, 1), (4, 4), (7, 1)

static bool comp(const vector<int> &a, const vector<int> &b){
	if(a[0] < b[0])	return true;
	if(a[0] == b[0] && a[1] > b[1])	return true;
	return false;
}

vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
	int n = people.size();
	sort(people.begin(), people.end(), comp);

	for(int i = n-2; i >= 0; i--){
		auto p = people[i];
		int itr = people[i][1];
		for(int k = i; k < i + itr; k++){
			people[k] = people[k+1];
		}
		people[i + itr] = p;
	}
}
```

```java

```

```java

```



