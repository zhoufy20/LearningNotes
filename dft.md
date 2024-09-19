# *1. Vasp manual*

## *1.1. 参考资料*

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



## *1.2. 文件解读*

### *1.2.1. 输入文件*

在VASP的输入文件中，我们用 POSCAR 来存储模型的结构信息。当我们使用 VASP 优化完成之后，就会得到一个新的结构，而 CONTCAR 就是用来存储新结构的文件。

- INCAR 
- KPOINTS 
- POSCAR：通过VESTA建模得到，包括体系名称，晶胞基矢信息，原子元素种类、数量和具体坐标
- POTCAR：从赝势库中得到的赝势信息，一般不需要进行操作



### *1.2.2. 输出文件*

VASP 的输出文件主要有 OUTCAR, CHG, CHGCAR, WAVECAR, DOSCAR, EIGENVAL,  OSZICAR, CONTCAR, PCDAT, IBZKPT, XDATCAR。

**OUTCAR 文件**包含了 vasp 计算后得到的绝大部分结果，每步迭代的详细情况。



### *1.2.3. 文件示例*

- **INCAR**

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



- **POSCAR**

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



- **POTCAR**

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



- **KPOINTS**

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



- **ENCUT**

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







## *1.3. VASP 的迭代计算过程*

> 首先，计算体系对电子密度进行初猜；
>
> 其次，据此计算体系的有效势能及求解K-S方程；
>
> 然后，给出体系的总能量及对应的电子密度，即电子步的优化；
>
> 最后，当前后两者之差达到我们预设的收敛标准时，计算收敛结束。

### *1.3.1. VASP计算的收敛标准：*

VASP计算的收敛标准由参数 EDIFF & EDIFFG 决定：

**（1）EDIFF：**控制电子步收敛（前后能量差值），默认EDIFF=1E-4，建议1E-5即可。

**（2）EDIFFG：**控制离子步收敛。默认采用能量收敛，EDIFFG=EDIFF×10。

力作为收敛标准，此时EDIFFG为负值。一般取-0.01到-0.05之间；能量作为收敛标准，此时EDIFFG为正值。一般取0.0001到0.001即可。

### *1.3.2. 控制优化步数的参数 NELM & NSW：* 

VASP中控制优化步数的参数 NELM & NSW：

**（1）NELM：**控制每一离子步中电子步数的最大值。

**（2）NSW：**控制几何优化的步数；NSW = 0：不进行结构优化；NSW = N>0：进行结构优化 。

### *1.3.3 控制结构优化的参数 IBRION：*

控制结构优化的参数 IBRION

**（1）IBRION：决定原子如何移动或弛豫**

- **IBRION = 1：**采用**准牛顿算法**来优化原子的位置；
- **IBRION = 2：**采用**共轭梯度算法**来优化原子的位置；
- **IBRION = 3：**采用**最速下降算法**来优化原子的位置；
- **IBRION = 8：**采用**密度泛函微扰法**来计算声子谱。

**（2）IBRION=2时的计算步骤：**

- **Step1：**从初始结构出发，**计算体系中离子间的作用力；**
- **Step2：**VASP尝试着把离子沿着前面估算的方向移动**，尝试移动的大小由POTIM决定；**
- **Step3：**计算尝试移动后能量和力的大小，据此**加入一个矫正项来控制真实移动的大小；**
- **Step4：**移动后，重新计算能量和力，重复前三步**直至能量或力收敛到设置的EDIFFG值**。

**（3）IBRION = 2时，对POTIM的依赖性很强，需设置一个合理值**

- 在计算中，由于初始原子间距离很小。**第一步计算得到原子间的初始排斥力很强**；第二步中，VASP默认POTIM值是0.50，**前面两步导致了尝试步中离子的移动过大**，以至于后面无法矫正回来，最后导致O2分子计算出错。
- 如果要正确计算，**需设置POTIM一个更小的值**。POTIM = 0.1，虽然从初始值算出来的力很大，但可以通过POTIM强制VASP一点一点调节，来保证计算的准确。
- 若初始结构不合理，用IBRION=2时，POTIM可以很好的控制收敛。建议初始结构搭建的合理些，省时省力。

