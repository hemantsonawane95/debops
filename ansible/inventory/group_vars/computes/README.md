# Numa topology


```
node 0 cpus: 0 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 32 33 34 35 36 37 38 39 40 41 42 43 44 45 46 47
node 0 size: 515599 MB
node 0 free: 27797 MB
node 1 cpus: 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30 31 48 49 50 51 52 53 54 55 56 57 58 59 60 61 62 63
node 1 size: 516084 MB
node 1 free: 26751 MB
node distances:
node   0   1 
  0:  10  20 
  1:  20  10
```

Let's have IRQ on 0,1,3 and 16,17,18.
It means that `IRQBALANCE_BANNED_CPULIST` should be `3-15,19-63`.