:orphan:

# Release 0.31.0-dev (development release)

<h3>New features since last release</h3>

* Added a new function `qml.ops.functions.bind_new_parameters` that creates a copy of an operator with new parameters
  without mutating the original operator.
  [(#4113)](https://github.com/PennyLaneAI/pennylane/pull/4113)

* Added the `TRX` qutrit rotation operator, which allows applying a Pauli X rotation on a
  given subspace.
  [(#2845)](https://github.com/PennyLaneAI/pennylane/pull/2845)

<h3>Improvements 🛠</h3>

* `DiagonalQubitUnitary` now decomposes into `RZ`, `IsingZZ` and `MultiRZ` gates
  instead of a `QubitUnitary` operation with a dense matrix.
  [(#4035)](https://github.com/PennyLaneAI/pennylane/pull/4035)
  
* The Jax-JIT interface now uses symbolic zeros to determine trainable parameters.
  [(4075)](https://github.com/PennyLaneAI/pennylane/pull/4075)

* Accelerate Jordan-Wigner transforms caching Pauli gate objects.
  [(#4046)](https://github.com/PennyLaneAI/pennylane/pull/4046)

* The `qchem.molecular_hamiltonian` function is upgraded to support custom wires for constructing
  differentiable Hamiltonians. The zero imaginary component of the Hamiltonian coefficients are
  removed.
  [(#4050)](https://github.com/PennyLaneAI/pennylane/pull/4050)
  [(#4094)](https://github.com/PennyLaneAI/pennylane/pull/4094)

* An error is now raised by `qchem.molecular_hamiltonian` when the `dhf` method is used for an 
  open-shell system. This duplicates a similar error in `qchem.Molecule` but makes it easier to
  inform the users that the `pyscf` backend can be used for open-shell calculations.
  [(#4058)](https://github.com/PennyLaneAI/pennylane/pull/4058)

* Added a `shots` property to `QuantumScript`. This will allow shots to be tied to executions instead of devices more
  concretely.
  [(#4067)](https://github.com/PennyLaneAI/pennylane/pull/4067)
  [(#4103)](https://github.com/PennyLaneAI/pennylane/pull/4103)
  [(#4106)](https://github.com/PennyLaneAI/pennylane/pull/4106)
  [(#4112)](https://github.com/PennyLaneAI/pennylane/pull/4112)

* Integrated `QuantumScript.shots` with `QNode` so that shots are placed on the `QuantumScript`
  during `QNode` construction.
  [(#4110)](https://github.com/PennyLaneAI/pennylane/pull/4110)

* `qml.specs` is compatible with custom operations that have `depth` bigger than 1.
  [(#4033)](https://github.com/PennyLaneAI/pennylane/pull/4033)

* `qml.Tracker` when used with `default.qubit` or `null.qubit` devices, will track resources of the quantum circuit.
  [#(4045)](https://github.com/PennyLaneAI/pennylane/pull/4045)
  [(#4110)](https://github.com/PennyLaneAI/pennylane/pull/4110)

* `qml.prod` now accepts a single qfunc input for creating new `Prod` operators.
  [(#4011)](https://github.com/PennyLaneAI/pennylane/pull/4011)

* Added `__repr__` and `__str__` methods to the `Shots` class.
  [(#4081)](https://github.com/PennyLaneAI/pennylane/pull/4081)

* Added `__eq__` and `__hash__` methods to the `Shots` class.
  [(#4082)](https://github.com/PennyLaneAI/pennylane/pull/4082)

* Added a function `measure_with_samples` that returns a sample-based measurement result given a state
  [(#4083)](https://github.com/PennyLaneAI/pennylane/pull/4083)
  [(#4093)](https://github.com/PennyLaneAI/pennylane/pull/4093)

* Wrap all objects being queued in an `AnnotatedQueue` so that `AnnotatedQueue` is not dependent on
  the hash of any operators/measurement processes.
  [(#4087)](https://github.com/PennyLaneAI/pennylane/pull/4087)

* All drawing methods changed their default value for the keyword argument `show_matrices`
  to `True`. This allows quick insights into broadcasted tapes for example.
  [(#3920)](https://github.com/PennyLaneAI/pennylane/pull/3920)

* Support for adjoint differentiation has been added to the `DefaultQubit2` device.
  [(#4037)](https://github.com/PennyLaneAI/pennylane/pull/4037)

* Support for sample-based measurements has been added to the `DefaultQubit2` device.
  [(#4105)](https://github.com/PennyLaneAI/pennylane/pull/4105)
  [(#4114)](https://github.com/PennyLaneAI/pennylane/pull/4114)
  [(#4133)](https://github.com/PennyLaneAI/pennylane/pull/4133)

* Added a keyword argument `seed` to the `DefaultQubit2` device.
  [(#4120)](https://github.com/PennyLaneAI/pennylane/pull/4120)

* Added a `dense` keyword to `ParametrizedEvolution` that allows forcing dense or sparse matrices.
  [(#4079)](https://github.com/PennyLaneAI/pennylane/pull/4079)
  [(#4095)](https://github.com/PennyLaneAI/pennylane/pull/4095)

* Adds the Type variables `pennylane.typing.Result` and `pennylane.typing.ResultBatch` for type hinting the result of
  an execution.
  [(#4018)](https://github.com/PennyLaneAI/pennylane/pull/4108)

* `qml.devices.ExecutionConfig` no longer has a `shots` property, as it is now on the `QuantumScript`.  It now has a `use_device_gradient` property. `ExecutionConfig.grad_on_execution = None` indicates a request for `"best"`, instead of a string.
[(#4102)](https://github.com/PennyLaneAI/pennylane/pull/4102)

* `DefaultQubit2.preprocess` now returns a new `ExecutionConfig` object with decisions for `gradient_method`,
  `use_device_gradient`, and `grad_on_execution`.
  [(#4102)](https://github.com/PennyLaneAI/pennylane/pull/4102)

* `pulse.ParametrizedEvolution` now uses _batched_ compressed sparse row (`BCSR`) format. This allows computing Jacobians of the unitary directly even when `dense=False`.
  ```python
  def U(params):
      H = jnp.polyval * qml.PauliZ(0) # time dependent Hamiltonian
      Um = qml.evolve(H)(params, t=10., dense=False)
      return qml.matrix(Um)
  params = jnp.array([[0.5]], dtype=complex)
  jac = jax.jacobian(U, holomorphic=True)(params)
  ```
  [(#4126)](https://github.com/PennyLaneAI/pennylane/pull/4126)

* Updated `pennylane/qnode.py` to support parameter-shift differentiation on qutrit devices.
  ([#2845])(https://github.com/PennyLaneAI/pennylane/pull/2845)

* The new device interface in integrated with `qml.execute` for autograd, backpropagation, and no differentiation.
  [(#3903)](https://github.com/PennyLaneAI/pennylane/pull/3903)

* `CZ` now inherits from the `ControlledOp` class. It now supports exponentiation to arbitrary powers with `pow`, which is no longer limited to integers. It also supports `sparse_matrix` and `decomposition` representations.
  [(#4117)](https://github.com/PennyLaneAI/pennylane/pull/4117)

* The construction of the pauli representation for the `Sum` class is now faster.
  [(#4142)](https://github.com/PennyLaneAI/pennylane/pull/4142)

<h3>Breaking changes 💔</h3>

* All drawing methods changed their default value for the keyword argument `show_matrices` to `True`.
  [(#3920)](https://github.com/PennyLaneAI/pennylane/pull/3920)

* `DiagonalQubitUnitary` does not decompose into `QubitUnitary` any longer, but into `RZ`, `IsingZZ`
  and `MultiRZ` gates.
  [(#4035)](https://github.com/PennyLaneAI/pennylane/pull/4035)

* Jax trainable parameters are now `Tracer` instead of `JVPTracer`, it is not always the right definition for the JIT 
  interface, but we update them in the custom JVP using symbolic zeros.
  [(4075)](https://github.com/PennyLaneAI/pennylane/pull/4075)

* The experimental Device interface `qml.devices.experimental.Device` now requires that the `preprocess` method
  also returns an `ExecutionConfig` object. This allows the device to choose what `"best"` means for various
  hyperparameters like `gradient_method` and `grad_on_execution`.
  [(#4007)](https://github.com/PennyLaneAI/pennylane/pull/4007)
  [(#4102)](https://github.com/PennyLaneAI/pennylane/pull/4102)

* Gradient transforms with Jax do not support `argnum` anymore,  `argnums` needs to be used.
  [(#4076)](https://github.com/PennyLaneAI/pennylane/pull/4076)

* `pennylane.collections`, `pennylane.op_sum`, and `pennylane.utils.sparse_hamiltonian` are removed.

<h3>Deprecations 👋</h3>

* `Operation.base_name` is deprecated. Please use `Operation.name` or `type(op).__name__` instead.

* ``QuantumScript``'s ``name`` keyword argument and property are deprecated.
  This also affects ``QuantumTape`` and ``OperationRecorder``.
  [(#4141)](https://github.com/PennyLaneAI/pennylane/pull/4141)

* `qml.grouping` module is removed. The functionality has been reorganized in the `qml.pauli` module.

<h3>Documentation 📝</h3>

* The description of `mult` in the `qchem.Molecule` docstring now correctly states the value
  of `mult` that is supported.
  [(4058)](https://github.com/PennyLaneAI/pennylane/pull/4058)

<h3>Bug fixes 🐛</h3>

* Removes a patch in `interfaces/autograd.py` that checks for the `strawberryfields.gbs` device.  That device
  is pinned to PennyLane <= v0.29.0, so that patch is no longer necessary.

<h3>Contributors ✍️</h3>

This release contains contributions from (in alphabetical order):

Isaac De Vlugt,
Soran Jahangiri,
Edward Jiang,
Korbinian Kottmann,
Christina Lee,
Vincent Michaud-Rioux,
Romain Moyard,
Mudit Pandey,
Borja Requena,
Matthew Silverman,
Jay Soni,
David Wierichs.
