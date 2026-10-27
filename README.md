# Interactive Mathematics Projects in Desmos
A collection of interactive mathematical projects created in the Desmos 2D Graphing Calculator.

## About This Repository

This repository contains five mathematical projects that I created using the [Desmos Graphing Calculator](https://www.desmos.com/). Each project explores a different application of mathematics, from geometric constructions and musical frequencies to interactive displays and games.

I enjoy using Desmos to experiment with mathematical concepts and find creative ways to turn equations into interactive systems. As I developed these projects, I explored how different types of functions could be combined to produce increasingly complex results.

Each project has its own folder containing a detailed README with explanations of the mathematics involved, important equations, screenshots, and a link to the interactive Desmos graph.

## Projects

### 1. [Synchronized Analog and Digital Clock](projects/synchronized-analog-digital-clock/README.md)

An interactive clock featuring synchronized analog hands, a horizontal time slider, and a four-digit seven-segment display constructed entirely from mathematical expressions.

The analog clock uses trigonometric functions to control the rotation of its hands, while the digital clock uses piecewise functions, modular arithmetic, and periodic cosine functions to illuminate the appropriate segments. Each of the four digital digits required a different mathematical approach, from relatively simple piecewise conditions to longer functions controlling individual segments of each seven-segment display. I also used modular arithmetic to simplify repeating patterns and complementary cosine functions to alternate between digits without additional piecewise restrictions. All components are synchronized through a single variable.

This is my most extensive Desmos project, combining several different mathematical techniques into one interactive system.

### 2. [Touchpad-Controlled Pixel Display](touchpad-controlled-pixel-display/README.md)

A 16 × 20 pixel display containing 320 individually controlled pixels, operated using a draggable point on a smaller touchpad.

The project uses piecewise functions, inequalities, and list indexing to translate the position of the draggable point into the corresponding illuminated pixel. I constructed functions that identify the row and column selected by the touchpad, then combined their conditions to determine which individual pixel should be illuminated. This required finding a way to represent two-dimensional position and conditional behavior entirely through mathematical expressions, rather than conventional programming. It explores how mathematical expressions can create conditional behavior similar to the logic used in computer programming.

### 3. [Bézier Curve](bezier-curve/README.md)

An interactive construction of a cubic Bézier curve, originally developed as my AP Calculus BC final project.

Using linear interpolation and parametric equations, I constructed the curve through the geometric process known as de Casteljau's algorithm. The graph shows the intermediate points and line segments involved in generating the curve, with draggable control points that allow its shape to change dynamically. By repeatedly interpolating between points, I was able to visualize how a smooth curve emerges from a sequence of simpler linear relationships. The project also allows the interpolation parameter to change, illustrating how the construction progresses from one endpoint of the curve to the other.

### 4. [Take Me Out to the Ball Game](take-me-out-to-the-ball-game/README.md)

A mathematical recreation of *Take Me Out to the Ball Game* that plays both its melody and harmony using Desmos.

The project uses exponential functions to calculate musical frequencies and piecewise functions to organize the sequence of notes. I used the mathematical relationship between pitch and frequency to assign the appropriate frequencies to different musical notes, then combined these expressions into a piecewise representation of the song. A variable also allows the music to be transposed into different keys, demonstrating the mathematical relationships between musical pitches. This project gave me an opportunity to explore how equations can represent not only visual patterns but also sound and musical structure.

### 5. [Follow the Line Game](follow-the-line-game/README.md)

An interactive game in which the player attempts to follow a moving line and receives visual feedback based on their accuracy.

I used absolute value to measure the difference between the player's position and the target, then created piecewise functions to translate that error into a continuous range of colors. The feedback changes between green, yellow, and red depending on how closely the player follows the line. Rather than using only three fixed colors, I constructed functions that adjust the RGB values according to the size of the error, producing gradual transitions between colors. This project demonstrates how mathematical functions can transform a simple numerical measurement into responsive visual feedback.

## Mathematical Exploration

These projects reflect my interest in exploring mathematics beyond traditional classroom problems. I particularly enjoy discovering how mathematical concepts can be applied in unexpected ways, whether that involves constructing a geometric curve, representing music with exponential functions, or using equations to simulate the behavior of a digital clock.

One of my favorite aspects of working in Desmos is that even complicated interactive systems can be built entirely from mathematical expressions. Developing these projects encouraged me to experiment with unfamiliar functions, recognize patterns, and find different approaches when my original ideas became too complicated.
