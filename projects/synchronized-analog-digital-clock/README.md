# Synchronized Analog and Digital Clock

I built this clock entirely in the Desmos Graphing Calculator. It combines a rotating analog clock, a labeled time slider, a four-digit seven-segment digital display, and a blinking colon. The analog hands and all four digits are synchronized by one parameter, $a$, which moves from $0$ to $2\pi$.

[**Open the Interactive Clock in Desmos**](PASTE_YOUR_DESMOS_SHARE_LINK_HERE)

Every visible component is constructed from equations: circular inequalities for the clock face, trigonometric coordinates for the hands, rectangular inequalities for the digital segments, and functions that control which segments appear bright or dim. Different parts of the digital clock required different mathematical solutions, from long piecewise functions to modular arithmetic and trigonometric oscillation.

## How It Works

### Constructing the Analog Clock

I began with the analog clock as the visual foundation. Its face is centered at $(0,2.2)$, and the circular outline is an annular region:

$$
8.8\leq x^2+(y-2.2)^2\leq 10
$$

The small center circle is defined by $x^2+(y-2.2)^2\leq 0.05$. I arranged the twelve hour markers around a circle of radius $2.4$ using coordinates of the form

$$
\left(2.4\cos\theta,\;2.4\sin\theta+2.2\right),\qquad \theta=\frac{k\pi}{6}\quad(k=0,1,\ldots,11)
$$

The clock hands are formed from connected points in Desmos tables. Each table starts at the center, $(0,2.2)$, and ends at a point whose coordinates depend on $a$.

**Hour hand endpoint:**

$$
\left(-1.8\cos\left(a+\frac{\pi}{2}\right),\;1.8\sin\left(a+\frac{\pi}{2}\right)+2.2\right)
$$

**Minute hand endpoint:**

$$
\left(-2.4\cos\left(12a+\frac{\pi}{2}\right),\;2.4\sin\left(12a+\frac{\pi}{2}\right)+2.2\right)
$$

The hour hand is shorter, and the minute hand rotates twelve times as quickly because its angle uses $12a$ instead of $a$. When $a=0$, both hands point to 12. When $a$ completes one full cycle, the hour hand has made one revolution and the minute hand has made twelve. Connecting the center to each moving endpoint produces the visible hands without needing to draw them individually at different times.

This part of the graph is primarily visual. The greater challenge was translating the same moving parameter into four separate numerical displays.

### The Horizontal Time Slider

Above the clock, I drew a line segment using $y=5$ with the restriction $-12\leq x\leq-3$, then placed a moving point on it. Its coordinates are

$$
\left(\frac{9a}{2\pi}-12,\;5\right)
$$

I labeled points along this line with 12:00, 1:00, 2:00, and so on until it returns to 12:00. As $a$ moves from $0$ to $2\pi$, the point travels along the labeled line and the two clock hands rotate accordingly.

### Converting the Angle into Five-Minute Steps

For the digital clock, I needed a way to describe the twelve-hour cycle with numerical intervals. The expression I repeatedly use in the original functions is

$$
\frac{72a}{\pi}
$$

It maps the interval $0\leq a\leq2\pi$ onto $0\leq72a/\pi\leq144$. A complete twelve-hour cycle contains 144 five-minute increments, so each **hour corresponds to 12 units** and each **five-minute step corresponds to 1 unit**.

For example, $a=0$ represents 12:00; $a=\pi/72$ represents 12:05; $a=\pi/6$ represents 1:00; and $a=5\pi/3$ represents 10:00. These values let me control the digital clock using inequalities that describe when a specific digit segment should be illuminated.

### Creating the Seven-Segment Digits with Inequalities

Each digital digit is made of seven small rectangles: a top, middle, and bottom horizontal segment, plus upper-left, lower-left, upper-right, and lower-right vertical segments. Every rectangle is the intersection of an $x$-interval and a $y$-interval.

For example, the **top horizontal** segment of the ones-of-hours display uses

$$
-2.5\leq x\leq-1.25\;\left\{-1.85\leq y\leq-1.45\right\}
$$

while its **upper-right** vertical segment uses

$$
-3.2\leq y\leq-1.95\;\left\{-1.15\leq x\leq-0.75\right\}
$$

