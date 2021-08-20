### Graph Algorithm

Default setup:

```Java

//set up the adjacency list


Map<Integer, List<Integer>> adj = new HashMap<>();

//注意这里的i可以变成 for (int num : courses)等等
for (int i = 0; i < numCourses; i++) {
  adj.put(i, new LinkedList<Integer>());
}



//populate the adjacency list with all nodes' neighbors



for (int i = 0; i < numCourses; i++) {
  adj.get(edges[i][0]).add([i][1]);
  adj.get(edges[i][1]).add([i][0]); //if directed, no need to do this line
}




//create a visited array, visited == 1, unvisited == 0;
int[] visited = new int[numCourses];
```





