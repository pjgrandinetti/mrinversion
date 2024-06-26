
#####################################
Welcome to Mrinversion documentation!
#####################################

.. only:: html

    .. cssclass:: table-bordered table-striped centered

    .. list-table::
      :widths: 25 75
      :header-rows: 0

      * - Deployment
        - .. image:: https://img.shields.io/pypi/v/mrinversion.svg?style=flat&logo=pypi&logoColor=white
            :target: https://pypi.python.org/pypi/mrinversion
            :alt: PyPI version

          .. image:: https://img.shields.io/pypi/pyversions/mrinversion
            :alt: PyPI - Python Version

      * - Build Status
        - .. image:: https://img.shields.io/github/actions/workflow/status/deepanshs/mrinversion/continuous-integration-pip.yml?logo=GitHub
            :target: https://github.com/DeepanshS/mrinversion/actions/workflow/continuous-integration-pip.yml
            :alt: GitHub Workflow Status

          .. image:: https://readthedocs.org/projects/mrinversion/badge/?version=latest
            :target: https://mrinversion.readthedocs.io/en/latest/?badge=latest
            :alt: Documentation Status

      * - License
        - .. image:: https://img.shields.io/badge/License-BSD%203--Clause-blue.svg
            :target: https://opensource.org/licenses/BSD-3-Clause
            :alt: License

      * - Metrics
        - .. image:: https://codecov.io/gh/DeepanshS/mrinversion/branch/master/graph/badge.svg
            :target: https://codecov.io/gh/DeepanshS/mrinversion

          .. image:: https://img.shields.io/github/issues-raw/deepanshs/mrinversion
            :target: https://github.com/DeepanshS/mrinversion/issues
            :alt: GitHub issues

.. - .. image:: https://img.shields.io/github/contributors/DeepanshS/mrinversion.svg?logo=github
..     :target: https://github.com/DeepanshS/mrinversion/graphs/contributors
..     :alt: GitHub contributors

----

**About**

The ``mrinversion`` python package is based on the statistical learning technique for
determining the underlying distribution of the magnetic resonance (NMR) parameters.

The library utilizes the `mrsimulator <https://mrsimulator.readthedocs.io/en/stable/>`_
package for generating solid-state NMR spectra and
`scikit-learn <https://scikit-learn.org/stable/>`_ package for statistical learning.

----

**Features**

The ``mrinversion`` package includes

- **Spectral Inversion**: Two-dimensional solid-state NMR spectrum of dilute spin-systems
  correlating the isotropic to anisotropic frequencies to a three-dimensional distribution
  of tensor parameters. Presently, we support the inversion of

  - **Magic angle turning (MAT), Phase adjusted spinning sidebands (PASS)**, and similar
    spectra correlating the isotropic chemical shift resonances to pure anisotropic
    spinning sideband resonances into a three-dimensional distribution of
    nuclear shielding tensor parameters, :math:`\rho(\delta_\text{iso}, \zeta_\sigma, \eta_\sigma)`,
    where :math:`\delta_\text{iso}` is the isotropic chemical shift, and :math:`\zeta_\sigma`
    and :math:`\eta_\sigma`, are the shielding anisotropy and asymmetry parameters,
    respectively, defined using the Haeberlen convention.

  - **Magic angle flipping (MAF)** spectra correlating the isotropic chemical shift
    resonances to pure anisotropic resonances into a three-dimensional distribution of
    nuclear shielding tensor parameters, :math:`\rho(\delta_\text{iso}, \zeta_\sigma, \eta_\sigma)`,
    where :math:`\delta_\text{iso}` is the isotropic chemical shift, and :math:`\zeta_\sigma`
    and :math:`\eta_\sigma`, are the shielding anisotropy and asymmetry parameters,
    respectively, defined using the Haeberlen convention.

- **Relaxometry Inversion**: Inversion of NMR relaxometry measurements to the distribution of
  relaxation parameters (T1, T2).

.. only:: html

    .. raw:: html

        <br>

    **View our example gallery**

    .. image:: https://img.shields.io/badge/View-Example%20Gallery-Purple?s=small
        :target: galley_examples/index.html

----



Getting Started
---------------

.. toctree::
    :maxdepth: 1
    :caption: User Documentation

    installation/install
    introduction
    before_getting_started
    getting_started_lineshape
    getting_started_relaxation


Examples
--------

.. toctree::
    :maxdepth: 1
    :caption: Examples

    galley_examples/index

Project details
---------------

.. toctree::
    :maxdepth: 1
    :caption: Project details

    referenceAPI
    changelog
    credits/license
    credits/acknowledgment

How to cite
-----------

If you use this work in your publication, please cite the following.

- Srivastava, D. J.; Grandinetti P. J., Statistical learning of NMR tensors from 2D
  isotropic/anisotropic correlation nuclear magnetic resonance spectra, J. Chem. Phys.
  **153**, 134201 (2020). https://doi.org/10.1063/5.0023345.

- Deepansh J. Srivastava, Maxwell Venetos, Philip J. Grandinetti, Shyam Dwaraknath, & Alexis McCarthy. (2021, May 26). mrsimulator: v0.6.0 (Version v0.6.0). Zenodo. http://doi.org/10.5281/zenodo.4814638

Additionally, if you use the CSDM data model, please consider citing

- Srivastava DJ, Vosegaard T, Massiot D, Grandinetti PJ (2020) Core Scientific Dataset Model: A lightweight and portable model and file format for multi-dimensional scientific data. PLOS ONE 15(1): e0225953. https://doi.org/10.1371/journal.pone.0225953

----

.. only:: html

    Indices and tables
    ^^^^^^^^^^^^^^^^^^

    * :ref:`genindex`
    * :ref:`modindex`
    * :ref:`search`
