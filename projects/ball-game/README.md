# Take Me Out to the Ball Game — Mathematical Music Player

An interactive Desmos project that uses exponential functions, domain restrictions, and the `tone()` operator to recreate the melody and harmony of *Take Me Out to the Ball Game*.

After discovering that Desmos could generate sound from mathematical expressions, I experimented with representing musical notes as frequencies and combining them into a complete composition.

I initially constructed the song in A, then expanded the project by creating an interactive key-transposition system. This allows the entire melody and harmony to shift between musical keys without changing their original note sequences or timing.

[**Explore the Interactive Graph in Desmos**](https://www.desmos.com/calculator/ifstrdlxoa)

## How It Works

The project represents music mathematically by assigning frequencies to notes and defining the intervals during which those frequencies are played.

The graph displays the melody in black and the harmony in dotted red. The horizontal axis represents progression through the composition, while the vertical axis represents frequency in hertz.

### Converting Musical Notes into Frequencies

The foundation of the project is a function based on the twelve-tone equal-tempered tuning system:

$$
f(x)=440\cdot2^{(x+K_{ey})/12}
$$

The constant 440 represents the frequency, in hertz, of A4. The variable $x$ represents a number of semitones relative to A4, while $K_{ey}$ provides an additional semitone adjustment for changing musical keys.

In equal temperament, moving up one semitone multiplies the frequency by $2^{1/12}$, while moving up twelve semitones doubles the frequency.

For example, when $K_{ey}=0$:

- $f(0)=440$ Hz, corresponding to A4.
- $f(4)\approx554.37$ Hz, corresponding to C-sharp 5.
- $f(7)\approx659.26$ Hz, corresponding to E5.
- $f(12)=880$ Hz, corresponding to A5.

Rather than manually assigning a separate frequency to every musical note, I used this exponential relationship to calculate frequencies from semitone intervals.

The same function is used throughout the entire composition, allowing the melody, harmony, and key-transposition system to share one mathematical foundation.

### Constructing the Melody

I represented the melody using a list of 60 domain-restricted expressions.

Each expression contains a frequency calculation and a restriction determining when that frequency should be active.

For example, the beginning of the melody includes:

$$
f(0)\{0\leq x\leq2\}
$$

$$
f(12)\{2\leq x\leq3\}
$$

$$
f(9)\{3\leq x\leq4\}
$$

The first expression plays the frequency corresponding to $f(0)$ over the interval from 0 to 2. The second plays $f(12)$ from 2 to 3, and the third plays $f(9)$ from 3 to 4.

This structure allows each note to have its own pitch, starting position, and duration.

I also introduced small gaps between certain expressions to separate repeated notes. For example, three consecutive occurrences of $f(9)$ are assigned the intervals:

- $36\leq x\leq37.8$
- $38\leq x\leq38.8$
- $39\leq x\leq40$

The gaps distinguish the repeated notes rather than allowing them to sound as one uninterrupted tone.

By combining these restrictions into a single list, I encoded the melody as a mathematical sequence of frequencies distributed across a timeline.

### Constructing the Harmony

I created a separate list containing 25 domain-restricted expressions to represent the harmony.

The harmony follows the same frequency function as the melody, but its notes generally extend over longer intervals, providing accompaniment beneath the main tune.

For example, the beginning of the harmony includes:

$$
f(0)\{0\leq x\leq6\}
$$

$$
f(4)\{6\leq x\leq9\}
$$

$$
f(-5)\{9\leq x\leq12\}
$$

Because the melody and harmony are stored in separate lists, both can be evaluated at the same position on the timeline and played simultaneously.

An additional feature is the use of overlapping intervals within the harmony itself.

Near the end of the composition, several harmony expressions are active at the same time. For example, during $90\leq x\leq96$, the harmony contains:

$$
f(0),\quad f(4),\quad f(7)
$$

In the original key, these correspond to A, C-sharp, and E, forming an A major chord.

This demonstrates how overlapping domain restrictions can produce several simultaneous frequencies instead of a single note.

The result is a composition in which mathematical expressions determine not only individual pitches and durations, but also the relationships between notes played together.

### Generating Sound and Controlling Playback

To convert the frequency expressions into sound, I used Desmos's `tone()` operator.

The melody is played using:

` tone(M_elody(a)) `

The harmony is played using:

` tone(H_armony(a),0.6) `

The second argument in the harmony expression reduces its volume relative to the melody.

The variable $a$ acts as a playback-position parameter. As its value increases, Desmos evaluates the melody and harmony expressions at successive positions along the timeline.

I also constructed a green vertical playback indicator to show the currently selected position on the graph.

As the slider moves, the indicator travels across the visualized notes while the corresponding frequencies are passed to the `tone()` expressions.

This connects the visual representation of the composition with the sounds generated by the mathematical functions.

### Musical Key Transposition

After completing the original composition in A, I realized that the exponential frequency function could be modified to change the key of the entire song.

Instead of rewriting the melody and harmony expressions for different keys, I introduced the variable $K_{ey}$ into the exponent:

$$
f(x)=440\cdot2^{(x+K_{ey})/12}
$$

When $K_{ey}=0$, the composition plays in its original key of A.

Changing $K_{ey}$ by a value $k$ multiplies every frequency by the same factor:

$$
2^{k/12}
$$

Because both the melody and harmony use the same function, this adjustment changes every note simultaneously while preserving the intervals between notes.

For example, setting $K_{ey}=-10$ gives:

$$
f(x)=440\cdot2^{(x-10)/12}
$$

Every frequency is multiplied by $2^{-10/12}$, transposing the composition ten semitones downward from A to B.

Alternatively, setting $K_{ey}=10$ gives:

$$
f(x)=440\cdot2^{(x+10)/12}
$$

Every frequency is multiplied by $2^{10/12}$, transposing the composition ten semitones upward from A to G.

Although these adjustments change the absolute frequencies, the mathematical relationships between the notes remain unchanged.

For any two semitone inputs $x_1$ and $x_2$, their frequency ratio is:

$$
\frac{f(x_2)}{f(x_1)}=2^{(x_2-x_1)/12}
$$

The key-adjustment variable cancels from this ratio, showing mathematically why the intervals between notes are preserved during transposition.

I also created a blue key-selection bar at the top of the graph, labeled with musical notes. Moving the selector changes $K_{ey}$ and allows the composition to be transposed interactively.

This was an extension of the original project: a single additional variable made it possible to transform the entire musical composition without modifying its 60 melody expressions or 25 harmony expressions individually.

## Full Mathematical Expressions

The complete melody and harmony are constructed from 85 domain-restricted expressions.

The expressions are included below to document the mathematical structure of the composition. They are expandable because of their length.

### Complete Melody

<details>
<summary>View Full Melody Expression — 60 Entries</summary>

```text
M_elody(x) = [
  f(0){0 ≤ x ≤ 2},       f(12){2 ≤ x ≤ 3},
  f(9){3 ≤ x ≤ 4},       f(7){4 ≤ x ≤ 5},
  f(4){5 ≤ x ≤ 6},       f(7){6 ≤ x ≤ 9},
  f(2){9 ≤ x ≤ 12},      f(0){12 ≤ x ≤ 14},
  f(12){14 ≤ x ≤ 15},    f(9){15 ≤ x ≤ 16},
  f(7){16 ≤ x ≤ 17},     f(4){17 ≤ x ≤ 18},
  f(7){18 ≤ x ≤ 24},     f(9){24 ≤ x ≤ 25},
  f(8){25 ≤ x ≤ 26},     f(9){26 ≤ x ≤ 27},
  f(4){27 ≤ x ≤ 28},     f(5){28 ≤ x ≤ 29},
  f(7){29 ≤ x < 30},     f(9){30 ≤ x ≤ 32},
  f(5){32 ≤ x ≤ 33},     f(2){33 ≤ x ≤ 36},
  f(9){36 ≤ x ≤ 37.8},   f(9){38 ≤ x ≤ 38.8},
  f(9){39 ≤ x ≤ 40},     f(11){40 ≤ x ≤ 41},
  f(12){41 ≤ x ≤ 42},    f(14){42 ≤ x ≤ 43},
  f(11){43 ≤ x ≤ 44},    f(9){44 ≤ x ≤ 45},
  f(7){45 ≤ x ≤ 46},     f(4){46 ≤ x ≤ 47},
  f(2){47 ≤ x ≤ 48},     f(0){48 ≤ x ≤ 50},
  f(12){50 ≤ x ≤ 51},    f(9){51 ≤ x ≤ 52},
  f(7){52 ≤ x ≤ 53},     f(4){53 ≤ x ≤ 54},
  f(7){54 ≤ x ≤ 57},     f(2){57 ≤ x ≤ 60},
  f(0){60 ≤ x ≤ 61},     f(2){61 ≤ x ≤ 62},
  f(4){62 ≤ x ≤ 63},     f(5){63 ≤ x ≤ 64},
  f(7){64 ≤ x ≤ 65},     f(8){65 ≤ x ≤ 66},
  f(9){66 ≤ x ≤ 70},     f(9){70.2 ≤ x ≤ 71},
  f(11){71 ≤ x ≤ 72},    f(12){72 ≤ x ≤ 74.8},
  f(12){75 ≤ x ≤ 77.8},  f(12){78 ≤ x ≤ 79},
  f(11){79 ≤ x ≤ 80},    f(9){80 ≤ x ≤ 81},
  f(7){81 ≤ x ≤ 82},     f(6){82 ≤ x ≤ 83},
  f(7){83 ≤ x ≤ 84},     f(9){84 ≤ x ≤ 87},
  f(11){87 ≤ x ≤ 90},    f(12){90 ≤ x ≤ 96}
]
```

</details>

### Complete Harmony

<details>
<summary>View Full Harmony Expression — 25 Entries</summary>

```text
H_armony(x) = [
  f(0){0 ≤ x ≤ 6},       f(4){6 ≤ x ≤ 9},
  f(-5){9 ≤ x ≤ 12},     f(0){12 ≤ x ≤ 18},
  f(4){18 ≤ x ≤ 24},     f(5){24 ≤ x ≤ 30},
  f(2){30 ≤ x ≤ 36},     f(-1){36 ≤ x ≤ 42},
  f(7){42 ≤ x ≤ 45},     f(-5){45 ≤ x ≤ 48},
  f(0){48 ≤ x ≤ 54},     f(4){54 ≤ x ≤ 57},
  f(-5){57 ≤ x ≤ 60},    f(-3){60 ≤ x ≤ 63},
  f(-1){63 ≤ x ≤ 66},    f(0){66 ≤ x ≤ 72},
  f(5){72 ≤ x ≤ 75},     f(6){75 ≤ x ≤ 78},
  f(7){78 ≤ x ≤ 84},     f(5){84 ≤ x ≤ 87},
  f(7){90 ≤ x ≤ 96},     f(4){90 ≤ x ≤ 96},
  f(0){84 ≤ x ≤ 87},     f(-5){87 ≤ x ≤ 90},
  f(0){90 ≤ x ≤ 96}
]
```

</details>

These expressions use Desmos lists rather than conventional single-valued piecewise functions. This allows multiple restricted expressions to remain active over the same interval, producing simultaneous notes and chords.

## Screenshots

### Musical Composition and Harmony

**1. Complete Composition in A**

The original composition displays the melody in black and harmony in dotted red. The expressions panel shows the shared exponential frequency function, the two note lists, and the `tone()` commands used to generate sound.

![Original Composition in A](images/ball-game-01.png)

**2. Melody and Harmony Detail**

A closer view of the later portion of the composition shows the different note durations and the relationship between the melody and harmony. Multiple harmony frequencies appear simultaneously near the ending, demonstrating how overlapping restrictions produce chords.

![Melody and Harmony Detail](images/ball-game-02.png)

### Musical Key Transposition

**3. Transposition to B**

Setting $K_{ey}=-10$ modifies the frequency function to:

$$
f(x)=440\cdot2^{(x-10)/12}
$$

This multiplies every note frequency by $2^{-10/12}$, transposing the composition ten semitones downward from A to B.

The melody and harmony move into a lower frequency range while retaining their original timing and relative musical intervals.

![Transposition to B](images/ball-game-03.png)

**4. Transposition to G**

Setting $K_{ey}=10$ modifies the frequency function to:

$$
f(x)=440\cdot2^{(x+10)/12}
$$

This multiplies every note frequency by $2^{10/12}$, transposing the composition ten semitones upward from A to G.

Unlike the previous example, the entire composition shifts into a higher frequency range. The same melody and harmony expressions remain unchanged, demonstrating how modifying one exponent can transpose the entire piece.

![Transposition to G](images/ball-game-04.png)

### Interactive Playback

**5. Playing the Composition**

The green playback indicator is positioned at $a=42.5$.

At this value, the melody and harmony lists are evaluated at the same position on the timeline, and the corresponding frequencies are supplied to the `tone()` operators.

As the playback slider moves through the composition, the active frequencies change according to the domain restrictions, allowing the graph to produce the musical sequence.

![Interactive Playback](images/ball-game-05.png)

## Mathematical Concepts

This project combines exponential functions, equal-tempered musical tuning, domain restrictions, lists of conditional expressions, simultaneous function evaluation, frequency ratios, and parameter-based transformations.

The composition demonstrates how mathematical expressions can encode musical information, including pitch, timing, note duration, repeated notes, and harmony.

The key-transposition system extends this idea by showing how a single variable in an exponential function can transform an entire composition while preserving its internal frequency relationships.

By constructing the song through Desmos, I explored how mathematical functions can be used not only to visualize information, but also to generate sound and create an interactive musical experience.
