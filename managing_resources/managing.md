# Processes Execution

## Resource Considerations

- Memory
- Execution Cost
- Bandwidth

The resources are provided by processors.

Applications involve both **static resources** and **dynamic resources**:

- **Static resources** are easy to allocate from the beginning of the allocation.
- **Dynamic resources** are created during execution (or not created at all).

### Resource Allocation

- **Static Resources**: Allocated statically.
- **Dynamic Resources**: Allocated either statically (actuated as needed) or dynamically.

The goal is to allocate processes over processors in the most efficient way possible.

### Allocation Methods

#### Static Allocation (Working Out of Band)

- **Optimal Allocation**: Can be defined through complex algorithms.
  - **Pros**: Allocation cost is paid out of band.
  - **Cons**: Inflexible during execution (not suitable for long-life applications).

#### Dynamic Allocation (Working In Band)

- Uses simple policies to minimize intrusion.
- The algorithms for allocating processes compete with the business application, requiring a fast and cost-effective policy for a good enough allocation solution (sub-optimality is acceptable).
  - **Pros**: Can adapt to the current situation as needed.
  - **Cons**: Impacts execution.

#### Hybrid Approach

- The system applies a default policy for resource allocation and migration.
- User inputs and advice are considered to improve performance.

## Deployment Support

- **Manual**: Proper sequence of commands.
- **File Script Approach**: Script files guide the configuration in steps, specifying dependencies between objects.
- **Model-Driven**: Automatic configuration through declarative languages.

## Monitoring vs. Observation

### Monitoring

- Enables control and management of the system.
- Observes at limited intervals and specified parameters, providing statistical and historical data to forecast resource trends (continuity assumption).
- Must be cost-effective (minimal intrusion principle).

### Observation

- Allows users to explore the application on non-considered parameters to understand why the system works in a certain way.
- Provides a deep look into the application.

## Resource Services

An application can use a resource through an interface:

- **Service Request**: Client/server model.
- **Distributed File System or Middleware Approach**: Transparent allocation (free of responsibility). Coordinated by agent systems which must collaborate to provide the best result.

## Resource Handling: Allocation and Reallocation Policies

- **Load Sharing**: Defined before execution with no reallocation.
- **Load Balancing**: Done during execution; already allocated and active resources can migrate to improve global efficiency.

### Static Policies

- **Logical Ring**: A token-based ring for strategy making and load distribution.
- **Logical Hierarchy (Farm Pattern)**: One master node distributes load to several workers. Fault-tolerant and can expand with new master nodes.
- **Worm**: Dynamic discovery of free nodes to clone and share load.

## Migration (Load Balancing)

Migration involves moving established resources at runtime with minimal overhead, balancing the need to minimize interference with normal execution.

### Migration Mechanisms

- **Message Redirection (Pessimistic)**: Original node forwards messages to the new location.
- **Client Recovery**: Clients find the new location on their own.

### Entities That Can Migrate

- Processes, passive objects (files), active objects.
- Resource composition: Data and visible resources.
- Computation blocks: Messages arriving can be refused or forwarded.

### Phases of Migration

1. **Load Evaluation**: Local and global.
2. **Transfer**: Determine what to move.
3. **Location**: Decide where to move.

### Migration Policies

#### Static

- Fixed load.
- Move newer processes.
- Always move to a sink node.

#### Semi-Static

- Cyclic ID among processes.
- Cyclic allocation on sink node.

#### Dynamic

- Dynamic average node.
- Information on processes.
- Discovery of sink nodes via messages.

Probe messages are used to find sink nodes (unconditioned acceptance for static, conditioned acceptance for dynamic).

### Migration Decisions

- **Centralized**: Single entity controls migration (high data transport cost, long control time).
- **Distributed (Piggybacking)**: Favors local movements (local strategies, not optimal values).

#### Initiatives

- **Sender**: Requests nodes to receive load (low system load).
- **Receiver**: Finds potential source sender (medium to high system load).

Migration has a cost but can be effective, with simple policies potentially offering significant enhancements compared to no migration. More sophisticated policies may not provide significant improvements.

## Containers

Containers support execution. An orchestrator manages several containers and automates simple operations.

For large numbers of containers, a **service mesh** provides networking support: discovery, secure communication, monitoring, separation, etc.

## Computational Model for Parallelization

- **Problem Dimension $(N)$**
- **Complexity in Time $(CT(N), T(N))$**
- **Level of Parallelism $(P)$**
- **Loading Factor $(L = N/P)$**
- **Speedup**: Improvement from sequential to parallel. $S(P, N) = \frac{T_s(N)}{T_p(N)}$
- **Efficiency**: Resource usage. $E_p(N) = \frac{S_p(N)}{P} = \frac{E_s(N)}{P \times E_p(N)}$

Speedup is limited to $P$ at most, efficiency to $1$ at most.

### Grosh’s Law

Optimal deployment is sequential execution on a single processor.

### Amdahl’s Law

The speed-up limit is due to the intrinsic sequential part, which limits the speed-up.

Example:

- 80% of operations can be parallelized.
- 20% must be executed sequentially.

The speed-up has a linear zone at P growing, then constant speed-up with decreasing efficiency.

### Amdahl’s Law Analysis

- **$TP(N)$**: Total parallel time = Parallel computation time + Sequential computation time + Communication time.
- **Heavily Loaded Limit $(THL(N))$**: Optimum when $N/P$ is high.
- **Total Overhead $(T_o)$**: Time and resources spent in other actions, such as communication. $T_o(N,P) = |P \times TP(N) - T1(N)|$

When working at optimal efficiency, overhead is zero: $T_o(N,P) = 0$.

### Isoefficiency Function K

Indicates whether a parallel system can maintain constant efficiency, using $K=T_o(P,N)$ as index, while $P$ varies and $N$ is constant:

- Small K: High scalability.
- High K: Less scalable.
- Non-constant K: Non-scalable systems (mostly all).

All real systems are non-scalable; a heavily loaded limit is a good target. Good efficiency results from highly loaded processors.
