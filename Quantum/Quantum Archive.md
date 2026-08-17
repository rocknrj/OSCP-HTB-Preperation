- Unzip the zip file (password in download link area)
- pass the following exploit:
```
from qiskit import QuantumCircuit
from qiskit_aer import AerSimulator
circuit = QuantumCircuit.from_qasm_file('challenge_circuit.qasm')
sim = AerSimulator()
job = sim.run(circuit)
result = job.result()
counts = result.get_counts()  # extract the counts (number of times each measurement outcome was observed)
bn = int(list(counts)[0], 2)
print(int.to_bytes(bn, length=(bn.bit_length()+7)//8))
```
- Create the environment
```
python3 -m venv venv
source venv/bin/activate
pip install qiskit
pip install qiskit_aer
python3 exploit.py

---OUTPUT---
b'HTB{th3r3_1s_st1ll_s0m3_h0p3}'
```
![[Pasted image 20251208063720.png]]