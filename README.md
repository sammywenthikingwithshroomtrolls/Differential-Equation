# Differential-Equation
# Discrete Dynamical Systems: Probabilistic Weather Model

This project implements a simple **Markov Chain** to model and predict weather probabilities over time using discrete matrix-vector multiplication. 

The implementation is based on the introductory lecture on dynamical systems and differential equations by **Dr. Steve Brunton**.

---

## 1. The Mathematical Framework

Instead of predicting an absolute weather state, this system models the **probability distribution** of three distinct weather states on any given day $k$:

$$\mathbf{x}_k = \begin{bmatrix} \text{Probability of Rain} \\ \text{Probability of Nice Weather} \\ \text{Probability of Clouds} \end{bmatrix}$$

### The Transition Matrix ($A$)
The rules of the environment are governed by a $3 \times 3$ transition probability matrix $A$. Each column represents "today's weather" (the cause) and each row represents "tomorrow's weather" (the effect):

$$A = \begin{bmatrix} 0.5 & 0.5 & 0.25 \\ 0.25 & 0 & 0.25 \\ 0.25 & 0.5 & 0.5 \end{bmatrix}$$

Because a transition must happen, every column in matrix $A$ sums to exactly $1.0$ ($100\%$).

### The Time-Stepping Feedback Loop
The timeline moves forward discretely using the matrix difference equation:

$$x_{k+1} = A x_k$$

By feeding tomorrow's calculated state back into the loop as today's state, the mathematical chain naturally unfolds over an entire month:
* **Day 1:** $x_1 = A x_0$
* **Day 2:** $x_2 = A x_1 = A(A x_0) = A^2 x_0$
* **Day 3:** $x_3 = A x_2 = A(A^2 x_0) = A^3 x_0$
* **Day $k$:** $x_k = A^k x_0$

---

## 2. Python Implementation

Copy and paste this code block directly into a single cell inside a Jupyter Notebook (`.ipynb`) file to watch the mathematical vectors step forward continuously through a 30-day month.

```python
import numpy as np

# 1. Define Transition Matrix A
A = np.array([
    [0.5,  0.5,  0.25],
    [0.25, 0.0,  0.25],
    [0.25, 0.5,  0.5 ]
])

# 2. Initialize Day 0 State Vector (e.g., 100% chance of Rain)
xtoday = np.array([[1.0], 
                   [0.0], 
                   [0.0]])

print("=" * 40)
print("  Day 0 (Initial State Vector x_0) ")
print("=" * 40)
print(xtoday)
print("\nStarting the 30-day feedback loop...\n")

# 3. Run the continuous update loop for a 30-day month
for day in range(1, 31):
    # Move the system forward mathematically: x_{k+1} = A @ x_k
    xtomorrow = A @ xtoday
    
    # Display the updated mathematical vector
    print(f"--- Day {day} (Vector x_{day}) ---")
    print(np.round(xtomorrow, 6))
    print("-" * 35)
    
    # Feedback Step: Tomorrow's state becomes today's baseline
    xtoday = xtomorrow
