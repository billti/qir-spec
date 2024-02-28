# Instruction Set

A backend that supports quantum computations is composed of multiple processing
units that include one or more quantum processors as well as classic computing
resources. A quantum program is a combination of instructions that execute on a
quantum processor, instructions that execute on an adjacent classical processor,
and data transfer between processors.

The QIR profile defines which instructions are used/supported for classical
computations and data transfer, while the Quantum Instruction Set (QIS) defines
which instructions are supported by the quantum processor. The profile and QIS
can be defined/chosen mostly independently, with the caveat that a profile
specification may contain restrictions that need to be satisfied for an QIS to
be compatible (compliant) with that profile. To target a program to a specific
backend hence requires selecting both a profile and QIS that is supported by
that backend, see also [Compilation and
Targeting](Compilation_And_Targeting.md).

## Runtime Functions

Instructions that execute on a classical processor or serve data transfer are
expressed as LLVM instructions, including calls to `__quantum__rt__*` functions.
These functions are forward declared in the IR and defined by the executing
runtime. Which LLVM instructions and runtime functions are used/supported is
captured in the QIR profile specification. The complete list of all runtime
functions that are needed for executing QIR programs independent on which
profile they have been compiled for is given in the Library_Reference.
Most backends do not support the full set of runtime functions but merely a
subset that is sufficient for the supported QIR profile(s). The [Base
Profile](profiles/Base_Profile.md) defines the minimal set of runtime functions
that need to be supported for by a backend to permit computations that make use
of qubits and measurements of qubits. Additional functions may be needed to
support the use of data types beyond those for qubit and measurement result
values.

## Quantum Instruction Set (QIS)

The table below lists known quantum instructions along with their signatures and
a description of their functionality. Backends are **not** required to support
all of these. Instead, each backend will declare which of these instructions it
supports. We encourage to make instructions that are supported by a context
independent implementation in terms of other instructions as a library rather
than listing them as part of the backend specification. A library provided
either in the form of a bitcode file can be linked in as part of a QIR
compilation stage.

QIR does not specify the contents of the quantum instruction set. However, in
order to ensure some amount of uniformity, implementations that provide any of
the following quantum instructions must match the specified definition:

Who should add a function to the list of qis naming conventions and when?
-> backend providers, asap; meaning even speculative ones should be added.

| Operation Name | LLVM Function Declaration  | Description | Matrix |
|----------------|----------------------------|-------------|--------|
| CCx, CCNOT, Toffoli | `__quantum__qis__ccx__body (%Qubit* control1, %Qubit* control1, %Qubit* target)` | Toffoli or doubly-controlled X | $\begin{bmatrix} 1 & 0 & 0 & 0 & 0 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 & 0 & 0 & 0 & 0 \\ 0 & 0 & 1 & 0 & 0 & 0 & 0 & 0 \\ 0 & 0 & 0 & 1 & 0 & 0 & 0 & 0 \\ 0 & 0 & 0 & 0 & 1 & 0 & 0 & 0 \\ 0 & 0 & 0 & 0 & 0 & 1 & 0 & 0 \\ 0 & 0 & 0 & 0 & 0 & 0 & 0 & 1 \\ 0 & 0 & 0 & 0 & 0 & 0 & 1 & 0 \\ \end{bmatrix}$ |
| Cx, CNOT | `__quantum__qis__cx__body (%Qubit* control, %Qubit* target)` | CNOT or singly-controlled X | $\begin{bmatrix} 1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 0 & 1 \\ 0 & 0 & 1 & 0 \\ \end{bmatrix}$ |
| Cz | `__quantum__qis__cz__body (%Qubit* control, %Qubit* target)` | Singly-controlled Z | $\begin{bmatrix} 1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & -1 \\ \end{bmatrix}$ |
| H | `__quantum__qis__h__body (%Qubit* target)` | Hadamard | $\frac{1}{\sqrt{2}}\begin{bmatrix} 1 & 1 \\ 1 & -1 \\ \end{bmatrix}$ |
| Mz or Measure | `__quantum__qis__mz__body (%Qubit* target, %Result* result)` | Measure a qubit along the the Pauli Z axis |
| Reset | `__quantum__qis__reset__body (%Qubit* target)` | Prepare a qubit in the \|0⟩ state |
| Rx | `__quantum__qis__rx__body (%Qubit* target, double theta)` | Rotate a qubit around the Pauli X axis | $\begin{bmatrix} \cos \frac {\theta} {2} & -i\sin \frac {\theta} {2} \\ -i\sin \frac {\theta} {2} & \cos \frac {\theta} {2} \\ \end{bmatrix}$ |
| Ry | `__quantum__qis__ry__body (%Qubit* target, double theta)` | Rotate a qubit around the Pauli Y axis | $\begin{bmatrix} \cos \frac {\theta} {2} & -\sin \frac {\theta} {2} \\ \sin \frac {\theta} {2} & \cos \frac {\theta} {2} \\ \end{bmatrix}$ |
| Rz | `__quantum__qis__rz__body (%Qubit* target, double theta)` | Rotate a qubit around the Pauli Z axis | $\begin{bmatrix} e^{-i \theta/2} & 0 \\ 0 & e^{i \theta/2} \\ \end{bmatrix}$ |
| S | `__quantum__qis__s__body (%Qubit* target)` | S (phase gate)  | $\begin{bmatrix} 1 & 0 \\ 0 & i \\ \end{bmatrix}$ |
| S&dagger; | `__quantum__qis__s_adj (%Qubit* target)` | The adjoint of S | $\begin{bmatrix} 1 & 0 \\ 0 & -i \\ \end{bmatrix}$ |
| T | `__quantum__qis__t__body (%Qubit* target)` | T | $\begin{bmatrix} 1 & 0 \\ 0 & e^{i\pi/4} \\ \end{bmatrix}$ |
| T&dagger; | `__quantum__qis__t__adj (%Qubit* target)` | The adjoint of T operation | $\begin{bmatrix} 1 & 0 \\ 0 & e^{-i\pi/4} \\ \end{bmatrix}$ |
| X | `__quantum__qis__x__body (%Qubit* target)` | Pauli X | $\begin{bmatrix} 0 & 1 \\ 1 & 0 \\ \end{bmatrix}$ |
| Y | `__quantum__qis__y__body (%Qubit* target)` | Pauli Y | $\begin{bmatrix} 0 & -i \\ i & 0 \\ \end{bmatrix}$ |
| Z | `__quantum__qis__z__body (%Qubit* target)` | Pauli Z | $\begin{bmatrix} 1 & 0 \\ 0 & -1 \\ \end{bmatrix}$ |
