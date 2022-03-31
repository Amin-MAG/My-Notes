# CI Quiz - M.Amin Ghasvari 97521432

---

# Solution

## Diagrams

### This is the temperature diagram.

![CI%20Quiz%20-%20%2032fc3/temp.png](temp.png)

$VeryLow < Low < Medium < Large < VeryLarge$

### This is the humidity diagram.

![CI%20Quiz%20-%20%2032fc3/hum.png](hum.png)

$Low < Medium < Large < VeryLarge$

### This is the power diagram.

![CI%20Quiz%20-%20%2032fc3/power.png](power.png)

$VeryLow < Low < Medium < Large < VeryLarge$

## Rules

![Rules of our problem based on the items (1-7)](rules.png)

Rules of our problem based on the items (1-7)

## Status

- Temperature = 27
- Humidity = 35

## Temperature

$20 < Temperature = 27 < 30$

Medium Temperature: $(-1/10) * t + 3 = 0.3$

Large Temperature: $(1/10) * t - 2 = u = 0.7$

## Humidity

$20 < Humidity = 35 < 45$

Low Humidity: $(1/25) * h - (4/5) = u = 0.4$

Medium Humidity: $(-1/25) * h + (9/5) = u = 0.6$

## States

![States of humidity and temperture](table.png)

States of humidity and temperture

So now we calculate the minimum for each row.

LT, SH = $min(0.7, 0.4) = 0.4$

LT, MH = $min(0.7, 0.6) = 0.6$

MT, SH = $min(0.3, 0.4) = 0.3$

MT, MH = $min(0.3, 0.6) = 0.3$

So we calculate the maximum the minimums:

$max(0.4, 0.6, 0.3, 0.3) = 0.6$

0.6 is for Large temperature and Medium Humidity.

---

## Result

Large temperature and Medium humidity cause Large power.

### Power

$50 < Large Power < 100$

For $50 < p < 75$  : $u = (1/25) * p - 2$

For $75 < p < 100$  : $u = (-1/25) * p + 4$

$u = 0.6$

$p = 65, 85$

Using Mean of Maxima: $(65 + 85)/2 = 75$

So the power is about 75.

For calculating linear one you should use the sugonu method. but here is not needed.