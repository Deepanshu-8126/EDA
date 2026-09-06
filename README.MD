
**Difference between pd.cut and pd.qcut:**
  - `pd.cut()` creates bins with custom fixed values (bins can have different row counts).
  - `pd.qcut()` splits data into **equal-sized quartiles** (each group has the exact same 25% of rows).
- **Quartiles created:** Q1 (Lowest 25% views), Q2, Q3, and Q4 (Top 25% views).
