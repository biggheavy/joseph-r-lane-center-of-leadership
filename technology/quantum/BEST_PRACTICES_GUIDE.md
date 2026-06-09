# Quantum Computing Best Practices Guide

## 1. Circuit Design Best Practices

### Gate Count Optimization
```python
# ❌ INEFFICIENT - High gate count
def inefficient_circuit(qubits):
    circuit = QuantumCircuit(qubits)
    circuit.h(0)
    circuit.x(0)
    circuit.x(0)  # Unnecessary
    circuit.h(0)
    return circuit

# ✅ EFFICIENT - Optimized
def efficient_circuit(qubits):
    circuit = QuantumCircuit(qubits)
    # Single H gate achieves same result
    circuit.h(0)
    return circuit
```

### Circuit Depth Reduction
```python
# ✅ Best Practice: Parallelize gates
def optimal_circuit(n_qubits):
    circuit = QuantumCircuit(n_qubits)
    
    # Apply all single-qubit gates in parallel (depth 1)
    for i in range(n_qubits):
        circuit.h(i)
    
    # Apply two-qubit gates efficiently
    for i in range(0, n_qubits-1, 2):
        circuit.cx(i, i+1)
    for i in range(1, n_qubits-1, 2):
        circuit.cx(i, i+1)
```

### Native Gate Sets
```python
# ✅ Use native gates for target backend
from qiskit.transpiler import passes

def transpile_for_hardware(circuit, backend):
    """Transpile to native gate set"""
    from qiskit import transpile
    
    # Get native gates
    native_gates = backend.configuration().basis_gates
    
    # Transpile
    return transpile(
        circuit,
        backend,
        basis_gates=native_gates,
        optimization_level=3  # Maximum optimization
    )
```

### Qubit Mapping Optimization
```python
# ✅ Efficient qubit mapping
def optimize_qubit_layout(circuit, backend):
    """Find optimal qubit layout"""
    from qiskit.transpiler import Layout
    from qiskit.transpiler.passes import SetLayout, VF2Layout
    
    layout = VF2Layout(backend)
    layout.run(circuit)
    
    return layout
```

---

## 2. Algorithm Selection Guide

### When to Use QAOA
**Pros:**
- ✅ Handles discrete optimization
- ✅ Combines classical & quantum
- ✅ Good for scheduling problems
- ✅ Proven performance

**Cons:**
- ❌ Requires many iterations
- ❌ Sensitive to parameters
- ❌ May get stuck in local minima

**Use Cases:**
```python
# Schedule optimization
# Resource allocation
# Max-cut problems
# Traveling salesman problem
# Graph coloring
```

### When to Use Grover's Algorithm
**Pros:**
- ✅ √N speedup over classical
- ✅ Simple implementation
- ✅ Good for search problems

**Cons:**
- ❌ Limited practical advantage
- ❌ Requires quantum oracle
- ❌ Doesn't work well with noise

**Use Cases:**
```python
# Constraint satisfaction
# Database search
# Pattern matching
# Solution verification
```

### When to Use VQE
**Pros:**
- ✅ Hybrid approach
- ✅ Noise tolerant
- ✅ Good for chemistry
- ✅ Near-term feasible

**Cons:**
- ❌ Requires classical optimization
- ❌ Many parameters to tune
- ❌ Slow convergence

**Use Cases:**
```python
# Molecular simulation
# Ground state energy
# Financial modeling
# Complex optimization
```

### Algorithm Comparison Table

| Algorithm | Problem Type | Speedup | Qubits Needed | Gate Depth | Noise Tolerance |
|-----------|-------------|---------|---------------|-----------|-----------------|
| QAOA | Combinatorial | 2-3x | 20-100 | High | Medium |
| Grover | Search | √N | 10-50 | Medium | Low |
| VQE | Eigenvalue | N/A | 10-100 | Medium | High |
| HHL | Linear Systems | Exponential | 50+ | Very High | Low |

---

## 3. Error Mitigation Techniques

