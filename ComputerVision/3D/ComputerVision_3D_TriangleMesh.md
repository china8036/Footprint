- [计算机视觉 - 三维模型：三角网格](#计算机视觉---三维模型三角网格)
	- [1. 基本概念](#1-基本概念)
	- [2. 基本信息](#2-基本信息)
	- [3. 额外信息](#3-额外信息)
		- [3.1. 法向量](#31-法向量)
		- [3.2. 邻接面](#32-邻接面)
		- [3.3. 点法向](#33-点法向)
		- [3.4. 邻接点](#34-邻接点)
	- [4. 表示方法](#4-表示方法)
		- [4.1. Face-vertex meshes](#41-face-vertex-meshes)
			- [4.1.1. 指针表示](#411-指针表示)
			- [4.1.2. 索引表示](#412-索引表示)
			- [4.1.3. 比较](#413-比较)
		- [4.2. Winged-edge meshes （翼边数据结构）](#42-winged-edge-meshes-翼边数据结构)
		- [4.3. Half-edge meshes （半边数据结构）](#43-half-edge-meshes-半边数据结构)
		- [4.4. Quad-edge meshes](#44-quad-edge-meshes)
		- [4.5. Corner-tables meshes](#45-corner-tables-meshes)
		- [4.6. Vertex-vertex meshes](#46-vertex-vertex-meshes)
		- [4.7. Render dynamic meshes](#47-render-dynamic-meshes)
	- [5. 其它操作](#5-其它操作)
		- [5.1. 拉普拉斯平滑](#51-拉普拉斯平滑)
		- [5.2. 拉普拉斯矩阵](#52-拉普拉斯矩阵)
			- [5.2.1. 简单拉普拉斯](#521-简单拉普拉斯)
			- [5.2.2. 余切拉普拉斯 (Cotangent Laplacian)](#522-余切拉普拉斯-cotangent-laplacian)
	- [6. Refer Links](#6-refer-links)

# 计算机视觉 - 三维模型：三角网格

## 1. 基本概念

网格 (Mesh) 是一个点（Vertex）、法向量（Normal Vector）和面（Face）的集合，它定义了一个 3D 物体的形状。现实世界中的物体表面直观上看都是由曲面构成的，而在计算机世界中，由于只能用离散的结构去模拟现实中连续的事物，所以现实世界中的曲面实际上在计算机里是由无数个小的多边形面片去组成的。计算机内部使用了大量的小三角形片去组成了这样的形状，但渲染后由肉眼看是十分平滑的曲面。**这些小面片的集合就被称作网格 (Mesh)**。Mesh 既可以由三角形组成，也可以由其他平面形状如四边形、五边形等组成。

- **[多边形网格 (Polygon Mesh)](https://en.wikipedia.org/wiki/Polygon_mesh)**

	多边形网格是三维计算机图形学中表示多面体形状的顶点与多边形的集合，它也叫作非结构网格。

- **[三角网格 (Triangle Mesh)](https://zh.wikipedia.org/wiki/%E4%B8%89%E8%A7%92%E7%B6%B2%E6%A0%BC)**

	三角网格是多边形网格的一种，是计算机图形学中用于为各种不规则物体建立模型的一种数据结构。**由于平面多边形实际上也能再细分成三角形，因此使用全由三角形组成的三角网格（Triangle Mesh）来表示物体表面也是具有一般性的**。任意多边形网格都能转换成三角网格，三角网格以其简单性而吸引人，相对于一般多边形网格，许多操作对三角网格更容易。

e.g.

![image](http://img.cdn.firejq.com/jpg/2019/3/15/177ac5a1ec7499eb37b06725aa2adc7d.jpg)

实际上，使用无数三角形面片来组成物体，现实世界存在的实物都能以这样的方式建模。在计算机内存中一旦有了一个物体的 Mesh，即意味着计算机有能力渲染显示这样的物体。具体的渲染算法，以及渲染着色方式等问题，可以参考相关的资料（OPENGL 的知识），都有详细的叙述。

TODO: 拓扑信息：点、线、面之间的联系信息

## 2. 基本信息

> Objects created with polygon meshes must store different types of elements. These include vertices, edges, faces, polygons and surfaces. In many applications, only vertices, edges and either faces or polygons are stored. A renderer may support only 3-sided faces, so polygons must be constructed of many of these, as shown above. However, many renderers either support quads and higher-sided polygons, or are able to convert polygons to triangles on the fly, making it unnecessary to store a mesh in a triangulated form.

三角网格需要至少存储三类信息：
- 顶点。每个三角形都有三个顶点，各顶点都有可能和其他三角形共享。
- 边。连接两个顶点的边，每个三角形有三条边。
- 面。每个三角形对应一个面，我们可以用顶点或边列表表示面。

## 3. 额外信息

### 3.1. 法向量

面法向量是 Mesh 中每一个三角形的法向量的集合，要得出这个集合，就应当为这个 Mesh 的每一个三角形计算法向量。

![image](http://img.cdn.firejq.com/jpg/2019/3/18/bf2e6b6504f2e85e0fcfcb26c27be91e.jpg)

根据几何上对向量叉积的定义，面法向量可以使用三角形边向量的叉积计算：

![image](http://img.cdn.firejq.com/jpg/2019/3/18/6049e181e574acb28878c8ea99593051.jpg)

NOTE:

在立体几何中，一个面的法向量是方向垂直于这个面的向量（注意这里的向量都是指单位向量）。也就是说，对于一个平面的法向量其实有两个方向，既可以指向上面也可以指向下面。但是对于 Mesh 中的三角形，法向量的方向需要符合右手法则的方向，即对于一个按 P0、P1、P2 顺序存储的三角形，它的法向量方向就是用右手按 P0、P1、P2 的方向握过去，大拇指所指的方向。

![image](http://img.cdn.firejq.com/jpg/2019/3/18/0c64b487ddd768ba8f6aa17a6c933cb9.jpg)

区分法向量的方向是为了计算机图形的渲染，在渲染的时候，一般视法向量正对的方向为三角形的正面，而另一个相反的方向则是背面，在单侧光进行照射的时候，正面会根据材质进行适当的反光而显得明亮，而背面则暗淡一些。

代码实现：
```cpp
struct Vector
{
public:
    float X;
    float Y;
    float Z;
    Vector()
    {
        X = 0;
        Y = 0;
        Z = 0;
    }
    ~Vector()
    {
    }
    Vector(float x, float y, float z)
    {
        this->X = x;
        this->Y = y;
        this->Z = z;
    }
};

Vector CaculateTriangleNormal(Point3d& p0, Point3d& p1, Point3d& p2)
{
    Vector Normal;
    float v1x = p1.X - p0.X;
    float v1y = p1.Y - p0.Y;
    float v1z = p1.Z - p0.Z;
    float v2x = p2.X - p1.X;
    float v2y = p2.Y - p1.Y;
    float v2z = p2.Z - p1.Z;
    Normal.X= v1y * v2z - v1z * v2y;
    Normal.Y = v1z * v2x - v1x * v2z;
    Normal.Z = v1x * v2y - v1y * v2x;
    float len = (float)sqrt(Normal.X * Normal.X + Normal.Y * Normal.Y + Normal.Z * Normal.Z);
    if (len == 0)
    {
        //throw Exception();
    }
    else
    {
        Normal.X /= len;
        Normal.Y /= len;
        Normal.Z /= len;
    }
    return Normal;
}

void CaculateFaceNormals()
{
    FaceNormals.reserve(Faces.size());
    for(size_t i=0;i<Faces.size();i++)
    {
        Point3d& p0=Vertices[Faces[i].P0Index];
        Point3d& p1=Vertices[Faces[i].P1Index];
        Point3d& p2=Vertices[Faces[i].P2Index];
        Vector v=CaculateTriangleNormal(p0,p1,p2);
        FaceNormals.push_back(v);
    } // 对每一个三角形计算面法向并存至 FaceNormals
} // 	计算面法向量
```

### 3.2. 邻接面

邻接面 / 邻接三角形，指的是对于 Mesh 中的任意一个点，包含这个点的三角形的集合。

e.g. 对于点 V 而言，它的邻接面就是被标记为红色的那些三角形：

![image](http://img.cdn.firejq.com/jpg/2019/3/18/d508fd3e6d4e34284cd0ac0a42b41c90.jpg)

每一个顶点都有自己的邻接三角形集合，因此用 `std::vector<int>` 来存储这些三角形的索引，添加 Mesh 的成员 `AdjacentFacesPerVertex`，类型为 `std::vector<std::vector<int>*>`，用来存储所有点对应的邻接三角形索引集组成的集合。

那么如何从 Mesh 中求出所有点的邻接面呢？
- 一个简单的思路就是对于给定 Mesh 中的点 P，既然已知 P 在点集中的索引 i，就遍历一遍三角形集合，凡是三角形的 P0Index、P1Index、P2Index 中有一个等于 i 则这个三角形就在 P 的邻接面集合里面。如果按这个思路，对于规模比较大的 Mesh，要求出所有点的邻接面，就需要执行 `顶点数 × 三角形数 × 3` 的比较。这对于时间资源紧张的应用是不可接受的。
- 另一个思路是将问题反过来解决：遍历三角形集合，将这个三角形的索引直接添加到这个三角形的三个点的邻接面集合中去。这样只需要执行 `三角形数×3` 的操作，从数量级上减少了操作，一般计算邻接面的实现都选择此方案。

代码实现：
```cpp
void CaculateAdjacentFacesPerVertex()
{
    AdjacentFacesPerVertex.reserve(Vertices.size());
    for (size_t i = 0; i < Vertices.size(); i++)
    {
        std::vector<int>* list=new std::vector<int>();
        list->reserve(4);// 预先假设每个点大概有 4 个邻接面
        AdjacentFacesPerVertex.push_back(list);
    } // 首先分配好存储空间
    for (size_t i = 0; i < Faces.size(); i++)
    {
        Triangle& t = Faces[i];
        std::vector<int> *t0list= AdjacentFacesPerVertex[t.P0Index];
        std::vector<int> *t1list= AdjacentFacesPerVertex[t.P1Index];
        std::vector<int> *t2list= AdjacentFacesPerVertex[t.P2Index];
        t0list->push_back(i);
        t1list->push_back(i);
        t2list->push_back(i);
    } // 遍历三角形集合，使用三角形信息补充邻接面表
} // 计算邻接面
```

### 3.3. 点法向

在计算机图形学中，点法向是用于渲染图形的很重要的量，点法向是 Mesh 上一个点 P 的法向量为其邻接三角形法向量的均值。在求点法向之前，要计算面法向和邻接面。

![image](http://img.cdn.firejq.com/jpg/2019/3/18/7c4f86f92268cacac87269abb0fd3379.jpg)

点法向能较好贴合穿过该点处曲面的法线，因而采用点法向渲染的图像显得更为平滑。

代码实现：
```cpp
void CaculateVertexNormals()
{
    if(FaceNormals.size()==0)
        CaculateFaceNormals(); // 计算点法向前需计算完面法向
    if(AdjacentFacesPerVertex.size()==0)
        CaculateAdjacentFacesPerVertex(); // 计算点法向前也必须计算完邻接面
    VertexNormals.reserve(Vertices.size());
    for (size_t i = 0; i < Vertices.size(); i++)
    {
        float sumx = 0;
        float sumy = 0;
        float sumz = 0;
        std::vector<int>& tlist = *(AdjacentFacesPerVertex[i]);
        if(tlist.size()!=0)
        {
            for (size_t j = 0; i < tlist.size(); j++)
            {
                sumx += FaceNormals[tlist[j]].X;
                sumy += FaceNormals[tlist[j]].Y;
                sumz += FaceNormals[tlist[j]].Z;
            }
            VertexNormals.push_back(Vector(sumx / tlist.size(), sumy / tlist.size(), sumz /tlist.size())); // 求邻接面发向均值
        }
        else
        {
            VertexNormals.push_back(Vector(0,0,0));// 若无邻接面，则为孤立点，使用默认值
        }
    }
} // 计算点法向
```

### 3.4. 邻接点

邻接点指的是，对于一个 Mesh 上的点 V，其邻接点就是和他有一个三角形边相连的其他点。

e.g. 在下图用红点标出了点 V 的邻接点：

![image](http://img.cdn.firejq.com/jpg/2019/3/18/677c7d33918b763d75e028514c70c43f.jpg)

类似邻接面的计算，邻接点的计算同样使用了逆向的思维：只遍历一次三角形表，对于每个三角形，P0 的邻接点表添加 P1、P2 的索引；P1 的邻接点表添加 P0、P2 的索引；P2 的邻接点表添加 P1，P0 的索引。当然，由于同时有 Pi 和 Pj 两个点的三角形往往不止一个，所以在添加的时候注意检测该点的索引是否已经被加进去了。

代码实现：
```cpp
void CaculateAdjacentVerticesPerVertex()
{
    AdjacentVerticesPerVertex.reserve(Vertices.size());
    for (size_t i = 0; i < Vertices.size(); i++)
    {
        std::vector<int>* list=new std::vector<int>();
        list->reserve(4);// 预先假设每个点大概有 4 个邻接点
        AdjacentVerticesPerVertex.push_back(list);
    }// 首先分配好存储空间
    for (size_t i = 0; i < Faces.size(); i++)
    {
        Triangle &t = Faces[i];
        std::vector<int> *p0list= AdjacentVerticesPerVertex[t.P0Index];
        std::vector<int> *p1list= AdjacentVerticesPerVertex[t.P1Index];
        std::vector<int> *p2list= AdjacentVerticesPerVertex[t.P2Index];

        if (std::find(p0list->begin(), p0list->end(), t.P1Index)==p0list->end())
            p0list->push_back(t.P1Index);
        if (std::find(p0list->begin(), p0list->end(), t.P2Index)==p0list->end())
            p0list->push_back(t.P2Index);

        if (std::find(p1list->begin(), p1list->end(), t.P0Index)==p1list->end())
            p1list->push_back(t.P0Index);
        if (std::find(p1list->begin(), p1list->end(), t.P2Index)==p1list->end())
            p1list->push_back(t.P2Index);

        if (std::find(p2list->begin(), p2list->end(), t.P0Index)==p2list->end())
            p2list->push_back(t.P0Index);
        if (std::find(p2list->begin(), p2list->end(), t.P1Index)==p2list->end())
            p2list->push_back(t.P1Index);
    }// 遍历三角形集合，使用三角形信息补充邻接面表
} // 计算邻接点
```

## 4. 表示方法

Polygon meshes may be represented in a variety of ways, using different methods to store the vertex, edge and face data. These include:

### 4.1. Face-vertex meshes

A simple list of vertices, and a set of polygons that point to the vertices it uses.

在这种方法中，为了表示 Mesh，首先需要定义点和三角形的数据结构，而 Mesh 则应该表示为这些结构的集合。

根据存储顶点方式的不同，Mesh 的表示方法可分为指针表示和索引表示：

#### 4.1.1. 指针表示

```cpp
// 首先是点 Point3d 结构，表示三维空间的一个点，具有 float 类型的 XYZ 坐标
struct Point3d
{
public:
float X;
　float Y;
　float Z;
Point3d(float x, float y, float z)
　{
　　　this->X=x;
　　　this->Y=y;
　　　this->Z=z;
　}
};
// 三角形结构按常理理解应该是包含了组成其顶点的 3 个 Point3d 的信息
struct Triangle
{
public :
	Point3d* P0;
	Point3d* P1;
	Point3d* P2;
	Triangle(Point3d* p0, Point3d* p1, Point3d* p2)
　 {
　　　this->P0=p0;
　　　this->P1=p1;
　　　this->P2=p2;
　 }
};
class Mesh
{
public:
	std::vector<Point3d*> Vertices;
	std::vector<Triangle*> Faces;
	Mesh();
	~Mesh();
	void AddVertex(Point3d* toAdd);
	void AddFace(Triangle* tri);
};
```

#### 4.1.2. 索引表示

```cpp
// 首先是点 Point3d 结构，表示三维空间的一个点，具有 float 类型的 XYZ 坐标
struct Point3d
{
public:
	float X;
	float Y;
	float Z;
	Point3d()
	{
			X = 0;
			Y = 0;
			Z = 0;
	}
	~Point3d()
	{
	}
	Point3d(float x, float y, float z)
	{
			this->X = x;
			this->Y = y;
			this->Z = z;
	}
};
// 三角形结构按常理理解应该是包含了组成其顶点的 3 个 Point3d 的信息
struct Triangle
{
public :
	int P0Index;
	int P1Index;
	int P2Index;
	Triangle(int p0index, int p1index, int p2index)
	{
			this->P0Index=p0index;
			this->P1Index=p1index;
			this->P2Index=p2index;
	}
	Triangle()
	{
			P0Index=-1;
			P1Index=-1;
			P2Index=-1;
	}
};

class Mesh
{
public:
	std::vector<Point3d> Vertices;
	std::vector<Triangle> Faces;
	int AddVertex(Point3d& toAdd)
	{
			int index = Vertices.size();
			Vertices.push_back(toAdd);
			return index;
	}
	int AddFace(Triangle& tri)
	{
			int index = Faces.size();
			Faces.push_back(tri);
			return index;
	}
};
```

NOTE：
- 索引定义方式只有针对 Mesh 中的三角片才有意义，因为这个索引表示这个三角形的顶点在 Mesh 的点集数组中的位置。

#### 4.1.3. 比较

- 比较推荐使用索引表示的 Mesh 结构。

	采用点地址实际上比采用索引要占更多的空间。因为在对于一个有效的 Point3d 地址来说，它必然位于堆上，而不能使用栈上的地址或者 vector 中的地址（使用 vector 中的地址必须保证 vector 不因为添加新点而自动执行内部扩容操作，实际应用中这点一般不能保证），因而基于点地址的 Triangle 结构实际上多存了指针的空间。而若采用索引，则占用的最小空间等于点集的空间加索引的空间，所以一般推荐采用索引表示的三角形来构成 Mesh 中的三角形集合。

- Mesh 除了点集和三角形集合外，根据具体的应用还可以有很多附加的信息。

	比如三角形结构可以增加成员变量存储法向量，点结构也可以增加成员存放点法向量。当然这些结构也可以不存在相应的三角形和点中，可以放在 Mesh 中以数组形式来存，位置和三角形集合和点集合分别一一对应。

### 4.2. Winged-edge meshes （翼边数据结构）

in which each edge points to two vertices, two faces, and the four (clockwise and counterclockwise) edges that touch them. Winged-edge meshes allow constant time traversal of the surface, but with higher storage requirements.

翼边结构 (Winged-edge structure) 是由美国 Stanford 大学的 B. G. Baumgar t 提出的。它的基本出发点是以边为核心，每条边上有上下两个顶点，左右两个邻面以及和顶点相连的四条边，这些边分别在两个邻面的边构成的环上，从而建立起边与顶点、边与边、边与面的关系。

翼边结构的特点是数据结构有固定数目和长度的数据域，可以从一条已知边出发，有规律地找到这个几何体的所有面、边和顶点。

但由于在翼边结构中与边相邻的环有两个，且翼边结构没有明确边的正向，因此要确定当前边所在的环与面较困难。

### 4.3. Half-edge meshes （半边数据结构）

Similar to winged-edge meshes except that only half the edge traversal information is used. (see OpenMesh)

最质朴的顶点 + 索引方式存储网格数据不能实现高效地完成一些功能的需求，例如：访问某个点的所有相邻点、访问某条边的所有邻边、访问包含某个点的所有面，而这些需求往往是一些图形算法的基础，所以很有必要寻求一种更加优秀的数据结构来实现功能。

- 基本概念

	所谓的半边结构就是将每一条边拆成两条有方向的半边，如图所示：

	![image](http://img.cdn.firejq.com/jpg/2019/3/18/ef316f4741e5dd28b2d9bd9761b67ac4.jpg)

	半边结构是网格数据结构的一种表达方式，它是一个有向图，把一条边表达为两个有向半边，如下图所示。

	![image](http://img.cdn.firejq.com/jpg/2019/3/18/5c3216016cd0f5d66730f69260b78970.jpg)

- 优缺点

	- 优点：对于网格信息的查询速度较其他的数据结构而言快很多，可以方便、快速地获得以下信息：
  	- 哪些面使用了这个顶点
		- 哪些边使用了这个顶点
		- 哪些面使用了这条边
		- 哪些边构成了这个面
		- 哪些面和这个面相邻
		如果用最简单的顶点 + 索引的方式存储网格，问题 1，2，3，5 都是很难解决的，需要遍历所有顶点。

	- 缺点：
  	- 网格连接关系变动后，需要维护的信息也比较多。
  	- 半边结构表达的网格需要是流形结构，半边结构的构造也需要一定的时间开销。

- 基本元素

	半边数据结构的基本元素有顶点、面和半边（有向边）：

	![image](http://img.cdn.firejq.com/jpg/2019/3/19/dd198b52aa971575cfa59e5d9bd298a2.jpg)

	- 每一个顶点存储一个外向半边（即该点为半边的起点）
	- 每一个面存储一个边界半边
	- 每一个半边包含以下指针（句柄）
		- 终点
		- 属于的面
		- 所在面的下一条半边（逆时针方向）
		- 相反的半边
		- （可选）所在面的上一条半边

- 代码实现：
	```cpp
	struct HE_Vertex {
		HE_Edge * v_edge; // 以该顶点为首顶点的出边（任意一条)
		HE_Face * v_face; // 包含该顶点的面（任意一张)
		float x, y, z;
	};

	struct HE_Edge {
		HE_Edge * e_pair;		// 对偶边
		HE_Edge * e_succ;		// 后继边
		HE_Vertex * e_vert; // 首顶点
		HE_Face * e_face;		// 右侧面
	};

	struct HE_Face {
		std::vector<HE_Vertex*> f_verts;
		HE_Edge * f_edge;
	};

	// 完全基于 .obj 的模型数据
	void HE_Mesh::LoadFromObj( const char * file )
	{
		ifstream fs;
		fs.open(file);
		while (!fs.eof())
		{
			char line[100];
			fs.getline(line, 100);
			if (line[0] == 'v' && line[1] == ' ')
			{
				char * str = strtok(line, " ");
				str = strtok(NULL," ");
				float x = atof(str);
				str = strtok(NULL , " ");
				float y = atof(str);
				str = strtok(NULL, " ");
				float z = atof(str);
				InsertVertex(x, y, z);
			}
			if (line[0] == 'f' && line[1] == ' ')
			{
				char * str = strtok(line, " ");
				verts fverts;
				char * v1s = strtok(NULL, " ");
				char * v2s = strtok(NULL, " ");
				char * v3s = strtok(NULL, " ");
				int v1 = atoi(v1s);
				int v2 = atoi(v2s);
				int v3 = atoi(v3s);
				fverts.push_back(m_verts[v1 - 1]);
				fverts.push_back(m_verts[v2 - 1]);
				fverts.push_back(m_verts[v3 - 1]);
				InsertFace(fverts);
			}
		}
		fs.close();
	}
	HE_Vertex* HE_Mesh::InsertVertex(float x, float y, float z)
	{
		HE_Vertex * vert = new HE_Vertex();
		vert->x = x;
		vert->y = y;
		vert->z = z;
		m_verts.push_back(vert);
		return vert;
	}

	```

### 4.4. Quad-edge meshes

which store edges, half-edges, and vertices without any reference to polygons. The polygons are implicit in the representation, and may be found by traversing the structure. Memory requirements are similar to half-edge meshes.

### 4.5. Corner-tables meshes

which store vertices in a predefined table, such that traversing the table implicitly defines polygons. This is in essence the triangle fan used in hardware graphics rendering. The representation is more compact, and more efficient to retrieve polygons, but operations to change polygons are slow. Furthermore, corner-tables do not represent meshes completely. Multiple corner-tables (triangle fans) are needed to represent most meshes.

### 4.6. Vertex-vertex meshes

A "VV" mesh represents only vertices, which point to other vertices. Both the edge and face information is implicit in the representation. However, the simplicity of the representation does not allow for many efficient operations to be performed on meshes.

Each of the representations above have particular advantages and drawbacks, further discussed in Smith (2006).[1] The choice of the data structure is governed by the application, the performance required, size of the data, and the operations to be performed. For example, it is easier to deal with triangles than general polygons, especially in computational geometry. For certain operations it is necessary to have a fast access to topological information such as edges or neighboring faces; this requires more complex structures such as the winged-edge representation. For hardware rendering, compact, simple structures are needed; thus the corner-table (triangle fan) is commonly incorporated into low-level rendering APIs such as DirectX and OpenGL.

### 4.7. Render dynamic meshes

Winged-edge meshes are not the only representation which allows for dynamic changes to geometry. A new representation which combines winged-edge meshes and face-vertex meshes is the render dynamic mesh, which explicitly stores both, the vertices of a face and faces of a vertex (like FV meshes), and the faces and vertices of an edge (like winged-edge).

Render dynamic meshes require slightly less storage space than standard winged-edge meshes, and can be directly rendered by graphics hardware since the face list contains an index of vertices. In addition, traversal from vertex to face is explicit (constant time), as is from face to vertex. RD meshes do not require the four outgoing edges since these can be found by traversing from edge to face, then face to neighboring edge.

RD meshes benefit from the features of winged-edge meshes by allowing for geometry to be dynamically updated.

## 5. 其它操作

### 5.1. 拉普拉斯平滑

网格的平滑目的是使原本形状粗糙的网格表面变得更加光滑。在 Mesh 平滑前后，只是顶点的位置发生改变，顶点之间的连接情况是不变的。平滑操作是可以反复迭代的，每迭代一次都是对所有的点计算一次位置，在迭代的过程中，Mesh 也会越来越平滑。

e.g. 左边模型经过平滑之后变成了右边较为平滑的模型：

![image](http://img.cdn.firejq.com/jpg/2019/3/18/af2df76328e7ae48f076d0337e96ffd0.jpg)

拉普拉斯平滑是网格平滑算法中最简单高效的一个，其实是对邻接点的一个简单应用。

拉普拉斯平滑的原理是将 Mesh 的每个顶点移到他邻接顶点的平均位置，所以此算法执行前首先要计算完邻接点。要注意不能在一次循环中计算完某个点的平均位置后立刻修改该点的位置，因为这样会影响到后面顶点计算平均位置；因此算法采用和原点集相同大小的辅助空间来存放每个点将移到的新位置，所有点的新位置计算完成后再将这些位置的值覆盖修改原对应点的值。其原理用图表示如下：

![image](http://img.cdn.firejq.com/jpg/2019/3/18/eab439053acf6923f664bf60c22e1181.jpg)

![image](http://img.cdn.firejq.com/jpg/2019/3/18/bc9a77450711a94d0aa754ed0e3f2f26.jpg)

代码实现：
```cpp
void LaplacianSmooth(int time)
{
    if(AdjacentVerticesPerVertex.size()==0)
        CaculateAdjacentVerticesPerVertex(); // 平滑前需要计算邻接点
    Point3d* tempPos=new Point3d[Vertices.size()]; // 辅助空间存储每次平滑后的点将要挪到的位置
    for(int k=0;k<time;k++)
    {
        for (size_t i = 0; i < Vertices.size(); i++)
        {
            float xav = 0;
            float yav = 0;
            float zav = 0;
            std::vector<int>& adjlist =*(AdjacentVerticesPerVertex[i]);
            size_t adjcount=adjlist.size();
            if(adjcount==0)
                continue;
            for (size_t j = 0; j < adjcount; j++)
            {
                xav += Vertices[adjlist[j]].X;
                yav += Vertices[adjlist[j]].Y;
                zav += Vertices[adjlist[j]].Z;
            }
            xav /= adjcount;
            yav /= adjcount;
            zav /= adjcount;
            tempPos[i].X = xav;
            tempPos[i].Y = yav;
            tempPos[i].Z = zav;
        } // 对所有点计算邻接点平均位置并存到辅助空间
        for (size_t i = 0; i < Vertices.size(); i++)
        {
            Vertices[i].X = tempPos[i].X;
            Vertices[i].Y = tempPos[i].Y;
            Vertices[i].Z = tempPos[i].Z;
        }// 将计算出的新点位置覆盖原来点的位置
    } // 每次循环意味着一次平滑
    delete[] tempPos;
}
```

### 5.2. 拉普拉斯矩阵

[三角网格的拉普拉斯矩阵 (Laplacian Matrix) 的并行计算](https://www.licc.vip/article.aspx?id=39)

在网格参数化中，拉普拉斯矩阵是一个出现频率很高的字眼，相当一部分的参数化方法都直接或间接地与拉普拉斯矩阵有关。

**拉普拉斯矩阵一般是一个稀疏的正定矩阵，在 Eigen 中适合使用 SparseMatrix 存储**。

#### 5.2.1. 简单拉普拉斯

如果只考虑三角网格的拓扑关系，那么三角网格的拉普拉斯矩阵和简单图的拉普拉斯是一样的。

#### 5.2.2. 余切拉普拉斯 (Cotangent Laplacian)

余切形式的拉普拉斯在三角网格上使用情形更多，在开头提出的诸多应用里，除了图特映射外，其余方法使用的拉普拉斯都为余切形式的拉普拉斯。这里的拉普拉斯不再与嵌入方式无关。

## 6. Refer Links

[FreemanLu: 三角网格数据结构](https://blog.csdn.net/u013339596/article/details/19167653)

[FreemanLu: 三角网格数据结构 2](https://blog.csdn.net/u013339596/article/details/19167723)

[FreemanLu: 几种网格平滑算法的实现](https://blog.csdn.net/u013339596/article/details/19167359#comments)

[计算机图形学中的 Mesh 数据结构](https://blog.csdn.net/everyting_is_full/article/details/53462660)

[三维网格表示](http://geometryhub.net/notes/meshdatastructure)

[知乎：Half Edge 数据结构有什么优点？](https://www.zhihu.com/question/25337780)

TODO:

[The Half-Edge Data Structure](http://www.flipcode.com/archives/The_Half-Edge_Data_Structure.shtml)

[三角网格 (Triangle Mesh) 的理解](https://www.cnblogs.com/haoyul/p/5738364.html)

[三角网格下的 Half-Edge 数据结构实现方法](https://blog.csdn.net/yjr3426619/article/details/80875271)

[【数字几何处理】网格数据结构](https://tandy123.github.io/2017/06/02/mesh-data-structures/)