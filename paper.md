# 文献阅读总结

> 不断涌现的论文，其实就是学术界的升级安装包。你不断读论文，就是为了跟学术共同体**保持同步**。但是，只有你跟这篇论文所**依赖**的那些基础知识同步以后，安装这个升级包才有意义，也才能水到渠成。
>
> 文献阅读总结聚焦：研究了什么、创新点在哪、研究方法是什么、得出的结论是什么。



## Section1: Osmotic energy generation

## 1.1 aramid nanofiber (ANF) semiconductor-based membranes

### *1.1.1. Light-enhanced osmotic energy generation with an aramid nanofiber membrane*

==CONTENT:== Here, we utilized **aramid nanofiber (ANF) semiconductor-based membranes** to enable light-driven proton
transport for osmotic energy generation. Under unilateral illumination, the light-driven proton transport system
converted light energy into electrical energy and showed wavelength- and intensity-dependent transmembrane
potentials and currents. 





### *1.1.2. A synergistic interfacial and topological strategy for reinforcing aramid nanofiber films*

==CONTENT:== Herein, we present **a synergistic enhancement strategy** by achieving strong interfacial interactions between aramid nanofibers and graphene oxide nanosheets through a neutralization reaction in a dipolar solvent and regulating the topological properties using polymer micelles to form a compact structure, leading to the formation of a super-strong and super-tough nanofiber film.





### *1.1.3. Transition metal hydroxides@conducting MOFs on carbon nanotube yarns for ultra-stable quasi-solid-state supercapacitors with a ship-in-a-bottle architecture*

==CONTENT:== Herein, we report **a ship-in-a-bottle architecture on carbon nanotube yarn (CNTY) based SCs**, in which transition metal hydroxide (TMH) nanoparticles (Ni(OH)2 or Co(OH)2) are confined in conducting nanoporous metal–organic frameworks (MOFs, Ni3(HITP)2) which anchor onto CNTY, involves the synergy of nanoconfinement and hydrogen bonding network to mutually support each phase toward improved electrochemical performance.

























## Section2: Graduation Project

### 2.1 AGAT(atomic graph attention network )

