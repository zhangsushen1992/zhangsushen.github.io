## Solution to Square Root using Gradient Descent

The gradient descent method finds the minimum of a function with the update rule: x:=x-λ⋅g(x), where λ is the learning rate and g(x) is the gradient of the function at x. Suppose we would like to utilise this equation to find the square root of a value.

Let √(a) = x, where x is the square root of a number a. Thus, a=x<sup>2</sup>. x<sup>2</sup>-a=0.

Let f(x) = x<sup>2</sup>-a, we would like to find values of x where f(x)=0. Suppose F(x) is the integral of f(x). If we minimise F(x), we will find values of x where f(x) is zero. Therefore, we perform gradient descent on F(x).

Using the rule of gradient descent, we have x:=x-λ (x<sup>2</sup>-a).

Let's take a look at how it is achieved in code. First, we import the module math.
```
import math
```
Then we enter the main function:
```
if __name__=="__main__":
  learning_rate = 0.01
  for a in range (1100):
    cur = 0
    for i in range (1000):
      cur -= learning_rate * (cur**2 - a)
    print('The approximated square root of %d is: %.8f, the real value is: %.8f'%(a, cur, math.sqrt(a)))
```
The outer iteration iterates through 1 to 1100 to find the square root of all values in the range. Cur is a cursor to track the square root of the values. For each calculation of square root, we iterate 1000 times of gradient descent. The key updating function is the line:
```
      cur -= learning_rate * (cur**2 - a)
```