The four digits share the same vertical layout, with the upper horizontal segment at $-1.85\leq y\leq-1.45$, the middle at $-3.7\leq y\leq-3.3$, and the bottom at $-5.55\leq y\leq-5.15$. The upper vertical segments use $-3.2\leq y\leq-1.95$ and the lower ones use $-5.05\leq y\leq-3.8$.

The horizontal positions change from digit to digit:

| Display | Horizontal bars | Left vertical bars | Right vertical bars |
| :--- | :--- | :--- | :--- |
| Tens of hours | $-5.2\leq x\leq-3.95$ | $-5.7\leq x\leq-5.3$ | $-3.85\leq x\leq-3.45$ |
| Ones of hours | $-2.5\leq x\leq-1.25$ | $-3\leq x\leq-2.6$ | $-1.15\leq x\leq-0.75$ |
| Tens of minutes | $1.25\leq x\leq2.5$ | $0.75\leq x\leq1.15$ | $2.6\leq x\leq3$ |
| Ones of minutes | $3.95\leq x\leq5.2$ | $3.45\leq x\leq3.85$ | $5.3\leq x\leq5.7$ |

I assigned mathematical expressions to the rectangles' **Fill** properties. An opacity of $1$ makes a segment black; an opacity of $0.2$ makes it light gray. Keeping the inactive segments gray leaves the seven-segment outlines visible at all times. The fill functions do not replace entire numerals: they control individual rectangles, which combine to form each numeral.

The following sections contain **all seventeen fill functions**, written with the same numerical conditions as in my Desmos graph. The long functions are kept on single lines so their full patterns can be read by scrolling horizontally, rather than splitting them into stacked cases.

## 1. Tens-of-Hours Display — A Simple Piecewise Function

The leftmost position is the simplest. It shows **1** when the hour is 12, 10, or 11, and remains blank for the single-digit hours from 1 through 9.

Only the **upper-right and lower-right vertical segments** need to become fully illuminated; the other five rectangles stay light gray. Both active segments use the same fill equation.

**$f_{17}(x)$ — upper-right and lower-right segments:**

$$
f_{17}(x)=\left\{\frac{72a}{\pi}<11.5:1,\;11.5<\frac{72a}{\pi}<119.5:0.2,\;\frac{72a}{\pi}>119.5:1\right\}
$$

The first part of the function lights the two vertical segments at the beginning of the cycle. The middle interval dims them through the single-digit hours. The final interval lights them again when the hour reaches 10. I chose half-integer boundaries to put the switch between consecutive five-minute settings.

These three screenshots show every distinct state of this position: **1 → blank → 1**.

| **12:00 — 1** | **1:00 — blank** | **10:00 — 1** |
| :---: | :---: | :---: |
| ![Tens of hours showing 1 at 12:00](<images/tens-hours-1.png>) | ![Tens of hours blank at 1:00](<images/tens-hours-off.png>) | ![Tens of hours showing 1 at 10:00](<images/tens-hours-1 (second).png>) |

## 2. Ones-of-Hours Display — Seven Long Piecewise Functions

The second position must display the entire sequence of ones-of-hours digits:

$$
2\rightarrow1\rightarrow2\rightarrow3\rightarrow4\rightarrow5\rightarrow6\rightarrow7\rightarrow8\rightarrow9\rightarrow0\rightarrow1\rightarrow2
$$

This requires a different combination of illuminated rectangles at every hour. I could not control the whole position with one simple condition, so I made **a separate piecewise fill function for each of its seven segments**. The functions use $72a/\pi$ to determine which intervals require full opacity and which intervals require reduced opacity.

Some segments are bright for most numerals and need only a few conditions. Others switch many times during the cycle, creating much longer expressions. Below are all seven equations, with the location of each segment.

### Middle horizontal segment — $f_{10}(x)$

$$
f_{10}(x)=\left\{\frac{72a}{\pi}<11.5:1,\;11.5<\frac{72a}{\pi}<23.5:0.2,\;23.5<\frac{72a}{\pi}<83.5:1,\;83.5<\frac{72a}{\pi}<95.5:0.2,\;95.5<\frac{72a}{\pi}<119.5:1,\;119.5<\frac{72a}{\pi}<143.5:0.2,\;\frac{72a}{\pi}>143.5:1\right\}
$$

The middle horizontal segment is dim for numerals that do not use a central bar, such as 1, 7, and 0. Its function alternates between bright and dim ranges as the hour changes.

### Upper-right vertical segment — $f_{11}(x)$

