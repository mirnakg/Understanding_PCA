# Understanding PCA

Principal Component Analysis (PCA) transforms and combines features to project data into a lower dimension (e.g. a 2D plane) such that the **maximum variance** in the data is captured and displayed.

---

## The Covariance Matrix

To obtain principal components from the data, we first need the **covariance matrix** of the features. If the number of features is *p*, the covariance matrix is of size *p × p*:

|       | x₁            | x₂            | ...  | xₚ            |
|-------|---------------|---------------|------|---------------|
| **x₁** | Cov(x₁, x₁)  | Cov(x₁, x₂)  | ...  | Cov(x₁, xₚ)  |
| **x₂** | Cov(x₁, x₂)  | Cov(x₂, x₂)  |      |               |
| **...**  | ...           |               | ...  |               |
| **xₚ** | Cov(x₁, xₚ)  | ...           |      | Cov(xₚ, xₚ)  |

where $\text{Cov}(x_i, x_i) = \text{Var}(x_i)$.

---

## Eigenvalues & Eigenvectors

From the covariance matrix, we compute eigenvalues and eigenvectors. Two relevant properties of the covariance matrix make this possible:

1. **It is symmetric** ($C = C^T$) — entries are symmetric with respect to the main diagonal.
2. **Its eigenvalues are real non-negative numbers.**

The covariance matrix $C$ has eigenvalues $\lambda_1, \lambda_2, \ldots, \lambda_p$ corresponding to each eigenvector. Eigenvalues capture the *amount of information* held by that eigenvector (i.e. the principal component), so they describe the **importance** of the corresponding PC. Dividing a given eigenvalue by the sum of all eigenvalues gives the **percentage of variance** captured by that component.

The covariance matrix $C$ has eigenvectors $u_1 \perp u_2 \perp \ldots \perp u_p$ that are **orthogonal** to each other — meaning they capture different information. These eigenvectors specify the directions in the data space that capture the highest variance. They are used to compute new descriptive components (principal components) from the data. For a given sample $k$, this component is computed as the dot product:

$$u \cdot x_k^T$$

---

## Intuition: The Dot Product

For a given data vector $x_k$, the dot product is **maximum** when $x_k$ is parallel to $u$, and **zero** when $x_k$ is perpendicular to $u$. In an abstract sense, the dot product represents *information preserved*.

![Dot Product Illustration](dot_product.png)

$$\vec{A} \cdot \hat{B} = |A||B|\cos(\theta)$$

If the magnitude of $B$ is 1, then:

$$C = \vec{A} \cdot \hat{B} = |A|\cos(\theta)$$

---

## The PCA Objective Function

PCA maximizes the sum of all these dot products — hence maximizing information preserved (or equivalently, minimizing information lost). The dot product is squared to neutralize the direction of data:

$$\max \sum_{k} \left( x_k^T \cdot u \right)^2$$

We aim to find $u$ that satisfies this objective. The following derivation shows why the best solution is equivalent to finding the eigenvalues and eigenvectors of the covariance matrix.

---

## Derivation

**Constraint:** the eigenvector $u$ is a unit vector:

$$u^T u = 1$$

**Expanding the objective:**

$$\max \sum_{k} x_k^T u \; x_k^T u = \max \sum_{k} u^T x_k \; x_k^T u = \max \; u^T \left( \sum_{k} x_k \; x_k^T \right) u = \max \; u^T C \; u$$

**Applying the constraint via a Lagrange multiplier** (since $u^T u - 1 = 0$):

$$u^T C \; u = u^T C \; u - \lambda(u^T u - 1)$$

To find the maximum, take the derivative with respect to $u$ and set it to zero:

$$2Cu - 2\lambda u = 0$$

$$Cu = \lambda u$$

This is precisely the **eigenvalue equation** for $C$. The amount of information preserved after projection onto a given eigenvector is given by the corresponding eigenvalue:

$$u^T C \; u = \lambda$$

$$\max \; u^T C \; u = \max \; \lambda$$

The solution vector $u$ has the **largest eigenvalue**. This gives us $u_1$ — the first principal component.

Because eigenvectors are orthogonal to each other, $u_2$ is orthogonal to $u_1$ and corresponds to the **second largest** $\lambda$. We solve the same problem again in the subspace orthogonal to $u_1$ to get the second principal component, and so on.

---

Below is a video that provides a good intuitive explanation

🎥 [Visual explanation of PCA (YouTube)](https://www.youtube.com/watch?v=FD4DeN81ODY&t=238s)
