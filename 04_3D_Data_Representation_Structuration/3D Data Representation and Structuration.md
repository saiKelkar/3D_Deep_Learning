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

3D meshes:
- Mesh is a geometric data structure that enables a collection of polygons to represent surface subdivisions. 
- Mesh is how computers translate a smooth, mathematical shape into a digital object made of connected triangles that they can actually calculate, store, and display. 
- Triangle meshing - when all the faces are triangles. (most common in 3D workflows)
- Quadrilateral meshes - often obtained through mesh optimization techniques to get more compact representations. 
- Meshes -> based on boundary representation -> dependent upon wireframe model
- The topology (elemental arrangement) and geometry make up most of the boundary representation of 3D objects (surfaces, curves, and points). Faces, edges, and vertices are the primary topological elements. ![[Screenshot_2026-10-04_09-52-36.png]]