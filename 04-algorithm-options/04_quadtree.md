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
