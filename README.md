# Region-Mapper

Region Mapper is a Python class used to identify contiguous regions and the adjacent regions.

Basically, it generates a graph from continuous regions.

## Usage:

See the documentation of the RegionMapper class. Copied here:

```
Public-facing variables:

ortho_map = ((1,0),  (0,-1), (-1,0), (0,1))
diag_map  = ((1,-1), (-1,-1),(-1,1), (1,1))
    
    These are used to calculate adjacent pixels.
    Useful for the contiguities and adjacencies dictionaries for RegionMapper

RegionMapper(image,
             class_dict,
             contiguities = {},
             adjacencies = {},
             sparse = True,
             wrap = False)
    Parameters:
        image: A Numpy array of shape (width, height, n_channels)
            E.g. image[x,y] = [255, 0, 0]
                Note: Axis 0 and 1 should represent x and y, respectively!
                If they're wrong, use np.swapaxes(picture, 0, 1)            
        class_dict: A dictionary mapping tuple of size n_channels --> int
            E.g. class_dict = { (255,0,0):1, (0,255,0):2, (0,0,255):3 }
                # Red is class 1, green is class 2, blue is class 3
        contiguities: A dictionary mapping classes to what they consider 'contiguous regions'
            E.g. contiguities = { 1 : ortho_map + diag_map, 3: diag_map }
                # Red regions are contiguous if they touch orthogonally or diagonally
                # Blue regions are contiguous only if they touch diagonally
                # Green regions are orthogonally contiguous (default)
        adjacencies: Same format as contiguities. Defines what is considered a neighbour for a given class.
            E.g. adjacencies = {} means (by default) regions of any class will only consider
            other regions to be its neighbour if a pixel of that region is orthogonally adjacent
        sparse: Boolean. Set True if most of the pixels in the image do not belong to a given class.
        wrap: Boolean. If True, the image is considered a torus. The top edge is adjacent to the bottom edge,
            and the left edge is adjacent to the right edge.
    
    Provides:
    RegionMapper.region_at_pixel(x,y):
        Returns the ID of the region at that pixel, or
        -1 if that region does not belong to a class.
    RegionMapper.regions(region_id):
        Given the ID of a region, return (class_number, list of pixels in that region)
    RegionMapper.regions_with_class(class_number):
        Given the number of a class, return all regions with that class.
    RegionMapper.adjacent_regions(region_id):
        Given the ID of a region, return all regions adjacent to it (as defined by
        its entry in the adjacencies dictionary)
```

## To be done:

* More thorough unit tests can't hurt
* Replimenting this in C should be faster