$$
f_{11}(x)=\left\{\frac{72a}{\pi}<59.5:1,\;59.5<\frac{72a}{\pi}<83.5:0.2,\;\frac{72a}{\pi}>83.5:1\right\}
$$

The upper-right vertical segment stays bright for most of the hours; it dims during the interval corresponding to the numerals 5 and 6.

### Lower-right vertical segment — $f_{12}(x)$

$$
f_{12}(x)=\left\{\frac{72a}{\pi}<11.5:0.2,\;11.5<\frac{72a}{\pi}<23.5:1,\;23.5<\frac{72a}{\pi}<35.5:0.2,\;35.5<\frac{72a}{\pi}<143.5:1,\;\frac{72a}{\pi}>143.5:0.2\right\}
$$

The lower-right vertical segment is dim for 2, but it is needed by many of the other digits. The function changes its opacity as the displayed hour changes.

### Bottom horizontal segment — $f_{13}(a)$

$$
f_{13}(a)=\left\{\frac{72a}{\pi}<11.5:1,\;11.5<\frac{72a}{\pi}<23.5:0.2,\;23.5<\frac{72a}{\pi}<47.5:1,\;47.5<\frac{72a}{\pi}<59.5:0.2,\;59.5<\frac{72a}{\pi}<83.5:1,\;83.5<\frac{72a}{\pi}<95.5:0.2,\;95.5<\frac{72a}{\pi}<107.5:1,\;107.5<\frac{72a}{\pi}<119.5:0.2,\;119.5<\frac{72a}{\pi}<131.5:1,\;131.5<\frac{72a}{\pi}<143.5:0.2,\;\frac{72a}{\pi}>143.5:1\right\}
$$

The bottom horizontal segment has one of the longest expressions because its fill changes repeatedly throughout the twelve-hour cycle. Each interval specifies the opacity for that segment at the corresponding hour.

### Upper-left vertical segment — $f_{14}(x)$

$$
f_{14}(x)=\left\{\frac{72a}{\pi}<47.5:0.2,\;47.5<\frac{72a}{\pi}<83.5:1,\;83.5<\frac{72a}{\pi}<95.5:0.2,\;95.5<\frac{72a}{\pi}<131.5:1,\;\frac{72a}{\pi}>131.5:0.2\right\}
$$

The upper-left vertical segment is used by digits such as 4, 5, 6, 8, 9, and 0. It can therefore be dimmed across several earlier hour values before becoming bright.

### Lower-left vertical segment — $f_{15}(x)$

$$
f_{15}(x)=\left\{\frac{72a}{\pi}<11.5:1,\;11.5<\frac{72a}{\pi}<23.5:0.2,\;23.5<\frac{72a}{\pi}<35.5:1,\;35.5<\frac{72a}{\pi}<71.5:0.2,\;71.5<\frac{72a}{\pi}<83.5:1,\;83.5<\frac{72a}{\pi}<95.5:0.2,\;95.5<\frac{72a}{\pi}<107.5:1,\;107.5<\frac{72a}{\pi}<119.5:0.2,\;119.5<\frac{72a}{\pi}<131.5:1,\;131.5<\frac{72a}{\pi}<143.5:0.2,\;\frac{72a}{\pi}>143.5:1\right\}
$$

The lower-left vertical segment requires another long sequence of conditions, including bright intervals for numerals such as 2, 6, 8, and 0.

### Top horizontal segment — $f_{16}(x)$

$$
f_{16}(x)=\left\{\frac{72a}{\pi}<11.5:1,\;11.5<\frac{72a}{\pi}<23.5:0.2,\;23.5<\frac{72a}{\pi}<47.5:1,\;47.5<\frac{72a}{\pi}<59.5:0.2,\;59.5<\frac{72a}{\pi}<131.5:1,\;131.5<\frac{72a}{\pi}<143.5:0.2,\;\frac{72a}{\pi}>143.5:1\right\}
$$

The top horizontal segment is bright for most numerals, but dims for 1 and 4. The piecewise boundaries encode those exceptions.

All seven functions evaluate the same slider position. Their combined opacities determine which strokes appear, so the digit changes continuously with the graph rather than switching between images.

### All Thirteen Hour States

These screenshots capture the ones-of-hours display at each hour, starting at 12:00, progressing through 11:00, and returning to 12:00. They belong together as a single visual progression.

