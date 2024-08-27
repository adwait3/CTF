## SekaiCTF - CalcQL Challenge

**Challenge Objective:**

- Write a CodeQL query to detect functions returning the value 42 in a large, randomly generated Python codebase.

**Key Takeaways:**

* **CodeQL:**
  - Converts code into a relational database for analysis using a specialized query language similar to SQL.
* **Working of CodeQL:**
  - Code Conversion: Converts source code into a database format representing the code structure (functions, variables, control flow).
  - Query Execution: Users write CodeQL queries against this database to find specific code patterns, analyze data flow, or identify security issues.
  - Results Interpretation: Results highlight potential issues in the code with their location, aiding developers in fixing them.

**Solution:**

```codeql
// find all functions
from Function g
where
  // get the return statement, calculate the value,
  // check if it's 42 and the function name starts with "entry_"
  value(g.getBody().getItem(0).(Return).getASubExpression()) = 42 and
  g.getName().matches("entry_%")

// output matching function names
select g, "called " + g.getName()
```

ref
>https://boxmein.github.io/posts/2024-08-25-sekaictf-2024/
