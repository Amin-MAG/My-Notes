# Cryptocurrency
The most considerable trouble in digital currencies is double-spending. The initial solution is to have a central system checking these transactions. As you may consider, this system is the single point of failure!
Bitcoin uses Blockchain. Blockchain is a kind of distributed database.


# Security in Cryptocurrency
- Is the coin genuine?
- Is the history written on stone?
- Can anybody double spend?

## Hash
General properties:
-	Free input size
-	Specific output length
-	Efficiently computable; O(n)

```python
import hashlib

# For example sha256
print(hashlib.sha256('amin'.encode()).hexdigest()) 

# Result
# 29a669940f66f7d5d5539801dc422018a92d4799060e2c576d9a30887eba605a
```

```bash
sha256sum
hi my name is amin
^D
#d6461ef3e7d11b9952ba8732c8650808c3e2e1ee68c407e38233917c580297b9 
```

## Cryptographic Hash
General Properties:
- Collision Resistance
- Hiding
- Puzzle friendliness: to be difficult to bruteforce

# Resources
- A Comprehensive Introduction: BITCOIN AND CRYPTOCURRENCY TECHNOLOGIES
- [Jadi Blockchain Course](https://docs.google.com/presentation/d/1sqgx2gQE0G2UXa4MpOYNPyDkFPsjuBZuKLBhAtfBXC0/edit#slide=id.p)