---
title: "Computer Graphics :: Project Overview"
date: 2019-03-04T07:02:54+05:45
draft: false
---
IOE Pulchowk Campus assigns a project each year, for the practical fulfillment in Computer Graphics course allocated in the first semester of third year (fifth semester). It is for the same, that we decided to create a 3D rendering engine depicting a human face. The engine would be written in C++ and backed by OpenGL. However, none of the functions within OpenGL could be used, as the specification of the project clearly stated that we were to use only the 2D point plotting function available as the OpenGL standard library. None of the Vertex Buffer thingies exposed by OpenGL could be used, and each pixel had to be calculated, transformed and plotted individually. Not only this, the projection for the final image had to be calculated and the .obj file used for the model had to be read seperately. It all sounds *tedious* and believe me, it is. But in the end, the exhilarating feel of doing something on your own using simple primitives such as a point plotting function overcomes everything. Mind you, **everything**.

The first pain for me personally was to find a suitable resource to learn, as deadline for the project was just days away, and I was not proficient in either OpenGL or Computer Graphics Algorithms for that matter. I went on a frantic search to find learning resources. I will list out some of them at the end of the post. This post has been broken down into a list of items, that a programmer needs to oversee while completing the Computer Graphics Project for fulfillment of practical requirement in the fifth semester in IOE Pulchowk Campus. Let's begin!

### 1. Finding / Writing a suitable Object file loader.

This was one of the major pains for me. An object file, denoted by extension .obj is a text file that gives information about the points, textures and normals in a 3D object created in a modeling software like Blender. However, I strongly recommend you to pull some 3D models from the internet, which can be found royalty free and free of cost at some sites like [turbosquid](http://www.turbosquid.com) or [Free3D](https://www.free3d.com). Making these models on your own is too much of a hassle and simply not worth it if you are trying to complete the project quickly. However, you might be scored based on if you have made the 3D model yourself or not. Confirm this with your teacher first. 

The next part is the reader for the obj file. You can find many implementations for the same, or even use libraries like __assimp__ or use header-only loaders available on github. (As a matter of fact, I have my own loader inside the github link posted below this page). BUT. I **strongly** recommend you to write your own. This will be a major learning part and to be honest, it is quite easy. It's as  easy as this:

``` c++
ifstream infile;
infile.open("objmodel.obj", std::ios::in);
std::string line;
while(getline(infile, line)){
    if(line[0] == 'v' && line[1] == 'n'){
        // This is a vertex normal.
        // Denoted in file as v <float> <float> <float>
        std::stringstream s;
        s << line;
        char ch;
        float x, y, z;
        s >> ch >> x >> y >> z;
    }
}
```

And you have the X Y Z components of a vertex normal! Simple as that! You might be wondering, there is not much of a learning curve to it. But trust me, writing your own loader gives you much more power over the data structures that are going to be used and you will rarely get confused about what to use in the later segments.  Another most important thing to remember is that, **always** triangulate your faces before exporting obj files. GPUs are designed to work on triangles, not quads. And doing this will reduce your pain of writing code for the face if it contains 4 or more vertices. Phew!

### 2. Writing Code. Dirty nitty gritty code.

Now that we have our obj file loaded and sitting nicely in one of our data structures, perhaps a `vector<struct facevertices> face` , or a `vector<struct vertex> vertices`, we can actually go ahead and contribute more to our project. What does this entail? Yes. Writing code. So go ahead and intitialize your OpenGL project. Draw a triangle, draw a cube and by then you are ready to go. While choosing a OpenGL library, always keep in mind to choose the one that gives you full control while also containing helpful functions to guide you along the way. Libraries rarely do much for the project, as all the calculations need to be carried out on your own. So, it is adequate to choose a library that gives you a window manager, and allows you to use openGL functions. For this purpose, I chose **glut**. In the landscape of myriad OpenGL libraries it is quite easy to get lost. Always remember the KISS principle ;) . My initialization function looked like the following:

```c++
glutInit(&argc, argv);
glutInitDisplayMode(GLUT_SINGLE | GLUT_RGBA);
glutInitWindowSize(600, 600);
glutCreateWindow("Learning Opengl");
glutDisplayFunc(render);
glutReshapeFunc(reshape);
glutMouseFunc(mouse);
glutKeyboardFunc(keyboard);
glutIdleFunc(idle);

config::shaderFunction = experimentalShader;
glutMainLoop();
```

You can tweak this to your own liking. And choosing libraries is rarely an important part for this project. Libraries will do nothing for you. You can even use matplotlib and whatnot, as this project requires you to just plot points. So don't worry too much about the libraries you are going to use. ( Fun fact: We had even done a project using linux framebuffers, that could write pixels directly to the screen. So OpenGL or no OpenGL, it really is not going to matter as long as you need to implement all the algorithms yourself. )

