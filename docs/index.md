## Retinal angiogenesis

**We are interested in the hypothesized effects of DEE on blood vessel formation (angiogenesis).**

We consider post-natal day 3 ("P3") retinal cells from mice; some have mutant genes while others are normal.

At this age, the blood vessels are on a single plane (superficial layer) which means we don't need to worry about complicating 3D effects. 

## AngioTool
AngioTool, a self-contained program for analyzing blood vessels, will be used to avoid re-inventing the wheel. In this specific case, the NIH also gives away the wheels which have been used in over 100 papers.

### AngioTool's processing chain
We use [AngioTool 0.6a](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3217985/), and we are also using a table and figure from their paper in this section.

- [Multi-scale second order local structure (Hessian)](http://www.tecn.upf.es/~afrangi/articles/miccai1998.pdf)
    - Second order derivatives through convolutions with derivatives of Gaussians
        - Computationally favorable
        - Introduces a scale factor that matches vessel size
    - Map the eigenvectors of the Hessian onto an ellipsoid where the axis' semi-lengths are magnitudes of respective eigenvalues
        - Magnitudes of eigenvalues describe structure
        ![]({{"/img/HessianEigenValues.PNG"|absolute_url}})
- [Convex Hull](http://scikit-image.org/docs/dev/auto_examples/edges/plot_convex_hull.html#sphx-glr-auto-examples-edges-plot-convex-hull-py)
    - Given a binary image, the convex hull is the set of pixels contained within the smallest convex polygon that will surround all of the foreground pixels.
    - Used for calculating area
- [Skeletonization](http://scikit-image.org/docs/dev/auto_examples/edges/plot_skeleton.html#sphx-glr-auto-examples-edges-plot-skeleton-py)
    - Given a binary image, skeletonization yields a frame which is 1 pixel-wide.
    - Used for calculating vessel lengths

AngioTool outputs values such as 
![]({{"/img/angiotooltable.png"|absolute_url}})

I was handed unlabelled data (i.e. I was blinded) and told to tweak the knobs and dials of AngioTool. 

- Vessel Thickness
  - Values for Vessel Thickness varied between 8 and 15.
  - The difference in wild type and mutant is most reflected here.
- Small Particle Size
  - Setting this to a high value such as 3000 was occasionally necessary for removing "islands" in the background.
- Fill Holes
  - Frequently set to 0 due to being unncessary.

We obtain images such as 
![]({{"/img/36-4b-field1result.jpg"|absolute_url}})

where the features are

| Blue dots       | Red lines       | Yellow outlines  |
| --------------- |:---------------:| ----------------:|
| Vessel junction | Vessel skeleton | Vessel area      |

and the AngioTool results are saved in an Excel file.

At this point, we can dive into our Jupyter Notebook titled RetinalCells-PaireTtest.ipynb

### Misc. info

GSL I-B4 isolectin was used as a marker for endothelial cells. 