### Measurement Error Mitigation
```python
from qiskit.utils import QuantumInstance
from qiskit_aer.utils import MeasurementErrorMitigation

def mitigate_measurement_errors(circuit, backend, shots=1024):
    """Apply measurement error mitigation"""
    
    # Get measurement error mitigation object
    meas_filter = MeasurementErrorMitigation(backend)
    
    # Execute circuit
    job = backend.run(circuit, shots=shots)
    result = job.result()
    
    # Apply mitigation
    mitigated_counts = meas_filter.apply(result.get_counts())
    
    return mitigated_counts
```

### Zero-Noise Extrapolation
```python
def zero_noise_extrapolation(circuit, backend):
    """Extrapolate to zero noise condition"""
    
    # Run at different noise levels
    results = []
    for noise_factor in [1.0, 1.5, 2.0]:
        # Apply noise scaling
        scaled_circuit = apply_noise_scaling(circuit, noise_factor)
        
        job = backend.run(scaled_circuit)
        result = job.result()
        results.append(result)
    
    # Extrapolate to zero noise
    zero_noise_result = extrapolate_results(results)
    
    return zero_noise_result

def apply_noise_scaling(circuit, factor):
    """Scale circuit noise by factor"""
    from qiskit.circuit import Gate
    
    scaled = circuit.copy()
    
    # Apply extra gates to increase noise
    if factor > 1.0:
        for _ in range(int((factor - 1) * len(scaled))):
            scaled.i(0)  # Identity gates add noise
    
    return scaled
```

### Probabilistic Error Cancellation
```python
def probabilistic_error_cancellation(circuit, backend):
    """Cancel errors probabilistically"""
    
    # Get calibration data
    calibration = backend.defaults()
    
    # Decompose circuit with error correction
    corrected_circuits = []
    for _ in range(4):  # Multiple runs with different corrections
        corrected = add_error_correction(circuit, calibration)
        corrected_circuits.append(corrected)
    
    # Execute all
    results = []
    for circ in corrected_circuits:
        job = backend.run(circ)
        results.append(job.result())
    
    # Combine results
    combined = combine_error_corrected_results(results)
    
    return combined

def add_error_correction(circuit, calibration):
    """Add error correction based on calibration"""
    corrected = circuit.copy()
    
    # Insert correction gates
    # This would use actual calibration data
    
    return corrected
```

### Active Reset
```python
def active_reset_circuit(circuit):
    """Add active reset to initialize qubits"""
    
    # Measure all qubits
    circuit.measure_all()
    
    # Reset measurement
    from qiskit.circuit import Reset
    for qubit in circuit.qubits:
        circuit.append(Reset(), [qubit])
    
    return circuit
```

---

## 4. Performance Optimization

### Parameter Optimization Strategy
```python
from scipy.optimize import minimize
import numpy as np

class ParameterOptimizer:
    def __init__(self, circuit_template, backend, target_hamiltonian):
        self.circuit = circuit_template
        self.backend = backend
        self.hamiltonian = target_hamiltonian
    
    def optimize(self, initial_params):
        """Optimize parameters using COBYLA"""
        
        def objective(params):
            # Create circuit with parameters
            circuit = self.circuit.bind_parameters(dict(
                zip(self.circuit.parameters, params)
            ))
            
            # Execute
            job = self.backend.run(circuit, shots=1024)
            result = job.result()
            counts = result.get_counts()
            
            # Calculate expectation value
            expectation = self._calculate_expectation(counts)
            
            return expectation
        
        # Minimize
        result = minimize(
            objective,
            initial_params,
            method='COBYLA',
            options={'maxiter': 100}
        )
        
        return result.x

    def _calculate_expectation(self, counts):
        """Calculate energy expectation value"""
        total = 0
        for bitstring, count in counts.items():
            energy = self.hamiltonian.eval_expectation(bitstring)
            total += energy * count / sum(counts.values())
        
        return total
```

### Ansatz Design
```python
# ✅ GOOD: Shallow ansatz for NISQ
def shallow_ansatz(n_qubits, depth=2):
    """Shallow circuit for noisy hardware"""
    from qiskit.circuit import ParameterVector
    
    params = ParameterVector('θ', depth * n_qubits)
    circuit = QuantumCircuit(n_qubits)
    
    for layer in range(depth):
        # Single qubit rotations
        for i in range(n_qubits):
            circuit.ry(params[layer * n_qubits + i], i)
        
        # Light entanglement
        for i in range(n_qubits - 1):
            circuit.cx(i, i + 1)
    
    return circuit

# ⚠️ AVOID: Deep ansatz for noisy hardware
def deep_ansatz_avoid(n_qubits, depth=10):
    """Deep circuit - avoid for noisy hardware"""
    # Too many gates = too much noise accumulation
    pass
```

