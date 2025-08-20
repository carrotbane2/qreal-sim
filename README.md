# qreal-sim

This is my simulation of the QREAL procedure described in doi:10.1038/s41377-022-01039-5 ("Quantum Receiver Enhanced by Adaptive Learning", Cui et al.)
Because of the desire to use low-intensity coherent states to transmit information, there is a demand for devices that correctly identify and distinguish between such states, i.e. a quantum receiver. Since any pair of coherent states is non-orthogonal, such a device will necessarily have some non-zero error rate.
I use the quaternary phase-shift key, or QPSK, which consists of four coherent states with the same intensity and pi/2 phase difference between them. The lower the intensity, the closer the states in phase space, the greater the overlap, and the more difficult the task of discrimination becomes.

As indicated by the name, the QREAL procedure uses reinforced learning to adapt to a given noise profile. The procedure involves a tree of complex values which is dynamically applied to the state.
The quantum circuit itself proceeds as follows: an input coherent state is divided into N "slices" (either spatially, using N beam splitters, or temporally). The first slice is subject to a displacement D(\alpha), where \alpha is the root node of the tree. The slice is measured in the truncated Fock basis (|0>, |1>, ... |M-1>), where M-1 is the largest number the photon number detector can reliably measure, and the result k_1 \in {0, 1, ... M-1} is recorded .
The second slice is also subject to D(\alpha_(k_1)), where the parameter is now the k_1'th child of the root node, and so on. Thus every measurement affects all subsequent measurements. In total, the decision tree has depth N, with each node apart from the leaves having M children.

After running the circuit with a given tree Q times, one obtains ordered pairs of input states |\beta_i> and complete measurement histories x_N = (k_1, k_2, ... k_N). I can then define a mapping from measurement histories x_N to input states y as whichever input state most often gave rise to that particular measurement history (that is, the one that maximizes the probability P(x_N AND y)).

The loss function to minimize, then, is the error rate of this mapping. Since it derives from the tree, this characterizes the tree and its suitability.

The idea is that in the presence of noise, the parameters of the tree should "learn" the noise profile and optimally compensate for it. For example, if we make a list of noise profiles, and train one tree on each, and test each tree on each profile, hopefully we should see that each tree "dominates" within its native category.

For the sake of simplicity, I am constraining the noise to Gaussian phase noise. This is just the coherence-conserving transform |\beta> -> |\beta e^(i \gamma)>, where \gamma is from a normal distribution with zero mean and std between about 0 and 0.08. This is 
