# Critical connections in a network
## https://leetcode.com/problems/critical-connections-in-a-network/

There are n servers numbered from 0 to n - 1 connected by undirected server-to-server connections forming a network where connections[i] = [ai, bi] represents a connection between servers ai and bi. Any server can reach other servers directly or indirectly through the network.

A critical connection is a connection that, if removed, will make some servers unable to reach some other server.

Return all critical connections in the network in any order.


# Implementation :
```java
class Solution {
    int time = -1;
    public List<List<Integer>> criticalConnections(int n, 
        List<List<Integer>> connections) {
        List<List<Integer>> res = new ArrayList<>();
        List<Integer>[] graph = new List[n];
        buildGraph(connections, graph);
        
        int[] disc = new int[n];
        int[] low = new int[n];
        int[] parents = new int[n];
        Arrays.fill(disc, -1);
        Arrays.fill(low, -1);
        Arrays.fill(parents, -1);
        for (int i = 0; i < n; i++) {
            if (disc[i] == -1) {
                dfs(i, parents, disc, low, graph, res);
            }
        }
        
        return res;
    }
    
    private void dfs(int u, int[] parents, int[] disc, int[] low, 
       List<Integer>[] graph, List<List<Integer>> res) {
        if (disc[u] != -1) return;
        
        low[u] = disc[u] = time++;
        
        for (int v : graph[u]) {
            if (disc[v] == -1) {
                parents[v] = u;
                dfs(v, parents, disc, low, graph, res);
                if (disc[u] < low[v]) {
                    res.add(Arrays.asList(u, v));
                }
                
                low[u] = Math.min(low[u], low[v]);
            } else if (parents[u] != v) {
                low[u] = Math.min(low[u], disc[v]);
            }
        }
    }
    
    private void buildGraph(List<List<Integer>> connections,
         List<Integer>[] graph) {
        for (List<Integer> c : connections) {
            int from = c.get(0);
            int to = c.get(1);
            if (graph[from] == null) {
                graph[from] = new ArrayList<>();
            }
            
            if (graph[to] == null) {
                graph[to] = new ArrayList<>();
            }
            
            graph[from].add(to);
            graph[to].add(from);
        }
    }
}
```
