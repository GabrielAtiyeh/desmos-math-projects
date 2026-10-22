# Follow the Line Game

An interactive Desmos project where the goal is to move a draggable point as close as possible to the line $y=x$.

The square changes color based on the point's distance from the line, giving immediate visual feedback.

[**Explore the Interactive Graph in Desmos**](https://www.desmos.com/calculator/dh5ypl5lae)

## How It Works

The game centers on a draggable point, $(a,b)$, inside a square bounded by -1 ≤ x ≤ 1 and -1 ≤ y ≤ 1. The objective is to position the point on the diagonal line $y=x$.

I designed a system that measures how closely the point follows this line and translates that measurement into visual feedback.

### Measuring the Error

Two functions track the point's coordinates after shifting each value by 1:

- $f_1(x)=a+1$ represents the shifted x-coordinate.
- $f_2(x)=b+1$ represents the shifted y-coordinate.

Adding 1 shifts both values into the interval $[0,2]$, providing a common zero baseline for the visual indicators.

The error is calculated using the absolute difference:

$$
E=|f_1(x)-f_2(x)|=|a-b|
$$

When $a=b$, the error is zero, meaning the point lies exactly on $y=x$. As the coordinates diverge, the error increases. This measurement is proportional to the point's perpendicular distance from the line.

The graph displays the shifted x- and y-coordinate values using green and red indicator lines, while a blue indicator represents the error.

### Dynamic Color System

The square itself is created using an inequality with a domain restriction:

```text
-1 ≤ y ≤ 1 {-1 ≤ x ≤ 1}
```

Rather than assigning this region a fixed color, I used a variable RGB expression to make its fill color depend directly on the point's position.

First, the error is scaled to determine the RGB channel values:

$$
c_1=255E
$$

I then defined the square's color using a piecewise RGB function:

```text
C_1 = {c_1 ≤ 127.5: rgb(2c_1,255,0), rgb(255,2(255-c_1),0)}
```

The two branches control different stages of the color transition:

- **Green to yellow:** As the error initially increases, the red channel increases while the green channel remains at full intensity.
- **Yellow to red:** Once the error passes the piecewise threshold, the red channel stays at full intensity while the green channel decreases.
- **Red:** At larger errors, the square remains red, indicating that the point is far from the target line.

Because this RGB expression is assigned directly to the square's inequality, moving the draggable point causes the entire region to change color dynamically.

This connects the algebraic relationship between two coordinates to a continuous visual response. Instead of merely displaying a numerical error, the graph uses mathematical functions to make that error immediately observable.

## Screenshots

### On the Line

![Green](follow-the-line-green.png)

### Moderate Error

![Yellow](follow-the-line-yellow.png)

### Large Error

![Red](follow-the-line-red.png)

## Mathematical Concepts

This project combines coordinate geometry, absolute value, domain restrictions, functions, piecewise definitions, and RGB color transformations.

It illustrates how a simple mathematical relationship can be used to create an interactive system: equations determine the behavior, and visual feedback makes the underlying mathematics immediately observable.
