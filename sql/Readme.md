# Quick Notes for SQL


Collecting of personal notes while working with SQL


## Joining as Relation in Mathematics

We view the join in SQL as mathematical relation (Take my discrete mathematics!).

Let $A$ and $B$ be two sets. We define a relation on $A\times B$ as $a\theta b$ if and only if $\theta(a, b)$ is TRUE.

The key components are:
- **Matched pairs**: Pairs $(a, b)$ where $\theta(a, b)$ holds.
- **Unmatched left**: Tuples from $A$ with no match in $B$.
- **Unmatched right**: Tuples from $B$ with no match in $A$.


### Basic Matching/Unmatching Sets

We need the following subsets to represent the different types of joins later.


- Matched set (inner join core):
$$M = \{ (a, b) \mid a \in A, b \in B, \theta(a, b) \text{ is true} \}$$
  
- Unmatched from $A$ (left anti-join projection):
$$U_A = \{ a \in A \mid \nexists b \in B \text{ such that } \theta(a, b) \text{ is true} \}$$
  
- Unmatched from $B$ (right anti-join projection):
$$U_B = \{ b \in B \mid \nexists a \in A \text{ such that } \theta(a, b) \text{ is true} \}$$
  
- Padded unmatched from $A$:
$$P_A = \{ (a, NULL) \mid a \in U_A \}$$
  
- Padded unmatched from $B$:
$$P_B = \{ (NULL, b) \mid b \in U_B \}$$


### Cross Join (Cartesian Product)

The simplest join, with no condition ($\theta$ is always true).

- **Definition**:
$$A \times B = \{ (a, b) \mid a \in A, b \in B \}$$
  
- **Remark**: This is the set of all possible pairs, obtained by nested iteration over $A$ and $B$. Size: $|A| \times |B|$.


### Full Outer Join

Includes all from both $A$ and $B$, with matches or `null`.

- **Definition**:
$$A \bowtie_{\theta,\text{full}} B = M \cup P_A \cup P_B$$
  
- **Remark**: Union the left outer join and right outer join, but since $M$ is shared, no duplicates arise. Equivalently: $(A \bowtie_{\theta,\text{left}} B) \cup (A \bowtie_{\theta,\text{right}} B)$.


### Inner Join

Combines only matching tuples.

- **Definition**:
$$A \bowtie_\theta B = M = \{ (a, b) \mid a \in A, b \in B, \theta(a, b) \text{ is true} \}$$
  
- **Remark**: Start from the cross join and filter with $\theta$: $A \bowtie_\theta B = \{ (a, b) \in A \times B \mid \theta(a, b) \text{ is true} \}$.


### Left Outer Join

Includes all from $A$, with matches or `null`.

- **Definition**:
$$A \bowtie_{\theta,\text{left}} B = M \cup P_A$$
  
- **Remark**: Union the matched pairs $M$ with padded unmatched from $A$ ($P_A$). This ensures every $a \in A$ appears exactly once: matched if possible, else with `null`.


### Left Semi Join

Projects only from $A$ where a match exists.

- **Definition**:
$$A \ltimes_\theta B = \{ a \in A \mid \exists b \in B \text{ such that } \theta(a, b) \text{ is true} \}$$
  
- **Remark**: Project the first component of $M$: $A \ltimes_\theta B = \{ a \mid \exists b \in B: (a, b) \in M \}$. This is the complement of $U_A$ in $A$: $A \setminus U_A$.


### Left Anti Join

Projects from $A$ with no match.

- **Definition**:
$$A \vartriangleleft_\theta B = U_A = \{ a \in A \mid \nexists b \in B \text{ such that } \theta(a, b) \text{ is true} \}$$
  
- **Remark**: The complement of the left semi join in $A$: $A \vartriangleleft_\theta B = A \setminus (A \ltimes_\theta B)$.



### Right Outer Join

Includes all from $B$, with matches or `null`.

- **Definition**:
$$A \bowtie_{\theta,\text{right}} B = M \cup P_B$$
  
- **Remark**: Similar to left, but union $M$ with padded unmatched from $B$ ($P_B$). Symmetric to left join.



### Right Semi Join

Projects only from $B$ where a match exists.

- **Definition**:
$$A \rtimes_\theta B = \{ b \in B \mid \exists a \in A \text{ such that } \theta(a, b) \text{ is true} \}$$
  
- **Remark**: Symmetric to left semi: Project the second component of $M$: $A \rtimes_\theta B = \{ b \mid \exists a \in A: (a, b) \in M \}$. Or $B \setminus U_B$.



### Right Anti Join

Projects from $B$ with no match.

- **Definition**:
$$A \vartriangleright_\theta B = U_B = \{ b \in B \mid \nexists a \in A \text{ such that } \theta(a, b) \text{ is true} \}$$
  
- **Remark**: Symmetric: $A \vartriangleright_\theta B = B \setminus (A \rtimes_\theta B)$.








