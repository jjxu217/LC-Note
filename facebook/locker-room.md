# Locker Room

It is the end of the school year, and the graduating class of Moore Science High \(n students in all\) has decided to end the year by  
making quite a lot of noise. They line up in the locker room, where all n lockers are closed. The first student runs down the row of lockers,  
opening each one. Then, the second runs down, slamming every other locker \(this is where the noise comes in\).  
The third student then runs down, opening \(or slamming\) every third locker, and so on until all n students have gone through,  
each opening \(or closing\) each locker evenly divisible by i, where i is the ith student. Write a function that prints each locker state after each student is done.

Output:  
\["O", "O", "O", "O"\]  
\["O", "C", "O", "C"\]  
\["O", "C", "C", "C"\]  
\["O", "C", "C", "O"\]

```python
def lockerMusic(n):
    string = ['C'] * n:
    for i in range(n)：
        for j in range(i, n, i + 1):
            if string[j] = 'C' if string[j] == 'O' else 'O'
        print(string)
```

