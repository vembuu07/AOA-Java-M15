
# EX 5A 0/1 Knapsack Problem - Branch&Bound 
## AIM:
To Write a Java program to solve 0/1 Knapsack problem using Branch and Bound Approach.
You are heading a college entrepreneurship cell that can invest in up to N student‑startups.

For each startup i you know: cost[i]  — the amount (in ₹ lakh) required to join the showcase profit[i] — the estimated profit (in ₹ lakh) you’ll gain if it succeeds You have a total budget of B ₹ lakh. Pick a subset of startups so that the sum of costs ≤ B and the sum of profits is maximised.

Because N can be as large as 50, a plain exhaustive search (2^N) is too slow.

The recommended approach is Branch & Bound with a fractional‑knapsack upper bound (but any algorithm that meets the constraints is accepted). 

Input Format

N

B

cost[1] cost[2] … cost[N]

profit[1] profit[2] … profit[N]

1 ≤ N ≤ 50

1 ≤ B ≤ 1 000 000

1 ≤ cost[i], profit[i] ≤ 10 000 

Output Format

maxProfit

For example:




## Algorithm
1.Start and read the number of startups N, total budget B, and each startup’s cost and profit.

2.Sort the startups in descending order of profit-to-cost ratio to improve bounding efficiency.

3.Define a bound function to compute the upper bound of achievable profit (including fractional profit if budget remains).

4.Use DFS with Branch and Bound:

Explore two possibilities for each startup — include it (if within budget) or exclude it.

Prune any branch where the upper bound ≤ current best profit.

5.Return the highest profit (best) obtained from feasible startup selections within the budget.
   

## Program:
```

import java.util.*;

public class StartupShowcaseOptimizer {

    // ---------- Global data ----------
    static int N, B;
    static int[] c, p;          // cost, profit after sorting by ratio
    static int best = 0;        // incumbent best profit

    // ---------- Fractional upper bound ----------
    static double bound(int idx, int cw, int cv) {
        if (cw >= B) return cv;                 // bag full or overweight
        double val = cv;
        int rem = B - cw;

        while (idx < N && c[idx] <= rem) {      // add full items
            rem -= c[idx];
            val += p[idx];
            idx++;
        }
        if (idx < N) val += p[idx] * (rem / (double) c[idx]); // fractional part
        return val;
    }

    // ---------- DFS Branch & Bound ----------
    static void dfs(int idx, int cw, int cv) {
        if (idx == N) {                 // leaf
            best = Math.max(best, cv);
            return;
        }
        if (bound(idx, cw, cv) <= best) return; // prune

        // 1. include current startup if it fits
        if (cw + c[idx] <= B)
            dfs(idx + 1, cw + c[idx], cv + p[idx]);

        // 2. exclude current startup
        dfs(idx + 1, cw, cv);
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        N = sc.nextInt();
        B = sc.nextInt();
        int[] cost = new int[N];
        int[] prof = new int[N];
        for (int i = 0; i < N; i++) cost[i] = sc.nextInt();
        for (int i = 0; i < N; i++) prof[i] = sc.nextInt();
        sc.close();

        // Sort by profit/cost ratio descending → tighter bounds
        Integer[] idx = new Integer[N];
        Arrays.setAll(idx, i -> i);
        Arrays.sort(idx, Comparator.comparingDouble(i -> -(double) prof[i] / cost[i]));

        c = new int[N];
        p = new int[N];
        for (int i = 0; i < N; i++) {
            c[i] = cost[idx[i]];
            p[i] = prof[idx[i]];
        }

        dfs(0, 0, 0);
        System.out.println(best);
    }
}
```

## Output:

<img width="328" height="208" alt="image" src="https://github.com/user-attachments/assets/e2b69770-db78-47bd-b494-21c1d2fb752e" />


## Result:
The program successfully solved 0/1 Knapsack problem using branch & bound and output is verified. 