#### 2.1 Pain #1: Writing the Object Loader

I have already discussed on this topic in the above section, so not going to delve too deep here.

#### 2.2 Pain #2: Perspectives:

You have the vertices in a 3D coordinate system. Now, you need to convert those 3D vertices to 2D coordinates to be read on the screen. One simple way is to remove the Z part of the coordinate and plot whatever is in the file using something like ```glVertex3f(x, y, 0) ``` or if using a 2D coordinate system explicitly, ```glVertex2f(x, y)```. Choosing either of them is a matter of choice. 

However, your course requirement does not recommend you to do this. You NEED to demonstrate how different kinds of projections work. For an example, you can use Perspective projection, Isometric projection or Parallel projection. You have already studied these projections in the second part of your Engineering Drawing course. ( Yeah right. Here's where it is going to be used, if you were ever confused ). But do not panic. You won't require a drafter. Just some basic knowledge about these projections. I recommend Perspective projection as it is quite easy to implement and I did the same :P. You can use any other form of projection.

For converting the 3D coordinates to 2D coordinates on a projection plane, follow the following steps:

1. Find the coordinate to be plotted (x, y, z).
2. Choose a projection plane, e.g. z = 3, in the direction of +ve viewing axis, i.e. z axis. (You can use any axis. But the general choice is Z axis).
3. Choose a point for eye, let (a, b, c).
4. Find lines from point (x, y, z) to (a, b, c) and find the intersection of those lines with the projection plane (z = 3). This is general maths and if you need help here, I seriously recommend you to stop reading this post and start reading high school mathematics. Sarcasm intended.
5. Plot the points on the device at the specified plane.
6. For ease of use, you can shift the plane from z=3 to z=0. This means that you can simply use (x, y, 0) as the intersection coordinates. 

You might even be wondering. Why not simply choose z=0 as the perspective plane as opposed to choosing a perspective plane? The answer is zooming. Most of you will, at the end of the project, show scaling as zooming operation to the teacher, and they might even buy it. But they are completely different things. Zooming means coming closer to the object, which is accomplished by moving your eye closer to it. In case you chose to use z=0 as the projection plane, your vertices will stay stationary and will not give the impression of zooming, and gail bhais paani mein. 

#### 2.3 Pain #3: Plotting the faces

Now here's where your project is going to get interesting. You have a triangle. You have the vertices, and normals at those vertices. You also have the intensities at those vertices and you can plot those vertices onto the screen using. If you have followed this blog well upto this point, you can see something like this on the screen ( a screenshot from our project ):

![alt text](https://i.imgur.com/tp6A79S.png "Plot of Vertices")

Okay. Now you have the vertices. Now you need to plot a triangle between those vertices. (You can use either Gouraud shading or Phong shading to calculate illuminations at points). How would you go about doing this? You can use **linear Interpolation** for this same purpose. Linear interpolation has already been discussed in your course, and you can get more resources (including the source code) somewhere in a tutorial around [here](http://www-users.mat.uni.torun.pl/~wrona/3d_tutor/tri_fillers.html). I highly recommend you to study the code first than copy-pasting it into your project -_- 

Now that you have drawn a triangle, and interpolated the values at each of the vertices, you can go ahead and implement rotations, scaling and illumination models. These are basic things and you should not be consulting a blog for this purpose.

### 3. Bonus

An interesting thing I came across while completing this project was the use of Barycentric Coordinate system for the interpolation purpose ( I used barycentric interpolation in my project as opposed to linear interpolation, and it gave really good results. ) I will include the links for studying the same below

### 4. Promised Links:

1. [Learning OpenGL](https://www.learnopengl.com)
2. [OpenGL Documentation (important)](https://docs.gl)
3. Incoming more...

### 5. Project Requirements:

Your final project should contain the following things:

1. 3D model created using Blender or downloaded from the internet (duh!)
2. Obj file reader (code)
3. Projection model (perspective / parallel / isometric)
4. Triangle Interpolation (linear interpolation / barycentric interpolation)
5. Shading model (Phong / Gaoraud shading model)
6. Lighting ( Specular + Diffused + Ambient )
7. Visible Surface Detection ( Z Buffer / Scan Line method )
8. 3D tranformations (Rotation, Translation, Scaling)
9. Optional:
   1. Texture maps and painting

Remember that you are required to show the model of 1 3D object. You can choose more than 1. (We showed 3 models) However, this probably won't make a difference in your scores.

### 6. Conclusion

Project is completed. Phew! Now all that remains is the presentation and report. ( I will include a sample report below ) Remember to ask your teacher how the presentation is going to be conducted always remember, the teacher has every right to ask you questions about your project, including how your code works and theory portions. Enjoy!
