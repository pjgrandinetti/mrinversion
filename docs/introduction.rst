.. _introduction:

============
Introduction
============

Objective
---------

In ``mrinversion``, we solve for the distribution of the second-rank traceless
symmetric tensor parameters, through an inversion of pure anisotropic NMR spectrum.

.. whose frequency
.. contributions are assumed to arise predominantly from the second-rank traceless
.. symmetric tensors.

**Nuclear shielding tensor parameters**

In the case of the shielding tensor, this corresponds to solving for the distribution
of anisotropy and asymmetry parameters of the second-rank traceless shielding tensor
through an inversion of a pure anisotropic frequency spectrum. The pure anisotropic
frequency spectra are the cross-sections of the 2D isotropic `v.s` anisotropic
correlation spectrum, such as the magic-angle hopping (MAH), 2D One Pulse (TOP) MAS,
magic-angle turning (MAT), magic-angle flipping (MAF), Variable Angle Correlation
Spectroscopy (VACSY), phase adjusted spinning sideband (PASS), extended chemical shift
modulation (XCS), and it's variant.

.. Linearizing the problem
.. -----------------------

Linear problem
--------------

Linear inverse problems are frequently encountered in the scientific community and
have the following generic form

.. math::
   :label: eq_1

   {\bf K \cdot f} = {\bf s},

where :math:`{\bf K} \in \mathbb{R}^{m\times n}` is the transforming kernel (matrix),
:math:`{\bf f} \in \mathbb{R}^n` is the unknown and desired solution, and
:math:`{\bf s} \in \mathbb{R}^m` is the known signal, which includes the
measurement noise. When the matrix :math:`{\bf K}` is non-singular and :math:`m=n`,
the solution to the problem in Eq. :eq:`eq_1` has a simple closed-form solution,

.. math::
    :label: eq_2

    {\bf f} = {\bf K}^{-1} {\bf \cdot s}.

But practical science isn't easy that way! Let's see how.

The deciding factor whether the solution :math:`{\bf f}` exists in Eq. :eq:`eq_2`
is whether or not the kernel :math:`{\bf K}` is invertible.
Often, most scientific problems with practical applications suffer from singular,
near-singular, or ill-conditioned kernels, where :math:`{\bf K}^{-1}` doesn't exist.
Such types of problems are termed as `ill-posed`. The inversion of a purely anisotropic
NMR spectrum to the distribution of the tensorial parameters is one such ill-posed
problem.



Regularized linear problem
''''''''''''''''''''''''''

A common approach in solving ill-posed problems is to employ the regularization
methods of form

.. math::
    :label: eq_3

    {\bf f^\dagger} = \underbrace{\text{argmin}}_{\bf f} \left(
        \|{\bf K \cdot f} - {\bf s}\|^2_2 + g({\bf f})
    \right),

where :math:`\|{\bf z}\|_2` is the `l-2` norm of :math:`{\bf z}`, :math:`g({\bf f})`
is the regularization term, and :math:`{\bf f}^\dagger` is the regularized solution.
The choice of the regularization term, :math:`g({\bf f})`, is often based on prior
knowledge of the system for which the linear problem is defined. For anisotropic NMR
spectrum inversion, we choose the smooth-LASSO regularization.

.. Elastic net regularization
.. ''''''''''''''''''''''''''

.. When the matrix, :math:`{\bf J}_i`, in Eq. :eq:`slasso` is identity, the regularization
.. term is the elastic net regularization.


.. For example, in a more familiar linear-inverse problem, the inverse Fourier transform, the two dimensions are the frequency and time dimensions, where the frequency dimension undergoes the inverse transformation, and the time dimension is where the inversion method transforms the data.

.. _smooth_lasso_intro:

