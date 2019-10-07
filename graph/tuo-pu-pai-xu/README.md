# 拓扑排序

## Topological Sorting

Topological sorting for Directed Acyclic Graph \(DAG\) is a linear ordering of vertices such that for every directed edge uv, vertex u comes before v in the ordering. Topological Sorting for a graph is not possible if the graph is not a DAG.

For example, a topological sorting of the following graph is “5 4 2 3 1 0”. There can be more than one topological sorting for a graph. For example, another topological sorting of the following graph is “4 5 2 3 1 0”. The first vertex in topological sorting is always a vertex with in-degree as 0 \(a vertex with no incoming edges\).

[![graph](https://media.geeksforgeeks.org/wp-content/cdn-uploads/graph.png)](https://media.geeksforgeeks.org/wp-content/cdn-uploads/graph.png)

_**Topological Sorting vs Depth First Traversal \(DFS\)**_:  
In [DFS](https://www.geeksforgeeks.org/depth-first-traversal-for-a-graph/), we print a vertex and then recursively call DFS for its adjacent vertices. In topological sorting, we need to print a vertex before its adjacent vertices. For example, in the given graph, the vertex ‘5’ should be printed before vertex ‘0’, but unlike [DFS](https://www.geeksforgeeks.org/depth-first-traversal-for-a-graph/), the vertex ‘4’ should also be printed before vertex ‘0’. So Topological sorting is different from DFS. For example, a DFS of the shown graph is “5 2 3 1 0 4”, but it is not a topological sorting

_**Algorithm to find Topological Sorting:**_  
We recommend to first see implementation of DFS [here](https://www.geeksforgeeks.org/depth-first-traversal-for-a-graph/). We can modify [DFS ](https://www.geeksforgeeks.org/depth-first-traversal-for-a-graph/)to find Topological Sorting of a graph. In [DFS](https://www.geeksforgeeks.org/depth-first-traversal-for-a-graph/), we start from a vertex, we first print it and then recursively call DFS for its adjacent vertices. In topological sorting, we use a temporary stack. We don’t print the vertex immediately, we first recursively call topological sorting for all its adjacent vertices, then push it to a stack. Finally, print contents of stack. Note that a vertex is pushed to stack only when all of its adjacent vertices \(and their adjacent vertices and so on\) are already in stack.

Below image is an illustration of the above approach:

![](https://media.geeksforgeeks.org/wp-content/cdn-uploads/20190704153933/TopologicalSorting1.png)

```python
#Python program to print topological sorting of a DAG 
from collections import defaultdict 

#Class to represent a graph 
class Graph: 
	def __init__(self,vertices): 
		self.graph = defaultdict(list) #dictionary containing adjacency List 
		self.V = vertices #No. of vertices 

	# function to add an edge to graph 
	def addEdge(self,u,v): 
		self.graph[u].append(v) 

	# A recursive function used by topologicalSort 
	def topologicalSortUtil(self,v,visited,stack): 

		# Mark the current node as visited. 
		visited[v] = True

		# Recur for all the vertices adjacent to this vertex 
		for i in self.graph[v]: 
			if visited[i] == False: 
				self.topologicalSortUtil(i,visited,stack) 

		# Push current vertex to stack which stores result 
		stack.insert(0,v) 

	# The function to do Topological Sort. It uses recursive 
	# topologicalSortUtil() 
	def topologicalSort(self): 
		# Mark all the vertices as not visited 
		visited = [False]*self.V 
		stack =[] 

		# Call the recursive helper function to store Topological 
		# Sort starting from all vertices one by one 
		for i in range(self.V): 
			if visited[i] == False: 
				self.topologicalSortUtil(i,visited,stack) 

		# Print contents of the stack 
		print stack 

g= Graph(6) 
g.addEdge(5, 2); 
g.addEdge(5, 0); 
g.addEdge(4, 0); 
g.addEdge(4, 1); 
g.addEdge(2, 3); 
g.addEdge(3, 1); 

print "Following is a Topological Sort of the given graph"
g.topologicalSort() 
#This code is contributed by Neelam Yadav 
```

Output:

```text
Following is a Topological Sort of the given graph
5 4 2 3 1 0
```

**Time Complexity:** The above algorithm is simply DFS with an extra stack. So time complexity is the same as DFS which is O\(V+E\).

**Note :** Here, we can also use vector instead of stack. If the vector is used then print the elements in reverse order to get the topological sorting.

