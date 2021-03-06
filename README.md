# Sections
## 1 – [Intro](https://github.com/GenericSweetener/cycloidal-manual/blob/main/README.md#11--introduction)
## 2 – [The Concepts and Components](https://github.com/GenericSweetener/cycloidal-manual/blob/main/README.md#21--the-concepts-and-components)
## 3 – [Snap Back to Reality](https://github.com/GenericSweetener/cycloidal-manual/blob/main/README.md#3--snap-back-to-reality-1)
## 4 – [Design & CAD](https://github.com/GenericSweetener/cycloidal-manual/blob/main/README.md#4--design--cad)
## 5 – [Closing and Sources](https://github.com/GenericSweetener/cycloidal-manual/blob/main/README.md#5--closing)

This guide was created for a senior capstone project at Templeton Academy based on this research question: 

What are the appropriate use cases for, benefits and drawbacks of, and best construction practices for common complex gear sets in terms of 3d printed hobbyist construction for high torque applications?

It does not answer that question (see section 5).

# 1.1 – Introduction
Cycloidal drives/reducers are extremely useful components to have at one’s disposal when working in hobbyist robotics, however a lot of the material aimed at beginners tend to be lacking in substance, missing practical application, or in some way fail to offer a complete guide through creating cycloidal drives from scratch. This is not to say that there aren’t useful materials out there, it’s just that one often has to cobble a number of them together to integrate cycloidal gearing into their project. Thus, this document was created to be a plain English single resource for understanding the theory of operation behind cycloidal drives, and understanding the methods of and best practices for their construction.

## 1.2 – The Target and Scope
Cycloidal drives have a broad range of uses in a number of fields and applications, but with that comes an extremely broad range of appropriate contents for this document. To that end, the scope of this guide has been limited somewhat by the project that sparked it, namely the joints of a low budget quadrupedal robot, called S.N.O.T (Sub-Nominal Operations Testbed) (will be linked here if files are ever published). S.N.O.T uses nema 17 stepper motors for all joints (a poor decision in retrospect) which are capable of outputting ~0.34 N⋅m of torque. The gearboxes created for this project are intended to fit into a modular system created for S.N.O.T that has space for gearboxes 60mm in diameter and ~22mm thick. The chosen target gear ratio for the cycloidal drives is 30:1, which while likely not enough to walk with, is plenty for experimenting on the test stand. The reducers are also required to be as cost reduced as possible to fit within the budget, and will be primarily 3D printed.

TLDR; We’re using nema 17 stepper motors to power 3D printed cycloidal drives of a similar footprint in robotic leg joints.

## 1.3 – Materials, Tooling, and Software
Unless otherwise stated, all modeling for this project has been done in FreeCAD, sliced for printing in Ultimaker Cura, and printed on an Creality Ender 3 using Overture PLA filament.

While all modeling was done in FreeCAD, this guide is intended to be usable with any parametric modeling software, save for the creation of the cycloidal disk (see section 4.1.1 for more). 

# 2.1 – The Concepts and Components
While this guide gets around having the user complete the majority of the complex math behind the concept of cycloidal gearing, an understanding of the theory of operation and components therein is still vital to utilizing the rest of this guide.

## 2.2 – The Basics

Cycloidal drives offer a relatively high torque density, especially compared to standard involute gearing. The way this is accomplished is visually complex, but simple enough when stripped down to the base concepts. As the name suggests, cycloidal drives rely on the concept of cycloidal, or more specifically, hypocycloidal paths. A cycloidal path is effectively the result of rolling a circle along a straight line, tracking a single point on the diameter of the circle, and plotting the position of that point as the circle rolls as a new line.  

