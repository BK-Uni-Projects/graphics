
---
title: lincoln Lectures On Graphics - 02
author: shearer12345
---



---
title: Graphics Assessment
author: shearer12345
---

#Assessment Methods and Weighting

- Assignment (Coursework)
    - weighting = 70%
- Exam (Closed-book)
    - weighting = 30%

- From the Module Specification - Graphics 2015-16
    - Blackboard=>Graphics (CGP2012M)->About this module

---

##Learning Outcomes

- Both assessments assess both Learning Outcomes
- Learning Outcome 1

    > design and develop interactive 3D graphics software, applying appropriate mathematical/algorithmic techniques for efficient 2D and 3D graphics
- Learning Outcome 2

    > Demonstrate deep understanding of computer graphics programming techniques and approaches
- Both are *individually* assessed

---

##What to expect in the assignment

- based around developing an interactive 3D scenario
    - such as a simple computer game:
        - Pong
        - Space Invaders
        - Virtual plane

- will submit to blackboard, but live-demo in the following workshop, affording:
    - faster mark turn around
    - immediate verbal feedback
    - written feedback with mark release
    - opportunity to ask questions

---

##What features in the assignment will be assessed

- aims to assess practical capability in writing programs that generate 3D graphics and updating the 3D scene based on human input
- requires a set of basic 3D graphics features, such as:
    - 3D geometry
    - 3D (perspective) projections
    - moving/rotating objects
    - moving/rotating view points
    - coloured objects
    - advanced features, such as:
        - lighting simulation
        - texturing.

---

##What to expect on the Exam

- will assess the more theorectical side of computer graphics
- wider understanding of the area
- questions on the following would be appropriate (examplar, other questions could occur):
    - how standard algorithms or techniques work
    - the factors that affect the performance of graphical systems
    - writing graphics "shaders" from scratch
    - or critique/modify/correct provided "shaders"


#Recommended Reading for Graphics

---

##Required

- **Learning Modern 3D Graphics Programming**
    - Open book of GL tutorials
    - Jason L. McKesson
    - http://www.arcsynthesis.org/gltut/

---

##Recommended

- **OpenGL Superbible**: Comprehensive Tutorial and Reference
    - Graham Sellers, Richard S. Wright, Nicholas Haemel
    - **5th or 6th edition**. NOT 4th or previous
    - Edition: 5 ISBN-13: 978-0321712615
    - Edition: 6 ISBN-13: 978-0321902948

OR

- **Interactive Computer Graphics**: A Top-Down Approach with Shader-Based OpenGL
    - Dave Shreiner, Edward Angel - **6th edition only**. NOT 5th or previous
    - Edition: 6 ISBN-13: 978-0273752264

---

##Additional

- OpenGL specification - v3.3 or 4
    - Version 3.3: http://www.opengl.org/sdk/docs/man3/
    - Version 4: http://www.opengl.org/sdk/docs/man4/


#It's Triangles All The Way Down

![triangle](assets\triangle.png)

---

##It's Triangles All The Way Down

- Remember: Everything is a shell
- All of these shells are made of **triangles**
- Even surfaces that appear to be round are merely **triangles** if you look closely enough
- There are techniques that generate **more triangles** for objects that appear closer or larger, so that the viewer can almost never see the faceted silhouette of the object, but they are **always made of triangles**

---

##It's Triangles All The Way Down

- Note: Some rasterizers use planar quadrilaterals: four-sided objects, where all of the lines lie in the same plane. One of the reasons that hardware-based rasterizers always use triangles is that all of the lines of a triangle are guaranteed to be in the same plane. Knowing this makes the rasterization process less complicated.


#Objects

- An object is made out of a collection of adjacent triangles that define the outer surface of the object
- Such a collection of triangles is often called:
    - a model
    - a mesh
    - geometry
    - **these terms are used interchangeably**

#Pipeline

- Rasterization has several phases ordered into a pipeline
- triangles enter from the left and a 2D image is filled in at the right
- A pipeline is very ammeneable to hardware acceleration
- it operates on each triangle one at a time, in a specific order
- triangles can be fed into the left of the pipeline while triangles that were sent earlier can still be in some phase of rasterization.

---

##OpenGL Pipeline

