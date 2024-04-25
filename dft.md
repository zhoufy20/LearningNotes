![image-20240422091448779](./assets/image-20240422091448779.png)



[共价键](https://zhidao.baidu.com/search?word=共价键&fr=iknow_pc_qb_highlight)从不同的角度可以进行不同的分类，每一种分类都包括了所有的共价键（只是分类角度不同）。



**按成键方式**

[共价键_百度百科 (baidu.com)](https://baike.baidu.com/item/共价键/549226)

σ键

由两个[原子轨道](https://zhidao.baidu.com/search?word=原子轨道&fr=iknow_pc_qb_highlight)沿轨道[对称轴](https://zhidao.baidu.com/search?word=对称轴&fr=iknow_pc_qb_highlight)方向相互重叠导致电子在核间出现概率增大而形成的共价键，叫做σ键，可以简记为“头碰头”。σ键属于定域键，它可以是一般共价键，也可以是配位共价键。一般的单键都是σ键。原子轨道发生杂化后形成的共价键也是σ键。由于σ键是沿轨道对称轴方向形成的，轨道间重叠程度大，所以，通常σ键的键能比较大，不易断裂，而且，由于有效重叠只有一次，所以两个原子间至多只能形成一条σ键。

π键

成键原子的未杂化p轨道，通过平行、侧面重叠而形成的共价键，叫做π键，可简记为“肩并肩”。π键与σ键不同，它的成键轨道必须是未成对的p轨道。π键性质各异，有两中心，两电子的定域键，也可以是共轭Π键和反馈Π键。两个原子间可以形成最多2条π键，例如，[碳碳双键](https://zhidao.baidu.com/search?word=碳碳双键&fr=iknow_pc_qb_highlight)中，存在一条σ键，一条π键，而碳碳三键中，存在一条σ键，两条π键。

δ键

由两个d轨道四重交盖而形成的共价键称为δ键，可简记为“面对面”。δ键只有两个节面（[电子云](https://zhidao.baidu.com/search?word=电子云&fr=iknow_pc_qb_highlight)密度为零的平面）。从键轴看去，δ键的轨道对称性与d轨道的没有区别，而[希腊字母](https://zhidao.baidu.com/search?word=希腊字母&fr=iknow_pc_qb_highlight)δ也正来源于d轨道。





在VASP中，常常计算Bader电荷来得到原子周围的电子数，从而近似得到原子的化合价。Bader电荷分析是理查德·贝德（RichardBader）开发的一种将分子分解为原子的直观方法。Bader电荷分析对原子的定义纯粹是基于电子电荷密度。Bader使用所谓的零磁通表面来划分原子。零通量表面是2D表面，其上电荷密度垂直于表面。

通常在分子系统中，电荷密度在原子之间达到最小值，这是将原子彼此分开的自然位置。除了作为分子中原子可视化的直观方案外，Bader的定义通常也可用于电荷分析。例如，Bader体积内的电荷与原子的总电子电荷很接近。电荷分布可用于确定相互作用的原子或分子的多极矩。Bader的分析也被用来定义原子的硬度，可以用来量化从原子中去除电荷的成本。



考虑的分子有







## 2. Electron transfer in a chemical process

### 2.1 Difference charge density

**The differential charge density** is the difference in the charge density distribution obtained by subtracting the charge density before the operation from the charge density after the operation such as adsorption or substitution of a system.











### 2.2 Bader Charge

>http://theory.cm.utexas.edu/henkelman/code/bader/
>
>http://theory.cm.utexas.edu/vtsttools/
>
>[VASP从入门到入土：Bader电荷的计算 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/673557738)

#### 2.2.1 Introduction

[Richard Bader](http://www.chemistry.mcmaster.ca/bader/), from McMaster University, developed an intuitive way of dividing molecules into atoms. His definition of an atom is based purely on the electronic charge density. Bader uses what are called **zero flux surfaces** to divide atoms. A zero flux surface is a 2-D surface on which the charge density is a minimum perpendicular to the surface. Typically in molecular systems, the charge density reaches a minimum between atoms and this is a natural place to separate atoms from each other.

#### 2.2.2 Output files

The following output files are generated: `ACF.dat`, `BCF.dat`, `AtomVolumes.dat`.

- `ACF.dat` contains the coordinates of each atom, the charge associated with it according to Bader partitioning, percentage of the whole according to Bader partitioning and the minimum distance to the surface. This distance should be compared to maximum cut-off radius for the core region if pseudo potentials have been used.

- `BCF.dat` contains the coordinates of each Bader maxima, the charge within that volume, the nearest atom and the distance to that atom.

- `AtomVolumes.dat` contains the number of each volume that has been assigned to each atom. These numbers correspond to the number of the BvAtxxxx.dat files.

#### 2.2.3 Note for VASP users

One major issue with the charge density (CHGCAR) files from the VASP code is that they only contain the valance charge density. The Bader analysis assumes that charge density maxima are located at atomic centers (or at pseudoatoms). Aggressive pseudopotentials remove charge from atomic centers where it is both expensive to calculate and irrelevant for the important bonding properties of atoms.

1. VASP contains a module (aedens) which allows for the core charge to be written out from PAW calculations. By adding the LAECHG=.TRUE. to the INCAR file, the core charge is written to AECCAR0 and the valance charge to AECCAR2. 

```bash
# the INCAR file
Global Parameters
ISTART = 1 # read the WAVECAR file
ICHARG = 1 # Read the charge density from CHGCAR file

LAECHG =.TRUE. #the all-electron charge density will be reconstructed explicitly and written to files.
LCHARG = .TRUE. # determines whether the charge densities (files CHGCAR and CHG) are written.
NSW    = 0 # sets the maximum number of ionic steps
IBRION = -1 # The ions are not moved, but NSW outer loops are performed.
```

2. These two charge density files can be summed using the [chgsum.pl](http://theory.cm.utexas.edu/vtsttools/scripts.html) script, and the total charge will be written to CHGCAR_sum.

```bash
chgsum.pl AECCAR0 AECCAR2
```

3. The bader analysis can then be done on this total charge density file:

```bash
bader CHGCAR -ref CHGCAR_sum
```

4. One finally note is that you need a fine fft grid to accurately reproduce the correct total core charge. It is essential to do a few calculations, increasing NG(X,Y,Z)F until the total charge is correct.

   The number of electrons printed in the `ACF.dat` must be an integer! This is very important.

```bash
grep ZVAL POTCAR
grep NGX OUTCAR
```





## 3. Vasp manual

# VASP 相关教程

> [VASP软件 INCAR文件参数含义速查表 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/151725218)
>
> [VASP | 北辞 (northword.cn)](https://northword.cn/dft-learning/pages/df30ef/)
>
> [教程: LVASPTHW | Learn VASP The Hard Way (bigbrosci.com)](https://www.bigbrosci.com/categories/LVASPTHW/page/3/)
>
> [The VASP Manual - Vaspwiki](https://www.vasp.at/wiki/index.php/The_VASP_Manual)
>
> 

Rubbish in，Rubbish out! 程序只负责算，对错由你决定！错误主要有3类：

模型错误：也就是建模这一块出错了，主要对应的是POSCAR；

计算参数：INCAR，KPOINTS，POTCAR

提交任务的脚本或者命令出错。

遇到不懂的参数： (思考 + 看官网)！ (思考 + 看官网)！ (思考 + 看官网)！通过查阅官网解决问题，可以保证你的血统纯正，进而提高你的杀伤力。



## 1. 文件解读

### 1.1 输入文件

#### 1.1.1 INCAR 文件



#### 1.1.2 KPOINTS 文件



#### 1.1.3 POSCAR 文件

通过VESTA建模得到，包括体系名称，晶胞基矢信息，原子元素种类、数量和具体坐标



#### 1.1.4 POTCAR 文件

POTCAR 从赝势库中得到的赝势信息，一般不需要进行操作





### 1.2 输出文件

#### 1.2.1 CONTCAR

在VASP的输入文件中，我们用 POSCAR 来存储模型的结构信息。当我们使用 VASP 优化完成之后，就会得到一个新的结构，而 CONTCAR 就是用来存储新结构的文件。



VASP 的输出文件主要有 OUTCAR, CHG, CHGCAR, WAVECAR, DOSCAR, EIGENVAL,  OSZICAR, CONTCAR, PCDAT, IBZKPT, XDATCAR。

**OUTCAR 文件**包含了 vasp 计算后得到的绝大部分结果，每步迭代的详细情况。







### 1.3 脚本解释

#### 1.3.1 INCAR

```shell
Global Parameters
# ISTART 参数指定了计算的起始方式。在这里，设置为1表示从头开始计算
ISTART = 1
# ICHARG 指定了电荷密度的初始值。在这里，设置为1表示使用原子核电荷密度
ICHARG = 1
# LREAL 控制是否使用投影算符。在这里，设置为.FALSE.表示不使用投影算符
LREAL  = .FALSE.       (Projection operators: automatic)
# ENCUT 指定了平面波基组的截断能量，单位为电子伏特（eV）
ENCUT  =  600        (Cut-off energy for plane wave basis set, in eV)
# PREC 指定了计算的精度级别。N表示使用普通精度。结构松弛计算时，可以设置为Accurate以获得更高的精度。
PREC   =  N   (Precision level: Normal or Accurate, set Accurate when perform structure lattice relaxation calculation)
# LWAVE 控制是否写入WAVECAR文件，即波函数文件。在这里，设置为.T.表示写入WAVECAR文件
LWAVE  = .T.        (Write WAVECAR or not)
# LCHARG 控制是否写入CHGCAR文件，即电荷密度文件。在这里，设置为.T.表示写入CHGCAR文件
LCHARG = .T.        (Write CHGCAR or not)
# 这个参数控制是否增加网格密度，有助于广义梯度近似（GGA）的收敛。在这里，设置为.TRUE.表示增加网格密度
ADDGRID= .TRUE.        (Increase grid, helps GGA convergence)

#电子弛豫相关的参数设置
Electronic Relaxation
# ISMEAR 指定了电子态密度的平滑方式。在这里，设置为0表示使用高斯平滑，对于金属可以设置为1
ISMEAR =  0            (Gaussian smearing, metals:1)
# SIGMA 指定了平滑值的大小，单位为电子伏特（eV）
SIGMA  =  0.05         (Smearing value in eV, metals:0.2)
# NELM 指定了电子自洽场（SCF）迭代的最大步数
NELM   =  200           (Max electronic SCF steps)
# NELMIN 电子自洽场（SCF）迭代的最小步数
NELMIN =  6            (Min electronic SCF steps)
# EDIFF 指定了SCF能量的收敛标准，单位为电子伏特（eV）
EDIFF  =  1E-06        (SCF energy convergence, in eV)

# 离子弛豫相关的参数设置 
Ionic Relaxation
# NSW 指定了离子弛豫的最大步数
NSW    =  500          (Max ionic steps)
# IBRION 指定了离子弛豫的算法。在这里，设置为2表示使用共轭梯度（CG）算法
IBRION =  2            (Algorithm: 0-MD, 1-Quasi-New, 2-CG)
# ISIF 指定了应力/松弛的类型。在这里，设置为2表示离子松弛
# 对晶胞的弛豫方法（3:全弛豫 2:固定体积的弛豫 4:固定体积但允许形状改变）
ISIF   =  2            (Stress/relaxation: 2-Ions, 3-Shape/Ions/V, 4-Shape/Ions)
# EDIFFG 指定了离子弛豫的收敛标准，单位为电子伏特/埃
EDIFFG = -0.01        (Ionic convergence, eV/AA)
# ISYM 指定了是否考虑晶体的对称性。在这里，设置为0表示不考虑对称性
ISYM =  0            (Symmetry: 0=none, 2=GGA, 3=hybrids)
 
# 指定了计算中使用的核心数
NCORE  =  4
```



#### 1.3.2 POSCAR

```shell
C-C\(2)     			  # 表示这是一个碳原子之间的键长                           
   1.0000000000000000     # Scale factor，称为缩放系数，这里是1.0
    15.0000000000000000    0.0000000000000000    0.0000000000000000
     0.0000000000000000   10.0000000000000000    0.0000000000000000
     0.0000000000000000    0.0000000000000000   10.0000000000000000
   C    H 
     4     6
Selective dynamics  # 表示后面的原子坐标是否可变
Direct 				# 分数坐标系，此外 Cartesian 代表笛卡尔坐标
0.5904858841833516  0.2999999999999359  0.5000000000000000  F F F
0.6713877475914174  0.5000000000000000  0.5000000000000000  F F F
0.5092485101414116  0.3486004850937958  0.4401903376439380  T T T
0.7528247550253407  0.4517523283058748  0.4406847646577373  T T T
0.7560925602145590  0.3609595616932350  0.3779702892060639  T T T
0.7731187935138405  0.4180510752949021  0.5447197789568976  T T T
0.8012232933537877  0.5291787107286893  0.4104881617495468  T T T
0.4611125037393218  0.2712905301115817  0.4087773373855958  T T T
0.5063321265215300  0.4397926979914503  0.3780212380844355  T T T
0.4881043420912850  0.3814741743575465  0.5440764710599447  T T T

# 这一行主要描述的是体系中原子在xyz三个方向移动相关的信息
  0.00000000E+00  0.00000000E+00  0.00000000E+00	
  0.00000000E+00  0.00000000E+00  0.00000000E+00
  0.00000000E+00  0.00000000E+00  0.00000000E+00
  0.00000000E+00  0.00000000E+00  0.00000000E+00
  0.00000000E+00  0.00000000E+00  0.00000000E+00
  0.00000000E+00  0.00000000E+00  0.00000000E+00
  0.00000000E+00  0.00000000E+00  0.00000000E+00
  0.00000000E+00  0.00000000E+00  0.00000000E+00
  0.00000000E+00  0.00000000E+00  0.00000000E+00
  0.00000000E+00  0.00000000E+00  0.00000000E+00

```



#### 1.3.3 POTCAR

```shell
PAW_PBE C 08Apr2002					# 用于描述碳原子的PAW（投影缀加平面波）参数的文件         
	4.00000000000000     			# C 原子的原子质量为4.0
# 这是一个注释，表示接下来的参数是从PSCTR（假设是一个参数文件）中获取的。
parameters from PSCTR are:			
VRHFIN =C: s2p2						# 表示碳原子的电子构型为s2p2
LEXCH  = PE							# 表示使用的交换-相关泛函为PE
EATOM  =   147.1560 eV,  10.8157 Ry # 表示碳原子的原子能为147.1560 eV或10.8157 Ry

TITEL  = PAW_PBE C 08Apr2002		# 表示PAW参数的标题
LULTRA =        F    use ultrasoft PP ?				# 是否使用超软赝势
IUNSCR =        1    unscreen: 0-lin 1-nonlin 2-no	# 表示是否进行屏蔽计算
RPACOR =    1.200    partial core radius			# 部分核心半径为1.200
POMASS =   12.011; ZVAL   =    4.000    mass and valenz	# 质量为12.011，价电子数为4.000
RCORE  =    1.500    outmost cutoff radius			# 最外层截断半径
RWIGS  =    1.630; RWIGS  =    0.863    wigner-seitz radius (au A)	# 维格尼兹半径
ENMAX  =  400.000; ENMIN  =  300.000 eV				# 能量截断
ICORE  =        2    local potential				# 使用局域势
LCOR   =        T    correct aug charges			# 是否修正增强电荷
LPAW   =        T    paw PP							# 使用PAW赝势
EAUG   =  644.873									# 增强电荷的能量为644.873
DEXC   =    0.000									# 交换能的修正
RMAX   =    1.529    core radius for proj-oper		# 投影算符的核心半径为1.529	
RAUG   =    1.300    factor for augmentation sphere # 增强球的因子为1.300
RDEP   =    1.501    radius for radial grids		# 径向网格的半径为1.501
RDEPT  =    1.300    core radius for aug-charge		# 增强电荷的核心半径为1.300
```



#### 1.3.4 KPOINTS

```shell
# 用于生成K-Mesh 的 K-间距值的说明
# K-间距是在固体材料的能带结构计算中使用的一个参数，用于确定K点的密度。
# 较小的K-间距值表示更高的K点密度，从而提供更准确的能带结构信息
K-Spacing Value to Generate K-Mesh: 0.030 
# 零，格子自动生成
0
# Gamma是一个特殊的K点，代表晶体中的原点。
# 在能带结构计算中，Gamma点通常被用作参考点，用于计算其他K点的能带结构。
Gamma
# 1*1*1 格子
   1   1   1
# S1 S2 S3， 一般保持 0 0 0 不变
0.0  0.0  0.0
```



## 2.1 ENCUT

平面波的切断动能。采用默认值还是手动的输入。推荐的做法是**采用后者**，在任何性质的计算之前，进行 ENCUT 收敛情况的计算，由此来确定一个合适的切断动能值，然后手动地设置。

```shell
#!/bin/sh 
rm WAVECAR 
for i in 150 200 250 300 350 400 
do 
cat > INCAR <<! 
SYSTEM = Si-Diamond 
ENCUT = $i 
ISTART = 0 ; ICHARG = 2 
ISMEAR = -5 
PREC = Accurate 
! 
echo "ENCUT = $i eV" ; time vasp 
E=`grep "TOTEN" OUTCAR | tail -1 | awk '{printf "%12.6f \n", $5 }'` 
echo $i $E >>comment 
done 
```

> `cat > INCAR <<! xxxx ! ` : 这是重定向符号，表示将下面的内容（两个感叹号中间的内容）作为输入，直到遇到另一个感叹号（!）为止。





















## 3. VASP 的迭代计算过程

> 首先，计算体系对电子密度进行初猜；
>
> 其次，据此计算体系的有效势能及求解K-S方程；
>
> 然后，给出体系的总能量及对应的电子密度，即电子步的优化；
>
> 最后，当前后两者之差达到我们预设的收敛标准时，计算收敛结束。

### 3.1 VASP计算的收敛标准由参数 EDIFF & EDIFFG 决定:

**（1）EDIFF：**控制电子步收敛（前后能量差值），默认EDIFF=1E-4，建议1E-5即可。

**（2）EDIFFG：**控制离子步收敛。默认采用能量收敛，EDIFFG=EDIFF×10。

力作为收敛标准，此时EDIFFG为负值。一般取-0.01到-0.05之间；<br>能量作为收敛标准，此时EDIFFG为正值。一般取0.0001到0.001即可。

### 3.2 VASP中控制优化步数的参数 NELM & NSW： 

**（1）NELM：**控制每一离子步中电子步数的最大值。

**（2）NSW：**控制几何优化的步数；NSW = 0：不进行结构优化；NSW = N>0：进行结构优化 。

### 3.3 控制结构优化的参数 IBRION

**（1）IBRION：决定原子如何移动或弛豫**

- **IBRION = 1：**采用**准牛顿算法**来优化原子的位置；
- **IBRION = 2：**采用**共轭梯度算法**来优化原子的位置；
- **IBRION = 3：**采用**最速下降算法**来优化原子的位置。

**（2）IBRION=2时的计算步骤：**

- **Step1：**从初始结构出发，**计算体系中离子间的作用力；**
- **Step2：**VASP尝试着把离子沿着前面估算的方向移动**，尝试移动的大小由POTIM决定；**
- **Step3：**计算尝试移动后能量和力的大小，据此**加入一个矫正项来控制真实移动的大小；**
- **Step4：**移动后，重新计算能量和力，重复前三步**直至能量或力收敛到设置的EDIFFG值**。

**（3）IBRION = 2时，对POTIM的依赖性很强，需设置一个合理值**

- 在计算中，由于初始原子间距离很小。**第一步计算得到原子间的初始排斥力很强**；第二步中，VASP默认POTIM值是0.50，**前面两步导致了尝试步中离子的移动过大**，以至于后面无法矫正回来，最后导致O2分子计算出错。
- 如果要正确计算，**需设置POTIM一个更小的值**。POTIM = 0.1，虽然从初始值算出来的力很大，但可以通过POTIM强制VASP一点一点调节，来保证计算的准确。
- 若初始结构不合理，用IBRION=2时，POTIM可以很好的控制收敛。建议初始结构搭建的合理些，省时省力。



**注意：**计算一个体系，我们有`2`个优化过程

- 电子结构的优化： 可以理解为对某一固定的几何结构，迭代求解薛定谔方程来获得体系能量极小值的一个过程。这个迭代过程，每一次迭代求解都可以认为是电子结构的一个优化。（通常被大伙称为：电子步）
- 几何结构的优化：可以理解为在电子结构优化的结果上，获取原子的受力情况，然后根据受力情况，调节原子的位置，再进行电子结构优化，获取新的受力情况，然后再调节原子位置，一直重复这样的过程，直至找到体系势能面上一个极小值的过程。（通常被大伙称为：离子步）