![Cycloid_f](https://user-images.githubusercontent.com/79012344/120701874-6b013a80-c481-11eb-81f9-e52ea82c9533.gif)
> By Zorgit - Own work, CC BY-SA 3.0, https://commons.wikimedia.org/w/index.php?curid=4552689

Hypocycloidal paths are the same as cycloidal paths, except the circle is rolling around another circle instead of on a straight line.

<img src="https://user-images.githubusercontent.com/79012344/120703537-6fc6ee00-c483-11eb-83df-f946946918f4.gif" width="400">

While every element of a cycloidal drive is not a direct product of this concept, they all have something to do with the idea of a circle rolling around within another circle, so just keep that in mind.

## 2.3 – Getting a Grip (base understanding)
<img src="https://user-images.githubusercontent.com/79012344/120367491-ec6c9780-c2de-11eb-9e0b-7f542085e93b.gif" width="500">

> By Petteri Aimonen - Own work, Public Domain, https://commons.wikimedia.org/w/index.php?curid=7732225

If the above animation makes sense to you immediately, good job, you get to skip most of this section. For the rest of us, this is going to take some unpacking. Rather than pick apart the actual components of a cycloidal drive, let’s start with something more conceptual. A cycloidal drive is at its core a circle, called the cycloidal disk, rolling around the inside of another slightly larger circle, called the housing (soon to be drive pins). At face value, this does not appear to be any sort of speed reducer, until one considers how it can be driven.

![without shaft](https://user-images.githubusercontent.com/79012344/120705548-f7156100-c485-11eb-9ab7-4d2f0bfc740b.gif)
> Apologies for the poor quality. Also, please note that the video loops where you see the tracked point jump.

This is where the bearing and eccentric shaft (green component of the main 3D animation) come into play. To drive the cycloidal disk’s rolling motion, there are a number of ways one could theoretically transfer power to it, but to achieve the effect of a gear reduction, there is a rather clever and simple method. First, a bearing is placed in the center of the cycloidal disk, and then the motor is attached off center to a shaft inside it. 


![with shaft](https://user-images.githubusercontent.com/79012344/120705736-32179480-c486-11eb-9539-e033b872181f.gif)
> Yeah, still not sure what the yellow in the gif is. This shows 1 rotation of the input shaft / motor. Note how much the disk actually rotates.

This both means that the power source/motor is isolated in terms of rotational speed from the cycloidal disk, because the inner and outer race of the bearing can spin at different speeds, and, because the motor is off center by the same amount as the difference between the diameters of the cycloidal disk and housing, the disk is always pressed against the edge of housing. The degree to which the motor is off center is called eccentricity, and will play more of a role later. Put this all together, and when running the motor the cycloidal disk will not spin with the motor, but instead will roll along the inside of the housing, and because the circumference of the disk and housing are similar, the disk can be rolled around for one rotation of the motor but only actually spins a small amount. This is in effect a gear reduction because 1 revolution of the motor on the eccentric shaft results in less than one rotation of the cycloidal disk.

However, even if this were taking place in an ideal world where no power was lost to friction, and the cycloidal disk never slipped, there is still no mechanism by which to actually utilize the power being transferred, because the cycloidal disk is not centered and can’t have an output shaft mounted directly to it.

## 2.5 – Getting an Output (output pins and holes)
Getting power out of a cycloidal drive is less about directly connecting the cycloidal disk to an output shaft and more to do with rectifying the wobbling motion of the disk as it rotates so that the gear reduction created can actually be used. This is where the output holes and pins seen in the animation (purple) come into play. The rectifying method of choice here uses at least 3 (normally more) holes in the cycloidal disk spaced equally in a circular pattern on its face, shown below on a reference circle of a known size. 

![Screenshot from 2021-06-03 16-17-28](https://user-images.githubusercontent.com/79012344/120706625-53c54b80-c487-11eb-936c-33393a05f40b.png)

These holes then correspond to output pins/rollers that are arranged in the same fashion and on a reference circle of the same diameter as the reference circle of the holes.

![image](https://user-images.githubusercontent.com/79012344/120706877-a141b880-c487-11eb-906b-b87bfa480181.png)

hese pins are mounted to a surface that can rotate, called the output disk (seen in purple in the animation), which is centered by a bearing of some sort in reference to the housing. What makes it possible for these output pins to mesh with the holes of the disk even though the disk is not centered while rotating is that the disk’s holes are the same as the diameter as the output pins plus the eccentricity of the disk. This in effect means that while the output pins are never centered in the output holes, their sides are able to contact the edges of the holes, rolling along the inside of the output holes and being pushed in the correct direction. This is why there need to be more than one set of pins and holes to prevent slop in the output from the pins not being directly attached to the disk, instead requiring multiple angles of contact.

## 2.6 - Getting It To Work (the cycloidal disk and drive pins)
Up until this point, a simplified model of the actual mechanism by which the gear reduction is created has been used to demonstrate the concepts around it, however this model would be extremely inefficient and borderline useless in any practical application. So, instead of relying on the friction between a smooth cycloidal disk and its housing, cycloidal drives use a set of drive  pins/rollers in the housing (silver), and a disk with corresponding lobes for those pins which is the cycloidal disk (yellow). 

<img src="https://user-images.githubusercontent.com/79012344/120367491-ec6c9780-c2de-11eb-9e0b-7f542085e93b.gif" width="500">

> By Petteri Aimonen - Own work, Public Domain, https://commons.wikimedia.org/w/index.php?curid=7732225

The same movement is happening here as before, that wobbling roll is the same as when it was 2 simple circles, but now there’s multiple points of contact being made so the disk can’t just slip around, it can only rotate on its rolling path. The interaction between the pins and lobes is also a similar motion to the circle rolling around another circle, one could think of the disk as the pattern made by rolling one of the pins around a circle. It is also at this point that the number of pins in the housing compared to the number of lobes on the cycloidal disk come into play. Knowing that the lobes on the disk and the drive pins are in roughly the same scale to each other, similar to gear teeth, if there were the same number of pins and lobes then the disk would not be able to move because it would just be sitting locked into the pins.

![image](https://user-images.githubusercontent.com/79012344/120707296-275dff00-c488-11eb-9c4f-971e62db18af.png)

That’s just going to bind up.

This is why, in this guide and in most cycloidal reducer designs, there is one more pin on the housing than there are lobes on the disk. Not only does this make room for the rolling motion to be possible in the first place, it defines the transmission ratio of the reducer in more solid terms than the difference between the circumference of 2 circles. This is to say that for every full rotation of the input shaft, the disk moves 1 (or whatever the difference between the number of pins and lobes is) pin over. With this knowledge, the gear ratio can be defined. In this example of 30 pins on the disk and 31 pins in the housing, it would take 30 rotations of the input shaft to make one full rotation of the cycloidal disk, making the transmission ratio 30:1.

## 3 – Snap Back to Reality
An understanding of the theory of operation is only so good as the practical application of those concepts. To that end, this section will translate the concepts from section 2 to what one may actually want to manufacture.

## 3.1 – Variants of the Design

This guide offers 2 main variants of the cycloidal reducer. The first is what is generally considered to be the standard design and is what is described throughout section 2, using a plate connected to the output pins as the power output for the reducer. This design will be called the driven disk design going forwards. The second variant is conceptually the same as the first, but has fixed output pins and instead rotates the housing (called the driven housing design in this guide). 

> [Standard cycloidal reducer full assembly (video)](https://youtu.be/JAcRD82Ofww)

### 3.1.2 – Driven Housing

The best way to conceptualize driven housing designs is to imagine the housing as being free floating and the cycloidal disk as limited in its range of motion to just its wobble. This is accomplished by mounting the output pins to a stationary plate, in this case the one the motor mounts to. This component is referred to here as the motor plate or lower pin plate. Now, when the housing interacts with the disk, the disk can’t rotate but the housing can, so it does.

> [Moving housing cycloidal drive in operation (video)](https://youtu.be/8ANe9f_OlMg)

Of course, the housing needs to be kept centered, won’t survive much torsion if only supported on one side, and the forces applied to it shouldn’t be transferred to the cycloidal disk. For those reasons it’s stabilized by a bearing on the top and bottom. The bottom bearing connects to the motor plate, and the top bearing connects to a plate that has screws running down through the drive pins to connect to the motor plate.

> [Drivin Housing cycloidal reducer full assembly (video)](https://youtu.be/nldJB2Xw-ws)

### 3.1.3 – A Note on Stacked Disks

While not utilized in this guide, it is common practice to reduce the vibration caused by the off center rotation of the cycloidal disk by having 2 disks in the same housing 180 degrees offset from each other. 

![Screenshot from 2021-06-03 16-28-52](https://user-images.githubusercontent.com/79012344/120707902-ddc1e400-c488-11eb-9ce5-67808c2e525d.png)

These disks will rotate in the same direction and at the same speed so their output holes still line up with each other (provided they are symmetrical across both the x and y axis). This means that while rotation is still taking place, the shifting weight of one disk is always offset by the other. To accomplish this the eccentric shaft is split into 2 cams so the top and bottom disks are driven 180 degrees offset from each other.

![Screenshot from 2021-06-04 09-00-27](https://user-images.githubusercontent.com/79012344/120805139-70a76080-c513-11eb-99f8-6b5d8582ba58.png)

## 3.2 – Bearings – An Epic Saga
Anyone genuinely considering manufacturing a cycloidal drive by means of 3D printing will have likely noticed the very high number of surfaces that need to be in contact with each other in even the most basic of cycloidal designs. The good news is that all but the edge face of the cycloidal disk can be replaced by a friction reducing element, however this may not always be the best design decision. At any rate, a cycloidal drive that will carry any sort of load requires at the very least 2 bearings: 1 in the center of the cycloidal disk and another to keep the output ring centered. 

### 3.2.1 – The Required Bearings – Cycloidal Disk
The bearing in the center of the cycloidal disk (drive bearing) both relies on and can limit some of the primary dimensions of the cycloidal drive, so care should be taken when finding a bearing to use. Firstly, keep in mind that this bearing is what makes it possible to use an eccentric shaft, so some thought needs to be given to the inner dimension of the bearing so that the the desired level of eccentricity is both possible and won’t create a situation where the hole in the eccentric shaft that accepts the input shaft of the motor is extremely thin on one side. (learned that the hard way).

![Screenshot from 2021-06-04 09-03-49](https://user-images.githubusercontent.com/79012344/120805439-ce3bad00-c513-11eb-9826-c05b82470724.png)


A good rule of thumb is that (assuming the eccentric shaft is accepting a metal output D-shaft from the motor) the inner diameter of the bearing should be equal to the diameter of the motor output shaft plus the disk’s eccentricity plus however thick you want the thinnest part of the eccentric shaft to be (at least 3mm).

Another point to consider is that the cycloidal disk should be a similar thickness to the drive bearing. This ensures that the bearing will be able to transfer power to the entire edge of the cycloidal disk. It’s also a good idea to have a ridge of some sort modeled in the hole for the bearing so it can be pressure fit until bottoming out.

![Screenshot from 2021-06-04 09-05-26](https://user-images.githubusercontent.com/79012344/120805664-093de080-c514-11eb-99e9-ee21f2e4e896.png)

One final element of note is that, especially with a 3D printed disk, the bearing is likely to be the heaviest spinning component, and will be spinning off center to the rest of the assembly. This can be dealt with by using a stacked design (see 3.1.1), but suffice it to say that a lighter bearing will create less vibration.

### 3.2.2 – The Required Bearings – Output Pins / Output Ring (output bearings)
To center the output disk/pins to the housing, it is advised that a relatively large bearing is used with an inner diameter that is able to fit around the outside of the output disk, and is therefore larger than the outer edge of the output pins. This then mounts into the housing.

![image](https://user-images.githubusercontent.com/79012344/120806667-21fac600-c515-11eb-84f3-34a339cc81d5.png)

This method allows the forces applied to the output ring to be braced against the housing, which can be reinforced depending on the load required. While this method is strong and robust, it does have some drawbacks. Cost is by far the largest issue bearings of this specification can add to a project, with the example bearings (inner diameter = 45mm, outer diameter = 85mm, thickness = 7mm) costing roughly $10 per bearing, which can get quite expensive for something like a quadrupedal robot, especially when using cycloidal designs that require 2 output bearings (3.1.2). These larger bearings are also quite heavy, the aforementioned bearings in particular weigh ~50g.

### 3.2.3 – Output Pins

Using something other than 3D printed plastic for output pins is by far the most valuable upgrade one can make to their budget cycloidal drive, because it increases the strength of the assembly by a massive degree without adding too many more individual components. The drawbacks of 3D printed output pins is quite obvious when one considers the relatively low force required to shear off the pins on the layer lines. 

![Screenshot from 2021-06-04 09-15-10](https://user-images.githubusercontent.com/79012344/120806940-6f773300-c515-11eb-8493-91bb29bfee7f.png)

> Video of pin failure [here.](https://youtu.be/O2v3M0Q1Ow4)

The 2 routes one can take in making reinforced output pins are using small bearings or using round nylon standoff spacers in place of 3D printed output pins. The only real difference from a modeling and construction perspective is that rather than modeling pins to be printed on the output disk, one would have holes through it, potentially with hexagonal holes on the other side going partially through it to capture a nut. And of course a bolt/screw and a nut is necessary to keep the bearing / nylon spacer in place (as seen in 3.2.2).

This guide uses 7mm outer diameter 3mm inner diameter nylon spacers as output pins.

### 3.2.4 - Drive Pins
Using bearings in place of 3D printed drive pins can significantly reduce wear and friction within a cycloidal drive, however these benefits come with increased weight, drive pin diameter, cost, and complexity. To that end this guide did not include testing with bearings in place of drive pins and does not have modeling instructions for them. 

### 3.2.5 – A Note on Pressure Fitting Bearings
This guide requires a fair number of bearings to be pressure fit into 3D printed housings, so some thought and testing of bearing fitment should be done before printing components. It is therefore advised that you create test pieces for bearings to fit into / to fit into bearings and undergo some trial and error to find dimensions that can tightly hold or fit into a bearing without being deformed by it or requiring forceful persuasion (Take it from someone who’s broken a bearing with a hammer). When making test pieces, be sure to use the same print settings as on the final components. Time spent in this process will save on test prints later down the line.

## 3.3 – Choosing an Output Pin Circle
The output pin circle is the reference circle that passes through the center of the output holes on the cycloidal disk and through the center of the drive pins as mentioned in section 2.5. Especially on the smaller cycloidal disks used in this guide, it is of the upmost importance to ensure that the output holes on the cycloidal disk have enough material surrounding them as to not create thin weak spots in the disk for deformation or cracking to occur. A good rule of thumb is to have the output pin circle roughly centered on the disk, and try to have ~3mm of material around the holes.

![Screenshot from 2021-06-04 09-17-33](https://user-images.githubusercontent.com/79012344/120807237-bb29dc80-c515-11eb-8b3c-a01079115bd4.png)

However, this is not always possible because the inner diameter of the drive bearing can limit the maximum size of the output pin circle, and should also be taken into account. This is why the output holes on the reference models provided are not perfectly centered on the cycloidal disk.

![image](https://user-images.githubusercontent.com/79012344/120806667-21fac600-c515-11eb-84f3-34a339cc81d5.png)

# 4 – Design & CAD
Now for the long boring part. This section is a step-by-step guide for designing the various components of driven housing and driven disk cycloidal reducers. The reference models created for this project are provided here in .FCStd format, editable and exportable in FreeCAD.

## 4.1 – Cycloidal Disk
This guide uses the process of modeling the cycloidal disk as the focal point for setting all major parameters of the cycloidal drive.

### 4.1.1 – Why FreeCAD?
The two common ways to model a cycloidal disk are mathematically defining it from scratch as a curve, or using a disk generation tool that creates a disk based on easily inputted parameters. This guide is aimed at beginners to cycloidal gearing, and so it was decided that a disk generation tool should be used to lower the overall difficulty for the end user. Because FreeCAD was used for all modeling in this project, the [FCGear workbench](https://wiki.freecadweb.org/FCGear_Workbench) was used to generate the disk. However, because this guide is intended to be usable agnostic of the chosen modeling software, this section has been split into a video which walks a user through the complete process, from installing FreeCAD to exporting the disk for printing; and a written section intended for those already familiar with FreeCAD.

If, however, you would prefer to mathematically define the disk in the program of your choice, [this paper](https://blogs.solidworks.com/teacher/wp-content/uploads/sites/3/Building-a-Cycloidal-Drive-with-SOLIDWORKS.pdf) has been provided to aid in that process. 

### 4.1.2 – Modeling for Complete FreeCAD Beginners

> [Video](https://youtu.be/ldznm4VQLB4)

### 4.1.3 – Modeling for FreeCAD Users
As mentioned in the introduction, this section will be making use of a cycloidal disk generated using the FCGear workbench, so start by installing it through the Addon Manager (in the Tools tab) or through the manual instructions found on the [wiki page](https://wiki.freecadweb.org/FCGear_Workbench). It should be noted that while cycloidal disk generation is a feature of the FCGear workbench, it is not documented on its wiki.freecadweb.org page.

### 4.1.4 – Generating the Disk

* Create a new file and add a new Part Design body. This guide will be using the Part Design workbench for all modeling except for the generation of the disk.
* Within the body, use the “Gear” workbench to create a new hypocyloid gear ![image](https://user-images.githubusercontent.com/79012344/120709101-70af4e00-c48a-11eb-845b-1f0beacbdb09.png)
* Notice that it generated 2 separate disks and a set of pins, however these pins and extra disk will be disabled shortly
* Selecting the hypocycloid gear in the combo view will allow for the adjustment of all perimeters, the details of which are listed below:

        ◦ Base
            ▪ Label
                • The label of the model within FreeCAD.
        ◦ Disks
            ▪ disk_height
                • The thickness of the individual disks generated
                    ◦ This should be set to the thickness of the center bearing
            ▪ show_disk0
                • Shows or hides the top disk
                    ◦ FCGear generates a set of 2 disks in the offset configuration as detailed in section 3.1.1, however obviously for the sake of 3d printing the individual disks this is not ideal. This guide advises setting show_disk1 to false and only working with disk0.
            ▪ show_disk1
                • Shows or hides the bottom disk
        ◦ Pins (mostly irreverent to this guide)
            ▪ center_pins 
                • This setting determines if the pins generated by FCGear will be vertically centered in reference to the disks generated. 
                    ◦ Note: This is only applicable when one of the disks is set to not be shown, as it will center the pins regardless of this setting if both disks are shown.
            ▪ pin_height
                • The height of the pins.
            ▪ show_pins
                • Determines if the pins will be shown or not.
                    ◦ This guide does not use the pins generated by FCGear, so this should be set to false.
        ◦ gear_parameter
            ▪ eccentricity
                • This sets how far off center the rotation of the disk will be.
                    ◦ Finding the right eccentricity is a balance between what can be produced using your drive bearing (see 3.2.1 for more) and what is possible given the constraints of how tightly packed the drive pins are.
                    ◦ Eccentricity also refers to how aggressive the lobes of the disk are, basically how far into the disk the drive pins go.
                        ▪ Because of this, it’s best to have a relatively high eccentricity to increase the lifespan of the drive, because less aggressive lobes will round over more easily.
                            • Eccentricity cannot be set infinitely large, though. To this end is advised that eccentricity is the last value set if all other values are known, so that trial and error can be used to get the highest eccentricity disk that is still solvable.
            ▪ hole_radius
                • The radius of the center hole that will accept the drive bearing. See 3.2.5 for pressure fitting this bearing properly.
            ▪ pin_circle_radius
                • This sets the radius of the reference circle that passes through the center of each drive pin.
                    ◦ This roughly corresponds to the radius of the housing, but to exactly equate the two see section 4.2.1
            ▪ pressure_angle_lim
                • The naming of this value suggests that it would limit the possible pressure angles between the pins and disk, but it only seems to inform how pressure_angle_offset effects the disk. Without FCGear’s wiki defining these values, it is difficult to say the exact nature of these settings, however leaving them at their default settings has given no issues in testing.
            ▪ pressure_angle_offset
                • See previous value (leave at default)
            ▪ roller_diameter
                • The diameter of the drive pins
                    ◦ Ideally this should be over 2mm
            ▪ segment_count
                • This determines the number of b-spline segments used to define the disk. In testing, using the same number of segments as teeth_number / lobes most frequently produced valid results.
                • It should also be noted that computation time for actions on the disk will increase   with more sections added.
            ▪ teeth_number
                • The number of lobes on the disk
                    ◦ Remember that there will be 1 more drive pin than lobes on the disk
                    ◦ The transmission ratio of the reducer will be teeth_number:1
        ◦ version
            ▪ version
                • Indicates the version of FCGear used to generate the gear
                    ◦ It cannot be changed

### 4.1.5 – Output Holes
The disk generated will be offset on the x axis by the eccentricity, which makes automatically spacing the output holes on a reference circle centered to the center of the disk rather than the origin of the model difficult, and can make creating the holes correctly after exporting to another program a pain. Ideally the disk would be moved to be centered in reference to the origin of the model, but I had difficulty making that work in the part design workbench. Those more familiar with FreeCAD may have a better solution to what is about to be shown, but this does work.

First, create a sketch mapped to the top face of the disk, and use the link to external geometry tool on the center circle. 

![image](https://user-images.githubusercontent.com/79012344/120709780-4a3de280-c48b-11eb-9455-85a9573c8cf3.png)

You can see the discrepancy between the center of the disk and the origin of the model here. Next toggle on construction mode ![image](https://user-images.githubusercontent.com/79012344/120709812-545fe100-c48b-11eb-9b20-0e137fddffd5.png) and use the “create a regular polygon” tool to first select a shape with the same number of points as you require output holes, and then create it centered to the newly linked external geometry. To fully constrain the sketch be sure to do something to fix the rotation of the regular polygon. In this case, a square is used to create 4 holes.

![image](https://user-images.githubusercontent.com/79012344/120709872-6a6da180-c48b-11eb-9a79-79a36d67a1f2.png)

Toggle out of construction mode and place a circle on each point, set them equal to each other, and then set their diameter. In this case, 7mm diameter nylon pins are to be used for the output pins and the disk has a 0.8mm eccentricity. However, the printer and filament used for testing has a habit of shrinking the final model slightly, so 0.1mm was added to the theoretical diameter (7.8) of the output holes.

Set the diameter of the reference circle. For more information on determining the best pin and reference circle dimensions, see 3.3.

![image](https://user-images.githubusercontent.com/79012344/120709944-85d8ac80-c48b-11eb-9532-ee564a8ff620.png)

Finally, use this sketch to cut a pocket through the disk.

![image](https://user-images.githubusercontent.com/79012344/120709967-8d985100-c48b-11eb-8a98-f56444e4f0f7.png)

## 4.2 – Housing
The primary function of a cycloidal drive’s housing is to support the drive pins and accept bearings for the output disk(s), and to that end, while being one of the more complex elements from a modeling perspective, is still a relatively simple component to create.

### 4.2.1 – The Main Body
When modeling the housing, it’s best to start with the section that will contain the drive pins and work outwards from there. The goal of this section is to mold the drive pins into the housing for extra support rather than have them free standing.

![image](https://user-images.githubusercontent.com/79012344/120710070-b4568780-c48b-11eb-841b-23003f12eb4e.png)

The most important value to consider here is the inner diameter of the hollow cylinder that makes up this section. The pins contact with the housing should be maximized, but not the the point of  interfering with the cycloidal disk’s travel. 

![image](https://user-images.githubusercontent.com/79012344/120710099-bcaec280-c48b-11eb-9a89-a7abdba5f51f.png)

To accomplish this, first take the pin circle diameter (remember to convert from the radius) as determined in (SECTION), and then subtract the pin diameter from it. In this example that would be 56 – 2.7 = 53.3. This gives a circle around the inner edge of the pins:

![image](https://user-images.githubusercontent.com/79012344/120710136-c6d0c100-c48b-11eb-8037-563e30016fae.png)

Because the eccentricity of the disk is just effectively how far it extends towards the pins and how deep the troughs are, adding 4 times the eccentricity to this value will give the maximum diameter of the area the disk will travel in, and thus the minimum size for the inner diameter of this part of the housing. In this case, 53.3 + 4(0.8) = 56.5. Multiplying the eccentricity by 4 may seem counterintuitive at first, but remember that it refers to the height of the peaks and depths of the troughs in terms of a center line in the middle of them, hence multiplying it by 2, and it is also in terms of the radius, so when working with diameters one has to multiply it by 2 again.

This plus preferably an extra 0.5mm at least (preferably more, but when working at this scale there simply isn’t much space to be spared) for extra clearance to accommodate for the disk being slightly too large or the housing flexing gives the inner diameter for the pin section of the housing.

The outer diameter  of this section will determine the external diameter of the rest of the gearbox, and may need to change later depending on if the bearing for the output disk requires more space to be mounted. This value should be no less than 3mm to prevent significant flexing of the housing.

![image](https://user-images.githubusercontent.com/79012344/120710160-ce906580-c48b-11eb-851c-71300c44abcf.png)

This section is then extruded to the thickness of the disk that will be within it plus a millimeter or so of extra clearance to prevent it from rubbing on the top and bottom of the gearbox. In this case, the housing will be extruded by 8mm.

![image](https://user-images.githubusercontent.com/79012344/120710184-d5b77380-c48b-11eb-8363-3e20c13e3f84.png)

## 4.2.2 – Drive Pins

The number of pins, their diameter, and their reference circle diameter were all determined in section (SECTION), so now all there is to do is model them appropriately. In this case, 31 pins (remember that the number of pins is one more than the number of lobes on the disk) with diameters of 2.7mm, on a reference circle with a radius of 28mm. Rather than model each pin in one sketch, a single pin will be modeled, extruded, and copied in a polar pattern. While not necessary, to have the pin line up visually with the default position of the disk generated in section 4.1, align this first pin on the x axis to the left of the y axis as seen in the model below.

![image](https://user-images.githubusercontent.com/79012344/120710219-e10a9f00-c48b-11eb-94f9-71ae9a8c9a9b.png)

The pin is then padded to the same thickness as the ring, in this case 8mm, and then copied in a polar pattern, in this case 31 times.

![image](https://user-images.githubusercontent.com/79012344/120710246-e9fb7080-c48b-11eb-8a8d-bf5811f2463f.png)

### 4.2.3 – Bearing Mounts

For both moving housing designs and output disk designs, there is a requirement for a large bearing at the top of the housing to center the output disk. The only difference between the two designs’ housings is that moving housing gearboxes will have another bearing on the bottom of the housing, and output disk variants will have a bearing at the top and a motor mount on the bottom. 

This is where cooperation between bearing choice and housing size come into play, because the ideal outer diameter for the output bearing is slightly larger than the inner diameter of the housing.  IT should be such that the hole it press fits into does not hang over the rest of the housing, which both means that during construction the disk can be inserted and during printing support material is not required. The outer diameter of the bearing should also not be too large as to require the housing’s total diameter to be increased to accommodate for it. To that end, a bearing with an outer diameter of 58mm was used. This was as close to perfect as the size constraints and bearing availability would allow.

![image](https://user-images.githubusercontent.com/79012344/120710303-ff709a80-c48b-11eb-9654-78ae81c9453b.png)

The modeling of this is extremely simple, only requiring a sketch on the top face of the housing with a central hole capable of fitting the bearing, in this case 58.15mm, and an external diameter set equal to the other housing section. If the bearing section’s wall is too thin (preferably 3mm), increase the overall diameter.

![image](https://user-images.githubusercontent.com/79012344/120710410-1adba580-c48c-11eb-8ec5-7f4a8f8cdb17.png)

This is then extruded to the thickness of the bearing that will mount within it.

### 4.2.4 Moving Housing Designs

For designs with a moving housing, simply repeat section 4.2.3 on the bottom side of the housing, as this will be used to mount a bearing on both sides of the gearbox.

![image](https://user-images.githubusercontent.com/79012344/120710516-39da3780-c48c-11eb-980e-a3bc5784c283.png)

How power is derived from this ring is up to the user, however for the example application a hole for REV Robotics 15mm x channel has been modeled.

![image](https://user-images.githubusercontent.com/79012344/120710537-4068af00-c48c-11eb-8529-8fcfb8d2f983.png)

### 4.2.5 Stationary Housing Designs
If a stationary housing is to be used, a motor mount needs to be created on the bottom of the housing. 

![image](https://user-images.githubusercontent.com/79012344/120710602-52e2e880-c48c-11eb-84fc-93a1081587b2.png)


To create a motor mount, create a sketch on the bottom face of the housing with an outer diameter matching that of the housing, and holes for the motor screws and a space for the drive shaft to enter the housing.

![image](https://user-images.githubusercontent.com/79012344/120710649-6726e580-c48c-11eb-9255-02e32c741d8a.png)

For the example nema 17 motor, a centered square pattern of 4 m3 screws spaced 31mm from each other. The center hole must be at least the outer diameter of the eccentric shaft, however if a bearing is desired to brace the input shaft, be sure to include spacing for the bearing.

The screw holes will need to be countersunk to avoid the cycloidal disk fouling on them. So, when extruding the motor mount plate, take into account both the space taken by countersinking the screws and having enough material for them to mount to. In this case, the screw heads are 2mm tall and ~6mm in diameter, so the bottom plate will be extruded 5mm to allow for 2mm of countersinking and 3mm of material underneath them.

![image](https://user-images.githubusercontent.com/79012344/120710667-6db55d00-c48c-11eb-89cd-cfe463059f3f.png)

Finally a chamfer (1mm) should be applied to the screw countersink holes to minimize friction between their edge and the disk.

![image](https://user-images.githubusercontent.com/79012344/120710693-7443d480-c48c-11eb-8333-1fdd41e25dea.png)

## 4.3 – Output Ring & Pins
Output disks and pins are perhaps the simplest component to model but bear the majority of the forces imparted onto the cycloidal drive.

### 4.3.1 – Driven Disk – 3D Printed Pins
This is the simplest version of the output disk, and integrates the output pins and output disk into one printed component. It is not advised for high load bearing applications (see 3.2.3).

First, the output disk is created, which is simply a cylinder intended to fill the interior dimensions of the output bearing. In this case a 45.25 mm disk extruded to 7mm thick.

![image](https://user-images.githubusercontent.com/79012344/120811211-bbc47200-c519-11eb-8bdd-4ed57fd648bd.png)

![image](https://user-images.githubusercontent.com/79012344/120811225-bff08f80-c519-11eb-9fc3-bb52c449f799.png)

Next, on one face of the disk, model a pin whose center is on the reference circle found in section 2.3.

![image](https://user-images.githubusercontent.com/79012344/120811260-c848ca80-c519-11eb-9553-211f0a10d6bf.png)

Next extrude that to the thickness of the cycloidal disk, and use a polar pattern to duplicate it evenly around the disk for however many pins are required.

![image](https://user-images.githubusercontent.com/79012344/120811284-ced74200-c519-11eb-8151-a5bb971388c9.png)

Mount whatever load is to be driven to the other side of this plate. Also, be sure to account for the motor drive shaft extending too far into the housing and fouling on the output disk. You may need to place a hole in its center to account for this.

### 4.3.2 – Driven Disk With Nylon Pins / Top Plate of Driven Housing Variants
This is effectively the same as the 3D printed pin version, except instead of pins being mirrored with the polar pattern, holes for screws/bolts, in this case 3mm screws, are used. This plate can either be used as the driven output plate or as the top plate of driven housing designs.

![image](https://user-images.githubusercontent.com/79012344/120811332-dc8cc780-c519-11eb-8c2b-8958727d3669.png)

For driven output disk designs, if a flush face on the outside face of the is desired, add hexagonal holes to capture nuts on the other side of the disk. 

![image](https://user-images.githubusercontent.com/79012344/120811354-e8788980-c519-11eb-8fed-614c278ef4e9.png)

This is not required for using this model in driven  housing designs as the screw heads will be on this plate, not the nuts. However countersinking can be done for anesthetics or to use a shorter screw.

For driven disk designs, it’s then just a matter of screwing on nylon spacers.

### 4.3.3 – Moving Housing Motor Plate
Ideally, the motor plate of a moving housing cycloidal drive would be identical to the top plate, save for space to capture nuts for the screws running through the pins, having holes to mount the motor, and a hole for the motor drive shaft, but because in this case the motor mounting holes could not fit within the inner diameter of the output bearing, a slightly larger adapter plate was created.

Example:

![image](https://user-images.githubusercontent.com/79012344/120811461-fd551d00-c519-11eb-8bff-2a0810af9050.png)

The same starting plate is taken to fill the inner diameter of the output bearing, but now a larger (55mm diameter and 5mm thick to allow the screw heads to have 2mm countersinking) plate is added to its bottom face to accommodate the motor mounting screw locations.

![image](https://user-images.githubusercontent.com/79012344/120811490-03e39480-c51a-11eb-926c-85989983351e.png)

Motor mounting holes are then cut from the bottom face through the model.

![image](https://user-images.githubusercontent.com/79012344/120811512-0b0aa280-c51a-11eb-915a-11aa577b9890.png)

These then have 7mm diameter holes cut from the top to give clearance for the screw heads.

![image](https://user-images.githubusercontent.com/79012344/120811586-22499000-c51a-11eb-910a-131ced7b64d6.png)

In this case, the pockets are 9mm, 7mm to clear the bearing mount section and another 2mm for countersinking of the screws.

Next, the same holes for mounting output pins are added, with corresponding hexagonal pockets on the other side to capture a nut.

![image](https://user-images.githubusercontent.com/79012344/120811605-283f7100-c51a-11eb-8de7-97336688384f.png)

Finally, a centered hole, in this case 15mm in diameter is added to accommodate for the eccentric shaft passing into the housing.

![image](https://user-images.githubusercontent.com/79012344/120811629-2e355200-c51a-11eb-908b-fe6a832149cf.png)

## 4.4 – The Eccentric Shaft
The design of the eccentric shaft is heavily dependent on the motor drive shaft that it interacts with, however the base concept of the shaft being offset from the input shaft is constant. In this case, a D shaft coming from a nema 17 motor is the input shaft, and a very simple eccentric shaft was created to cover the entire 18mm section of the motor output shaft with a flat side machined onto it. 

First, a hole tested to snugly accept the D shaft was modeled.

![image](https://user-images.githubusercontent.com/79012344/120811686-3ee5c800-c51a-11eb-8857-5ffca1bcf243.png)

Then a circle that has been test fit with the inner diameter of the drive bearing (in this case 7.89mm) is placed offset by the  eccentricity from the input shaft.

![image](https://user-images.githubusercontent.com/79012344/120811710-44dba900-c51a-11eb-8a65-8db1038313a3.png)

Note that the flat side of the D shaft, where most power will be transferred, is placed in the thicker area of the shaft. This is a poorly chosen bearing inner diameter because of the extremely thin section it creates. See 3.2.1 for best drive bearing practices.

This is then extruded by 18mm.

![image](https://user-images.githubusercontent.com/79012344/120811724-4b6a2080-c51a-11eb-855f-4b174eadde1e.png)


# 5 – Closing

The exact intent for this project has been a moving target from it’s conception to completion, and this is well reflected in the initial research question. This project was originally intended to be a research paper comparing different gearbox designs, however time constraints and what was viewed at the time as an apparent need for something like what this final product has become, have shaped it into something different entirely. This guide takes the primary focus of the research question, namely understanding at least one complex gear set that can be manufactured via 3D printing, and expands on it in a way that is more targeted to what may actually be useful to its intended audience.

Hopefully this guide was at least of some use someone, slapdash as it may be in places.

## 5.1 – Sources

Thube, Sandeep, and Bobak, Todd. *Dynamic Analysis of a Cycloidal Gearbox Using Finite Element Method*, American Gear Manufacturers Association, 2012, http://edge.rit.edu/content/P15201/public/MSD%20II/Research%20Papers%20Humanoids/Unknown%20-%20AGMA%20Technical%20Paper%20Dynamic%20Analysis%20of%20a%20Cycloidal%20Gearbox%20Using%20Finite%20Element%20Method%20-%20Thube,%20Bobak.pdf

Younis, Omar. *Building a Cycloidal Drive with SOLIDWORKS*, The SOLIDWORKS Blog, https://blogs.solidworks.com/teacher/wp-content/uploads/sites/3/Building-a-Cycloidal-Drive-with-SOLIDWORKS.pdf

Bruton, James. *Experiments with Cycloidal Drives*. YouTube, 2021, https://www.youtube.com/watch?v=pWMB5VbLb6w&t

Bruton, James. *3D printed Cycloidal Drive V2*. YouTube, 2021, https://www.youtube.com/watch?v=pWMB5VbLb6w&t

Muthe, Abhijit. et al. *Design and Fabrication of Cycloidal Gearbox*. International Research Journal of Engineering and Technology, 2018, https://www.irjet.net/archives/V5/i4/IRJET-V5I4565.pdf

![TL](https://user-images.githubusercontent.com/79012344/120854056-97cd5480-c54a-11eb-8f6f-5dc9a8482d97.png)
