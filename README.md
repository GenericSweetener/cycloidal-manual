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

## 2.5 – Getting an output (output pins and holes)
Getting power out of a cycloidal drive is less about directly connecting the cycloidal disk to an output shaft and more to do with rectifying the wobbling motion of the disk as it rotates so that the gear reduction created can actually be used. This is where the output holes and pins seen in the animation (purple) come into play. The rectifying method of choice here uses at least 2 (normally more) holes in the cycloidal disk spaced equally in a circular pattern on its face, shown below on a reference circle of a known size. 

![Screenshot from 2021-06-03 16-17-28](https://user-images.githubusercontent.com/79012344/120706625-53c54b80-c487-11eb-936c-33393a05f40b.png)

These holes then correspond to output pins/rollers that are arranged in the same fashion and on a reference circle of the same diameter as the reference circle of the holes.

file:///home/mike/Pictures/Screenshot%20from%202021-06-03%2016-20-17.png![image](https://user-images.githubusercontent.com/79012344/120706877-a141b880-c487-11eb-906b-b87bfa480181.png)

hese pins are mounted to a surface that can rotate, called the output disk (seen in purple in the animation), which is centered by a bearing of some sort in reference to the housing. What makes it possible for these output pins to mesh with the holes of the disk even though the disk is not centered while rotating is that the disk’s holes are the same as the diameter as the output pins plus the eccentricity of the disk. This in effect means that while the output pins are never centered in the output holes, their sides are able to contact the edges of the holes, rolling along the inside of the output holes and being pushed in the correct direction. This is why there need to be more than one set of pins and holes to prevent slop in the output from the pins not being directly attached to the disk, instead requiring multiple angles of contact.

## 2.6 - Getting it to work (the cycloidal disk and drive pins)
Up until this point, a simplified model of the actual mechanism by which the gear reduction is created has been used to demonstrate the concepts around it, however this model would be extremely inefficient and borderline useless in any practical application. So, instead of relying on the friction between a smooth cycloidal disk and its housing, cycloidal drives use a set of drive  pins/rollers in the housing (silver), and a disk with corresponding lobes for those pins which is the cycloidal disk (yellow). 

<img src="https://user-images.githubusercontent.com/79012344/120367491-ec6c9780-c2de-11eb-9e0b-7f542085e93b.gif" width="500">
> By Petteri Aimonen - Own work, Public Domain, https://commons.wikimedia.org/w/index.php?curid=7732225

The same movement is happening here as before, that wobbling roll is the same as when it was 2 simple circles, but now there’s multiple points of contact being made so the disk can’t just slip around, it can only rotate on it’s rolling path. The interaction between the pins and lobes is also a similar motion to the circle rolling around another circle, one could also think of the disk as the pattern made by rolling one of the pins around a circle. It is at this point that the number of pins in the housing compared the number of lobes on the cycloidal disk come into play. Knowing that the lobes on the disk and the drive pins are in roughly the same scale to each other, similar to gear teeth, if there were the same number of pins and lobes then the disk would not be able to move because it would just be sitting locked into the pins.

![image](https://user-images.githubusercontent.com/79012344/120707296-275dff00-c488-11eb-9c4f-971e62db18af.png)

That’s just going to bind up.

This is why, in this guide and in most cycloidal reducer designs, there is one more pin on the housing than there are lobes on the disk. Not only does this make room for the rolling motion possible in the first place, it defines the transmission ratio of the reducer in more solid terms than the difference between the circumference of 2 circles. This is to say that for every full rotation of the input shaft, the disk moves 1 (or whatever the difference between the number of pins and lobes is) pin over. With this knowledge, the gear ratio can be defined. In this example of 30 pins on the disk and 31 pins in the housing, it would take 30 rotations of the input shaft to make one full rotation of the cycloidal disk, making the transmission ration 30:1.

## 3 – Snap back to reality
An understanding of the theory of operation is only so good as the practical application of those concepts. To that end, this section will translate the concepts from section 2 to what one may actually want to manufacture.

## 3.1 – Variants of the design

This guide offers 2 main variants of the cycloidal reducer. The first is what is generally considered to the be standard design and is what is described throughout section 2, using a plate connected to the output pins as the power output for the reducer. This design will be called the driven disk design. The second variant is conceptually the same as the first, but has fixed output pins and instead rotates the housing. 

### 3.1.2 – Driven housing

The best way to conceptualize driven housing designs is to imagine the housing as being free floating and the cycloidal disk as limited in its range of motion to it’s wobble. This is accomplished by mounting the output pins to a stationary plate, in this case the one the motor mounts to. This component is referred to as the motor plate.

> place holder for video, please hold

Now, when the housing interacts with the disk, the disk can’t rotate but the housing can, so it does.

> place holder for video, please hold

Of course, the housing needs to be kept centered, won’t survive much torsion if only supported on one side, and the forces applied to it shouldn’t be transferred to the cycloidal disk, so it’s stabilized by a bearing on the top and bottom. The bottom bearing connects to the motor plate, and the top bearing connects to a plate that has screws running down through the drive pins to connect to the motor plate.

> photo

### 3.1.3 – A note on stacked disks

While not utilized in this guide, it is common practice to reduce the vibration caused by the off center rotation of the cycloidal disk by having 2 disks in the same housing 180 degrees offset from each other. 

![Screenshot from 2021-06-03 16-28-52](https://user-images.githubusercontent.com/79012344/120707902-ddc1e400-c488-11eb-9ce5-67808c2e525d.png)

These disks will rotate in the same direction and at the same speed so their output holes still line up with each other (provided they are symmetrical across both the x and y axis). This means that while rotation is still taking place, the shifting weight of one disk is always offset by the other. To accomplish this the eccentric shaft is split into 2 cams so the top and bottom disks are driven 180 degrees offset from each other.

> Screenshot from 2021-06-03 16-28-52

## 3.2 – Bearings – an epic saga
Anyone genuinely considering manufacturing a cycloidal drive by means of 3D printing will have likely noticed the very high number of surfaces that need to be in contact with each other in even the most basic of cycloidal designs. The good news is that all but the edge face of the cycloidal disk can be replaced by a friction reducing element, however this may not always be the best design decision. At any rate, a cycloidal drive that will carry any sort of load requires at the very least 2 bearings: 1 in the center of the cycloidal disk and another to keep the output ring centered. 

### 3.2.1 – The required bearings – cycloidal disk
The bearing in the center of the cycloidal disk both relies on and can limit some of the primary dimensions of the cycloidal drive, so care should be taken when finding a bearing to use. Firstly, keep in mind that this bearing is what makes it possible to use an eccentric shaft, so some thought needs to be given to the inner dimension of the bearing so that the the desired level of eccentricity is both possible and won’t create a situation where the hole in the eccentric shaft that accepts the input shaft of the motor is extremely thin on one side. 

> (image)

A good rule of thumb is that (assuming the eccentric shaft is accepting a metal output shaft from the motor) the inner diameter of the bearing should be equal to the diameter of the motor output shaft plus the disk’s eccentricity plus however thick you want the thinnest part of the eccentric shaft to be (at least 3mm).

> (bearing spacing inkscape image)

Another point to consider is that the cycloidal disk should be a similar thickness to the bearing. This ensures that the bearing will be able to transfer power to the entire edge of the cycloidal disk. It’s also a good idea to have a ridge of some sort modeled in the hole for the bearing so it can be pressure fit until bottoming out.

> (Image of ridge)

One final element of note is that, especially with a 3D printed disk, the bearing is likely to be the heaviest spinning component, and will be spinning off center to the rest of the assembly. This can be dealth with by using a stacked design (see 3.1.1), but suffice it to say that a lighter bearing will create less vibration.

### 3.2.2 – The required bearings – output pins / output ring
To center the output disk/pins to the housing, it is advised that a relatively large bearing with an inner diameter that is able to fit around the outside of the output disk, and is therefore larger than the outer edge of the output pins. This then mounts into the housing.

> (image)

This method allows the forces applied to the output ring to be braced against the housing, which can be reinforced depending on the load required. While this method is strong and robust, it does have some drawbacks. Cost is by far the largest issue bearings of this specification can add to a project, with the example bearings (inner diameter = 45mm, outer diameter = 85mm, thickness = 7mm) costing roughly $10 per bearing, which can get quite expensive for something like a quadrupedal robot, especially when using cycloidal designs that require 2 output bearings (3.1.2). These larger bearings are also quite heavy, the aforementioned bearings in particular weigh ~50g.

### 3.2.3 – Output pins

Using something other than 3D printed plastic for output pins is by far the most valuable upgrade one can make to their budget cycloidal drive, because it increases the strength of the assembly by a massive degree without adding too many more individual components. The drawbacks of 3D printing output pins is quite obvious when one considers the relatively low force required to sheer off the pins on the layer lines. 

> (Image of pins, failure gif, image of sheered pins)

The 2 routes one can take in making reinforced output pins are using small bearings or using round nylon standoff spacers in place of 3D printed output pins. The only real difference from a modeling and construction perspective is that rather than modeling pins to be printed on the output disk, one would have holes through it, potentially with hexagonal holes on the other side going partially through it to capture a nut. And of course a bolt/screw and a nut is necessary to keep the bearing / nylon spacer in place.

> (image)

This guide uses 7mm outer diameter 3mm inner diameter nylon spacers as output pins.

### 3.2.4 - Drive pins
Using bearings in place of 3D printed drive pins can significantly reduce wear and friction within a cycloidal drive, however these benefits come with increased weight, drive pin diameter, cost, and complexity. To that end this guide did not include testing with bearings in place of drive pins and  does not have modeling instructions for them. 

### 3.2.5 – A note on pressure fitting bearings
This guide requires a fair number of bearings to be pressure fit into 3D printed housings, so some thought and testing of bearing fitment should be done before printing components. It is therefore advised that you create test pieces for bearings to fit into / to fit into bearings and undergo some trial and error to find dimensions that can tightly hold or fit into a bearing without being deformed by it or requiring forceful persuasion (Take it from someone who’s broken a bearing with a hammer). When making test pieces, be sure to use the same print settings as on the final components. Time spent in this process will save on test prints down the line.

## 3.3 – Choosing the best output pin circle
The output pin circle is the reference circle that passes through the center of the output holes on the cycloidal disk and through the center of the drive pins as mentioned in section 2.5. Especially on the smaller cycloidal disks used in this guide, it is of the upmost importance to ensure that the output holes on the cycloidal disk have enough material surrounding them as to not create weak thin spots in the disk for deformation or cracking to occur. A good rule of thumb is to have the output pin circle roughly centered on the disk.

>(image)

However, this is not always possible because the inner diameter of the drive bearing can limit the maximum size of the output pin circle, and should also be taken into account. This is why the output holes on the reference models provided are not perfectly centered on the cycloidal disk.

# 4 – Design & CAD

## 4.1 – Cycloidal disk
This guide uses the process of modeling the cycloidal disk as the focal point for setting all major parameters of the cycloidal drive. To that end, the specific parameters to be considered and their effects on the final product will be discussed in this section.

### 4.1.1 – Why FreeCAD?
The two most common ways to model a cycloidal disk are mathematically defining it from scratch as a curve, or using a disk generation tool that creates a disk based on easily inputted parameters. This guide is aimed at beginners to cycloidal gearing, and so it was decided that a disk generation tool should be used to lower the overall difficulty for the end user. Because FreeCAD was used for all the modeling in this project, the FCGear workbench (link to wiki) was used to generate the disk. However, because this guide is intended to be usable agnostic of the chosen modeling software, this section has been split into a video which walks a user through the complete process, from installing FreeCAD to exporting the disk for printing; and a written section intended for those already familiar with FreeCAD.

If, however, you would prefer to mathematically define the disk in the program of your choice, this paper has been provided to aid in that process (link)

### 4.1.2 – Modeling for complete FreeCAD beginners
The video isn’t uploading properly, please hold

### 4.1.3 – Modeling for FreeCAD users
As mentioned in the introduction, this section will be making use of a cycloidal disk generated using the FCGear workbench, so start by installing it through the Addon Manager (in the Tools tab) or through the manual instructions found on the wiki page (link to wiki). It should be noted that while cycloidal disk generation is a feature of the FCGear workbench, it is not documented on its wiki.freecadweb.org page.

### 4.1.4 – Generating the disk

* Create a new file and and a new Part Design body. This guide will be using the Part Design workbench for all modeling except for the generation of the disk.
* Within the body, use the “Gear” workbench to create a new hypocyloid gear ![image](https://user-images.githubusercontent.com/79012344/120709101-70af4e00-c48a-11eb-845b-1f0beacbdb09.png)
* Notice that it generated 2 separate disks and a set of pins, however these pins and extra disk will be disabled shortly
* Selecting the hypocycloid gear that was generated in the combo view will allow for the adjustment of all perimeters, the details of which are listed below:
  * Base
   * Label
    * The label of the model within FreeCAD.
  * Disks
   * disk_height
    * The thickness of the individual disks generated
     * This should be set to the thickness of the center bearing

(default settings screenshot)

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
                            • Eccentricity cannot be set infinitely large, though. To this end is advised that eccentricity is the last value set if all other values are known, so that trial and error can be used to get the highest eccentricity disk that is still solvable. Add more here about disks not being valid and solvable
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
