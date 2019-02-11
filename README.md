# **Xingyu YAN' Blog**

Hi, welcome to my blog.

Currently, I'm a master student in computer science in SUNY at Buffalo. I'm interested in Computer Vision, Computational Geometry, and Software Development to implement these thecniques in industry.

Besides, I'm a mechanical engineer with expertise in coordinate metrology & precision engineering.

Please feel free to contact me.

Email:      <xingyuyan@outlook.com>  
LinkedIn:   <https://www.linkedin.com/in/xingyu-yan-8baa439b/>  
GitHub:     <https://github.com/i7242>


---


## Development of Virtual metrology for E-learning

The Virtual Laboratory is designed for engineering student to exercese the use of metrology equipments.
* Student can conduct simulation in this platform to enhance what they have learned.
* The deviations on the part can be amplified to help student understand specifications and their application cases.
* Various equipments will be introduced, including some high precision and expensive ones. Pre-exercise in virtual laboratory will reduce the risk of equipment broken.

Following is a scene of the Virtual Laboratory, and a link to the video demo.

<https://www.youtube.com/embed/c2Kuf2EE0kY>

![VirtualLab](\resources\VL.png)

In this work, I mainly contribute on the measurement simulations:
* Provide a simplex algorithm in ```Lua```.
    * <https://i7242.github.io/Simplex-in-Lua/>
* Customize optimization problem for different measurement equipments.
    * The measurement is considered as an optimization problem, which minimize the distance between the measurement equipment and part.
    * The part's position is fixed, and the equiment can have rigid translation and rotation.
    * The caliper and its slider can have constrained relative displacement.

Following is the measurement principle for caliper.

![Measurement Using Caliper](\resources\Caliper.png)

The Virtual Laboratory project was funded by ***Université de Bordeaux***.

> I2M (Institut de Mécanique et d'Ingénierie)  
https://www.i2m.u-bordeaux.fr/

It was developed based on Inscape3D platform (which is developed for serious games).

> Inscape3D  
http://www.inscape3d.com/en/

In the following article, you can find more details about the project.

> Ballu A, Yan X, Blanchard A, et al. Virtual metrology laboratory for e-learning[J]. Procedia CIRP, 2016, 43: 148-153.  
https://www.sciencedirect.com/science/article/pii/S2212827116003929



---

## Generation of Non-ideal Parts for Tolerance Analysis and Visualization

The shape of real manufactured mechanical parts are non-ideal. Defects exist in their surfaces and dimensions. Some of them are large enough that can influence the perceived quality, while others influences the product functionality.

In precision engineering, we design, analysis and control the geometrical and dimensional tolrances to guarantee the product's quality. To achive more precise and realiable simulation result, the form defect on part surface need to be considered.

Here is a method that generates discrete part model with defects. A figure that concludes the generation process is shown as below:
![SMSGen](\resources\SMSGen.png)

* First, discrete model is generated by tessellation form the nominal/ideal model.
    * The sampling rate depends on real engineering requirements. Here we just use a low sampling rate as an example.
    * The identification of different surfaces is easy when linked to CAD systems. Here we assume the model is from other sources, like 3D camera. A spectral clustering based method is used to segment the model.
* Simulate the surface defects on each surface. The methods used here includes homogenuous transformation, modal decomposition, and their combination. We can control the shape and value of defects by parameters.
* The combination of surfaces uses Finite Element Analysis and Penalty Function based method. This can guarantee:
    * there exist no self intersection on the model edge;
    * the defects on different surfaces are independed;
 
 For more details and source code in ```Matlab```, see link below:

 <https://i7242.github.io/Skin-Model-Shape-Generation/>



---

## Assembly Simulation Using Non-ideal Part Models

Using the generated non-ideal part models, we can conduct assembly simulation considering form defects.

A Linear Complementarity Condition (LCC) based simulation was proposed. Its principle is described as below:
* When mating surfaces are contacted:
    * distance between contact points are zero, but reaction forces between these points are possitive:
    D=0, F>=0
    * distance between non contact points can bigger than zero, but there exist no reaction forces:
    D>=0, F=0
* If the system can be balanced, in either cases, the product D*F = 0.

The principle can be explained using a figure:

![LCC1](\resources\LCC1.png)

Based on this simple principle, we transformed the assembly simulation problem to a quadratic optimization problem. Additional constraints are also added to express the balance of the system. Problem formulation is shown as below:

![LCC2](\resources\LCC2.png)

An assembly example with gaps between joints are shown as below. The red arrow indicates the direction of assembly forces.

![LCC3](\resources\LCC3.png)

![LCC4](\resources\LCC4.png)

There is also a video to show a simple simulation case:

<https://www.youtube.com/embed/-6INfS7PePk?rel=0>

For the detailed problem formulation, please refer to the following article:

> Yan X, Ballu A. Tolerance analysis using skin model shapes and linear complementarity conditions[J]. Journal of Manufacturing Systems, 2018, 48: 140-156.  
https://www.sciencedirect.com/science/article/pii/S0278612518302048

