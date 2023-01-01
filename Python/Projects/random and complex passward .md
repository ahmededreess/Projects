#### the project is to  How to create random and complex passward

```

import string 
import random

S1 = list(string.ascii_uppercase)
S2 = list(string.ascii_lowercase)
S3 = list(string.digits) 
S4 = list(string.punctuation)

while True:
    try:
        No_characters = input("Enter Number of characters : ")
        if int(No_characters) < 6 :
            print("Plesae enter at least 6 characters")
        else:
            break 
    except:
        print("the number you entered is not Integer")

part_1 = round(float(No_characters) * (30/100))
part_2 = round(float(No_characters) * (20/100))

passward = []

random.shuffle(S1)
random.shuffle(S2)
random.shuffle(S3)
random.shuffle(S4)

for i in range(part_1):
    passward.append(S1[i])
    passward.append(S3[i])
    
for x in range(part_2):
    passward.append(S2[x])
    passward.append(S4[x])      

if len(passward) > int(No_characters):
    passward.pop(-1)

print(passward)

```
