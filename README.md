# Introduction
Hello! In this first attempt, we will run our first calculation for the geometry optimization of the water molecule using Avogadro and ORCA.
Both programs are free and Avogadro is opensource too!
So, **let's start!**

- [Introduction](#introduction)
- [Draw the water molecule](#draw-the-water-molecule)
- [Prepare the ORCA input](#prepare-the-orca-input)
- [Run ORCA Geometry Optimization](#run-orca-geometry-optimization)
- [So many files! And now?](#so-many-files-and-now)
      - [water.out](#waterout)
      - [water.xyz](#waterxyz)
      - [water\_trj.xyz](#water_trjxyz)
- [Analysis](#analysis)
  - [Geometries and energies](#geometries-and-energies)
  - [What if we use different theories and basis set?](#what-if-we-use-different-theories-and-basis-set)
    - [Time](#time)
    - [Bonds and angles](#bonds-and-angles)


# Draw the water molecule
First things first: how to draw a molecule in Avogadro?
1. Open Avogadro
2. Select the *Draw Tool* (Ctrl+2) - pencil icon in the toolbar
3. Select the Oxygen atom from the menu on the left and click in an empty spot. You'll see that hydrogens are automatically added (thaks to Adjust Hydrogens option in the left panel)
4. Done!


# Prepare the ORCA input
Now that we drew our molecule, let's create the input file for ORCA.
Luckily Avogadro can do it for us:
1. *Input -> ORCA... ->*
2. Change the Filename Base in whatever you want (in this case I will call it *water*)
3. Select *Geometry optimization*
4. As theory select B3LYP and 6-31G(d) as basis set
5. Save the input file in the folder you choose (*tip:* always create a new folder for every molecule or process you run. ORCA produces a lot of files and can be very easy to get lost)

We can skip all the other parameters for now, just assume that we want to run this geometry optimization in the gas phase and, since it has just three atoms, we can use just 1 core.
We can see that the bottom part of this window changes every time we change some parameters and this is the preview of our input file

# Run ORCA Geometry Optimization
We suppose you already installed ORCA on your computer and added it to the $PATH in order to run the program system-wide.
To run the calculation is quite straightforward:
1. Open the terminal 
2. Go to the directory where you saved your *.inp* file
3. Run `orca water.inp > water.out` to perform ORCA calculation and write the output to the *water.out* file (otherwise the output will be printed in the terminal)
4. Wait few seconds
5. If no error message is returned, 

# So many files! And now?
When calculation finishes, you should end up with a bunch of files (11 in this case) that have different extensions (`xyz`, `out`, ...).
We can focus mainly on three files:
* water.out
* water.xyz
* water_trj.xyz
  
Let's see what they are
#### water.out
This file contains all the information we need (and much more...), but, as first step in computational chemistry, we will focus only on a couple of strings:
-  **Number of basis functions**: usually the higher, the better because we can better represent our orbitals
-  **Final Single Point Energy**: for every optimization, single point energy is computed until plateau is reached
-  **Runtime**: last line of the output file, it tells us how long the calculation took

#### water.xyz
This file contains the number of atoms (3), the energy of the optimized geometry and the coords of the optimized molecule. We can open this file with Avogadro to see the results

#### water_trj.xyz
Similar to the previous file, it contains number of atoms, energy and coords of **every** geometry obtained during optimization, so this file gives us a fast and easy way to analyze how energy changes based on bond length and angles.

# Analysis
## Geometries and energies
In this case we have 5 geometries (the starting one + 4 optimized geometries). As we can see the energy decreases very rapidly duting the first optimization and becomes almost constant in the last three


## What if we use different theories and basis set?
To test the difference between various basis set and functionals, we run HF, B3LYP and wB97x-D3 as functionals and 6-31G(d) & def2-TZVP as basis set.
### Time
Based on elapsed time we have that:
| Functional 	| Basis Set 	| Elapsed Time 	|
|------------	|-----------	|--------------	|
| HF         	| 6-31G(d)  	| 2.442 s      	|
|            	| def2-TZVP 	| 4.327 s      	|
| B3LYP      	| 6-31G(d)  	| 8.097 s      	|
|            	| def2-TZVP 	| 14.367 s     	|
| wB97x-D3   	| 6-31G(d)  	| 11.071 s     	|
|            	| def2-TZVP 	| 19.239 s     	|

As we can see the most time consuming is the wB97x-D3/def2-TZVP being almost 5 times higher than HF/6-31G(d), so we may think that HF/6-31(G) is the best option, but for computational chemistry time is important, but the accuracy and fitting with experimental data is **more** important.

### Bonds and angles
Let's see what happens when we compare angles and bond lengths obtained by different calculations$^{[1]}$:
| Functional 	| Basis Set 	| O-H (Å) 	| H-O-H (°) 	|
|------------	|-----------	|---------	|---------------	|
| HF         	| 6-31G(d)  	| 0.9476  	| 105.58        	|
|            	| def2-TZVP 	| 0.9420  	| 106.51        	|
| B3LYP      	| 6-31G(d)  	| 0.9689  	| 103.86        	|
|            	| def2-TZVP 	| 0.9630  	| 105.24        	|
| wB97x-D3   	| 6-31G(d)  	| 0.9636  	| 104.32        	|
|            	| def2-TZVP 	| 0.9580  	| 105.43        	|

Comparing obtained results with [experimental data](http://dx.doi.org/10.1016/0022-2852(79)90019-5)$^{[2]}$ (O-H = 0.958, H-O-H = 104.478°) we get that the lowest error is obtained with *wB97x-D3/6-31G(d)* ($Δ = 0.37\%$), while the highest is given by *HF/def2-TZVP* ($Δ = 1.81\%$):
|          	|           	| O-H (Å) 	| H-O-H (°) 	| Δ Å    	| Δ °    	| <Δ>   	|
|----------	|-----------	|---------	|-----------	|--------	|--------	|-------	|
| HF       	| 6-31G(d)  	| 0.95    	| 105.58    	| -1.09% 	| 1.05%  	| 1.07% 	|
|          	| def2-TZVP 	| 0.94    	| 106.51    	| -1.67% 	| 1.94%  	| 1.81% :x:|
| B3LYP    	| 6-31G(d)  	| 0.97    	| 103.86    	| 1.14%  	| -0.59% 	| 0.86% 	|
|          	| def2-TZVP 	| 0.96    	| 105.24    	| 0.52%  	| 0.73%  	| 0.63% 	|
| wB97x-D3 	| 6-31G(d)  	| 0.96    	| 104.32    	| 0.58%  	| -0.15% 	| 0.37% :white_check_mark:|
|          	| def2-TZVP 	| 0.96    	| 105.43    	| 0.00%  	| 0.91%  	| 0.46% 	|
| **EXP**      	|           	| **0.96**    	| **104.48**    	|        	|        	|       	|


[1] Values obtained by [xyz2tab](https://github.com/radi0sus/xyz2tab)
[2] Article found at [Computational Chemistry Comparison and Benchmark DataBase](https://cccbdb.nist.gov)
  