| **12:00 — 2** | **1:00 — 1** | **2:00 — 2** |
| :---: | :---: | :---: |
| ![Ones of hours showing 2 at 12:00](<images/ones-hours-2.png>) | ![Ones of hours showing 1 at 1:00](<images/ones-hours-1.png>) | ![Ones of hours showing 2 at 2:00](<images/ones-hours-2 (second).png>) |
| **3:00 — 3** | **4:00 — 4** | **5:00 — 5** |
| ![Ones of hours showing 3](<images/ones-hours-3.png>) | ![Ones of hours showing 4](<images/ones-hours-4.png>) | ![Ones of hours showing 5](<images/ones-hours-5.png>) |
| **6:00 — 6** | **7:00 — 7** | **8:00 — 8** |
| ![Ones of hours showing 6](<images/ones-hours-6.png>) | ![Ones of hours showing 7](<images/ones-hours-7.png>) | ![Ones of hours showing 8](<images/ones-hours-8.png>) |
| **9:00 — 9** | **10:00 — 0** | **11:00 — 1** |
| ![Ones of hours showing 9](<images/ones-hours-9.png>) | ![Ones of hours showing 0](<images/ones-hours-0.png>) | ![Ones of hours showing 1 at 11:00](<images/ones-hours-1 (second).png>) |
|  | **12:00 — 2 again** |  |
|  | ![Ones of hours showing 2 at the end of the cycle](<images/ones-hours-2 (third).png>) |  |

## 3. Tens-of-Minutes Display — Repetition with Modular Arithmetic

The third position displays **0, 1, 2, 3, 4, 5**, changing every ten minutes. Unlike the hour digits, this exact sequence repeats in every single hour. Writing separate piecewise ranges for twelve repetitions would have produced very long expressions for no new behavior.

To avoid that repetition, I used **modular arithmetic**. The repeated expression in the seven fill functions is

$$
\operatorname{mod}\left(\frac{72a}{\pi}-0,12\right)
$$

The modulo operation returns the remainder after division by 12. Since $72a/\pi$ increases by 12 units each hour, the remainder starts over at the beginning of every hour. At :00, :10, :20, :30, :40, and :50, its values are $0$, $2$, $4$, $6$, $8$, and $10$.

This allowed me to write the conditions for **one hour only**. The same functions then repeat automatically across the full twelve-hour cycle. I still needed seven separate opacity functions, because each segment has a different sequence of on and off states.

### Every Modulo-Based Fill Function

### Middle horizontal segment — $f_{3}(x)$

$$
f_{3}(x)=\left\{\operatorname{mod}\left(\frac{72a}{\pi}-0,12\right)<3.5:0.2,\;3.5<\operatorname{mod}\left(\frac{72a}{\pi}-0,12\right)<11.5:1,\;\operatorname{mod}\left(\frac{72a}{\pi}-0,12\right)>11.5:0.2\right\}
$$

The middle horizontal segment is dim for 0 and 1, then bright for 2, 3, 4, and 5. The final interval prepares it to return to its starting state.

### Upper-left vertical segment — $f_{4}(x)$

$$
f_{4}(x)=\left\{\operatorname{mod}\left(\frac{72a}{\pi}-0,12\right)<1.5:1,\;1.5<\operatorname{mod}\left(\frac{72a}{\pi}-0,12\right)<7.5:0.2,\;\operatorname{mod}\left(\frac{72a}{\pi}-0,12\right)>7.5:1\right\}
$$

The upper-left vertical segment is bright for 0, 4, and 5, but dim for 1, 2, and 3.

### Lower-left vertical segment — $f_{5}(x)$

$$
f_{5}(x)=\left\{\operatorname{mod}\left(\frac{72a}{\pi}-0,12\right)<1.5:1,\;1.5<\operatorname{mod}\left(\frac{72a}{\pi}-0,12\right)<3.5:0.2,\;3.5<\operatorname{mod}\left(\frac{72a}{\pi}-0,12\right)<5.5:1,\;5.5<\operatorname{mod}\left(\frac{72a}{\pi}-0,12\right)<11.5:0.2,\;\operatorname{mod}\left(\frac{72a}{\pi}-0,12\right)>11.5:1\right\}
$$

The lower-left vertical segment is bright for 0 and 2, but dim for 1, 3, 4, and 5.

### Bottom horizontal segment — $f_{6}(x)$