**注意：**计算一个体系，我们有2个优化过程

- 电子结构的优化： 可以理解为对某一固定的几何结构，迭代求解薛定谔方程来获得体系能量极小值的一个过程。这个迭代过程，每一次迭代求解都可以认为是电子结构的一个优化。（通常被大伙称为：电子步）
- 几何结构的优化：可以理解为在电子结构优化的结果上，获取原子的受力情况，然后根据受力情况，调节原子的位置，再进行电子结构优化，获取新的受力情况，然后再调节原子位置，一直重复这样的过程，直至找到体系势能面上一个极小值的过程。（通常被大伙称为：离子步）



## *1.4. VASP加速计算方案总结*

- 选择较好的计算资源，VASP计算对硬件的要求相对较高，合理选择计算资源是加速计算的第一步。对于不同规模的体系，选择合适的CPU/GPU核数和内存大小至关重要。通常情况下，对于小体系的计算（通常为几到几十个原子的体系），使用单节点适当的核数就足够了，跨界点多核反而会因为节点间通信而降低计算效率；而对于大体系（通常为一百个左右到几百个原子的体系），则需要多节点并行计算，确保足够的内存和并行效率。

- 优化并行设置VASP的并行效率在很大程度上取决于K点的划分和平面波基组的分布。合理设置KPAR和NCORE参数可以显著加快计算速度。调整KPAR值：使得K点的并行数目最优。 设置NCORE或NPAR：合理分配每个核心负责的平面波数目。如下笔者计算120多个原子的合金体系，服务器有56个核，不加`NCORE=7`,`NPAR=8`,`LREAL=Auto`和加了这三项后电子步弛豫速度可以相差10倍以上。因此，在大体系计算时，为了提升计算效率，在几何优化中强烈推荐使用该组参数。
- 调整计算精度VASP中的ENCUT（波函数截断动能）和EDIFF（电子步优化精度）参数直接影响计算的精度和效率。在保证结果可靠性的前提下，适当降低这些参数可以减少计算时间。合理选择ENCUT：通常设为体系中最硬元素的建议值(组合赝势中ENMAX最大值)的1.3倍。适度设置EDIFF：对于中间过程的计算(如体系的几何优化)，可以适当放宽收敛标准。
- 利用预处理技巧VASP计算中，合理的初始猜测可以加快收敛速度。例如，使用合适的初始磁矩或者从粗略计算体系结果中转移波函数作为初猜设置。设置良好的初始磁矩，特别是对于磁性材料如：波函数预处理：使用ISTART=1导入粗略计算的波函数。初猜磁矩设置示例：[**DFT磁性的计算(本例为用VASP计算FCC Ni的磁矩)**](http://mp.weixin.qq.com/s?__biz=MzU0Mzk2MjIzNQ==&mid=2247485964&idx=1&sn=aca485a36334cfcec32875b07d5d3bda&chksm=fb022f18cc75a60e2242bf30b52246f822bffd52c2896cd59f7168625f22189200b21695b671&scene=21#wechat_redirect)
- 启用一些电子结构优化算法。比如，对于电子步迭代困难的大体系，RMM-DIIS （ALGO = Fast）（残矢最小化直接逆迭代子空间优化方法）可能比简单的迭代方法（ALGO = Normal）更有效。
- 避免不必要的计算某些计算步骤可能并不是每次都需要执行。例如，大体系几何优化或第一性分子动力学计算可以不用输出电荷密度和波函数，这样可以使得计算效率提升。



# *2. Lammps manual*

LAMMPS是 Large-scale Atomic/Molecular Massively Parallel Simulator，是Steve Plimpton为主要作者开发一款开源的分子动力学模拟软件。LAMMPS可以模拟固态（金属，非金属，半导体等），液态（水，溶液），软物质（高分子，DNA，蛋白质），粗粒化物质。









# *3. 第一性原理*

Solve quantum mechanic Schrodinger equation to  obtain Eigen value and Eigen function, and thus  the electronic structure. The charm is only atomic number and crystal structure  as input, which can determine precisely the structure  and the properties of the real materials.











# *4.  Molecular Dynamics Simulation*

## *4.1. 模拟计算的一般步骤*

> [理论计算之分子动力学模拟：算什么+应用实例 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/644841606)

在分子动力学( Molecular Dynamics Simulation, MD）计算中，物质被看作是由原子构成的基本单元，通过模拟这些基本单元在时间上的演化，就可以探究物质的动力学性质，从而帮助人们更好地理解和预测物质的行为和性质。

分子动力学模拟是一种基于牛顿运动定律的分子模拟方法，用于计算分子体系与时间相关的性质。**MD模拟可以依据当前分子体系的位置、速度和动能等信息，推测该体系未来的位置、速度和动能，从而揭示分子运动的客观规律。**与单点能和分子构型优化不同，MD模拟需要考虑热运动，分子可包含足够的热能来穿越势垒。根据分子体系中各粒子（包括分子、原子、离子等）运动的统计分析，可推测体系的**各种性质，如可能的构型、热力学参数、分子在溶液中的扩散和行为，各种平衡态性质等。**

要模拟分子动力学，切入点非常直接，表示出**体系的势能面（Potential Energy Surface, PES）**。它描述了分子体系中原子间相互作用的能量随着原子坐标变化的关系。势能面不是我们通常理解的三维空间中的面，而是在反应坐标系中的一个概念，它可以是一维的曲线、二维的面或者更高维度的超面（hypersurface）。

在化学反应中，势能面可以展现反应物、产物、中间体和过渡态的能量变化。通过势能面的分析，我们可以确定化学反应的路径，包括反应物如何通过特定的过渡态转化为产物。这个反应路径通常被称为最小能量路径（Minimum Energy Path, MEP），它是连接反应物和产物的势能最低的路径。

分子动力学的基本任务就是获取物体在任意时刻组成原子的所有位置和动量，然后利用统计力学知识理解物体的性质和行为。它通过对分子间相互作用势函数及运动方程的求解，分析其分子运动的行为规律，模拟体系的动力学演化过程。**模拟计算的一般步骤包含以下几点：**

A. 分子体系模型建立和优化

B. 给定条件参数（温度、粒子数、时间等）

C. 计算作用于所有粒子上的力

D. 求解牛顿方程，计算极短时间内粒子的新位置

E. 计算粒子新的速度和加速度

F. 重复C-E直至体系达到平衡，然后记录原子的坐标位置。

G. 继续计算直到取得足够的信息，分析体系各粒子运动轨迹，得到体系的统计性质。

---

**分子动力学模拟、第一性原理和量子化学之间确实存在一定的区别和联系。**

**首先，分子动力学模拟是基于牛顿力学研究分子体系的一种分子模拟方法**。而第一性原理是基于量子力学研究周期性体系（比如晶体、界面等）的一类计算方法。目前，第一性原理计算主要参照密度泛函理论（DFT），即将电子看作是在原子核周围运动的独立粒子，并通过构建电子密度函数来描述电子在给定状态下的能量和动量。量子化学则是基于量子力学研究独立的化学体系（单个分子、独立团簇）的性质（构型、键能、尺寸、静电势等），目前量子化学计算也大量参照DFT理论。

那么，它们之间又有什么联系呢？我们知道**MD模拟是基于势能的**，而势能可以简单地看作两体势、三体势、[四体势](https://zhida.zhihu.com/search?q=四体势&zhida_source=entity&is_preview=1)、范德华作用及静电作用等。这些参数除了可以通过实验获得，还可以从量子力学的角度出发采用第一性原理计算获得。看到这里，你是否豁然开朗？其实，**将量子力学和分子动力学模拟结合，用以分析原子尺度上更宏伟为和更复杂的物理过程，**已逐步发展成一门新的学科，即量子力学分子动力学模拟。

----



## *4.2. MD模拟的应用范围* 

**MD模拟的应用范围**非常广泛，从复杂的材料科学问题，到生物大分子的结构和功能研究，再到化学反应动力学的研究，MD模拟都能够提供有价值的信息。

**药物设计：**预测化合物的性质，如药物的吸收、分布、代谢过程，以及药物分子与生物大分子（如蛋白质、核酸等）之间的相互作用。

**材料科学：**研究材料的结构、相变、生长和界面等方面，以预测和优化材料的性能，如电子器件、催化剂和生物材料等。

**凝聚态物理**：研究凝聚态系统中的结构、相变和动力学，如固体物理学、磁性材料和超导体等。

**生物分子系统**：研究生物分子系统（如蛋白质、核酸和细胞等）的结构、功能和相互作用，以及生物分子系统在生物过程中的行为。

**化学工程**：预测和优化化学反应过程，如催化剂的选择、反应机理和反应动力学等。

**环境科学：**研究物质在环境中的扩散、迁移、反应和降解过程，以及环境污染物的处理和控制方法。



## *4.3. Lammps Tutorials*

LAMMPS 即 Large-scale Atomic/Molecular Massively Parallel Simulator，可以翻译为大规模原子分子并行模拟器，主要用于分子动力学相关的一些计算和模拟工作。

> [(PDF) 分子动力学模拟及其LAMMPS实现-讲义 (researchgate.net)](https://www.researchgate.net/publication/360773643_fenzidonglixuemonijiqiLAMMPSshixian-jiangyi)
>
> [LAMMPS入门教程（1）——分子动力学模拟起源和LAMMPS简介 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/341613009)
>
> [LAMMPS 软件的安装与运行 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/657435571)

## 1. 安装cmake

编译 LAMMPS 源代码可以通过 `make` 和 `CMake` 两种方法，这里采用后者，因为这种方式具有很多优点，如可以自动检测当前环境和所依赖的库环境，还可以使用预配置文件。如果你缺乏编译软件经验或者想修改 LAMMPS 源码，推荐选择后者。

使用 `CMake` 编译 LAMMPS 需要两个步骤。第一步使用 `cmake` 创建一个 `build` 环境。在这一步中，`CMake` 会检查是否支持 `MPI`、`OpenMP`、`FFTW`、`gzip`、 `JPEG`、`PNG` 以及 `ffmpeg` 并启用相应的配置设置，这一过程会在终端显示。第二步是使用 `camke` 或 `make` 编译 LAMMPS。这两步完成以后，你可以选择性地安装 LAMPPS，相应的命令为 `make install`。

步骤一、安装gcc等必备程序包（已安装则略过此步）

```
yum install -y gcc gcc-c++ make automake 
```

步骤二、解压CMake源码包

```
tar -zxvf cmake-2.8.10.2.tar.gz
```

步骤三、进入目录

```
cd cmake-2.8.10.2
```

步骤四

```
./bootstrap
gmake
gmake install
```

![image-20231012233720863](https://s2.loli.net/2024/04/05/1PhlFTYUXCwnsBk.png)

> 报错：-- Could NOT find OpenSSL, try to set the path to OpenSSL root folder in the system variable OPENSSL_ROOT_DIR (missing: OPENSSL_CRYPTO_LIBRARY OPENSSL_INCLUDE_DIR)
>
> yum install -y openssl-devel



## 2. LAMMPS软件安装与运行

[LAMMPS Molecular Dynamics Simulator](https://www.lammps.org/)

步骤一、解压

```
tar -xzvf file.tar.gz
```

步骤二、编译LAMMPS

```
cd lammps-2Aug2023/
mkdir build
cd build
cmake ../cmake -D BUILD_SHARED_LIBS=yes -D LAMMPS_EXCEPTIONS=yes
cmake --build
make install
lmp
```

> 这里为了能在 Python 中调用 LAMMPS, 这里通过 `-D BUILD_SHARED_LIBS=yes` 将 LAMMPS 变成共享库。同时，官方也建议增加 `-D LAMMPS_EXCEPTIONS=yes` 选项。于是，配置 LAMMPS 安装特性的命令如下：
>
> ```text
> vim ~/.bashrc
> export PATH=/usr/local/bin:$PATH
> export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH
> ```



# LAMMPS 使用教程









































# *5.  Deepmd*











# *A. Electron transfer in a chemical process*

## *A1. Difference charge density*

**The differential charge density** is the difference in the charge density distribution obtained by subtracting the charge density before the operation from the charge density after the operation such as adsorption or substitution of a system.



## *A2. Bader Charge*

>http://theory.cm.utexas.edu/henkelman/code/bader/
>
>http://theory.cm.utexas.edu/vtsttools/
>
>[VASP从入门到入土：Bader电荷的计算 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/673557738)

### *A2.1. Introduction*

[Richard Bader](http://www.chemistry.mcmaster.ca/bader/), from McMaster University, developed an intuitive way of dividing molecules into atoms. His definition of an atom is based purely on the electronic charge density. Bader uses what are called **zero flux surfaces** to divide atoms. A zero flux surface is a 2-D surface on which the charge density is a minimum perpendicular to the surface. Typically in molecular systems, the charge density reaches a minimum between atoms and this is a natural place to separate atoms from each other.

### *A2.2. Output files*

The following output files are generated: `ACF.dat`, `BCF.dat`, `AtomVolumes.dat`.

- `ACF.dat` contains the coordinates of each atom, the charge associated with it according to Bader partitioning, percentage of the whole according to Bader partitioning and the minimum distance to the surface. This distance should be compared to maximum cut-off radius for the core region if pseudo potentials have been used.

- `BCF.dat` contains the coordinates of each Bader maxima, the charge within that volume, the nearest atom and the distance to that atom.

- `AtomVolumes.dat` contains the number of each volume that has been assigned to each atom. These numbers correspond to the number of the BvAtxxxx.dat files.

### *A2.3 Note for VASP users*

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



## *A3. charge density difference* 

差分电荷密度：成键后的电荷密度与对应的成键前的原子电荷密度之差。通过差分电荷密度的计算和分析，可以清楚地得到在成键和成键电子耦合过程中的电荷移动以及成键极化方向等性质。

> Charge density difference of system AB:  **∆ρ = ρAB − ρA − ρB**



1. AB, A, B 需放在相同大小的空间格子。也就是保持相同的KPOINTS吗？









# *B. Structural Optimization* 

结构优化是获得合理结构的过程。所谓合理的结构，大多数场景下为在某些限制下能量更低的结构。这些限制，可以是结构维度的限制（比如限制为二维材料）、晶格的限制（比如施加了应变、放置于衬底上）、元素分布的限制（比如进行吸附时要求吸附物的空间分布）、对称性的限制（比如双层二维材料不同的堆垛方式导致体系对称性不同）。若使用不合理的结构进行后续的计算，比如**体系能量、电子结构、磁态的计算**结果就会出现较大的误差甚至是致命错误。









# *C. Phonon Calculation* 

**Phonopy** is an open source package for phonon calculations at harmonic and quasi-harmonic levels.

- 主要使用phonopy软件计算声子谱和二阶力常数矩阵
- 声子简单来说就是描述晶格振动模式的量子化的准粒子，声子没有实体对应，它们是晶格振动模式的量子化表现。这种振动模式可以被看作是晶格中原子的集体运动，它们以波的形式在晶格中传播。

> [Welcome to phonopy — Phonopy v.2.24.0](https://phonopy.github.io/phonopy/)
>
> [VASP计算笔记_声子谱计算_vasp理论计算-CSDN博客](https://blog.csdn.net/tiandijunhao/article/details/110622248)

## C1.1 高精度地优化原胞结构

准备原胞的 `POSCAR` 文件，并且高精度地优化这个结构

```
#unit cell结构优化的INCAR文件
PREC = Accurate
ENCUT = 500
IBRION = 2	# 结构优化算法
ISIF = 3	# 3是既优化原子坐标也优化晶格

NSW = 100	# 优化步数
NELMIN = 5	# 最小电子自洽步数
EDIFF = 1.0e-08
EDIFFG = -1.0e-08
IALGO = 38
ISMEAR = 0
SIGMA = 0.1
LREAL = .FALSE.
LWAVE = .FALSE.
LCHARG = .FALSE.
NCORE = 4
```



## C1.2 使用phonopy扩胞

使用 `phonopy` 来扩胞，得到计算所需的 `SPOSCAR` 和 `POSCAR-00*`

```
#在Linux终端直接运行命令 
#1. 生成超胞，--dim='3 3 3'表示'x y z'方扩的大小，需要更改
phonopy -d --dim="3 3 3"

#2. 将生成的SPOSCAR拷贝成POSCAR进行DFPT计算，而POSCAR-00*是为了有限位移法计算
cp SPOSCAR POSCAR
```



## C2.1. 密度泛函微扰理论 (DFPT)

1. 必要的输入文件：`INCAR`，`KPOINTS`，`POTCAR`，`POSCAR-unitcell`(优化得到的初始晶胞)，`band.conf`

2. 使用密度泛函微扰法 DFPT：

   - 第一步：将POSCAR重命名为 `POSCAR-unitcell`

   - 第二步：将SPOSCAR重命名为 `POSCAR`

   - 第三步：准备POTCAR、INCAR和KPOINTS文件（需要对KPOINTS进行收敛测试，资源允许的情况下可以把K点设置成 3 3 1，4 4 1 等再算一遍声子谱。）

   - 第四步：进行vasp计算，并且数据处理

   - ```
     #密度泛函微扰法的VASP输入文件INCAR
     SYSTEM=IFC
     
     PREC = High
     ISTART=0
     ICHARG=2
     ISPIN=1
     
     NELM = 60
     NELMIN = 4
     NELMDL = -3
     EDIFF = 1.0e-08
     ENCUT = 500
     
     IALGO = 38
     ADDGRID = True
     LREAL = .FALSE.
     
     NSW = 1			#只需要走一个离子步
     IBRION = 8		#选择使用DFPT法计算力常数矩阵
     EDIFFG = -1.0e-07
     
     ISMEAR = 0
     SIGMA = 0.1
     ```

   - 准备 `band.conf` 文件，用于指导 `Phonopy` 软件包进行声子带结构的计算

     ```
     ATOM_NAME = Zn Pr
     DIM = 2 2 2
     BAND =0.0 0.0 0.0  0.5 0.0 0.0  0.5 0.5 0.0  0.0 0.5 0.0  0.0 0.0 0.0
     BAND_LABELS = G X S Y G
     FORCE_CONSTANTS= READ
     FC_SYMMETRY = .TRUE.
     ```

   在声子谱计算中，`BAND` 标签用于指定计算声子带结构时需要考虑的高对称点和路径。这些点通常是布里渊区中的特定点，它们在声子带结构分析中具有重要意义。`BAND` 标签后面跟着的是一系列三维坐标，这些坐标定义了布里渊区中的路径。

## C2.2. 有限位移法

对原胞进行高精度的优化，主要影响参数：`EDIFF`, `EDIFFG`, `ISIF`

```
#INCAR文件
PREC = Accurate
ENCUT = 500
IBRION = 2  # 结构优化算法
ISIF = 3  # 3是既优化原子坐标也优化晶格
NSW = 100  # 优化步数
NELMIN = 5  # 最小电子自洽步数
EDIFF = 1.0e-08  # 自洽能量收敛判据
EDIFFG = -1.0e-08  # 结构优化能量收敛判据
IALGO = 38  # 算法选择
ISMEAR = 0  # Gaussian smearing
SIGMA = 0.1  # smearing 宽度
LREAL = .FALSE # 以下三个都是控制输出文件的
LWAVE = .FALSE
LCHARG = .FALSE
```



## C3.1 计算声子谱

计算声子谱，连续运行以下三个命令 

```
#直接在终端运行
#1. 提取力常数，得到FORCE_CONSTANTS文件。
phonopy --fc vasprun.xml

#2. 计算声子谱并保存为pdf格式
phonopy -c POSCAR-unitcell band.conf -p -s

#3. 将声子谱进一步输出为数据文件，用于其它软件画图。
#旧版本phonopy
bandplot  --gnuplot> phonon.out

#新版本phonopy, phonon.out文件中首行是高对称点在x轴上的坐标
phonopy-bandplot --gnuplot band.yaml>band.dat

cp ../band.conf ./
phonopy --fc vasprun.xml
phonopy -c POSCAR-unitcell band.conf -p -s
phonopy-bandplot --gnuplot band.yaml>band.dat
```



## C3.2.  声子谱结果

原则上声子谱不应该有负频率，如果原点有负，则需要提高原胞结构优化的精度。如果其他地方有负频率，需要扩大超胞。

在`band.pdf`中观察声子谱，在 `band.yaml` 中可以看到原子的振动信息

-----

**1. 在声子谱中，声学模式和光学模式的区别在哪里？**

声学模式和光学模式是两种主要的声子振动模式，它们在**原子振动方式**和**频率范围**上有显著的区别：

**声学模式**：

- **原子振动**：在声学模式中，原子的振动方向相同，且相位相近。这意味着在一个单元格内的原子会同步振动，就像声波在介质中传播一样。
- **频率范围**：声学模式的频率通常较低，当波矢量（晶格波的动量）接近零时，声学模式的频率也接近零。它们是无能量耗散的，因为它们不涉及电荷的重新分布。

**光学模式：**

- **原子振动**：在光学模式中，原子的振动方向相反，这会导致材料的电偶极矩发生变化。这种振动模式在具有多种原子类型的复杂晶格中尤为重要，因为它们可以引起电子云的重新分布。
- **频率范围**：光学模式出现在声子谱的高频部分。即使当波矢量接近零，它们的频率也不为零，通常比声学模式的频率高。

---

**2. 能带图中的 k 点和声子谱中的 k 点一样吗？**

能带图中的 k 点和声子谱中的 k 点本质上是相同的概念，但它们应用于不同的物理情境。在这两种情况下， k 点都表示晶体动量空间（或布里渊区）中的一个点，但它们分别关联到电子的能量状态和晶格振动模式。

在数学上，无论是能带图还是声子谱，k 点都通过量子力学的波函数来描述，这些波函数与晶体的周期性结构相结合，形成能带和声子色散关系。注意：能带图通常在第一布里渊区内绘制，因为晶体的周期性使得超出这个区域的电子态可以通过周期性边界条件进行等效描述。

**能带图中的 k 点**：

- 在能带理论中， k 点表示电子的波矢，它是电子量子态的特征。
- 能带图展示了电子的能量与其波矢（ k 值）之间的关系，用于描述固体中电子的能量分布。

**声子谱中的 k 点：**

- 在声子谱中， k 点也表示波矢，但它是晶格振动模式或声子的特征。
- 声子谱显示了晶格振动模式的能量（或频率）与其相应的波矢（ k 值）之间的关系。

---

**3. 缺陷和畸变对声子谱有何影响？**

**缺陷对声子谱的影响：**

局部模式：晶体中的缺陷（如空位、杂质或替换原子）可以引入所谓的局部模式，通常表现为声子谱中的新峰或共振峰。

散射中心：缺陷作为声子散射中心，会降低声子的平均自由程，从而影响材料的热导率。这是通过增加声子的散射率来实现的，导致热能传递效率降低。

能带扁平化：在缺陷附近，声子的色散关系可能会出现扁平化，这意味着声子的能量变化不大，即使波矢变化。这种现象可以减少材料的热导率，因为声子的传播受到限制。

**畸变对声子谱的影响：**

对称性破坏：晶体结构的畸变（如晶格扭曲或应力）可以破坏晶体的对称性，这可能导致一些原本被禁止的声子模式变得允许，或者改变现有模式的频率。

能隙的形成或变化：在声子色散关系中，畸变可能导致能隙的形成或变化。例如，一些原子的位移可能导致声子频谱中出现新的能隙。

非谐效应的增强：结构畸变通常会增强声子模式之间的非谐相互作用，这会影响声子的寿命和热传导特性。

---

## C3.3. 声子态密度

声子谱的态密度（Density of States, DOS）用于描述在固体物理学中声子（晶格振动的量子）的分布情况。态密度是每单位能量范围内可用的声子态的数量。它告诉我们在某个特定能量水平上有多少声子模式可以被激发。态密度实际上也就对应着能量分布，有了态密度，我们就可以根据统计力学得到想要的热力学量（宏观性质）。

- **在低频（长波长）极限**，声子态密度通常呈线性增加，与声学模式相关。
- **在高频（短波长）极限**，态密度通常达到峰值后迅速下降，与光学模式相关。

DFPT计算得到FORCE_CONSTANTS文件，编写 `mesh.conf` 文件：

```
ATOM_NAME = C
DIM = 2 2 2
FORCE_CONSTANTS = READ
MP = 30 30 30
```

> `DIM = 2 2 2`：这行定义了模拟的尺寸，通常指的是模拟单元在三个维度上的重复次数。
>
> `FORCE_CONSTANTS = READ`：这行指示程序从文件中读取力常数。力常数是描述原子间相互作用的参数，
>
> `MP = 30 30 30`：这行可能是指定模拟中使用的 Monkhorst-Pack 网格点数，这是一种在第一布里渊区进行均匀采样的方法。在这里，`30 30 30` 表示在三个方向上每个方向都有30个k点，这通常用于计算电子结构和声子色散关系。

输入命令，即可得到 `total_dos.dat` ，进一步作图

```
phonopy -p mesh.conf POSCAR 
```





## C3.4. 投影声子态密度

DFPT计算得到 `FORCE_CONSTANTS` 文件，其 `pdos.conf` 文件为

```
ATOM_NAME = C
DIM = 2 2 2
FORCE_CONSTANTS = READ
MP = 10 10 10
PDOS = 1, 2 3 4
```



输入命令，即可得到 `projected_dos.dat` ，进一步作图

```
phonopy -p pdos.conf -c POSCAR-unitcell
```



## C3.5 热学性质

准备 `mesh.conf` 文件，温度范围[200，2000]，温度梯度为50

DFPT计算得到 `FORCE_CONSTANTS` 文件，其 `mesh.conf` 文件为：

```
ATOM_NAME = C
DIM = 2 2 2
FORCE_CONSTANTS = READ
MP = 10 10 10
TPROP = T
TMIN = 200
TMAX = 2000
TSTEP = 50
```

输入命令，即可得到 `thermal_properties.yaml` ，

```
phonopy -t mesh.conf -c POSCAR-unitcell
```

转换成 `thermal.dat` 文件

```
phonopy-propplot --gnuplot  thermal_properties.yaml > thermal.dat
```



在纳米尺度下，金刚石展现出了与宏观尺度截然不同的力学性能。根据搜索结果，当金刚石被制成纳米针时，它们可以承受巨大的弹性变形而不断裂。这种超强的弹性变形能力主要归因于纳米针的尺寸小、表面光滑以及缺陷极少。例如，一项研究中发现，纳米级单晶金刚石的弹性拉伸形变最大可以达到约9%，接近金刚石的理论弹性极限，而多晶金刚石纳米针的最大弹性应变约为3.5%，也远高于宏观金刚石样品通常所能达到的约0.3%的弹性应变。

首先对diamond金刚石文件进行优化，接着扩胞

并采取DFPT计算，得到无应变的情况

优化，进行单轴拉伸、双轴拉伸和三轴拉伸，最后进行DFPT计算

























