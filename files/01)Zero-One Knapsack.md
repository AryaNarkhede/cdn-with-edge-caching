or once I will call you once to clear up things  
# 0/1 Knapsack - DP Approach

## Identifying DP Problems  
- If we have identified that the given problem is of **DP**, then:  
  1. Start by writing its **recursive solution**.  
  2. Apply **memoization** (top-down DP with caching).  
  3. Convert it to **iterative DP (bottom-up DP)**.

---

## Recursive Approach  
To write recursive code, we need to:  
1. **Identify the base case**.  
2. **Construct the choice diagram**.

### Choice Diagram for 0/1 Knapsack  
- Given an item with weight `W1` and total capacity `W`, we have two choices:  
  1. **Include** the item (if weight allows).  
  2. **Exclude** the item.  
### Base Cases  
- If the **knapsack weight capacity** is `0`, no item can be placed → **return 0**.  
- If the **array size** is `0`, no items are present → **return 0**.  

### Recursive Code  
```cpp
int knapsack(int wt[], int val[], int w, int n) {
    if (w == 0 || n == 0)
        return 0;

    if (wt[n-1] > w) {
        return knapsack(wt, val, w, n-1);
    } else {
        return max(knapsack(wt, val, w, n-1), 
                   val[n-1] + knapsack(wt, val, w - wt[n-1], n-1));
    }
}
````

---

## Memoization (Top-Down DP)

- **To convert recursive code to DP, we have 2 ways:**
    
    1. **Memoization (Top-Down Approach)**
        
    2. **Iterative (Bottom-Up DP, preferred)**
        
- Memoization uses a **matrix of changing variables** (here `n, w`).
    
- We initialize the matrix with `-1` (default value).
    
- Before calling the recursive function, **check if the answer is already computed**.
    

### Key Modification

Just add these two lines before recursion:

```cpp
if (t[n][w] != -1) {
    return t[n][w];
}
```

### Memoized Code

```cpp
int t[1001][1001]; // Adjust size as needed

int knapsack(int wt[], int val[], int w, int n) {
    if (w == 0 || n == 0)
        return 0;

    if (t[n][w] != -1) 
        return t[n][w];

    if (wt[n-1] > w)
        return t[n][w] = knapsack(wt, val, w, n-1);
    
    return t[n][w] = max(knapsack(wt, val, w, n-1), 
                          val[n-1] + knapsack(wt, val, w - wt[n-1], n-1));
}
```

---

## Top-Down DP (Iterative)

- **Top-down DP is preferred over memoization** because recursion may cause **stack overflow**.
    
- The **time complexity of both is the same (`O(n*w)`)**, but top-down is safer.
    
- **Converting recursion to top-down DP:**
    
    - Convert **base case** into **initialization**.
        
    - Fill the **DP table iteratively** instead of recursion.
        

---

### Step 1: Base Case → Initialization

The recursive base case:

```cpp
if (w == 0 || n == 0)
    return 0;
```

Becomes initialization in top-down DP:

```cpp
for (int i = 0; i <= n; i++) {
    for (int j = 0; j <= w; j++) {
        if (i == 0 || j == 0) {
            t[i][j] = 0;
        }
    }
}
```

---

### Step 2: Convert Recursive Logic to Iterative

Recursive condition:

```cpp
if (wt[n-1] > w) {
    return knapsack(wt, val, w, n-1);
} else {
    return max(val[n-1] + knapsack(wt, val, w - wt[n-1], n-1), 
               knapsack(wt, val, w, n-1));
}
```

Converted to DP formula:

```cpp
if (wt[n-1] > w) {
    t[n][w] = t[n-1][w];
} else {
    t[n][w] = max(val[n-1] + t[n-1][w - wt[n-1]], t[n-1][w]);
}
```

---

### Step 3: Convert to Iterative DP

- **Replace `n` with `i` and `w` with `j`** in loops.
    

```cpp
for (int i = 1; i <= n; i++) {
    for (int j = 1; j <= w; j++) {

        if (wt[i-1] > j) {
            t[i][j] = t[i-1][j];
        } else {
            t[i][j] = max(val[i-1] + t[i-1][j - wt[i-1]], t[i-1][j]);
        }
    }
}
```

---

### Step 4: Return Final Answer

```cpp
return t[n][w]; // Maximum profit
```

---

## Summary

| Approach        | Time Complexity | Space Complexity                  | Notes                               |
| --------------- | --------------- | --------------------------------- | ----------------------------------- |
| **Recursion**   | `O(2^n)`        | `O(n)` (call stack)               | Exponential, slow                   |
| **Memoization** | `O(n*w)`        | `O(n*w) + O(n)` (recursion stack) | Faster but may cause stack overflow |
| **Top-Down DP** | `O(n*w)`        | `O(n*w)`                          | Most efficient                      |

- **Use recursion** for understanding the problem.
    
- **Use memoization** for optimizing recursion.
    
- **Use top-down DP** for best efficiency.