$$
f_{6}(x)=\left\{\operatorname{mod}\left(\frac{72a}{\pi}-0,12\right)<1.5:1,\;1.5<\operatorname{mod}\left(\frac{72a}{\pi}-0,12\right)<3.5:0.2,\;3.5<\operatorname{mod}\left(\frac{72a}{\pi}-0,12\right)<7.5:1,\;7.5<\operatorname{mod}\left(\frac{72a}{\pi}-0,12\right)<9.5:0.2,\;\operatorname{mod}\left(\frac{72a}{\pi}-0,12\right)>9.5:1\right\}
$$

The bottom horizontal segment is bright for 0, 2, 3, and 5 and dim for 1 and 4.

### Upper-right vertical segment — $f_{7}(x)$

$$
f_{7}(x)=\left\{\operatorname{mod}\left(\frac{72a}{\pi}-0,12\right)<9.5:1,\;9.5<\operatorname{mod}\left(\frac{72a}{\pi}-0,12\right)<11.5:0.2,\;\operatorname{mod}\left(\frac{72a}{\pi}-0,12\right)>11.5:1\right\}
$$

The upper-right vertical segment is bright for 0 through 4, then dims for 5.

### Lower-right vertical segment — $f_{8}(x)$

$$
f_{8}(x)=\left\{\operatorname{mod}\left(\frac{72a}{\pi}-0,12\right)<3.5:1,\;3.5<\operatorname{mod}\left(\frac{72a}{\pi}-0,12\right)<5.5:0.2,\;\operatorname{mod}\left(\frac{72a}{\pi}-0,12\right)>5.5:1\right\}
$$

The lower-right vertical segment is bright for every digit in this sequence except 2.

### Top horizontal segment — $f_{9}(x)$

$$
f_{9}(x)=\left\{\operatorname{mod}\left(\frac{72a}{\pi}-0,12\right)<1.5:1,\;1.5<\operatorname{mod}\left(\frac{72a}{\pi}-0,12\right)<3.5:0.2,\;3.5<\operatorname{mod}\left(\frac{72a}{\pi}-0,12\right)<7.5:1,\;7.5<\operatorname{mod}\left(\frac{72a}{\pi}-0,12\right)<9.5:0.2,\;\operatorname{mod}\left(\frac{72a}{\pi}-0,12\right)>9.5\right\}
$$

The top horizontal segment follows the same visible bright-and-dim pattern as the bottom segment. The final condition is reproduced as it appears in the exported Desmos equation, without an explicit `:1` value.

Modulo made this section considerably more manageable: the equations only need to describe a repeating six-digit pattern, rather than treating every hour as a completely separate case.

### All Six Tens-of-Minutes States

These screenshots show one complete sequence of the possible values, at the beginning of each ten-minute interval.

| **:00 — 0** | **:10 — 1** | **:20 — 2** |
| :---: | :---: | :---: |
| ![Tens of minutes showing 0](<images/tens-minutes-0.png>) | ![Tens of minutes showing 1](<images/tens-minutes-1.png>) | ![Tens of minutes showing 2](<images/tens-minutes-2.png>) |
| **:30 — 3** | **:40 — 4** | **:50 — 5** |
| ![Tens of minutes showing 3](<images/tens-minutes-3.png>) | ![Tens of minutes showing 4](<images/tens-minutes-4.png>) | ![Tens of minutes showing 5](<images/tens-minutes-5.png>) |

## 4. Ones-of-Minutes Display — Complementary Cosine Functions

The fourth digit changes every five minutes, so it only needs to alternate between **0 and 5**. The most interesting part of this display is that I did not use piecewise conditions to create the alternation. Instead, I used two trigonometric functions whose outputs move in opposite directions.

The top, upper-left, lower-right, and bottom segments stay bright for both 0 and 5. Only three segments need to change: the **middle**, **upper-right**, and **lower-left**.

### Middle horizontal segment — $f_1(x)$

$$
f_1(x)=0.2+0.8\left(\frac{1-\cos(72a)}{2}\right)
$$

This function controls the **middle horizontal segment**, which is dim for 0 and bright for 5.

### Upper-right and lower-left segments — $f_2(x)$

$$
f_2(x)=0.2+0.8\left(\frac{1+\cos(72a)}{2}\right)
$$

This function controls the **upper-right** and **lower-left vertical segments**. Both are bright for 0 and dim for 5.

