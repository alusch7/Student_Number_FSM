# Student Number FSM

## Problem:

The objective of this design project is to design an FSM that will cycle through my student number via the use of flip-flops and logic gates.

## Analytical Implementation:

To begin, I will create a state transition table that shows my BCD Student Number (400312849) in terms of the bits Q1, Q2, Q3, and Q4, as well as a counter C1 that will distinguish between repeated digits, as well as the required states for each flip-flop’s inputs. 

![Screenshot 2023-02-04 at 5 36 09 PM](https://user-images.githubusercontent.com/76706672/216792353-521981d4-6f61-40c6-a29a-fe9a303fcb0c.png)

While this is the most robust implementation of this design, I must make an effort to simplify the logic so that this FSM will be easier to implement physically. Unfortunately, a few options to simplify this further are not available to me. The options that are not available include having bits whose state are equal to each other (in my case Q1 ≠ Q2 ≠ Q3 ≠ Q4), and I have no bits that are all one value, so I can’t replace any given bit with just a hook up to VCC or GND, and from what I can tell, no bits are uniquely determined by any combination of the other bits.

However, there are still simplifications that I can make. In order to simplify this further, I will change the ‘don’t care’ states of C1 to unique states, and I will try to do that as best as I can (since there are only 8 numbers, one state will not be able to alternate from the last).

![Screenshot 2023-02-04 at 5 36 59 PM](https://user-images.githubusercontent.com/76706672/216792374-c9ae4c7b-b37c-46e9-8706-9343ed9fb0fb.png)

From this state transition table, I can determine the necessary inputs (J and !K) for each of my JK flip-flops.

![Screenshot 2023-02-04 at 5 37 25 PM](https://user-images.githubusercontent.com/76706672/216792383-502a3beb-da3c-4347-b629-3eaf94568fcf.png)

For my K-Maps, I decided to use a mirror layout.

### Flip Flop 1:
!K1= 0

SoP implementation of J1:

J1 = (Q3*!Q4) + (Q2*C1)

![Screenshot 2023-02-04 at 5 38 08 PM](https://user-images.githubusercontent.com/76706672/216792403-245477d4-07f6-494b-a23f-9fa7ba0519c5.png)

PoS implementation of J1:

J1 = (Q2 + Q3)*(!Q4)*(C1)

![Screenshot 2023-02-04 at 5 39 46 PM](https://user-images.githubusercontent.com/76706672/216792449-daaf9014-1417-4933-aa00-8f2d575efc91.png)

For flip flop 1, the SoP implementation requires 2 AND gates, and an OR gate. The PoS implementation also requires 2 AND gates, and an OR gate. Therefore, neither implementation is inherently any better or easier than the other, however, if later on I find an input for a different flip flop that uses one of the same operations, I will choose the implementation that matches that. Luckily, !K1 is always a 0 or “don’t care”, so it can just be hooked up directly to GND.

### Flip Flop 2:
!K2 = 0

J2 = Q1

For Flip Flop 2, !K2 is always a 0 or “don’t care”, so it can just be hooked up directly to GND, and J2 is always equal to Q1.

### Flip Flop 3:
!K3 = 0

SoP implementation of J3:

J3 = !Q1*!Q2*!C1

![Screenshot 2023-02-04 at 5 41 04 PM](https://user-images.githubusercontent.com/76706672/216792481-b0377d97-41c8-4266-9244-704b2d7a2dd8.png)

PoS implementation of J3: 

J3 = !Q1*!Q2*!C1 

![Screenshot 2023-02-04 at 5 41 59 PM](https://user-images.githubusercontent.com/76706672/216792505-06c161f2-f15b-496e-b577-ec2214df1bb5.png)

For flip flop 3, the SoP implementation requires 2 AND gates. The PoS implementation also requires 2 AND gates. Therefore, both implementations are the same, so it is trivial which one I choose. Again, !K3 is always a 0 or “don’t care”, so it can just be hooked up directly to GND.

### Flip Flop 4:
!K4 = Q3

SoP implementation of J4: 

J4 = (!Q1*!Q2*!C1) + (Q2*C1) 

![Screenshot 2023-02-04 at 5 42 37 PM](https://user-images.githubusercontent.com/76706672/216792523-e279c078-00cf-4589-a10f-f269c4b8a351.png)

PoS implementation of J4: 

J4 = (Q2 + !C1)*(!Q2 + C1)*(!Q1) 

![Screenshot 2023-02-04 at 5 43 15 PM](https://user-images.githubusercontent.com/76706672/216792543-8f92ca91-19d7-4d6f-81ea-c474dc122d14.png)

For flip flop 4, the J4 SoP implementation requires 3 AND gates and an OR gate. The J4 PoS implementation also requires 2 AND gates and 2 OR gates. I will determine which is more optimal in terms of the rest of the design at the end of this section. In this case, !K4 = Q3. 

### Flip Flop 5:
!KC1 = 0 

SoP implementation of JC1: 

JC1 = !Q1 + !Q4 

![Screenshot 2023-02-04 at 5 44 16 PM](https://user-images.githubusercontent.com/76706672/216792579-37750673-fb98-4013-8338-9980f591edda.png)

PoS implementation of JC1: 

JC1 = !Q1 + !Q4 

![Screenshot 2023-02-04 at 5 44 41 PM](https://user-images.githubusercontent.com/76706672/216792589-4685f34a-e1df-45d5-ab23-538f1b109167.png)

For flip flop 5, the SoP implementation requires 1 OR gate. The PoS implementation also requires 1 OR gate. Therefore, both implementations are the same, so it is trivial which one I choose. Again, !KC1 is always a 0 or “don’t care”, so it can just be hooked up directly to GND.

### Analytical Summary: 
Below is a table outlining the most optimal implementation of this FSM based on the SoP and Pos implementations. In total, I will need: 4 AND gates and 3 OR gates.

![Screenshot 2023-02-04 at 5 45 27 PM](https://user-images.githubusercontent.com/76706672/216792625-e4970ae4-159b-42da-8273-f1ed2d1a6113.png)

## MultiSim Implementation
![Screenshot 2023-02-04 at 5 46 36 PM](https://user-images.githubusercontent.com/76706672/216792664-017e0853-29f1-442c-b30f-b4ccc4ee4923.png)

### Output Timing Diagram:
![Screenshot 2023-02-04 at 5 47 10 PM](https://user-images.githubusercontent.com/76706672/216792680-24d45439-a139-41b9-80c7-ab6b0a959f9e.png)

In the above timing diagram, my student number is numbered in red, while each of the respective bits is labelled. Please note that the double 4 at the beginning is NOT an error, it is the 4-state repeating itself before I un-force the value (e.g. since I have to force the beginning state, it cycles on that value until I un- force it by switching the respective PR/CLR pins from a 0 back to a 1.)

### Link to YouTube video of MultiSim running:
https://youtu.be/Bklr8uTOh8Y?t=79

## Physical Implementation:
### Pre-build Diagram:
PLEASE NOTE: the teal coloured wire on the drawn diagram represents the white wire on my real build (since white doesn’t show up on a white background)

![Screenshot 2023-02-04 at 5 48 28 PM](https://user-images.githubusercontent.com/76706672/216792721-f44e256c-2188-474c-8a18-b67cc7a654e3.png)
![Screenshot 2023-02-04 at 5 48 45 PM](https://user-images.githubusercontent.com/76706672/216792734-ec32d981-666f-4635-8eb1-0d0353e671d5.png)

### Build Method:
First, I connected the 7SD to the decoder chip, and connected the respective pins using 1kΩ resistors.

![image](https://user-images.githubusercontent.com/76706672/216792751-5b123618-22d7-4287-8962-dfa72cb4830c.png)

Next, I placed all the OR, AND, and JK Flip-Flop chips on the board, and wired up their respective VCC, GND, and PR/CLR pins that won’t be changing, as well as any steady-state inputs to any of the chips. 

![image](https://user-images.githubusercontent.com/76706672/216792755-598a20bf-85d8-4005-8eeb-cb72a81e3231.png)

Next, I wired up all the clock signals as well as the PR/CLR pins that will require forcing. 

![image](https://user-images.githubusercontent.com/76706672/216792760-40e5e296-8ca9-4f08-82b6-00d8ffc3ac87.png)

Then, I wired up all the logic and flipflop connections. 

![image](https://user-images.githubusercontent.com/76706672/216792768-094f0939-3331-436b-9aea-2a291d7671c4.png)

I wired up the clock signal like so, where the yellow wire ran to an OR gate with the other input tied to GND so that I could have a square input wave. The red alligator clip was the output from the HANTEK. 

![image](https://user-images.githubusercontent.com/76706672/216792772-d4b0bb9f-e08f-47ae-b1e7-e2843b9e71da.png)

### Final Result:
The aforementioned steps left me with a final build that looked like this: 

![image](https://user-images.githubusercontent.com/76706672/216792780-45306d84-3bd9-4b3c-9594-d7455c9f68ce.png)

The colour code here was the same as the drawn one. 

Fortunately, my circuit worked on the first try, and I was saved from any real debugging. Initially, I had tried to test that the logic was working by manually moving a DC input from high to low for the clock signal, but soon realized that the logic chips expect steady intervals between the clock signal, which a I cannot easily provide by hand. 

### Presentation Link:
https://youtu.be/Bklr8uTOh8Y 
