
在三维空间中，所有的物体和相机都需要基于一个统一的坐标系来进行定位和操作。理解坐标系的基本概念，对于创建稳定、准确的三维效果至关重要。


# 基础


Three.js 采用的是右手坐标系，这意味着如果你将右手的三个手指伸直，分别指向 X、Y 和 Z 轴的方向，你的拇指指向的方向即为 X 轴，食指指向的方向即为 Y 轴，而中指指向的方向则是 Z 轴的正方向。


* X 轴：表示水平方向，正方向指向右侧。
* Y 轴：表示垂直方向，正方向指向上方。
* Z 轴：表示深度方向，正方向指向观察者外，负方向指向观察者内。
这一坐标系为我们提供了一个标准化的方式来描述三维空间中的物体位置、旋转和缩放。
![file](https://img2024.cnblogs.com/other/367486/202411/367486-20241126230519299-1555470577.png)


# 坐标轴的作用


在 Three.js 中，坐标轴用于表示和定位物体。每个物体都有一个 position 属性，这个属性决定了物体在三维空间中的位置。通过调整 position 属性的 X、Y 和 Z 值，我们可以在三维空间中自由移动物体。


此外，坐标轴还有助于理解物体的旋转和缩放。通过旋转和缩放矩阵，我们可以对物体进行旋转、缩放等操作，而这些操作的基础就是 Three.js 中的坐标系。


# Three.js 坐标系中的各个概念


## 世界坐标系与局部坐标系


在 Three.js 中，坐标系分为世界坐标系和局部坐标系。它们分别用于描述物体在全局场景中的位置与物体相对于自身的变换。


## 世界坐标系


世界坐标系（World Coordinate System）是整个 Three.js 场景的全局坐标系统，用于统一描述场景中所有物体的位置、旋转和缩放。它是绝对的、唯一的，类似于地图中的全球坐标，所有的物体都通过相对于这个全局坐标系的数值来定位。


#### **特点**


* **固定原点**：世界坐标系的原点 `(0, 0, 0)` 是整个场景的基准点。
* **统一参照系**：无论物体如何移动、旋转或缩放，世界坐标系始终不变。
* **绝对性**：物体的世界坐标描述的是它在整个场景中的绝对位置和状态。


#### **用途**


* 描述物体在场景中的绝对位置。
* 用于相机、灯光等全局性的设置。
* 渲染引擎根据物体的世界坐标来决定最终的显示效果。
* 创建一个物体并设置其位置：



```
const cube = new THREE.Mesh(
  new THREE.BoxGeometry(1, 1, 1),
  new THREE.MeshBasicMaterial({ color: 0xff0000 })
);
cube.position.set(5, 3, -2); // 设置物体在世界坐标系中的位置 scene.add(cube);`;

```

在这个例子中，立方体 `cube` 的位置为 `(5, 3, -2)`，这表示它相对于场景的原点 `(0, 0, 0)` 在 X 方向移动了 5，Y 方向移动了 3，Z 方向移动了 \-2。


## 局部坐标系


局部坐标系（Local Coordinate System）是物体自身的坐标系统，每个物体都有自己的局部坐标系，用于描述其内部的坐标参照。局部坐标系的原点通常位于物体的中心（除非模型本身定义了不同的原点）。


#### 特点


* 动态原点：物体的局部坐标系原点始终跟随物体的位置和旋转。
* 相对性：局部坐标系中的变换是相对于物体本身的，而不是世界坐标系。
* 层级关系：局部坐标系会受到父物体坐标系的影响。


#### 用途


* 用于物体的自定义变换（如局部旋转或局部平移）。
* 在复杂的层级场景中，子物体相对于父物体的位置和方向可以通过局部坐标系来定义。
* 在动画和交互中，对物体的局部坐标系进行操作更灵活。
* 在局部坐标系中，旋转物体：



```
const cube = new THREE.Mesh(
  new THREE.BoxGeometry(1, 1, 1),
  new THREE.MeshBasicMaterial({ color: 0x00ff00 })
);
cube.rotation.set(Math.PI / 4, 0, 0); // 绕局部 X 轴旋转 45 度
scene.add(cube);

```

在这里，立方体 cube 绕自身的局部 X 轴旋转，而不是绕场景的世界 X 轴旋转。


## 物体变换：位置、旋转与缩放


Three.js 中的物体变换通常由三个基本操作构成：位置、旋转和缩放。


位置（Position）：物体的位置由其在三维空间中的坐标来表示。通过修改物体的 .position 属性，可以改变物体在场景中的位置。



```
object.position.set(10, 5, -3);

```

旋转（Rotation）：旋转是指物体围绕某个轴进行旋转。Three.js 使用 Euler 角和四元数两种方式来描述物体的旋转。Euler 角通过三个角度来表示物体的旋转，而四元数则避免了万向锁问题。你可以使用 .rotation 属性来设置物体的旋转，或者使用 .quaternion 属性来直接应用四元数旋转。



```
object.rotation.set(Math.PI / 2, 0, 0); // 旋转 90 度

```

缩放（Scale）：缩放决定物体的大小。Three.js 中的 .scale 属性允许你分别调整 X、Y 和 Z 轴方向上的缩放比例。



```
object.scale.set(2, 2, 2); // 将物体缩放到原来的 2 倍大小

```

# 视图坐标系与世界坐标系的关系


相机视图坐标系是相对于相机的位置和方向而言的，与世界坐标系的关系通过相机的投影矩阵来转换。当我们渲染一个物体时，Three.js 会将物体的世界坐标转换为相机坐标系中的坐标，再通过投影矩阵将其映射到屏幕坐标系中。


通过这种方式，物体的实际位置、旋转和缩放信息最终影响它在屏幕上的显示。


# 坐标系在 Three.js 渲染流程中的应用


## 渲染时坐标系的转换


Three.js 渲染时涉及多个坐标系的转换过程：


从模型坐标系到世界坐标系：物体在其局部坐标系中的位置、旋转和缩放应用后，转换到世界坐标系。
从世界坐标系到相机坐标系：物体的世界坐标经过相机的视角变换后，得到相对于相机的坐标。
从相机坐标系到屏幕坐标系：通过投影矩阵将物体从三维坐标映射到二维屏幕上，最终显示在浏览器中。


## 常见坐标系变换的示例


举个例子，假设我们有一个物体，并希望将其从世界坐标系转换到屏幕坐标系，可以通过以下步骤：



```
const vector = new THREE.Vector3(10, 5, -10); // 物体在世界坐标系中的位置
vector.project(camera); // 将物体位置从世界坐标系转换为相机坐标系

```

这段代码演示了如何通过相机将物体的世界坐标转换为屏幕坐标，便于后续的渲染和交互操作。


# 坐标系调试与工具


坐标轴帮助工具
在 Three.js 中，AxesHelper 和 GridHelper 是两个常用的调试工具，它们分别用于显示三维坐标轴和网格。



```
const axesHelper = new THREE.AxesHelper(5);
scene.add(axesHelper); // 在场景中添加坐标轴帮助器

```

通过这些工具，我们可以直观地看到物体的坐标轴，帮助我们理解物体在三维空间中的位置和方向。


# 坐标变换工具


Three.js 提供了 applyMatrix4() 方法，可以帮助我们对物体进行手动的坐标变换。通过这种方法，我们可以对物体应用平移、旋转和缩放等操作。



```
const matrix = new THREE.Matrix4();
matrix.makeTranslation(10, 0, 0); // 创建一个平移矩阵
object.applyMatrix4(matrix); // 将矩阵应用到物体

```

# 坐标系常见问题与解决方法


## 万向锁问题


在使用 Euler 角进行旋转时，可能会遇到万向锁问题，即某些旋转轴会因为连续旋转而出现错误的行为。为了解决这个问题，Three.js 提供了四元数（Quaternion）来避免万向锁。



```
object.rotation.setFromQuaternion(
  new THREE.Quaternion().setFromEuler(new THREE.Euler(Math.PI / 2, 0, 0))
);

```

## 坐标系误差与漂移


在大规模场景中，浮动精度可能导致坐标系出现漂移现象。通常可以通过优化计算精度和使用大范围坐标系来解决这些问题。


 本博客参考[wgetcloud全球加速服务机场](https://wa7.org)。转载请注明出处！