The two functions are complementary: when one gives opacity $1$, the other gives $0.2$. The $0.8$ and $0.2$ in the formulas scale the cosine output from its natural range of $[-1,1]$ into the opacity range $[0.2,1]$.

At an exact five-minute setting, the slider has the form

$$
a=\frac{k\pi}{72},\qquad k=0,1,2,\ldots
$$

and therefore

$$
\cos(72a)=\cos(k\pi)=(-1)^k
$$

When $k$ is even, the cosine is $1$, making $f_1(x)=0.2$ and $f_2(x)=1$: the segments form **0**. When $k$ is odd, the cosine is $-1$, making $f_1(x)=1$ and $f_2(x)=0.2$: the segments form **5**. At the next five-minute position, they switch back.

Unlike the piecewise functions in the earlier digits, these cosine functions also move smoothly between the two opacity extremes. The digits look fully formed at the five-minute positions, and their changing fill is visible if I drag the slider continuously between them. This is periodic trigonometric behavior, not a literal alternating series.

### Two Complete 0-to-5 Alternations

These four screenshots show **0 → 5 → 0 → 5**, corresponding to 12:00, 12:05, 12:10, and 12:15.

| **12:00 — 0** | **12:05 — 5** |
| :---: | :---: |
| ![Ones of minutes showing 0 at 12:00](<images/ones-minutes-0.png>) | ![Ones of minutes showing 5 at 12:05](<images/ones-minutes-5.png>) |
| **12:10 — 0 again** | **12:15 — 5 again** |
| ![Ones of minutes returning to 0 at 12:10](<images/ones-minutes-0 (again).png>) | ![Ones of minutes returning to 5 at 12:15](<images/ones-minutes-5 (again).png>) |

## The Independently Animated Colon

I made the two dots between hours and minutes with two more rectangular inequalities:

$$
-0.2\leq x\leq0.2\;\left\{-2.6\leq y\leq-2.2\right\}
$$

$$
-0.2\leq x\leq0.2\;\left\{-4.8\leq y\leq-4.4\right\}
$$

Their fill is controlled by a second animated slider, $b$, which changes between $0.2$ and $1$ in increments of $0.8$. This makes the colon blink independently of $a$, even when the analog hands and digital digits are stopped at a specific time.

## Bringing the Entire Clock Together

The analog hands, horizontal marker, and four digit positions are all driven by $a$, but they each interpret it differently. The hands use trigonometric coordinates, the time marker uses a linear expression, the hour displays use piecewise intervals, the tens-of-minutes display uses remainders, and the ones-of-minutes display uses oscillating cosine functions.

For example, at 12:05, $a=\pi/72$ and $72a/\pi=1$. The hour hand has advanced slightly beyond 12, while the minute hand reaches the first five-minute mark. The hours still read 12. The tens-of-minutes digit is 0 because its modulo remainder is 1, and the ones-of-minutes digit is 5 because $\cos(72a)=-1$. The digital display reads **12:05**, matching the analog clock.

The four displays were not solved in exactly the same way because each one had a different pattern to represent. That was the central challenge of the project—and the reason I used several different mathematical ideas rather than one enormous function.

## Mathematical Concepts

- **Trigonometry:** Positioning the hour markers and rotating two clock hands at different angular speeds.
- **Inequalities and coordinate geometry:** Constructing circles, a circular ring, and the seven rectangles in every digit.
- **Parametric coordinates and Desmos tables:** Drawing clock hands between a fixed center and moving endpoints.
- **Linear scaling:** Translating an angular parameter into a numerical timeline measured in five-minute increments.
- **Piecewise functions:** Controlling the illumination of individual seven-segment strokes across the twelve-hour cycle.
- **Modular arithmetic:** Repeating the same tens-of-minutes pattern every hour without duplicating conditions.
- **Periodic functions:** Alternating opacity with a pair of complementary cosine expressions.
- **Synchronization:** Coordinating different mathematical representations of time through a single variable.

I particularly liked discovering that the most efficient mathematical technique depended on the behavior I needed. The tens-of-hours digit required a simple switch; the ones-of-hours digit required a large collection of piecewise rules; the tens-of-minutes digit was naturally expressed using modulo; and the ones-of-minutes digit could be built from cosine. Together, those different functions create one interactive clock using only Desmos mathematics.