> <Design high-entropy electrocatalyst via interpretable deep graph attention learning>
>
> Journal：Joule	Impact Factor: 39.80	Published: July 03, 2023
>
> Lead Author：Jun Zhang( City University of Hong Kong )
>
> Key words:   AGAT (Atomic Graph Attention networks);	HEECs
>
> Artical adress:  [Design high-entropy electrocatalyst via interpretable deep graph attention learning ](https://www.researchgate.net/publication/372090098_Design_high-entropy_electrocatalyst_via_interpretable_deep_graph_attention_learning)
>
> Code adress:   [jzhang-github/AGAT: Atomic Graph Attention networks](https://github.com/jzhang-github/AGAT)



**Method** The Author developed *an accurate and efficent atomic graph attention network* (AGAT) to accelerate the desigh of the high performance HEECs (high-entropy electrocatalyst). Finally, we apply the well-trained AGAT model to explore the compositional space and predict the high-performance catalysts.

**Content**

- **There are two problems need to be solved for DFT.** First, abundant computational resources are needed to reveal the energy distribution of a single adsorbed intermediate on one surface because of the huge compositional space and abundant active sites on the surfaces. Second, new calculations should be performed for new elemental combinations, making the search for promising HEECs extremely expensive.
- **There are no universal rules for selecting descriptors**, which strongly depends on domain knowledge for ML.

- **The graph neural network** naturally considers symmetry invariance, e.g. , traslation, rotation, and permutation. Additionally, GNN-based models extra compositional features and crystal topology automatically, avoiding empirical-dependent feature engineering.
- But all these published models were constructed based on **the graph convolution schemem**, in which atoms aggregate information from all connected neighbors with the same importance. 

==Innovation== 

- **A perfect message-passing mechanism** should pay attention to connected atoms, other than distant atoms. The AGAT is **interpretable** as the trainable attention scores on graph edges govern the information passed through.

==Question==

- What is scaling relations and classical d-band theory: Although the reliability of scaling relations and classical d-band theory is confirmed on HEEC surfaces, we prove that HEEC can effectively bypass the scaling relations by providing ample versatile local environments. 
- **We only focus on the network now.**

==perspective==

The AGAT is recommended strongly to apply for my project, with an accurate prediction of force and energy. In AGAT,  the graph nodes represent one-hot code of atoms as atomic feature and the graph edges store the distance with unit vector as bond properties.

In code, `ExtractVaspFiles`

![image-20231202115248028](https://s2.loli.net/2024/04/05/cqyuOfbI2M6VDWz.png)

Note: In one AGAT Layer, we separate the nodes into source and destination nodes according to the message-passing direction.

```shell
find . -name OUTCAR > paths.log
sed -i 's/OUTCAR$//g' paths.log
sed -i "s#^.#${PWD}#g" paths.log

awk '$1>1.00 {print}' force_test_pred_true.txt > force_break.txt
ls -l ./|grep "^d"|wc -l
```









### GAT (Graph Attention Network)

> <Graph Attention Network>
>
> Journal：ArXiv	Published: 4th Feburary 2018
>
> Lead Author：Petar Velickovic (Department of Computer Science and Technology University of Cambridge)
>
> Key words:  GAT (Graph Attention networks)
>
> Artical Adress:  [[1710.10903\] Graph Attention Networks (arxiv.org)](https://arxiv.org/abs/1710.10903)
>
> interpretation:  [Graph Attention Networks - (zhihu.com)](https://zhuanlan.zhihu.com/p/296587158)

==Content== As Graph Convolutional Neural Network (GCN) mainly focus on grid-like data, many interesting tasks involve data that lies in an irregular domain. We present graph attention networks (GATs), which is a novel network architecture and operates on graph-structured data, especially for arbitrarily structured graphs.

==Harvest==

- A detailed introduction of the aforementationed approaches about Graph Networks: Recursive Neural Network ---> Graph Neural Network ---> gated recurrent units ---> convolutions on the graph
- A better comprehension of attention mechanisms: one of the benefits is that it allow for dealing with variable sized inputs, focusing on the most relevant parts of the input to make decisions.

![image-20231214123056198](https://s2.loli.net/2024/04/05/D4IOm7GBhjeCNSn.png)







### BonDNet (bond dissociation energies)

> <BonDNet: a graph neural network for the prediction of bond dissociation energies for charged molecules>
>
> Journal:  Chemical Science	Impact Factor:  62.10	Accepted 3rd December 2020
>
> Lead Author：Mingjian Wen ( University of California)
>
> Key words:   the bond dissociation energy
>
> Artical adress:  [BonDNet: a graph neural network for the prediction of bond dissociation energies for charged molecules - Chemical Science (RSC Publishing)](https://pubs.rsc.org/en/content/articlelanding/2021/SC/D0SC05251E)
>
> Code adress:   [mjwen/bondnet](https://github.com/mjwen/bondnet)

==Content==

We propose a chemically inspired graph neural network machine learning model, BonDNet, for the rapid and accurate prediction of BDEs, capable of predicting both homolytic and heterolytic BDEs for molecules of any charge. 

==Innovation==

- First, we add global features to encode molecule-level information, such as the total charge of a molecule. 

- Second, BonDNet takes the differences of the atom, bond, and global features between the products and the reactant to represent a bond dissociation reaction.

==Harvest==

- BonDNet maps the difference between the molecular representations of the reactants and products to the reaction BDE. Because of the use of this difference representation and the introduction of global features, including molecular charge.
- **I have no idea how to describe the breaking bonds reaction** through the difference between the molecular representations of the reactants and products.

![image-20231214134325988](https://s2.loli.net/2024/04/05/Q3hPAf4HV72euUJ.png)





### ASNN (an Associative Neural Network)

><A big data approach to the ultra-fast prediction of DFT-calculated bond energies>
>
>Journal：Journal of Cheminformatics	Published:  2013
>
>Lead Author：Xiaohui Qu (Bristol Composites Institute)
>
>Key words:  interatomic force
>
>Artical adress:  [A big data approach to the ultra-fast prediction of DFT-calculated bond energies | Journal of Cheminformatics | Full Text (biomedcentral.com)](https://jcheminf.biomedcentral.com/articles/10.1186/1758-2946-5-34)
>
>dataset adress:  [Interatomic forces breaking carbon-carbon bonds - Datasets - data.bris](https://data.bris.ac.uk/data/dataset/1ycz4js3rgnzk2dxw07im4dat3)

==Content==





















### Breaking carbon-carbon bonds

><Interatomic forces breaking carbon-carbon bonds>
>
>Journal：Acs Partner Journal	Published: 18 January 2021
>
>Lead Author：Mat Tolladay (Bristol Composites Institute)
>
>Key words:  interatomic force
>
>Artical adress:  [Interatomic forces breaking carbon-carbon bonds - ScienceDirect](https://www.sciencedirect.com/science/article/pii/S0008622320312653)
>
>dataset adress:  [Interatomic forces breaking carbon-carbon bonds - Datasets - data.bris](https://data.bris.ac.uk/data/dataset/1ycz4js3rgnzk2dxw07im4dat3)

==Content==

We compare **computational methods for determining the force** between carbon atoms as a function of bond length, in order to establish which ones are capable of accurately simulating carbon-carbon bonds breaking due to applied mechanical strain in nanomaterials. 

| Tight-binding | density-functional theory | molecular mechanics potentials | Møller-Plesset perturbation theory | complete-active-space self-consistent-field method |
| ------------- | ------------------------- | ------------------------------ | ---------------------------------- | -------------------------------------------------- |

==Harvest==

- We should take electronic behaviour into consideration for the carbon-carbon interatomic forces relevant to the determination of the mechanical strength of materials at atomic-length scales. 
- Determining values for the peak stress experimentally is challenging, but has been achieved for polysaccharide molecules covalently bonded to a substrate, using an atomic force microscope tip to apply a measurable tensile force. As the description of `<How Strong Is a Covalent Bond?>`, the main difficulty is determining which bond breaks.





### CHGNet

><CHGNet as a pretrained universal neural network potential for charge-informed atomistic modelling>
>
>Journal：nature machine intelligence	Published: 14 September 2023
>
>Lead Author：Bowen Deng (Department of Materials Science and Engineering, University of California Berkeley)
>
>Key words:  machine-learning interatomic potential	magnetic moments	
>
>Artical adress:  [CHGNet as a pretrained universal neural network potential for charge-informed atomistic modelling](https://www.nature.com/articles/s42256-023-00716-3)
>
>Code adress:  [CederGroupHub/chgnet](https://github.com/CederGroupHub/chgnet)

==Content==

Large-scale simulations with complex electron interactions remain one of the greatest challenges for atomistic modelling. Although **classical force fields** often fail to describe the coupling between electronic states and ionic rearrangements, the more accurate **ab initio molecular dynamics** suffers from computational complexity that prevents long-time and large-scale simulations, which are essential to study technologically relevant phenomena. 

Here we present **the Crystal Hamiltonian Graph Neural Network** (CHGNet), a graph neural network-based machine-learning interatomic potential (MLIP) that models the universal potential energy surface. 

==Innovation==

- The explicit inclusion of magnetic moments enables CHGNet to learn and accurately represent the orbital occupancy of electrons, enhancing its capability to describe both atomic and electronic degrees of freedom.

==Harvest==

- **How to add electronic information to my network?**

![42256_2023_716_Fig1_HTML-1702534120554-3](https://s2.loli.net/2024/04/05/slJ1npKSOiAfeL6.png)





利用成键的非简谐性研究键能和键长的关系，非简谐性指的是分子振动偏离简谐近似的情况，它对于描述化学键在受到扰动时的行为至关重要。





- 找到峰值力对应的文件，提取能量；

- 找到初始能量，作差

- 查看键长





## Section2: Deep Potential Molecular Dynamics

### DeepMD-kit

><DeepMD kit: A deep learning package for many-body potential energy representation and molecular dynamics>
>
>Journal：Computer Physics Communications	Published: 21 March 2018
>
>Lead Author：Han Wang (Institute ofApplied Physics and Computational Mathematics, Beijing 100094, PR China)
>
>Key words: The Deep Potential for Molecular Dynamics	Many-body potential energy
>
>Artical Adress:
>
>Docs:  [DeePMD-kit’s documentation — DeePMD-kit documentation (deepmodeling.com)](https://docs.deepmodeling.com/projects/deepmd/en/master/#)





## Section3: Materials Design

><Learning Matter: Materials Design with Machine Learning and Atomistic Simulations>
>
>Journal：Acs Partner Journal	Published: 4th Feburary 2018
>
>Lead Author：Petar Velickovic (Department of Computer Science and Technology University of Cambridge)
>
>Key words:
>
>Artical Adress:
>
>interpretation:



