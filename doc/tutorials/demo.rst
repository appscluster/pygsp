========
GSP Demo
========

This tutorial shows basic operations of the toolbox.
To start open a python shell (IPython is recommended here) and import the pygsp. You would probably also import numpy as you will need it to create matrices and arrays.

>>> import pygsp
>>> import numpy as np

The first step is to create a graph, there's a general class that can be used to generate graph from it's weight matrix.

>>> np.random.seed(42) # We will use a seed to make reproducible results
>>> W = np.random.rand(400, 400)
>>> G = pygsp.graphs.Graph(W)


You have now a graph structure ready to be used everywhere in the box! If you want to know more about the Graph class and it's subclasses you can check the online doc at : #TODO
You can also check the included methods for all graphs:

>>> help(pygsp.graphs.Graph)

For the nexts steps of the demo, we will be using the logo graph bundled with the toolbox :

>>> G = pygsp.graphs.Logo()

You can now plot the graph:

>>> pygsp.plotting.plot(G)

#TODO insert image of logo here!

Looks good isn't it? Now we can start to analyse the graph. The next step to compute Graph Fourier Transform or exact graph filtering is to precompute the Fourier basis of the graph. This operation can be very long as it needs to to fully diagonalize the Laplacian. Happily it is not needed to filter signal on graphs.

>>> pygsp.operators.compute_fourier_basis(G)

You can now access the eigenvalues of the fourier basis with G.e and the eigenvectors G.U, they look like sinuses on the graph.
Let's plot the second and third eigenvector, as the one is only constant.

>>> pygsp.plotting.plot_signal(G, G.U[:, 2])
>>> pygsp.plotting.plot_signal(G, G.U[:, 3])

Let's discover basic filters operations, filters are usually defined in the spectral domain.

First let's define a filter object:

>>> F = pygsp.filters.Filter(G)

And we can assign this function 

.. math:: \begin{equation*} g(x) =\frac{1}{1+\tau x} \end{equation*}

to it:

>>> g = lambda x: 1./(1. + tau * x)
>>> F.g = g

You can also put multiple functions in a list to define a filterbank!

>>> pygsp.plotting.plot(F, plot_eigenvalues=True)

Here's our low pass filter.

The filter is plotted all along the spectrum of the graph, the cross at the bottom are the laplacian's eigenvalues. Those are the point where the continuous filter will be evaluated to create a discrete filter.
To apply it to a given signal, you only need to run:

>>> f2 = F.analysis(G, G.U)

Finally here's the noisy signal and the denoised version right under.

>>> pygsp.plotting.plot_signal(G, f)
>>> pygsp.plotting.plot_signal(G, f2)

So here are the basics for the PyGSP toolbox, if you want more informations you can check the doc at : #TODO.
Enjoy the toolbox!