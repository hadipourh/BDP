# BDP_ComplexLinearLayer
--------------------------
These are the codes for the paper "Finding Bit-Based Division Property for Ciphers with Complex Linear Layers" published at ToSC 2020 Issue 1, by Kai Hu, Qingju Wang and Meiqin Wang.

## About the STP solver
--------------------------
These are the source codes for finding the bit-based division property (BDP) for ciphers with a linear layer containing a non-binary matrix, such as the AES, LED, MISTY1, CLEFIA and CAMELLIA.
All codes are written in python3. 
When run the codes, we first get a CVC file, e.g. name.cvc which is the input file to the STP, then run name.cvc by STP.
Therefore, it is required to install the STP solver in advance, which is available at https://stp.github.io. 
The STP solver requires a SAT solver as its foundamental solver, and Minisat is the default SAT solver of STP.
However, we strongly recommend to install and call the cryptominisat solver when you run the STP solver.

## The structure of the source codes and their functions
-------------------------
The files for different ciphers are all independent.
> BDP_ComplexLinearLayer/
>> Matrix_if_then_else.py<br>
>> Matrix.py<br>
> AES_4_keydependent/
>> AES_4_round_keydependent.py<br>
>> AES.py<br>
>> AssertSbox.py<br>
>> constant.py<br>
>> Matrix_non_square.py<br>
>> Matrix.py<br>
>> BOOLFUNC/<br>
>>> GenInt.py<br>
>>> __init__.py<br>
>>> Polynomial.py<br>
>>> README.md<br>
>>> Sbox.py<br>
>>> Term.py<br>
>>> test.py<br>
>>> Vector.py<br>
> AES_5_keydependent/ <br>
> CAMELLIA/ <br>
> CLEFIA/ <br>
> LED_6r/ <br>
> LED_7r/ <br>
> Misty1_6_round_62_active/ <br>
> Misty1_6_round_63_active/ <br>
> README.md <br>

Note we expand the directory of "AES_4_keydependent" to show the details. It is similar for the other directories.

## The usage of the codes
------------------------------

## Matrix_if_then_else.py and Matrix.py: <br>
    There are two kinds of implementation of our model: 
    * Matrix_if_then_else.py: follows the principle introduced in Sect. 4.1.
    * Matrix.py: follows Theorem 2.
    For MISTY1 and CAMELLIA, only Matrix_if_then_else.py is executed, because there are many matrices used in the main function. For the other ciphers, both Matrix.py and Matrix_if_then_else.py are required to be executed.

## AES_4_keydependent: <br> 
    This code is to find the 4-round key-dependent distinguisher for AES introduced in Section 5.1.
    AES_4_round_keydependent.py is the main function. The command to run it is ($ is the prompt of bash):
```Bash
    $ python3 AES_4_round_keydependent.py 
```
    The output can be saved as a CVC file AES_4.cvc by the redirection command
```Bash
    $ python3 AES_4_round_keydependent.py > AES_4.cvc
```
    When the CVC file is obtained, you can run it by
```Bash
    $ stp AES_4.py 
```
    The default solver minisat is used to solve the foundamental SAT problems.
    If you have installed the cryptominisat, you can also manually call cryptominisat by
```Bash
    $ stp AES_4.py --cryptominisat --threads n
```
    Cryptominisat supports parallelism, so you can use --threads n to decide how many threads are required to solve your problem.
    For our experiments, we recommend that n should be at least 4.
    After running it, two possible results might be returned:
    * Valid,
    * Invalid.
    When input a CVC file ended with a statement "QUERY FALSE" to the solver, actually we ask the solver a question like 
    "No solutions to the model with constraints listed in the CVC file, right?" 
    "Valid" means there is no solution to the model.
    "Invalid" means there is at least one solution to the model.
    As is well known in division property, "no solution" i.e. "Valid" here, is a good news because it means some balanced bits are found. 

## AES_5_round_keydepent: <br>
    Similar with AES_4_round_keydependent
```Bash
    $ python3 AES_5_round_keydependent.py > AES_5.cvc  
    $ stp AES_5.cvc (--cryptominisat --threads 4)
```
## CAMELLIA
    In Camellia_base.py, we list the model of camellia.
    In Camellia.py, you can change the inVec to decide which bit should be active and how many rounds you want to check.  
```Bash
    $ python3 Camellia.py > camellia.cvc
    $ stp camellia.cvc (--cryptominisat --threads 4)
```
## CLEFIA
```Bash
    $ python3 CLEFIA.py > CLEIFA.cvc
    $ stp CLEFIA.cvc (--cryptominisat --threads 4)
```
## LED_6r
```Bash
    $ python3 led.py > led.cvc
    $ stp led.cvc (--cryptominisat --threads 4)
```
## LED_7r
```Bash
    $ python3 led.py > led.cvc
    $ stp led.cvc (--cryptominisat --threads 4)
```
## Misty1_6_round_62_active
```Bash
    $ python3 Misty1.py > Misty1_62.cvc
    $ stp Misty1_62.py (--cryptominisat --threads 4)
```
## Misty1_6_round_63_active
```Bash
    $ python3 Misty1.py > Misty1_63.cvc
    $ stp Misty1_63.py (--cryptominisat --threads 4)
```




