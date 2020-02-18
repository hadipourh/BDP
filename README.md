1. About the solver
This is the source codes for modeling a linear layer consisting of a matrix, and finding BDP for LED, MISTY1, CLEFIA, CAMELLIA and AES.
It is written with python3. 
When you run the codes, you will first get the STP input files *.cvc, then you need to run these CVC files in an STP solver.
Therefore, you need to install the STP solver in advance. 
The STP solver is available at https://stp.github.io. 
STP solver, as other SMT solver, requires a SAT solver as its fundamental solver. 
Minisat is the default SAT solver for STP.
However, we strongly recommend installing cryptominisat and call the cryptominisat solver when you run STP solver.

2. The structure of the source codes and their functions. 
The files marked by * are all independent, so you do not need to worry about the dependencies among them.
Below the comments line "file for modeling BDP for ciphers", there are 7 directories.
We list the structure of "AES_4_keydependent" in one directory, and it is similar for the other 6 directories.
----------------------------------------------------------
|---- Submission_ToSC
    # file for modeling matrix
    *|---- Matrix_if_then_else.py
    *|---- Matrix.py

    # file for modeling BDP for ciphers
    *|---- AES_4_keydependent
          |---- AES_4_round_keydependent.py
          |---- AES.py
          |---- AssertSbox.py
          |---- constant.py
          |---- Matrix_non_square.py
          |---- Matrix.py
          |---- BOOLFUNC
                |---- GenInt.py
                |---- __init__.py
                |---- Polynomial.py
                |---- README.md
                |---- Sbox.py
                |---- Term.py
                |---- test.py
                |---- Vector.py

    *|---- AES_5_keydependent
    *|---- CAMELLIA
    *|---- CLEFIA
    *|---- LED_6r
    *|---- LED_7r
    *|---- Misty1_6_round_62_active
    *|---- Misty1_6_round_63_active

    # readme
    *|---- README.md
-----------------------------------------------------------------

* Matrix_if_then_else.py and Matrix.py:
    There are two kinds of implementations of our model for our paper. 
    Matrix_if_then_else.py follows the principle introduced in Section 4.1.,
    while Matrix.py follows Theorem 2.
    For MISTY1 and CAMELLIA, only Matrix_if_then_else.py is executed, because there are many matrices used in the main function.
    For other ciphers, Matrix.py and Matrix_if_then_else.py are all executed.

* AES_4_keydependent:  
    This code is to verify the 4-round key-dependent distinguisher for AES introduced in Section 5.1.
    AES_4_round_keydependent.py is the main function. The command to run it is:
    ($ is the prompt of bash)
    _____________________________________
    $ python3 AES_4_round_keydependent.py 
    _____________________________________
    The output is our cvc file, you can use the redirection command to save it.

    $ python3 AES_4_round_keydependent.py > AES_4.cvc
    When the cvc file gotten, you can run it in your computer,
    _____________________________________
    $ stp AES_4.py 
    _____________________________________

    The default solver minisat is used to solve the fundamental SAT problems.
    If you have installed the cryptominisat, you can also manually call cryptominisat:
    _____________________________________
    $ stp AES_4.py --cryptominisat --threads n
    _____________________________________
    
    Cryptominisat supports parallel, so you can use --threads n to decide how many threads you need to solve your problem.
    For our experiments, we recommend n at least 4.
    
    After running it, you may get two kinds of result returned.
    1. Valid. 
    2. Invalid.
    CVC file ended with a statement "QUERY FALSE" is asking a question to the solver 
    "No solutions to the constraints I list in the CVC file, right?" 
    "Valid." means there is no solution for the model.
    "Invalid." means there is at least one solution for the model.
    As is well known in division property, no solution is a good news that we find some balanced bits. 

* AES_5_round_keydepent:
    similar with AES_4_round_keydependent
    -------------------------------------------------
    $ python3 AES_5_round_keydependent.py > AES_5.cvc  
    -------------------------------------------------
    $ stp AES_5.cvc (--cryptominisat --threads 4)
    -------------------------------------------------

* CAMELLIA
    In the Camellia_base.py, we list the model of camellia.
    In Camellia.py, you can change the inVec to decide which bits should be active and how many rounds you want to check.  
    -------------------------------------------------
    $ python3 Camellia.py > camellia.cvc
    -------------------------------------------------
    $ stp camellia.cvc (--cryptominisat --threads 4)
    -------------------------------------------------

* CLEFIA
    -------------------------------------------------
    $ python3 CLEFIA.py > CLEIFA.cvc
    -------------------------------------------------
    $ stp CLEFIA.cvc (--cryptominisat --threads 4)
    -------------------------------------------------

* LED_6r
    -------------------------------------------------
    $ python3 led.py > led.cvc
    -------------------------------------------------
    $ stp led.cvc (--cryptominisat --threads 4)
    -------------------------------------------------

* LED_7r
    -------------------------------------------------
    $ python3 led.py > led.cvc
    -------------------------------------------------
    $ stp led.cvc (--cryptominisat --threads 4)
    -------------------------------------------------

* Misty1_6_round_62_active
    -------------------------------------------------
    $ python3 Misty1.py > Misty1_62.cvc
    -------------------------------------------------
    $ stp Misty1_62.py (--cryptominisat --threads 4)
    -------------------------------------------------

* Misty1_6_round_63_active
    -------------------------------------------------
    $ python3 Misty1.py > Misty1_63.cvc
    -------------------------------------------------
    $ stp Misty1_63.py (--cryptominisat --threads 4)
    -------------------------------------------------

