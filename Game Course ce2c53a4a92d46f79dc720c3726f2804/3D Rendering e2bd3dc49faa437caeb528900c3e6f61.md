# 3D Rendering

We have Inverse-Mapping and Forward-Mapping (OpenGL, DirectX) approach for computer graphics rendering.

## Approaches to Graphic Rendering

### Ray-Tracing

It says that the camera or the eye is going to see the pixels and sends waves (Inverse-Mapping).

- Reflection: Into different environments like water.
- Refraction: The shadows on the edges.
- Diffraction

It’s really expensive.

### Pipeline approach

Actually the light is going to be received from the objects to the camera or eye (Forward-Mapping). OpenGL & DirectX.

## Graphic Pipeline

The start is from a geometric primitive (Like cube). 

The pipeline inputs are: 3D Scene (Geometric models), Camera parameters, Viewport model, and light sources.

The output of this pipeline is going to be Colors suitable for the frame buffer display.

> Graphic Pipeline is bunch of processing steps to display a computer graphic and the order in which they must occurred.
> 

Some of the are going to be executed on hardware and some of them in software.

### 1. Modeling Transformation

3D Models are defined in their own coordinate system. It’s the transformation from object space to world space.

### 2. Lighting

Fix the lightning in our world use some famous algorithms like phong shading to apply the lightning, and It’s based on the material of the objects.

Direct illumination:  The simple lightning mechanism.

Global illumination:  The object also reflect the lights based on their material.

### 3. Viewing Transformation

It will maps the world space into eye or camera. It should consider how much they are based on the distance of these objects from the camera. 

### 4. Projection Transformation

It specifies which layer you should see in the camera (perspective or orthographic).

### 5. Clipping

There is no need to see the  parts of objects that we don’t need to see. This just increase our processing for nothing. Everything that is important is what we see in the screen.

The input is a polygon and the output could be new polygons or nothing.

### 6. Viewport Transformations

It transforms the normalized device coordinates to the 3D viewport

### 7. Rasterization

It interpolates values to the screen. the color, depth, and...

### Back-Face Culling

It’s an optimization to only process the visible objects in the scene and avoids rendering the back-face (back of the camera cone). Actually, It depends on the object’s material again.

### Occlusion

It’s for the scenario in which bunch of object are on each other and we don’t need to see the back objects.