.. note::
    :class: sphx-glr-download-link-note

    Click :ref:`here <sphx_glr_download_auto_examples_hh_psc_alpha.py>` to download the full example code
.. rst-class:: sphx-glr-example-title

.. _sphx_glr_auto_examples_hh_psc_alpha.py:

Example using Hodgkin-Huxley neuron
----------------------------------------

This example produces a rate-response (FI) curve of the Hodgkin-Huxley
neuron ``hh_psc_alpha`` in response to a range of different current (DC) stimulations.
The result is plotted using matplotlib.

Since a DC input affetcs only the neuron's channel dynamics, this routine
does not yet check correctness of synaptic response.


.. code-block:: default


    import nest
    import numpy as np
    import matplotlib.pyplot as plt

    nest.set_verbosity('M_WARNING')
    nest.ResetKernel()

    simtime = 1000

    # Amplitude range, in pA
    dcfrom = 0
    dcstep = 20
    dcto = 2000

    h = 0.1  # simulation step size in mS

    neuron = nest.Create('hh_psc_alpha')
    sd = nest.Create('spike_detector')

    nest.SetStatus(sd, {'to_memory': False})

    nest.Connect(neuron, sd, syn_spec={'weight': 1.0, 'delay': h})

    # Simulation loop
    n_data = int(dcto / float(dcstep))
    amplitudes = np.zeros(n_data)
    event_freqs = np.zeros(n_data)
    for i, amp in enumerate(range(dcfrom, dcto, dcstep)):
        nest.SetStatus(neuron, {'I_e': float(amp)})
        print("Simulating with current I={} pA".format(amp))
        nest.Simulate(1000)  # one second warm-up time for equilibrium state
        nest.SetStatus(sd, {'n_events': 0})  # then reset spike counts
        nest.Simulate(simtime)  # another simulation call to record firing rate

        n_events = nest.GetStatus(sd, keys={'n_events'})[0][0]
        amplitudes[i] = amp
        event_freqs[i] = n_events / (simtime / 1000.)

    plt.plot(amplitudes, event_freqs)
    plt.show()


.. rst-class:: sphx-glr-timing

   **Total running time of the script:** ( 0 minutes  0.000 seconds)


.. _sphx_glr_download_auto_examples_hh_psc_alpha.py:


.. only :: html

 .. container:: sphx-glr-footer
    :class: sphx-glr-footer-example



  .. container:: sphx-glr-download

     :download:`Download Python source code: hh_psc_alpha.py <hh_psc_alpha.py>`



  .. container:: sphx-glr-download

     :download:`Download Jupyter notebook: hh_psc_alpha.ipynb <hh_psc_alpha.ipynb>`


.. only:: html

 .. rst-class:: sphx-glr-signature

    `Gallery generated by Sphinx-Gallery <https://sphinx-gallery.github.io>`_