![http://goanna.cs.rmit.edu.au/~gl/teaching/ Interactive3D/2011/lecture2.html](assets\pipeline01.png)

---

##Raster Ordering

- The order in which triangles and the various meshes are submitted to the rasterizer can affect its output
    - (if you are doing fancy stuff)
- No matter how you submit the triangular mesh data, the rasterizer will process each triangle in a specific order:
    - drawing the next one only when the previous triangle has finished being drawn

---

##Triangles and Vertices (in OpenGL)

- Triangles consist of 3 vertices
    - A vertex is a collection of arbitrary data
    - ???!!!???
    - For the sake of simplicity, in our context this data must contain a point in three dimensional space - it may contain other data (e.g. colour)
- Any 3 points that are not on the same line create a triangle
    - A triangle is defined by exactly 3 (three-dimensional) points - (X, Y, Z)



#What Is OpenGL?

- A *real* API
- There are a number of languages that have implemented it
    - but it all comes back to C in the end
   - and guess what language we’re running the course in?
- The API, in C, is defined by a number of `typedefs`, `#defined` enumerator values, and `functions`
    - `typedefs` define basic GL types like `GLint`, `GLfloat` and so forth
    - Complex aggregates like structs are never directly exposed in OpenGL

---

##C/C++ and OpenGL

```C
structObject{
    int anInteger;
    float aFloat;
    char *aString;
};

//Create the storage for the object.
ObjectnewObject;

//Put data into the object.
newObject.anInteger= 5;
newObject.aFloat= 0.4f;
newObject.aString= "Some String";
```

```C
//Create the storage for the object
GLuintobjectName;
glGenObject(1, &objectName);

//Put data into the object.
glBindObject(GL_MODIFY,objectName);
glObjectParameteri(GL_MODIFY, GL_OBJECT_AN_INTEGER, 5);
glObjectParameterf(GL_MODIFY, GL_OBJECT_A_FLOAT, 0.4f);
glObjectParameters(GL_MODIFY, GL_OBJECT_A_STRING, "Some String");
```

---

##Explaining C and OpenGL

```C
//Create the storage for the object
GLuintobjectName;
glGenObject(1, &objectName);

//Put data into the object.
glBindObject(GL_MODIFY,objectName);
glObjectParameteri(GL_MODIFY, GL_OBJECT_AN_INTEGER, 5);
glObjectParameterf(GL_MODIFY, GL_OBJECT_A_FLOAT, 0.4f);
glObjectParameters(GL_MODIFY, GL_OBJECT_A_STRING, "Some String");
```

- Not actual OpenGL commands but..
- OpenGL **owns the storage** for all OpenGL objects
    - the user can only access an object **by reference**.
    - almost all OpenGL objects are referred to by an unsigned integer (the **GLuint**)
        - **?? OMG !!**

---

##Explaining C and OpenGL

```C
GLuintobjectName;
glGenObject(1, &objectName);

//Put data into the object.
glBindObject(GL_MODIFY,objectName);
glObjectParameteri(GL_MODIFY, GL_OBJECT_AN_INTEGER, 5);
glObjectParameterf(GL_MODIFY, GL_OBJECT_A_FLOAT, 0.4f);
glObjectParameters(GL_MODIFY, GL_OBJECT_A_STRING, "Some String");
```

- Objects are created by a function of the form **`glGen*`**, where * is the type of the object
```C
glGen*(No.of objects to create)
```
    - `GLuint*` array that receives the newly created object names

---

##Explaining C and OpenGL

```C
GLuintobjectName;
glGenObject(1, &objectName);

//Put data into the object.
glBindObject(GL_MODIFY,objectName);
glObjectParameteri(GL_MODIFY, GL_OBJECT_AN_INTEGER, 5);
glObjectParameterf(GL_MODIFY, GL_OBJECT_A_FLOAT, 0.4f);
glObjectParameters(GL_MODIFY, GL_OBJECT_A_STRING, "Some String");
```

- To modify most objects, they must first be **bound** to the context

```C
GL_BindObject(Thefictitious targetisthelocation where, ObjectNameisbound)
```

- Many objects can be bound to different locations in the context
    - this allows the same object to be used in different ways

---

##Explaining C and OpenGL

```C
GLuintobjectName;
glGenObject(1, &objectName);

//Put data into the object.
glBindObject(GL_MODIFY,objectName);
glObjectParameteri(GL_MODIFY, GL_OBJECT_AN_INTEGER, 5);
glObjectParameterf(GL_MODIFY, GL_OBJECT_A_FLOAT, 0.4f);
glObjectParameters(GL_MODIFY, GL_OBJECT_A_STRING, "Some String");
```

- Functions that actually change values within the object are given a target parameter
    - so that they could modify objects bound to different targets

```C
glObjectParameterf(GL_MODIFY, An integer that points to an object, A new float value)
```

- Note: All OpenGL objects are not as simple as this example, and the functions that change object state do **not all follow** these naming conventions

---

##The Structure of OpenGL

- The OpenGL API is defined as a state machine
- Almost all of the OpenGL functions **set** or **retrieve** some state in OpenGL
    - the only functions that do not change state are functions that use the currently set state to cause **rendering to happen**
    - *except a bunch of other functions that we'll pretend don't exist*
- Think of the state machine as a very large `struct` with a great many different fields called the **OpenGL context**
    - each field in the context represents some information necessary for rendering

---

##The Structure of OpenGL

![http://www.opentk.com/files/OpenGL%20machine%20diagram%20v2.png](assets/OpenGLmachineDiagramv2.png)

---

##The Structure of OpenGL

- Objects in OpenGL are thus defined as
    - a list of fields in this struct that can be saved and restored
- Binding an object to a target within the context causes
    - the data in this object to replace some of the context's state
- After binding, future function calls that read from or modify this context state will
    - read or modify the state within the object

---

##Objects in OpenGL

```C
//Create the storage for the object
GLuint objectName;
glGenObject(1, &objectName);

//Put data into the object.
glBindObject(GL_MODIFY,objectName);
glObjectParameteri(GL_MODIFY,GL_OBJECT_AN_INTEGER, 5);
glObjectParameterf(GL_MODIFY, GL_OBJECT_A_FLOAT, 0.4f);
glObjectParameters(GL_MODIFY, GL_OBJECT_A_STRING, "Some String");
```

- Objects are usually represented as `GLuint` integers
    - these are handles to the actual OpenGL objects
- The **integer value 0 is special**; it acts as the object equivalent of a `NULL` pointer
    - binding object **0** means to **unbind** the currently bound object
- This means that the **original context state**, the state that was in place before the binding took place, now becomes the context state

---

##The OpenGLSpecification

- the specification defines
    - the initial OpenGLstate
    - what each function does to change or retrieve it
    - what happens when you call a rendering function
- The most important thing to understand about the specification is it describes **results**, **not implementation**

---

##The ARB

-  The specification is written by the OpenGL **Architectural Review Board** (ARB)
    - a group of representatives from companies like Apple, NVIDIA, and AMD (the ATI part), among others
    - The ARB is part of the **Khronos Group**
    - [https://www.opengl.org/archives/about/arb/](https://www.opengl.org/archives/about/arb/)

![OpenGl Logo](assets/opengl-logo.gif)
![Khronos Group Logo](assets/Khronos_Group_logo.png)

---

##The OpenGL Specification

- The OpenGL ARB controls the **specification**, but it does not control OpenGL's code
- It is up to the developers of that hardware to write an OpenGL **Implementation** for that hardware

---

##Who controls / builds the implementations

- Who controls the OpenGL implementation is different for different operating systems:
    - on **Windows**, OpenGL implementations are controlled virtually entirely by the hardware makers themselves
    - on **Mac OSX**, OpenGL implementations are controlled by Apple
    - ON **linux**, you have:
        - closed-source hardware controlled implementations
        - open-source hardware directed implementations
        - open source implementations
        - ...
- The upshot of this is if you are writing a program and it seems to be exhibiting off-spec behaviour, the fault is with the maker of your OpenGL implementation

---

##And finally, just for fun...

- There are many versions of the OpenGL Specification
    - [https://www.opengl.org/documentation/specs/](https://www.opengl.org/documentation/specs/)
- OpenGL versions are not like most Direct3D versions, which typically change most of the API
- Code that works on one version of OpenGL will almost always work on later versions of OpenGL
    - i.e. the hardware usually supports older versions
    - though not neccasraily efficiently

---

##GL vs GLSL version numbering

> GLSL versions have evolved alongside specific versions of the OpenGL API.
> It is only with OpenGL versions 3.3 and above that the GLSL and OpenGL major and minor version numbers match.
> These versions for GLSL and OpenGL are related in the following table.

    - http://en.wikipedia.org/wiki/OpenGL_Shading_Language#Versions


---

| GLSL Version | OpenGL Version | Date           | Shader Preprocessor |
|--------------|----------------|----------------|---------------------|
| 1.10.59      | 2.0            | April 2004     | #version 110        |
| 1.20.8       | 2.1            | September 2006 | #version 120        |
| 1.30.10      | 3.0            | August 2008    | #version 130        |
| **1.40.08**  | **3.1**        | March 2009     | **#version 140**    |
| 1.50.11      | 3.2            | August 2009    | #version 150        |
| 3.30.6       | 3.3            | February 2010  | #version 330        |
| 4.00.9       | 4.0            | March 2010     | #version 400        |
| 4.10.6       | 4.1            | July 2010      | #version 410        |
| 4.20.11      | 4.2            | August 2011    | #version 420        |
| 4.30.8       | 4.3            | August 2012    | #version 430        |
| 4.40         | 4.4            | July 2013      | #version 440        |
| 4.50         | 4.5            | August 2014    | #version 450        |

http://en.wikipedia.org/wiki/OpenGL_Shading_Language#Versions


