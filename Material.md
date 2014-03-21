#Adding Material

Neverball uses the standard OpenGL lighting model rather than a script-based shading system like Quake's. So, to add a new material you must create both a texture image and an OpenGL material specification. 

1. The texture must be an image in either JPG or PNG format. In general, use PNG for images that require an alpha channel, and JPG otherwise. Image width and height must both be powers of two. For this discussion, let us assume you have an image named wood.jpg. Place in it mtrl/. 

2. Create a material file in mtrl/, in this case mtrl/wood. This name must match the texture name, minus the image suffix. The file must contain 18 values, explained below. If you don't care about the material properties, the following values suffice in most any case.


    1.0 1.0 1.0 1.0
    1.0 1.0 1.0 1.0
    0.2 0.2 0.2 1.0
    0.0 0.0 0.0 1.0
    0.0
    1

Do a "Flush & Reload Shaders" in radiant and your new material should appear among the mtrl shaders.

Those 18 values define the material in terms of the standard OpenGL material model. They define not only the color of the material, but how it reacts to different types of lighting and different points of view. Their meaning is a follows: 

* Line 1: **diffuse material color (R G B A)**. This is the basic color of the material resulting from directional light falling upon it. The fourth component is the alpha channel. 0 is transparent, 1 is opaque. 

* Line 2: **ambient material color**. It gives the color of the material resulting from non-directional light. It doesn't make much of a contribution in practice. 

* Line 3: **specular material color**. This is the color of the material when directional light is glinting directly off of it into the eye of the user. A bright white here will make an object look shiny, while a darker color will give the object a matte appearance. 

* Line 4: **emissive material color**. This color contributes to the appearance of the object regardless of the light falling upon it. A bright color here will make an object appear to glow in darkness, or at least appear to be unaffected by external light. 

* Line 5: **material shininess**. Simply put, it defines the size of the specularity on an object. 0 will produce a dull finish, while 128 will produce a very tight specularity. 

* Line 6: **material type**. 1 is a normal opaque surface. 2 is a transparent or translucent surface. 4 is a reflective surface. 8 is an environment-mapped surface. 64 is a decal (1.5 or later only). Multiple material types may be ORed, though the results are unlikely to be useful (except merging 64 with 1). See the [Editing Recommendations](Editing.md) for more information on reflexive and transparent textures. 

The five of these together determine the base color of the material. This base color is then blended with (added to) the texture color.
