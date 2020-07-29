Def: Competitive Ratio:
  - In the worst assignment, how much money you would get with a particular algorithm compared to how much money you would get with the optimal assignment. 
  
Google AdWords Problem:
  Maximize Expected Revenue = CTR * bid amount

## Algorithms for Ad assignment (Online Graph Assignment)
### Greedy Algorithm:
  - Has competitive Ratio of 1/2 on two advertisers.
    |M_{opt}| \le |M_{greedy}| + |G|
    |M_{greedy}| \ge |B|
    $|G| \le |B|$
    $|G| \le |B| \le |M_{greedy}|
### BALANCE Algorithm:(1)
- Consider query $q$, bidder $i$.
  - Let the bid of $i$ = $x_i$
  - budget of i = $b_i$
  - Amount spent so fat = $m_i$
  - Fraction of budget left over is $f_i = 1-m_i/b_i$
  - Define $\psi_i(q) = x_i(1-e^{-f_i})$
  - Allocate query $q$ to bidder $i$ with largest value of $\psi_i(q)$
  
- Equal budget Heuristic: For each query, pick the advertiser with the **largest unspent budget**

- Has Competitive Ratio of 3/4 on two advertisers, or $1-1/e \approx 0.63$ with many advertisers. In addition, this is the best possivle competitive ratio for any online algorithm.

(1) Proof:
  Consider two advertisers, and optimial solution exhausts both advertiser's budgets.
    Optimal Revenue= 2B
  BALANCE must exhaust at least one budget.
    Balance Revenue = 2B -x = B+y
    Now, since $y \ge x$ by balance algo, balance revenue is min for $y = x \rightarrow y \ge B/2$
    
    

