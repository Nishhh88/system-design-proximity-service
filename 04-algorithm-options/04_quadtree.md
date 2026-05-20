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

  
