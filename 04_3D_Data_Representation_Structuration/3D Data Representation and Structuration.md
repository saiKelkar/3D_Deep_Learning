3D Data Representation -- point clouds, meshes, parametric models, voxels, and depth maps (various ways to describe and handle 3D geometric entities)

![[Pasted image 20250627132525.png]]

###### **Volumetric (Voxel) Models** 
- 3D analog of 2D pixels .
- A way to initially structure a 3D dataset that is unordered (like point clouds).
- You get an assembly of primitive blocks that can be easily linked together.
- Voxels only play with one type of base element (a cube).
- Many materials in our 3D universe may be approximated by extremely small voxels.
- If you combine enough density with suitable rendering methods, you can use voxels to replicate real-world objects that would be impossible to differentiate from the real thing - in appearance and behavior. 
- Voxel grid maps out the entire inside volume. It knows precisely what is solid matter, what is empty space, and what is inside the object from core to crust. 
- Because every little block has data about its space and density, it is much easier to simulate real-world physics - like how water flows around an object, how heat spreads through it, or how a structure breaks under pressure. 


| Advantages                                                                                               | Disadvantages                                                                                                                                            |
| -------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Voxels can depict geometries with a nice balance between structure and accuracy.                         | Building objects using voxels is not straightforward without using 3D scanning techniques.                                                               |
| Other modeling techniques would not be feasible without voxels, opening up new simulation methodologies. | Voxel modelling lacks the mathematical precision of other modelling methods, like B-representation.                                                      |
| Voxels are one of the quickest ways to model and visualize volumetric data.                              | We lack specialized technologies to generate high-resolution voxels efficiently, and our current computer hardware is optimized for generating polygons. |
```
vox_read = o3d.io.read_voxel_grid("../DATA/voxels.ply", format = 'auto')
o3d.viisualization.draw_geometries([vox_read])
```

###### High-Level 3D Data Representation
- Spatial graphs and 3D descriptors such as signed distance functions (SDFs)
- We can treat 3D data as a graph, with nodes as points and edges connecting nearby points. 
- Graph-based representations are helpful for showing complex relationship between points, especially dealing with noisy or irregular data. 

```
import networkx as nx
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D
import numpy as np

def create_3d_graph():
	# Create a random graph
	G = nx.erdos_renyi_graph(10, 0.3)
	
	# Generate 3D positions
	pos = {
		node: (np.random.uniform(0, 10), 
			   np.random.uniform(0, 10), 
			   np.random.uniform(0, 10)) for node in G.nodes()
	}
	
	# Create 3D plot
	fig = plt.figure(figsize=(10, 8))
	ax = fig.add_subplot(111, projection='3d')
	
	# Draw edges
	for edge in G.edges():
		x = [pos[edge[0]][0], pos[edge[1][0]]]
		y = [pos[edge[0]][1], pos[edge[1][1]]]
		z = [pos[edge[0]][2], pos[edge[1][2]]]
		ax.plot(x, y, z, color='gray', alpha=0.6)
	
	# Draw nodes	
	xs, ys, zs = zip(*[pos[node] for node n G.nodes()])
	ax.scatter(xs, ys, zs, c='red', s=100)
	
	ax.set_title('3D Network Graph')
	plt.show()

# Run the 3D graph visualization
create_3d_graph()
```

###### 3D Surface Models
- These models depict the object's surface or border rather than its volume. 
- Boundary representations (B-reps) are found in almost all visual models used in games, movies, and reality capture workflows. 
- Solid 3D models are used for simulations and are built with constructive solid geometry or voxel assemblies. 
- The main distinction between solid and surface model is the method to produce and modify them. 
- Three main strategies show geometry description through 3D modelling: constructive solid geometry, parametric modelling (including B-reps), and 3D meshes. 

**3D meshes:**
- Mesh is a geometric data structure that enables a collection of polygons to represent surface subdivisions. 
- Mesh is how computers translate a smooth, mathematical shape into a digital object made of connected triangles that they can actually calculate, store, and display. 
- Triangle meshing - when all the faces are triangles. (most common in 3D workflows)
- Quadrilateral meshes - often obtained through mesh optimization techniques to get more compact representations. 
- Meshes -> based on boundary representation -> dependent upon wireframe model
- The topology (elemental arrangement) and geometry make up most of the boundary representation of 3D objects (surfaces, curves, and points). Faces, edges, and vertices are the primary topological elements. ![[Screenshot_2026-10-04_09-52-36.png]]