### Initialization Strategies
```python
def smart_initialization(problem_data):
    """Initialize parameters using problem insights"""
    
    # Classical pre-processing
    classical_solution = solve_classically_approximate(problem_data)
    
    # Map to quantum parameters
    initial_params = extract_parameters(classical_solution)
    
    return initial_params

def solve_classically_approximate(problem):
    """Use classical algorithm for initialization"""
    from scipy.optimize import minimize
    
    def classical_objective(x):
        # Classical approximation of quantum objective
        return evaluate_classical(x, problem)
    
    result = minimize(classical_objective, x0=np.random.rand(10))
    
    return result.x
```

---

## 5. Hybrid Classical-Quantum Workflows

### Classical Preprocessing
```python
def hybrid_pipeline(problem):
    """Classical + Quantum pipeline"""
    
    # Step 1: Classical preprocessing
    simplified_problem = classical_preprocessing(problem)
    print(f"Problem size reduced: {len(problem)} → {len(simplified_problem)}")
    
    # Step 2: Quantum optimization
    quantum_solution = quantum_optimization(simplified_problem)
    
    # Step 3: Classical postprocessing
    final_solution = classical_postprocessing(quantum_solution)
    
    return final_solution

def classical_preprocessing(problem):
    """Reduce problem size classically"""
    # Constraint propagation
    # Variable elimination
    # Upper/lower bound tightening
    return problem

def classical_postprocessing(quantum_solution):
    """Improve quantum solution classically"""
    # Local search improvement
    # Constraint satisfaction
    # Feasibility verification
    return quantum_solution
```

### Resource Allocation
```python
class HybridOptimizer:
    def __init__(self, quantum_backend, budget_seconds=300):
        self.backend = quantum_backend
        self.time_budget = budget_seconds
    
    def solve(self, problem):
        """Allocate time between quantum and classical"""
        
        # 40% time: Classical heuristic
        classical_start = time.time()
        classical_solution = self._classical_heuristic(problem)
        classical_time = time.time() - classical_start
        
        # 40% time: Quantum optimization
        quantum_start = time.time()
        quantum_solution = self._quantum_optimize(
            problem,
            max_time=self.time_budget * 0.4
        )
        quantum_time = time.time() - quantum_start
        
        # 20% time: Local search refinement
        best_solution = self._local_search(
            quantum_solution,
            max_time=self.time_budget * 0.2
        )
        
        return best_solution
    
    def _classical_heuristic(self, problem):
        """Classical greedy solution"""
        pass
    
    def _quantum_optimize(self, problem, max_time):
        """Quantum optimization with time limit"""
        pass
    
    def _local_search(self, solution, max_time):
        """2-opt or similar local search"""
        pass
```

---

## 6. Debugging & Troubleshooting

### Common Issues & Solutions

#### Issue 1: High Circuit Depth
```python
# ❌ Problem
circuit.h(0).cx(0, 1).h(0).cx(0, 1).h(0)  # Depth = 5

# ✅ Solution
from qiskit.transpiler import passes
pm = passes.PassManager([passes.CommutativeCancellation()])
optimized = pm.run(circuit)  # Reduces to minimal depth
```

#### Issue 2: Poor Measurement Results
```python
# ✅ Solution 1: Increase shots
job = backend.run(circuit, shots=4096)  # More shots

# ✅ Solution 2: Apply error mitigation
from qiskit.utils import MeasurementErrorMitigation
meas_filter = MeasurementErrorMitigation(backend)
mitigated = meas_filter.apply(result.get_counts())

# ✅ Solution 3: Improve circuit
circuit = transpile(circuit, backend, optimization_level=3)
```

#### Issue 3: Timeout on Queue
```python
# ✅ Solution 1: Use simulator first
simulator = AerSimulator()
job = execute(circuit, simulator)

# ✅ Solution 2: Schedule off-peak
import datetime
now = datetime.datetime.now()
if now.hour < 6:  # Off-peak hours
    job = backend.run(circuit)

# ✅ Solution 3: Use smaller backend
backend = service.backend('ibmq_qasm_simulator')
```

