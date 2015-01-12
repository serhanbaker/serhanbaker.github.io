---
layout: post
title: Facebook Hacker Cup - New Year's Resolution
---
[Facebook Hacker Cup 2015 Qualification Round](https://www.facebook.com/hackercup/problems.php?round=742632349177460) has ended and I wanted to share an answer to its second problem, "_New Year's Resolution_". 

It asks us to find if it is possible for Alex to eat the exact amount of each macronutrient (protein, carbohydrate, fat), given the desired goal, food count and macronutrient values for each food, per test case. The catch is, Alex can only eat a food or not. He can not eat a fractional amount of food. So, it's 0 or 1.

This problem is a version of [Knapsack Problem](http://en.wikipedia.org/wiki/Knapsack_problem), the subset sum problem. Given a set of non-negative integers, and a value sum, determine if there is a subset of the given set with sum equal to given sum. 

The subset sum problem can be divided into two recursive subproblems
* _Include_ the _last element_, recurse for n = n-1, sum = sum – set[n-1]
* _Exclude_ the _last element_, recurse for n = n-1.
If any of the above the above subproblems return true, then we have an answer. Only, there are three values for an element instead of one, and there are three sums.

Following is simple recursive implementation that follows the solutions for subproblems mentioned above.

```java
import java.io.*;
import java.util.*;

public class NewYearsResolution {
    public static class Food {
        int p;
        int c;
        int f;

        public Food(int p, int c, int f) {
            this.p = p;
            this.c = c;
            this.f = f;
        }
    }

    public boolean isSubsetSum(Food[] set, int n, int sp, 
        int sc, int sf) {
        if (sp == 0 && sc == 0 && sf == 0)
            return true;
        if (n == 0 && (sp != 0 || sc != 0 || sf != 0))
            return false;

        if (set[n - 1].p > sp || set[n - 1].c > sc 
            || set[n - 1].f > sf)
            return isSubsetSum(set, n - 1, sp, sc, sf);

        return isSubsetSum(set, n - 1, sp, sc, sf)
                || isSubsetSum(set, n - 1, sp - set[n - 1].p,
                        sc - set[n - 1].c, sf - set[n - 1].f);
    }

    public static void main(String[] args) throws 
    FileNotFoundException, IOException {
        String inputFile = "new_years_resolution.txt";
        FileInputStream fis = new FileInputStream(inputFile);
        DataInputStream in = new DataInputStream(fis);
        BufferedReader br = new BufferedReader(
            new InputStreamReader(in));
        PrintWriter out = new PrintWriter(new BufferedWriter(
            new FileWriter("new_years_resolution_out.txt")));
        NewYearsResolution res = new NewYearsResolution();
        int t = Integer.parseInt(br.readLine());
        StringTokenizer st;
        for (int i = 1; i <= t; i++) {
            st = new StringTokenizer(br.readLine());
            int gP = Integer.parseInt(st.nextToken());
            int gC = Integer.parseInt(st.nextToken());
            int gF = Integer.parseInt(st.nextToken());
            int foodCount = Integer.parseInt(br.readLine());
            Food[] food = new Food[foodCount];
            out.print("Case #" + i + ": ");
            for (int j = 1; j <= foodCount; j++) {
                st = new StringTokenizer(br.readLine());
                int p = Integer.parseInt(st.nextToken());
                int c = Integer.parseInt(st.nextToken());
                int f = Integer.parseInt(st.nextToken());
                food[j - 1] = new Food(p, c, f);
            }
            out.println(
                res.isSubsetSum(food, food.length, gP, gC, gF) ? "yes" : "no");
        }
        br.close();
        out.close();
    }

}

```