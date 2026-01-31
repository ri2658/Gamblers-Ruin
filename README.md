# Gambler's Ruin in Python

The classic Gambler's Ruin problem; it goes like this.

Imagine a gambler playing a simple coin-flip game against a casino.

Rules of the Game

- The gambler starts with $i$ coins.
- The casino effectively holds the rest, so there are $N$ total coins in the system.

Each round:

- With probability $p$, the gambler wins and gains 1 coin
- With probability $1−p$, the gambler loses and gives up 1 coin

# Theoretical Probability

Now let $E_{i}$ be the event that you win with $i$ coins. Our boundary conditions natural arise where $P(E_{0})$ (i.e., you lose if you have 0 coins left as per the rules), and $P(E_{N})$ (i.e., you win if you have $N$, or in other words, all the coins). Then,

$$
\begin{aligned}
P(E_{i}) = P(E_{i} | \text{win})P(\text{win}) + P(E_{i} | \text{lose})P(\text{lose}) \\
P(E_{i}) = P(E_{i+1})p + P(E_{i-1})q \\
(p+q)P(E_{i}) = P(E_{i+1})p + P(E_{i-1})q \\
q(P(E_{i})-P(E_{i-1})) = p(P(E_{i+1})-P(E_{i})) \\
P(E_{i+1})-P(E_{i}) = \frac{q}{p}(P(E_{i})-P(E_{i-1})) \\
\end{aligned}
$$

Now recognize that this is a recursive relation. To isolate $P(E_{i})$, one should sum up the terms from $i=0$ to $i=N$.

$$
\begin{aligned}
i = 1 \quad & P(E_{2}) - P(E_{1}) &= \frac{q}{p}\big(P(E_{1}) - P(E_{0})\big) \\
i = 2 \quad & P(E_{3}) - P(E_{2}) &= \frac{q}{p}\big(P(E_{2}) - P(E_{1})\big)
                                   = \left(\frac{q}{p}\right)^2 \big(P(E_{1}) - P(E_{0})\big) \\
i = 3 \quad & P(E_{4}) - P(E_{3}) &= \frac{q}{p}\big(P(E_{3}) - P(E_{2})\big)
                                   = \left(\frac{q}{p}\right)^3 \big(P(E_{1}) - P(E_{0})\big) \\
& \vdots \\
i = N-1 \quad & P(E_{N}) - P(E_{N-1}) &= \frac{q}{p}\big(P(E_{N-1}) - P(E_{N-2})\big)
                                       = \left(\frac{q}{p}\right)^{N-1} \big(P(E_{1}) - P(E_{0})\big)
\end{aligned}
$$

Notice the cancellations on the left-hand side when summed. Note that we invoke the initial condition $P(E_{0})=0$ here.

$$
\begin{aligned}
P(E_{N})-P(E_{1})=(P(E_{1})-0)(\frac{q}{p} + (\frac{q}{p})^2 + (\frac{q}{p})^3 + ... + (\frac{q}{p})^{N-1})\\
P(E_{N})=P(E_{1})(1+(\frac{q}{p})^1 + (\frac{q}{p})^2 + (\frac{q}{p})^3 + ... + (\frac{q}{p})^{N-1})\\
\end{aligned}
$$

Notice the right hand side, which is a geometric series with common ratio $\frac{q}{p}$.Recall the formula for the sum of a geometric series, which is $S_{n} = \frac{a_0(1-r^N)}{1-r}$ where $r$ is the common ratio.

$$
\begin{aligned}
P(E_{N})=P(E_{1})(\frac{(1-(q/p)^N)}{1-q/p}) \\
\end{aligned}
$$

Now, invoke the intial condition $P(E_{N})=1$.

$$
\begin{aligned}
P(E_{1})=\frac{(1-q/p)}{1-(q/p)^N}  \\
\end{aligned}
$$

Finally, consider a sum to a specific index $i$.

$$
\begin{aligned}
P(E_{i})=P(E_{1})(1+(\frac{q}{p})^1 + (\frac{q}{p})^2 + (\frac{q}{p})^3 + ... + (\frac{q}{p})^{i-1})\\
P(E_{i})=P(E_{1})(\frac{(1-(q/p)^i)}{1-q/p}) \\
P(E_{i})=\frac{(1-q/p)}{1-(q/p)^N}(\frac{(1-(q/p)^i)}{1-q/p}) \\
P(E_{i})=\frac{1-(q/p)^i}{1-(q/p)^N} \\
\end{aligned}
$$

Now that we've obtained a formula, we can verify that this is true with a simulation in Python!