Smooth-LASSO regularization
"""""""""""""""""""""""""""

Our prior assumption for the distribution of the tensorial parameters is that it should
be smooth and continuous for disordered and sparse and discrete for crystalline
materials. Therefore, we employ the smooth-lasso method, which is a linear model
that is trained with the combined l1 and l2 priors as the regularizer. The method
minimizes the objective function,

.. math::
    :label: slasso

    \| {\bf K \cdot f - s} \|^2_2 + \alpha \sum_{i=1}^{d} \| {\bf J}_i \cdot {\bf f} \|_2^2
                + \lambda  \| {\bf f} \|_1 ,

where :math:`\alpha` and :math:`\lambda` are the hyperparameters controlling the
smoothness and sparsity of the solution :math:`{\bf f}`. The matrix :math:`{\bf J}_i`
follows

.. math::
    {\bf J}_i = {\bf I}_{n_1} \otimes \cdots \otimes {\bf A}_{n_i}
                \otimes \cdots \otimes {\bf I}_{n_{d}},

where :math:`{\bf I}_{n_i} \in \mathbb{R}^{n_i \times n_i}` is the identity matrix, and
:math:`{\bf A}_{n_i}` is the first difference matrix given as

.. math::
    {\bf A}_{n_i} = \left(\begin{array}{ccccc}
                    1 & -1 & 0 & \cdots & \vdots \\
                    0 & 1 & -1 & \cdots & \vdots \\
                    \vdots & \vdots & \vdots & \vdots & 0 \\
                    0 & \cdots & 0 & 1 & -1
                \end{array}\right) \in \mathbb{R}^{(n_i-1)\times n_i}.

The symbol :math:`\otimes` is the Kronecker product. The terms,
:math:`\left(n_1, n_2, \cdots, n_d\right)`, are the number of points along the
respective dimensions, with the constraint that :math:`\prod_{i=1}^{d}n_i = n`,
where :math:`d` is the total number of dimensions in the solution :math:`{\bf f}`,
and :math:`n` is the total number of features in kernel, :math:`{\bf K}`.

Understanding the `x-y` plot
----------------------------

A second-rank symmetric tensor, :math:`{\bf S}`, in a three-dimensional space, is
described by three principal components, :math:`s_{xx}`, :math:`s_{yy}`, and
:math:`s_{zz}`, in the principal axis system (PAS). Often, depending on the context of
the problem, the three principal components are expressed with three new parameters
following a convention. One such convention is the Haeberlen convention, which
defines three new parameters, :math:`\delta_\text{iso}^\text{CS}`, :math:`\zeta_\sigma`, and
:math:`\eta_\sigma`, as the isotropic chemical shift, shielding anisotropy, and shielding
asymmetry. Here, the parameters :math:`\zeta_\sigma` and :math:`\eta_\sigma` contribute
to the purely anisotropic frequencies, and determining the distribution of these two
parameters is the focus of this library.

Defining the inverse grid
''''''''''''''''''''''''''

When solving any linear inverse problem, one needs to define an inverse grid before
solving the problem. A familiar example is the inverse Fourier transform, where
the inverse grid is defined following the Nyquist–Shannon sampling theorem. Unlike
IFFT, however, there is no well-defined sampling grid for the second-rank traceless
symmetric tensor parameters. One obvious choice is to define a two-dimensional
:math:`\zeta_\sigma`-:math:`\eta_\sigma` Cartesian grid.

As far as the inversion problem is concerned, :math:`\zeta_\sigma` and :math:`\eta_\sigma`
are just labels for the subspectra. In simplistic terms, the inversion problem solves
for the probability of each subspectrum, from a given pre-defined basis of subspectra,
that describes the observed spectrum. If the subspectra basis is defined over a
:math:`\zeta_\sigma`-:math:`\eta_\sigma` Cartesian grid, multiple
:math:`(\zeta_\sigma, \eta_\sigma)` coordinates points to the same subspectra, therefore,
distinguishing these coordinates from the subspectra becomes impossible.

The issue of multiple coordinates pointing to the same object is not new. It is
a common problem when representing polar coordinates in the Cartesian basis. Try describing
the coordinates of the south pole using latitudes and longitudes.

To resolve the issue of multiple coordinates pointing to the same subspectra, we redefine
the :math:`\zeta_\sigma`-:math:`\eta_\sigma` parameters on an `x`-`y` Cartesian grid,
where the basis subspectra are distinguishable.

Introducing the :math:`x`-:math:`y` grid
""""""""""""""""""""""""""""""""""""""""

The `x`-`y` grid is a piece-wise polar grid of second-rank traceless tensor parameters,
:math:`\zeta_\sigma` and :math:`\eta_\sigma`. The mapping of :math:`\zeta_\sigma`, and
:math:`\eta_\sigma` coordinates onto the `x`-`y` grid is defined as

.. math::
    :label: zeta_eta_def

    x = \left\{ \begin{array}{l r}
                |\zeta_\sigma|\sin\theta, & \forall \zeta_\sigma\ge0, \\
                |\zeta_\sigma|\cos\theta, & \text{elsewhere}
               \end{array}
        \right. \\
    y = \left\{ \begin{array}{l r}
                |\zeta_\sigma|\cos\theta, & \forall \zeta_\sigma\ge0, \\
                |\zeta_\sigma|\sin\theta, & \text{elsewhere}
               \end{array}
        \right.

where :math:`\theta=\pi\eta_\sigma/4`.

.. plot::
    :format: doctest
    :context: close-figs
    :include-source:

    >>> import matplotlib.pyplot as plt # doctest: +SKIP
    >>> from mrinversion.utils import get_polar_grids # doctest: +SKIP
    ...
    >>> _ = plt.figure(figsize=(4,3.5)) # doctest: +SKIP
    >>> ax=plt.gca() # doctest: +SKIP
    >>> get_polar_grids(ax) # doctest: +SKIP
    >>> ax.set_xlabel('x / ppm') # doctest: +SKIP
    >>> ax.set_ylabel('y / ppm') # doctest: +SKIP
    >>> plt.tight_layout() # doctest: +SKIP
    >>> plt.show() # doctest: +SKIP

In the above figure, the shielding anisotropy parameter, :math:`\zeta_\sigma`, is
the radial dimension, and the asymmetry parameter, :math:`\eta`, is the angular
dimension, defined in Eq. :eq:`zeta_eta_def`. The region in red and blue corresponds
to the positive and negative values of :math:`\zeta_\sigma`. The radial grid lines
are drawn at every 0.2 ppm increments of :math:`\zeta_\sigma`, and the angular grid
lines are drawn at every 0.2 increments of :math:`\eta_\sigma`. The `x` and `y`-axis
are the :math:`\eta_\sigma = 0`, and the diagonal is :math:`\eta_\sigma = 1`.