| Operations                                                                                                                                                                                                  | Benefits                              | Disadvantages                   |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------- | ------------------------------- |
| Transformations: All points are transformed with the wireframe model (multiply the points in the point list with linear matrices). In addition, the surface equations or normal vectors can be transformed. | Well-adopted representation           | High memory requirements        |
| Combinations: Objects can be combined by grouping point lists and edges; operations on polygons (divide based on intersections, remove the redundant polygons, and combine them).                           | Model generation via new-gen scanning | Expensive combinations          |
| Rendering: Hidden surface or line algorithms can be used because the surfaces of the objects are known so that visibility can be calculated.                                                                | Transformations are quick and easy    | Curved objects are approximated |
```
import open3d as o3d

mesh = o3d.io.read_triangle_mesh("../DATA/mesh_terrain.ply")
mesh.compute_vertex_normals()

o3d.visualization.draw_geometries([mesh])
```

| File format  | Definition                              | Python library of choice     | Notes                                                                                                                                                               |
| ------------ | --------------------------------------- | ---------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| .stl         | Stereolithography                       | meshio, open3d               | Widely used in 3D printing. Simple, represents surfaces as triangles. Binary version is more compact. ASCII is easier for debugging.                                |
| .obj         | Wavefront OBJ                           | pywavefront, trimesh, open3d | Common in computer graphics. Supports materials, textures, and vertex normals. Often accompanied by .mtl (material library) files                                   |
| .ply         | Polygon File Format                     | plyfile, trimesh, open3d     | Versatile format that can also represent point clouds. Handles various data attributes.                                                                             |
| .glb / .gltf | glTF (GL Transmission Format)           | pygltf, trimesh, open3d      | Modern format optimized for web and real time applications. .glb is a single binary file, while .gltf uses separate JSON and binary files.                          |
| .fbx         | Autodesk FBX                            | Blender                      | Used in many 3D modeling and animation software packages. Handles complex scenes, animations, and rigging information. Often requires dealing with proprietary SDKs |
| .off         | Object File Format                      | trimesh, open3d              | A simple format primarily used in academic and research settings. Represents meshes as lists of vertices, faces, and edges.                                         |
| .dae         | COLLADA (COLLAborative Design Activity) | pycollada, cloudcompare      | Designed for data exchange between different 3D applications. Supports complex scenes and animations.                                                               |
**Parametric models (e.g. B-reps):**
- Parametric - Instead of drawing a fixed wall, you define it by parameters - height, length, thickness, and material. If you want a longer wall, you don't redraw it; you just change the length parameters from 3m to 5m, and model updates instantly. 
- It defines an entire type of object (e.g., a standard interior door that can vary in width) rather than just one frozen 3d mesh instance. 
- Components talk to each other. If you move a wall, attached windows, doors, and adjacent rooms automatically adjust to stay connected. This is what makes software like Revit or parametric design tools so powerful for real-world construction. 
- When you capture a building using a 3D scanner (point clouds), you just get a massive, messy cloud of millions of unorganized dots. Turning these into clean, smart parametric models requires extremely intelligent algorithms and parsing. This is one of the biggest bottlenecks in automated Scan-to-BIM research. 
- While Python is fantastic for handling point clouds and deep learning, writing code from scratch to handle complex parametric B-rep geometry is notoriously difficult. Because of this, developers usually have to rely on external CAD software or specialized geometry kernels rather than pure Python scripts. 

| File format | Definition       | Python library of choice     | Notes                                                                                                   |
| ----------- | ---------------- | ---------------------------- | ------------------------------------------------------------------------------------------------------- |
| .step/.stp  | STEP file format | FreeCAD, OCPNet              | ISO standard for exchanging product model data. Excellent for interoperability between CAD systems.     |
| .iges/.igs  | IGES file format | FreeCAD, pythonOCC           | An older but still widely used standard. Can be complex to parse due to its verbose nature.             |
| .stl        | STL file format  | open3d, meshio               | Primarily used for 3D printing and rapid prototyping. Represents surfaces as a collection of triangles. |
| .obj        | Wavefront OBJ    | pywavefront, trimesh, open3d | Common in computer graphics. Can represent both mesh and parametric surfaces.                           |
