# Computação Quântica
Neste tutorial, você será introduzido aos conceitos fundamentais da programação para computação quântica, proporcionando uma base sólida para explorar este campo emergente. Abordaremos os princípios essenciais, como qubits, operadores quânticos, medições, e algoritmos quânticos, além de explorar as principais ferramentas, plataformas e computadores quânticos disponíveis atualmente. Utilizando a plataforma Qiskit da IBM, você acompanhará exemplos práticos que ilustram os primeiros passos nesse universo desafiador, permitindo que você compreenda como aplicar esses conceitos em cenários reais de computação quântica. Este tutorial é ideal para quem deseja dar os primeiros passos na programação quântica e começar a desvendar as possibilidades dessa tecnologia revolucionária.

## Ferramentas Utilizadas:
<div>
<img src="https://img.shields.io/badge/Qiskit-6929C4.svg?style=for-the-badge&logo=Qiskit&logoColor=white" />
<img src="https://img.shields.io/badge/Python-3776AB.svg?style=for-the-badge&logo=Python&logoColor=white" />
<img src="https://img.shields.io/badge/VS_Code-007ACC?logo=visual-studio-code&logoColor=white&style=for-the-badge" />
</div>

```python
from qiskit import QuantumCircuit, Aer
from qiskit.visualization import
plot_bloch_multivector, plot_histogram
from qiskit.quantum_info import Statevector
from math import pi
# unárias
qc = QuantumCircuit(1)
qc.h(0)
qc.x(0)
qc.y(0)
qc.z(0)
qc.p(pi/4, 0)
qc.s(0)
qc.t(0)
qc.i(0)
# binárias
qc = QuantumCircuit(2)
qc.x(0)
qc.swap(0, 1)
qc.cx(0, 1)
qc.cy(0, 1)
qc.cz(0, 1)
# ternários
qc = QuantumCircuit(3)
qc.ccx(0, 1, 2)
qc.cswap(0, 1, 2)
# desenha o circuito
display(qc.draw(initial_state=True))
# plota na esfera de Bloch
state = Statevector.from_instruction(qc)
display(plot_bloch_multivector(state))
# adiciona medição
qc.measure_all()
# executa no simulador
sim = Aer.get_backend('aer_simulator')
result = sim.run(qc).result()
counts = result.get_counts()
display(plot_histogram(counts))
``

```python
from qiskit import QuantumCircuit, Aer, transpile, IBMQ
from qiskit.providers.ibmq import least_busy
from qiskit.visualization import plot_bloch_multivector, plot_histogram
from qiskit.quantum_info import Statevector
from math import pi
# cria o circuito
qc = QuantumCircuit(4, 2)
# codifica as entradas A e B nos qubits 0 e 1
qc.x(0) # Se A=1, usa a porta X
qc.x(1) # Se B=1, usa a porta X
# barreira para facilitar visualização
qc.barrier()
# operação Sum utilizando portas CNOT com saída no qubit 2
qc.cx(0,2)
qc.cx(1,2)
# operação Carry utilizando porta Toffoli com saída no qubit 3
qc.ccx(0,1,3)
# barreira para facilitar visualização
qc.barrier()
# cria os operadores de medição para extrair os resultados
qc.measure(2,0) # Sum
qc.measure(3,1) # Carry
# desenha o circuito
qc.draw(initial_state=True)
```

## Enviando para o computador Quântico
```python
# executa no protótipo quântico real
IBMQ.load_account()
provider = IBMQ.get_provider(hub='ibm-q')
backend = least_busy(provider.backends(filters=lambda x: x.configuration().n_qubits >= 4 and
not x.configuration().simulator and x.status().operational==True))
# Transpila e executa
shots = 1024
transpiled_bv_circuit = transpile(qc, backend)
job = backend.run(transpiled_bv_circuit, shots=shots)
# obtém os resultados
results = job.result()
device_counts = results.get_counts()
# mostra os resultados
plot_histogram(device_counts)
``
