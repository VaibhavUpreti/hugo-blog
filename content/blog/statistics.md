---
title: Stats Assignment 1
date: 2022-11-03T21:34:36+08:00
tags: ["assignment", "statistics"]
series: ["how to create your blog"]
featured: false
---


# **Probability distribution curve in Python and JS**

Consider 20 independent repetitions of drawing a bulb from a lot.

Probability that the bulb drawn will work for more than 500 hrs is 0.2.

Plot a probability distribution using python or scilab for getting k success in 20 trial

If I do something that has a constant probability of success - how many times do I need to do it to ensure that, on average, I'll be successful?

If you succeed with probability 𝑝 independently of all previous attempts, then the probability you succeed at least once after 𝑘 tries is 1−(1−𝑝)𝑘 . To succeed at least once on average you need

1−(1−𝑝)𝑘≥0.5 (1−𝑝)𝑘≤0.5 𝑘log2(1−𝑝)≤−1 𝑘≥−1log2(1−𝑝)

## Output 

#### Bar Chart

![](https://user-images.githubusercontent.com/85568177/202895560-5660903b-6ec2-4a0e-81fd-034b8b9fbe90.jpg)
#### Line Chart

![](https://user-images.githubusercontent.com/85568177/202895563-35887f2c-176d-47b0-ab83-8490063b55c7.jpg)
#### Scatter Chart

![](https://user-images.githubusercontent.com/85568177/202895565-26815ee3-9072-4f21-ae11-c348380b2e16.jpg)
## Code 

main.py
```python
    # import matplotlib as mtt
    import matplotlib.pyplot as mt
    numEvents = int(input("Enter number of times bulb is drawn: "))
    p = 0.2
    time = 500
    Event = {}
    # LOGIC
    # nCr * p ^k  * (1-p)^n-k
    def nCr(n, r):
        return (factorial(n) / (factorial(r) * factorial(n - r)))
    def factorial(n):
        if n == 0:
            return 1
        result = 1
        for i in range(2, n+1):
            result = result * i
        return result
    n = numEvents
    r = 0
    for k in range(numEvents+1):
        " Graph --- prob of success at kth trial vs trials"
        # z = (1-p)**(k)
        print(int(nCr(n, r)))
        ans = (int(nCr(n, r))) * p**(k) * (1-p)**(n-k)
        r = r + 1
        Event[k] = ans

    x_axis = list(Event.keys())
    y_axis = list(Event.values())
    print(x_axis)
    print(y_axis)
    mt.bar(x_axis, y_axis)
    # mt.plot(x_axis, y_axis)
    mt.title("k success")
    mt.xlabel("Number of Trials")
    mt.ylabel("Probability")
    mt.show()
```
#### index.js
```javascript
 const labelsBarChart = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20];
  const dataBarChart = {
    labels: labelsBarChart,
    datasets: [
      {
        label: "k success",
        backgroundColor: "hsl(252, 82.9%, 67.8%)",
        borderColor: "hsl(252, 82.9%, 67.8%)",
        data: [0.011529215046068483, 0.05764607523034241, 0.13690942867206324,
         0.20536414300809488, 0.21819940194610077, 0.17455952155688062,
          0.1090997009730504, 0.05454985048652519, 0.022160876760150862,
           0.007386958920050287, 0.0020314137030138287, 0.0004616849325031429,
            8.656592484433929e-05, 1.3317834591436814e-05, 1.6647293239296018e-06
            , 1.6647293239296019e-07, 1.3005697843200014e-08, 7.65041049600001e-10,
             3.187671040000004e-11, 8.388608000000009e-13, 1.0485760000000012e-14],
      },
    ],
  };

  const configBarChart = {
    label: "label",
    type: "bar",
    data: dataBarChart,
    options: {},
  };
  const configLineChart = {
    label: "label",
    type: "line",
    data: dataBarChart,
    options: {},
  };
  const configRadarChart = {
    label: "label",
    type: "scatter",
    data: dataBarChart,
    options: {},
  };

  var chartBar = new Chart(
    document.getElementById("chartBar"),
    configBarChart
  );
  var chartLine = new Chart(
    document.getElementById("chartLine"),
    configLineChart
  );
  

  var chartBar = new Chart(
    document.getElementById("chartRadar"),
    configRadarChart
  );

```
