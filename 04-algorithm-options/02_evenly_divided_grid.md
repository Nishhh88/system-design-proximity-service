# Option 2 : Evenly divided grid

The next direct idea after the basic indexing would be to divide the world into small grids. This way, one grid would have multiple businesses, and each business on the map belongs to one grid.

**Challenges**

- The distribution of businesses is not even. There could be a lot of businesses in downtowns, while other areas like desserts or oceans have no businesses at all.Therefore, by dividing the world in even grids, a very uneven data distribution is produced.
- To counter the above challenge, we could use more granular grids for dense areas and large grids in sparse areas.
- Another challenge would arise to find the neighboring grids of a fixed grid.
