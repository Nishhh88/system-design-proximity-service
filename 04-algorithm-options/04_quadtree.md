# Option 4 : Quadtree

A quadtree is a datastructure that is used to partition a two-dimentional space by recursively subdividing it into four quadrants until the contents of the grids meet a certain criteria. For example, the criterion can be to keep subdividing until the number of businesses in the grid is not more than 100. 

**Implementaion of Quadtree**
An in-memory tree structure is made with a quadtree to answer queries. The quadtree is an in-memory data structure ans is not a database solution. It runs on each LBS server, and the data structure is built at server start-up time. 

eg, if the world contains 200 million businesses. The quadtree can be made like below:

  |---------|        |---------|
  |         |        | 40m| 30m|
  |         |        |---------|
  |  200m   | ------>| 70m| 60m| -------> so on....   The root node contains 200million businesses, it is divided into further quadrants 
  |---------|        |----|----|                      until no quadrants are left with more than 100 businesses.

  The below shows how the quadtree is saved inside memory:

                      0 200m
                    /  \ \  \
                   /    \ \  \
                40m    30m 70m 60m
                    / \ \ \
                   /   \ \ \
                  12m  4m 5m 9m

Pseudocode for building the quadtree:
  public void buildQuadtree(TreeNode node) {
    if(countNumberOfBusinessesInCurrentGrid(node) > 100) {
      node.subdivide();
      for (TreeNode child : node.getChildren()) {
        buildQuadtree(child);
      }
    }
  }

# Data structure of each node

Data stored:

1) Data on leaf node

   NAME                                                                     |   SIZE
   Top left coordinates and botton right coordinates to identify the grid   |   32 bytes (8 * 4)
   List of business IDs on the grid                                         |   8 bytes per ID  * 100 (maximal number of businesses                                                                                 |   allowed in one grid)
   Total                                                                    |   832 bytes


2) Data on internal node

   NAME                                                                     |   SIZE
   Top left coordinates and botton right coordinates to identify the grid   |   32 bytes (8 * 4)
   Pointers to 4 children                                                   |   32
   Total                                                                    |   64

The number of businesses within a grid will be stored in a database. 

# Memory Usage

Each grid can store a maximum of 100 businesses, hence the number of leaf nodes : 200 million /100 = ~ 2 million
Number of internal nodes = 2 million * 1/3 
Therefore, total memory = 2 million * 832 bytes + 0.67 million * 64 bytes = ~ 1.71 gb

(Explanation : As, in a perfect quadtree, each node will be divided into 4 till the leaf nodes are reached, so, basically if we initially have one node and we divide it into 4, we will be having +1 internal node and +3 leaf nodes. Therefore, leaf nodes = 1 + 3*internal nodes)

Therefore, a quad tree doesnt take a lot of memory and can fit in one server. 

Question : Is it ok to save quadtree in one server?
Answer: No, depending on the read volume, a single quadtree server might not be enough to accomodate all the read requests. In that case, it would be better to spread the read load among multiple quadtree servers. 

# Time Complexity to build a quadtree

To make the time complexity least, we will add one business to each node instead of counting the number of remaining businesses again and again. Basically, each time we check if the number of businesses in a node is greater than 100, we divide it. 

So, the complexity becomes : n/100(log(n/100)) (As, one grid can have 100 businesses)

# Getting nearby businesses with quadtree

After building the quadtree, start search from the root and traverse according to the location required. Traverse until a node is reached with 100 businesses. If the leaf node contains feewer than 100 businesses, neighbouring nodes with until enough businesses are returned are called.

# Operational Considerations of using Quad Tree for Production

1) As showed, creating quadree for 200 million businesses might take few minutes at the server start-up time. Therefore, we need to consider the operational implications of the such start-up time. Therefore, a new release of the server should be roll out incrementally to a small subset of servers at a time. 

2) Blue/Green environment can be used. Where we maintain two production like environments, the blue environment is the current environment in use and the green is used as standby, when new businesses are added, the green one can be loaded while blue is operational and then, the traffic can be switched from blue to green. But, this will complicate the design.

3) Another approach would be to build the quadtree among the servers incrementally, this way, some of the servers will give stale data for short period of time. This can further be mitigated by setting an agreement that new businesses will be added/updated after one day, which means that cache can be updated with a nightly job. One potential problem with this approach would be that tons of keys would be invalidated at the same time, causing heavy load on cache servers.

4) It is also possible to update the quadtree on the fly, but, this would complicate the design, especially if it is being accessed of multiple threads, this would require locking mechanism to be implemented as well.






