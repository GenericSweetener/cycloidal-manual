A complete beginners guide to creating low-cost 3D printed cycloidal drives, complete with reference models.

# Sections
## 1 – Intro
## 2 – The concepts and components
## 3 – Reality
## 4 – 3D Modeling
## 5 – Closing and sources

# 1.1 – Introduction
Cycloidal drives/reducers are extremely useful components to have at one’s disposal when working in hobbyist robotics, however a lot of the material aimed at beginners tend to be lacking in substance, missing practical application, or in some way fail to offer a complete guide through creating cycloidal drives from scratch. This is not to say that there aren’t useful materials out there, it’s just that one often has to cobble a number of them together to integrate cycloidal gearing into their project. Thus, this document was created to be a plain English single resource for understanding the theory of operation behind cycloidal drives, and understanding the methods of and best practices for their construction.

## 1.2 – The target and scope
Cycloidal drives have a broad range of uses in a number of fields and applications, but with that comes an extremely broad range of appropriate contents for this document. To that end, the scope of this guide has been limited somewhat by the project that sparked it, namely the joints of a low budget quadrupedal robot, called S.N.O.T (Sub-Nominal Operations Testbed) (will be linked here if files are ever published). S.N.O.T uses nema 17 stepper motors for all joints (a poor decision in retrospect) which are capable of outputting ~0.34 N⋅m of torque. The gearboxes created for this project are intended to fit into a modular system created for S.N.O.T that has space for gearboxes 60mm in diameter and ~22mm thick. The chosen target gear ratio for the cycloidal drives is 30:1, which while likely not enough to walk with, is plenty for experimenting on the test stand. The reducers are also required to be as cost reduced as possible to fit within the budget, and will be primarily 3D printed.

TLDR; We’re using nema 17 stepper motors to power 3D printed cycloidal drives of a similar footprint in robotic leg joints.

## 1.3 – Materials, tooling, and software
Unless otherwise stated, all modeling for this project has been done in FreeCAD, sliced for printing in Ultimaker Cura, and printed on an Creality Ender 3 using Overture PLA filament.

While all modeling was done in FreeCAD, this guide is intended to be usable with any parametric modeling software, save for the creation of the cycloidal disk (see section 4.1.1 for more). 

# 2.1 – The concepts and components
While this guide gets around having the user complete the majority of the complex math behind the concept of cycloidal gearing, an understanding of the theory of operation and components therein is still vital to utilizing the rest of this guide.

## 2.2 – The basics

Cycloidal drives offer a relatively high torque density, especially compared to standard involute gearing. The way this is accomplished is visually complex, but simple enough when stripped down to the base concepts. As the name suggests, cycloidal drives rely on the concept of cycloidal, or more specifically, hypocycloidal paths. A cycloidal path is effectively the result of rolling a circle along a straight line, tracking a single point on the diameter of the circle, and plotting the position of that point as the circle rolls as a new line.  

![Cycloid_f](https://user-images.githubusercontent.com/79012344/120701874-6b013a80-c481-11eb-81f9-e52ea82c9533.gif)
> By Zorgit - Own work, CC BY-SA 3.0, https://commons.wikimedia.org/w/index.php?curid=4552689

Hypocycloidal paths are the same as cycloidal paths, except the circle is rolling around another circle instead of on a straight line.

<img src="https://user-images.githubusercontent.com/79012344/120703537-6fc6ee00-c483-11eb-83df-f946946918f4.gif" width="400">

While every element of a cycloidal drive is not a direct product of this concept, they all have something to do with the idea of a circle rolling around within another circle, so just keep that in mind.

## 2.3 – Getting a grip (base understanding)

<img src="https://user-images.githubusercontent.com/79012344/120367491-ec6c9780-c2de-11eb-9e0b-7f542085e93b.gif" width="500">
> By Petteri Aimonen - Own work, Public Domain, https://commons.wikimedia.org/w/index.php?curid=7732225

If the above animation makes sense to you immediately, good job, you get to skip most of this section. For the rest of us, this is going to take some unpacking. Rather than pick apart the actual components of a cycloidal drive, let’s start with something more conceptual. A cycloidal drive is at its core a circle, called the cycloidal disk, rolling around the inside of another slightly larger circle, called the housing (soon to be drive pins). At face value, this does not appear to be any sort of speed reducer, until one considers how it can be driven.

![without shaft](https://user-images.githubusercontent.com/79012344/120705548-f7156100-c485-11eb-9ab7-4d2f0bfc740b.gif)
> Note – video loops at the jump and quality will be fixed later

This is where the bearing and eccentric shaft (green component of the animation) come into play. To drive the cycloidal disk’s rolling motion, there are a number of ways one could theoretically transfer power to it, but to achieve the effect of a gear reduction, there is a rather clever and simple method. First, a bearing is placed in the center of the cycloidal disk, and then the motor is attached off center to a shaft inside it. 


![with shaft](https://user-images.githubusercontent.com/79012344/120705736-32179480-c486-11eb-9539-e033b872181f.gif)
> Yeah, still not sure what the yellow in the gif is.

This both means that the power source/motor is isolated in terms of rotational speed from the cycloidal disk, because the inner and outer race of the bearing can spin at different speeds, and, because the motor is off center by the same amount as the difference between the diameters of the cycloidal disk and housing, the disk is always pressed against the edge of housing. The degree to which the motor is off center is called eccentricity, and will play more of a role later. Put this all together, and when running the motor the cycloidal disk will not spin with the motor, but instead will roll along the inside of the housing, and because the circumference of the disk and housing are similar, the disk can be rolled around for one rotation of the motor but only actually spins a small amount. This is in effect a gear reduction because 1 revolution of the motor on the eccentric shaft results in less than one rotation of the cycloidal disk.

However, even if this were taking place in an ideal world where no power was lost to friction, and the cycloidal disk never slipped, there is still no mechanism by which to actually utilize the power being transferred, because the cycloidal disk is not centered and can’t have an output shaft mounted directly to it.





and then mote text

