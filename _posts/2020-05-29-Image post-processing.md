---
layout: post
title: Image post-processing.
---

It is time to address a small problem... The image processing that I apply after rendering the 3D models for the characters is to slow. 

The different parts of the image processing are:

- **Reading the image**: this part is done by the skimage package and it is already really fast, so we will not worry about the speed of this part.
- **Down-scaling image**: to downscale the image we also use a function of the skimage package that is really fast, so we won't worry about this part.
- **Reducing the amount of colours**:  this is a custom function that uses numpy arrays operations to reduce the amount of colours as much as possible without being noticeable for the human eye. 
- **Getting the colours of the palette**: this function reads a image a creates a list with its colours. Since this function is just used once in every batch of images it is not a problem and it is already quite fast.
- **Remapping the colours of the image**: this is the most important function and the one that takes most of the time. What it does is changing the colours of one image for the closest one in a palette.

## Benchmarks

To do the benchmarks I am using my own laptop, that usually is running more process in the background. The benchmark consist in processing 25 images with an input resolution of 640x900 to an output resolution of 64x90 with a palette of 47 colours. To reduce the image colours we are using a maximum distance of 18, which in the test has shown to work good enough.

Yes, it is not the best benchmark in the world, in fact is bad, but it should be good enough. These are the results: 

| Functions      | Time     |
| :------------- | -------- |
| load           | 0.475    |
| downscale      | 0.462    |
| reduce colours | 8.575    |
| get colours    | 0.000181 |
| remap colours  | 49.290   |

As you can see the two functions that deserve our attention is the colour reduction and the colour remapping ones.

## Colour reduction

This is the code that I am using right now:

```python
def reduce_colors(img, threshold):
    # First we create a empty list of colours
    colors = list()
	# Then we iterate through all the pixels
    for y, row in enumerate(img):
        for x, pixel in enumerate(row):
           	# We iterate over all the colours to get the
            # index of the closest colour
            dist = float("inf")
            index = 0
            for pos, color in enumerate(colors):
                new_dist = np.linalg.norm(color-pixel)
                if new_dist < dist:
                    dist = new_dist
                    index = pos
            # If the distance is bigger than the threshold we add
            # the colour to the list of colours. If this is not the
            # case we change the colour to the closest one.
            if dist > threshold:
                colors.append(pixel)
            else:
                img[y][x] = colors[index]

    return img
```

Yes, this code could have been done by a monkey, but I needed to make sure that this approach worked before worrying about how fast it was. I also know it is not the best way to do it because it just has into account the first occurrence of the colour and not the average of all the colours that are under the threshold. This is an example of the images I am working with:

![](./../assets/images/character_post_processing_pixel.png){:.img}

The biggest problem is that we are following an iterative approach by using 3 for loops when numpy is optimised for matrix operations. Seriously, this code is plain garbage, I am ashamed of myself, but we are still in time to amend our mistakes.

The new approach will be focused in dividing the problem into two sub-problems: getting the reduced palette of colours and remapping the colours.

### Getting the reduced palette

At this point we don't care about the geometry of the image so we could imagine that it is just a list of colours... A set of points in a 4D space (we are working with an RGBA colour model)... Okay, what if we use a clustering technique and the calculate the centroid (average) of all the members of the cluster? First of all, this approach should be way faster and secondly it solves the problem of taking just the first element.

First problem, which clustering algorithm to use. In order to find the answer lets refer to this document from scikit learn: [2.3. Clustering](https://scikit-learn.org/stable/modules/mixture.html). 

![../_images/sphx_glr_plot_cluster_comparison_0011.png](/home/edoelas/git/tfgblog/assets/images/sphx_glr_plot_cluster_comparison_0011.png)

Our data will have a shape similar to the fifth row and the speed shouldn't be a problem since this package is really fast. The most important thing to have into account is that we don't know the number of clusters that we will need so our loved K-Means is out of the game. After trying multiple options with different images the one that has given the best results is Mean Shift.

Before applying the clustering algorithm we should think if it is possible to process our data to improve the speed. As I said before we are using a RGBA model and in our image the alpha value is either 0 or 255. This is because when we downscale our image we disable the anti-aliasing [CHECK] to get this pixel-art effect. Since all the values with 0 alpha belong to the background and we don't care about them so we can delete all the members of our dataset that have 0 alpha and after that we can reduce the dimensionality to 3 by deleting the alpha value of all our members.

Let's see how we can do this:

```python
import skimage.io
import numpy as np

data = img.reshape(img.shape[0]*img.shape[1],img.shape[2])
data = data[data[:,3] != 0]
data = data[:,0:3]
```

Using a real image like the ones we are going to process for the real game we have reduced the amount of values from 23040 (64 * 90 * 4) to 2997 (999 * 3). Now it is time to apply the Mean Shift algorithm. We will use scikit-learn package for that:

```python
from sklearn.cluster import MeanShift, estimate_bandwidth

bandwidth = estimate_bandwidth(data, quantile=0.14, n_samples=len(data))
ms = MeanShift(bandwidth=bandwidth, bin_seeding=True).fit(data)
```

And by accessing `ms.cluster_centers_` we can get the centroids of the clusters. The quantile value has been decided based on experimentation with different images of the set we are going to work with.

### Remapping the image

Now we have a colour palette and an image, so we have to think how to apply this colour palette. The RGB colour model is oriented to the user and uniform, this means that it is based in how humans perceive the colours and that the difference between how humans percieve two colours can be calculated using the euclidean distance inside a 3D space. This means that the smaller the euclidean distance, the more similar are the colours, so we will have to find the euclidean distance between each pixel with all the colours and choose the one with the smallest.