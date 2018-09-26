.. nufhe documentation master file, created by
   sphinx-quickstart on Fri Aug 17 09:28:21 2018.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

NuFHE, a GPU-powered Torus FHE implementation
=============================================

.. toctree::
   :maxdepth: 2
   :caption: Contents:

   api


---------------
A short example
---------------

::

    import numpy
    import nufhe
    from reikna.cluda import any_api

    thr = any_api().Thread.create(interactive=True)

    rng = numpy.random.RandomState()
    secret_key, cloud_key = nufhe.make_key_pair(thr, rng)

    size = 32

    bits1 = rng.randint(0, 2, size=size).astype(numpy.bool)
    bits2 = rng.randint(0, 2, size=size).astype(numpy.bool)

    ciphertext1 = nufhe.encrypt(thr, rng, secret_key, bits1)
    ciphertext2 = nufhe.encrypt(thr, rng, secret_key, bits2)

    reference = ~(bits1 * bits2)

    result = nufhe.empty_ciphertext(thr, cloud_key.params, ciphertext1.shape)
    nufhe.gate_nand(thr, cloud_key, result, ciphertext1, ciphertext2)

    result_bits = nufhe.decrypt(thr, secret_key, result)
    assert (result_bits == reference).all()


Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`