### Monitoring & Logging
```python
import logging

# Configure logging
logging.basicConfig(
    level=logging.DEBUG,
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
    handlers=[
        logging.FileHandler('quantum_execution.log'),
        logging.StreamHandler()
    ]
)

logger = logging.getLogger(__name__)

def monitored_execution(circuit, backend):
    """Execute with monitoring"""
    
    logger.info(f"Executing circuit with {len(circuit)} gates")
    logger.debug(f"Circuit depth: {circuit.depth()}")
    
    try:
        job = backend.run(circuit)
        logger.info(f"Job submitted: {job.job_id()}")
        
        result = job.result()
        counts = result.get_counts()
        
        logger.info(f"Job completed successfully")
        logger.debug(f"Results: {counts}")
        
        return counts
    
    except Exception as e:
        logger.error(f"Execution failed: {str(e)}")
        raise
```

---

## 7. Quantum Algorithm Checklists

### Pre-Execution Checklist
```python
def pre_execution_check(circuit, backend):
    """Verify before execution"""
    
    checks = {
        'qubit_count': circuit.num_qubits <= backend.configuration().n_qubits,
        'gates_supported': all(gate in backend.configuration().basis_gates 
                               for gate in get_gates(circuit)),
        'circuit_valid': circuit.is_valid(),
        'depth_reasonable': circuit.depth() < 1000,
        'connectivity': verify_connectivity(circuit, backend)
    }
    
    for check, result in checks.items():
        status = "✅" if result else "❌"
        print(f"{status} {check}")
    
    return all(checks.values())

def get_gates(circuit):
    """Extract gate types from circuit"""
    return set(instr[0].name for instr in circuit.data)
```

### Post-Execution Analysis
```python
def analyze_results(result, ideal_solution=None):
    """Analyze execution results"""
    
    counts = result.get_counts()
    
    # Basic statistics
    total_shots = sum(counts.values())
    top_solution = max(counts, key=counts.get)
    top_probability = counts[top_solution] / total_shots
    
    print(f"Top solution: {top_solution}")
    print(f"Probability: {top_probability:.1%}")
    print(f"Total states: {len(counts)}")
    
    # Compare with ideal
    if ideal_solution:
        ideal_prob = counts.get(ideal_solution, 0) / total_shots
        print(f"Ideal solution probability: {ideal_prob:.1%}")
    
    # Entropy
    import numpy as np
    probs = np.array(list(counts.values())) / total_shots
    entropy = -np.sum(probs * np.log2(probs + 1e-10))
    print(f"Entropy: {entropy:.2f} bits")
```

---

## 8. Production Deployment Checklist

### Before Going Live
- [ ] All unit tests passing
- [ ] Integration tests completed
- [ ] Performance benchmarks documented
- [ ] Error handling implemented
- [ ] Logging configured
- [ ] Documentation complete
- [ ] Security audit passed
- [ ] Failover procedures tested
- [ ] Disaster recovery plan ready
- [ ] Team trained and ready

### Monitoring in Production
```python
def production_monitoring():
    """Monitor quantum service in production"""
    
    metrics = {
        'execution_success_rate': measure_success_rate(),
        'average_execution_time': measure_avg_time(),
        'queue_depth': measure_queue_length(),
        'error_rate': measure_error_rate(),
        'availability': measure_uptime()
    }
    
    thresholds = {
        'execution_success_rate': 0.95,
        'average_execution_time': 30.0,
        'queue_depth': 100,
        'error_rate': 0.05,
        'availability': 0.999
    }
    
    for metric, value in metrics.items():
        if value < thresholds[metric]:
            alert(f"Warning: {metric} = {value}")
```

---

## Summary

This comprehensive guide covers:
✅ Circuit optimization techniques
✅ Algorithm selection strategies
✅ Error mitigation approaches
✅ Performance optimization
✅ Hybrid workflows
✅ Debugging procedures
✅ Production checklists

**Next Steps:**
1. Review relevant sections for your use case
2. Implement best practices in your code
3. Test thoroughly before production
4. Monitor performance continuously
