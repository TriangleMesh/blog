+++
title = 'Standard Deviation Calculation for Common Probability Distributions'
date = 2024-10-21T15:58:43-07:00
# draft = true
+++
## 1. Binomial Distribution

### Definition:

The binomial distribution applies to independent repeated trials where each trial has only two possible outcomes (success or failure). Let the probability of success in each trial be \( p \), the probability of failure be \( 1 - p \), and the number of trials be \( n \).

### Standard Deviation Formula:

$$
\
\sigma = \sqrt{n \cdot p \cdot (1 - p)}
\
$$

### Explanation:

- \( n \): The number of trials.
- \( p \): The probability of success.
- \( 1 - p \): The probability of failure.

### Example:

Suppose you toss a fair coin \( n \) times and calculate the standard deviation for the number of heads (success).

- \( n = 10 \)
- \( p = 0.5 \)
- \( 1 - p = 0.5 \)

**Calculation:**

$$
\
\sigma = \sqrt{10 \cdot 0.5 \cdot 0.5} = \sqrt{2.5} \approx 1.58
\
$$

## 2. Hypergeometric Distribution

### Definition:

The hypergeometric distribution describes sampling without replacement from a finite population. The population size is \( N \), where \( K \) items are considered successes (e.g., red balls), and the remaining \( N - K \) are failures (e.g., blue balls). A sample of \( n \) is drawn, and we focus on the number of successes in the sample.

### Standard Deviation Formula:

$$
\
\sigma = \sqrt{n \cdot \frac{K}{N} \cdot \frac{N - K}{N} \cdot \frac{N - n}{N - 1}}
\
$$

### Explanation:

- \( N \): The population size.
- \( K \): The number of successes.
- \( n \): The sample size.

### Example:

Suppose there are \( N = 20 \) balls, of which \( K = 5 \) are red, and the remaining are blue. You draw \( n = 4 \) balls. The standard deviation of the number of red balls in the sample is:

**Calculation:**

$$
\
\sigma = \sqrt{4 \cdot \frac{5}{20} \cdot \frac{15}{20} \cdot \frac{16}{19}} \approx 0.87
\
$$

## 3. Normal Distribution

### Definition:

The normal distribution is a key continuous probability distribution, and many natural phenomena approximately follow it. The standard deviation measures the spread of data around the mean.

### Standard Deviation Formula for Sample Data:
$$
\\sigma = \sqrt{\frac{\sum_{i=1}^{n} (x_i - \mu)^2}{n}}\\
$$


### Explanation:

- <span><math xmlns="http://www.w3.org/1998/Math/MathML">
  <msub><mi>x</mi><mi>i</mi></msub>
</math>: The i-th data point.
- <span><math xmlns="http://www.w3.org/1998/Math/MathML">
  <mi>&#956;</mi>
</math>: The mean of the data.</span>
- <span><math xmlns="http://www.w3.org/1998/Math/MathML">
  <mi>n</mi>
</math>: The number of data points.</span>

### Example:

Suppose you have a set of measurements: 
    $$
    x_1 = 1, x_2 = 2, x_3 = 3, x_4 = 4, x_5 = 5
    $$

**Mean Calculation:**

$$
\
\mu = \frac{1 + 2 + 3 + 4 + 5}{5} = 3
\
$$

**Standard Deviation Calculation:**
$$
\
\sigma = \sqrt{\frac{(1-3)^2 + (2-3)^2 + (3-3)^2 + (4-3)^2 + (5-3)^2}{5}} = \sqrt{2} \approx 1.41
\
$$

## 4. Poisson Distribution

### Definition:

The Poisson distribution is used to describe the number of events occurring within a fixed interval of time or space, with the parameter <span><math xmlns="http://www.w3.org/1998/Math/MathML">
  <mi>&#955;</mi>
</math></span>, which represents the average number of occurrences.

### Standard Deviation Formula:

$$
\
\sigma = \sqrt{\lambda}
\
$$



### Explanation:

- <span><math xmlns="http://www.w3.org/1998/Math/MathML">
  <mi>&#955;</mi>
</math> : The average number of occurrences.</span>

### Example:

If a store receives an average of <span><math xmlns="http://www.w3.org/1998/Math/MathML">
  <mi>&#955;</mi>
</math></span> = 5 customers per hour, the standard deviation for the number of customers is:

$$
\
\sigma = \sqrt{5} \approx 2.24
\
$$

## 5. Geometric Distribution

### Definition:

The geometric distribution describes the number of failures before the first success, with the probability of success \( p \).

### Standard Deviation Formula:

$$
\
\sigma = \frac{\sqrt{1 - p}}{p}
\
$$

### Explanation:

- \( p \): The probability of success.

### Example:

If the probability of making a successful basketball shot is \( p = 0.3 \), the standard deviation for the number of attempts before the first success is:

$$
\
\sigma = \frac{\sqrt{1 - 0.3}}{0.3} \approx 2.19
\
$$


<!-- ## 6. Uniform Distribution

### Definition:

In a uniform distribution, all values within a certain range are equally likely to occur. Suppose a random variable \( X \) is uniformly distributed over the interval \([a, b]\).

### Standard Deviation Formula:

$$
\
\sigma = \frac{b - a}{\sqrt{12}}
\
$$

### Explanation:

- \( a, b \): The lower and upper limits of the interval.

### Example:

If a random variable is uniformly distributed between \( 0 \) and \( 10 \), the standard deviation is:

$$
\
\sigma = \frac{10 - 0}{\sqrt{12}} \approx 2.89
\
$$ -->

## 6. Exponential Distribution

### Definition:

The exponential distribution describes the time between events, with the parameter <span><math xmlns="http://www.w3.org/1998/Math/MathML">
  <mi>&#955;</mi>
</math></span>, representing the rate of occurrence.

### Standard Deviation Formula:

$$
\
\sigma = \frac{1}{\lambda}
\
$$

### Explanation:

- <span><math xmlns="http://www.w3.org/1998/Math/MathML">
  <mi>&#955;</mi>
</math></span>: The rate of occurrence.

### Example:

If a device fails on average <span><math xmlns="http://www.w3.org/1998/Math/MathML">
  <mi>&#955;</mi>
</math></span> = 2 times per hour, the standard deviation of the time between failures is:

$$
\
\sigma = \frac{1}{2} = 0.5 \text{ hours}
\
$$

