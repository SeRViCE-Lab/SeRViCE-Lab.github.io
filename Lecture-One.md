---
layout: page
permalink: /Lecture-1/
mathjax: true
---

<script type="text/javascript"
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>

>### The Three Laws of Robotics
1.  A robot may not injure a human being or, through inaction, allow a human being to come to harm.
2.  A robot must obey the orders given it by human beings except where such orders would conflict with the First Law.
3.  A robot must protect its own existence as long as such protection does not conflict with the First or Second Laws. [1]

Table of Contents:

- [Course Objectives](#objectives)
- [Topics](#topics)
- [Grading Policy](#policy)
- [Mathematical Modeling of Robots](#maths)
- [Summary](#summary)
- [Additional references](#add)

<a name='objectives'></a>
## 1. Course Learning Objectives

* Students will learn and utilize the mathematical representation of rigid body motions,
including homogeneous transformations, to solve for position and orientation and
velocities of objects. They will apply this by programming physical robotics systems.
* Students will formulate and solve forward and inverse kinematic equations and write
programs to solve these equations and carry them out on physical robots.
* Students will formulate and solve velocity kinematics equations and equations and write
programs to solve these equations and carry them out on physical robots.
* Student will learn the mathematical procedures for robot motion planning and trajectory
generation and execute such algorithms in simulation and programming robots.
* Students will expound the mathematical theory and physical implementation of common
robot sensors including rotary encoders and cameras, as well as write programs to read
and process data from such sensors to send control commands to robots.

<a name='topics'></a>
## 2. Topics

* Introduction: Historical development of robots; basic terminology and structure; robots in
automated manufacturing
* Rigid Motions and Homogeneous Transformation: Rotations and their composition;
Euler angles; roll-pitch-yaw; homogeneous transformations
* Forward Kinematics: Common robot configurations; Denavit-Hartenberg convention; Amatrices;
T-matrices
* Inverse kinematics: Planar mechanisms; geometric approaches; spherical wrist
* Velocity kinematics: Angular velocity and acceleration; The Jacobian; singular
configurations; singular values; pseudoinverse; manipulability
* Motion planning: Configuration space; artificial potential fields; randomized methods;
collision detection
* Trajectory generation: Joint space interpolation; polynomial splines; trapezoidal velocity
profiles; minimum time trajectories
* Vision-based control: The geometry of image formation; feature extraction; feature
tracking; the image Jacobian; visual servo control
* Advanced Topics (one or more of the following depending on time and class interest):
Lagrangian dynamics; path planning; mobile robots; force sensing and force control; 

<a name="policy"></a>
## 3. Grading Policy

* Labs: 25%
* Homeworks: 25%
* Exam 1: 25%
* Exam 2:  25%

* Targeted grade ranges:  

                        - A: 90-100%

                        - B: 80-89%

                        - C: 70-79%

                        - D: 60-69%

                        - F: <60%

This is a mixed class. Graduate and undergraduate students will be graded and
curved separately.

There will be two exams given during the semester. In the event of an excused
absence (illness, job-related travel, holy day absence, etc.), students
<u>**must**</u> inform the instructor ahead of time and provide proper documentation of
the conflict. An accommodation will be attempted for verifiable problems.
Homework assignments will be collected graded and discussed in class (as time
permits). 

Homework will be collected at the beginning of the class period when
it is due. Homework that is not reasonably neat and readable, or not bound,
will be marked down. <u >**Late Homework will not be accepted**</u>. It may not be
returned to you until a week after it is due, which means you may not have it
back for a problem-solving session, or to use in studying for an exam. <u >**If you
want to have it available at these times, you will have to make a photocopy
of it before you turn it in.**</u>

<a name='maths'></a>
##  4.  Mathematical Modeling of Robots

### 4.1.  DEFINITION OF A ROBOT

>_A robot is a reprogrammable, multifunctional manipulator designed to move material, parts, tools, or specialized devices through variable programmed motions for the performance of a variety of tasks._    

>             -- Spong, Hutchinson and Vidyasagar, Robot Modeling and Control (2006)    

Basically, a robot should be able to `sense`, `move` and `act intelligently`. Put difeerently, a robot should have sensors on it to enable it to be environmentally-aware. It should be equipped with mechanical parts such as electric motors to make it able to navigate its environment. Lastly, it needs some 'smarts' (otherwise referred to as `reprogrammability` in engineering literature) to make it intelligent. The reporgrammability aspect of the definition is very important because it gives a robot `adaptibility` and `usefulness` making the robot `reconfigurable`.   

### 4.2.  ROBOT MANIPULATORS

Robot manipulators are made up of <u> **links**</u>, \\(i\\), which are connected by <u>**joints**</u>, \\(j\_i\\), to form a <u>**kinematic chain**</u>. Joints are typically rotary (henceforth referred to as _revolute_) or linear (henceforth referred to as _prismatic_). 

A <u>**revolute joint**</u> is akin to a hinge and allows relative rotation between two links. A <u>**prismatic**</u> joint allows a linear relative motion between the adjoint links. We will denote the revolute joints by \\(R\\) and the prismatic joints by \\(P\\).

An example of a revolute joint is depicted in the figure below:

<div class="fig figcenter fighighlight">
  <img src="/assets/Lec1/Revolute.png" width="49%" height="250" align="middle">  
  <img src="/assets/Lec1/Prismatic.png" width="49%" height="250" style="border-left: 1px solid black;">
  <div class="figcaption" align="right">Fig. 1. An example of a revolute joint (left) and a prismatic joint (right).<a href="https://code.google.com/p/impsim/wiki/jmanual_jointRevolute">  Courtesy of impsim. </a></div>
</div>

The angle \\(\Phi\\) in the left figure is defined as the joint angle and it connects links **LK1** and **LK2**. Similarly, the displacement \\(\Phi\\) in the right figure connects  links **LK1** and **LK2**. Rotations follow the right hand rule. Essentially, this means if we have three orthonaormal vectors \\(x\\), \\(y\\), and \\(z\\) \\(\in\\) \\(\mathcal{R}^3\\) which define a coordinate frame, they must satisfy the mathematical relation, 
<center>\\(z\\) = \\(x\\) \\(\times\\) \\(y\\).</center>
We will denote the axis of rotation of a revolute joint or the axis of displacement of a prismatic joint as \\(z\^i\\) if the joint connects links \\(i\\) and \\(i+1\\). \\(\Phi\\) is referred to as the **joint variable** for a revolute or prismatic joint as the case may be.

### Things to remember

* There are two types of joints: prismatic and revolute joints.
* \\(\Phi\\) is referred to the joint variable.
* A revolute joint will allow rotation between two links.
* A prismatic joint will allow displacement between two links.
* The axis of rotation for a typical joint, \\(j\_i\\) that connects links \\(i\\) and \\(i+1\\) is \\(z^i\\).

A three link arm with 2 revolute joints for example is referred to as an **RRP** arm, where the R's stand for _revolute_ and the P stands for _prismatic_. AN example of an RRP robotic arm is the SCARA robot which we shall deal with shortly.

### 4.3.  Taxonomy of Robots

#### 4.3.1. Configuration

The configuration of a manipulator is a complete description of every point on the robot manipulator.
When we know the set of all possible configurations, we say we have the **configuration space** of the robot. The configuration space will correspond mathematically to knowing the set of all possible \\(\theta\_i\\) for a revolute joint or \\(d_i\\) for a prismatic robot where \\(\theta\_i\\) denotes the respective joint angles and \\(d\_i\\) denotes the respective displacements. 

**Example 1:** For a single revolute joint arm, the configuration space is \\(\mathcal{S}^1\\), which is geometrically a one-dimensional sphere (or 1-D sphere).

**Example 2:** A two-revolute joint arm will have \\(\mathcal{S}^1\\) \\(\times\\) \\(\mathcal{S}^1\\) configuration space. This is visually equivalent to moving an outer circle arrount an inner concentric circle. This is called a torus (donut-shaped).

<center> ![Torus](https://upload.wikimedia.org/wikipedia/commons/c/c6/Simple_Torus.svg)</center>
<center> Fig 2. An example of a Torus. </center>


**Example 3:** For a one revolute and one prismatic joint, the configuration space is \\(\mathcal{S}^1\\) \\(\times\\) \\(\mathcal{R}\\) which is geometrically the equivalent of a cylinder.

####  4.3.2.  Degrees of freedom

The number of degrees of freedom of a robot is the minimum number of parameters required to specify the configuration of a robot. Generally, this is equivalent to the size of the configuration space. Typically, the number of joints for a robotic manipulator will tell us about how many degrees of freedom it has. The [cyton arm robot](http://www.robai.com/robots/robot/cyton-epsilon-300/) which we will use in most of the labs that accompany this class has seven joints and hence seven degrees of freedom. A rigid object in three-dimensional space will have six degrees of freedom(DOF) because it will have 3 dedicated to orientation and 3 dedicated to translation.

<a name='add'></a>
## Additional References
- [Asimov, Isaac (circa 1950) I, Robot.](https://en.wikipedia.org/wiki/Three_Laws_of_Robotics)
