# Results

# 🤔 Input Samples

## Sample 1

```c
addi $4,$0,27
xori $5,$4,5
add $6,$4,$5
sub $7,$5,$4
slt $8,$7,$6
or $9,$7,$0	
and $10,$7,$0
sll $11,$7,1
srl $12,$7,1
```

![Results%208364e/Untitled.png](Results%208364e/Untitled.png)

```xml
2004001b
38850005
00853020
00a43822
00e6402a
00e04825
00e05024
00075840
00076042
```

## Sample 2

```xml
20020005
2003000c
2067fff7
00e22025
00642824
00a42820
10a7000a
0064202a
10800001
20050000
00e2202a
00853820
00e23822
ac670044
8c020050
08000011
20020001
ac0200
```

![Results%208364e/Untitled%201.png](Results%208364e/Untitled%201.png)

## Sample 3

```c
addi $5,$0,3	
beq $0, $0, next
add $6, $5, $0	
next: sw $5,4($5)
lw $7, 4($5)
```

![Results%208364e/Untitled%202.png](Results%208364e/Untitled%202.png)

![Results%208364e/Untitled%203.png](Results%208364e/Untitled%203.png)

```xml
20050003
10000001
00a03020
aca50004
8ca70004
```