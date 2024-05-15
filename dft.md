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



## *1.5. Electron transfer in a chemical process*

### *1.5.1 Difference charge density*

**The differential charge density** is the difference in the charge density distribution obtained by subtracting the charge density before the operation from the charge density after the operation such as adsorption or substitution of a system.



### *1.5.2 Bader Charge*

>http://theory.cm.utexas.edu/henkelman/code/bader/
>
>http://theory.cm.utexas.edu/vtsttools/
>
>[VASP从入门到入土：Bader电荷的计算 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/673557738)

#### *1.5.2.1. Introduction*

[Richard Bader](http://www.chemistry.mcmaster.ca/bader/), from McMaster University, developed an intuitive way of dividing molecules into atoms. His definition of an atom is based purely on the electronic charge density. Bader uses what are called **zero flux surfaces** to divide atoms. A zero flux surface is a 2-D surface on which the charge density is a minimum perpendicular to the surface. Typically in molecular systems, the charge density reaches a minimum between atoms and this is a natural place to separate atoms from each other.

#### *1.5.2.2. Output files*

The following output files are generated: `ACF.dat`, `BCF.dat`, `AtomVolumes.dat`.

- `ACF.dat` contains the coordinates of each atom, the charge associated with it according to Bader partitioning, percentage of the whole according to Bader partitioning and the minimum distance to the surface. This distance should be compared to maximum cut-off radius for the core region if pseudo potentials have been used.

- `BCF.dat` contains the coordinates of each Bader maxima, the charge within that volume, the nearest atom and the distance to that atom.

- `AtomVolumes.dat` contains the number of each volume that has been assigned to each atom. These numbers correspond to the number of the BvAtxxxx.dat files.

#### *1.5.2.3 Note for VASP users*

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





# *2. Lammps*

LAMMPS是 Large-scale Atomic/Molecular Massively Parallel Simulator，是Steve Plimpton为主要作者开发一款开源的分子动力学模拟软件。LAMMPS可以模拟固态（金属，非金属，半导体等），液态（水，溶液），软物质（高分子，DNA，蛋白质），粗粒化物质。















