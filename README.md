# V1, V2a, MN circuit simulation in zebrafish spinal cord


## About
This is a re-implementation in [Brian2](https://brian2.readthedocs.io/en/stable/introduction/index.html) of the python-based simulation from [Sengupta et al. (2021)](https://pubmed.ncbi.nlm.nih.gov/34289387/) of a spinal cord circuitry in zebrafish, shown in [fig. 7](https://www.cell.com/cms/attachment/e5ba15af-700f-4e21-b91b-f18988cf86d6/gr7.jpg) of the publication. The simulation shows how a locomotor wave propagates along the rosto-caudal axis of the spinal cord. A specific connectivity pattern is needed for the smooth signal propagation, namely the local inhibition by the V1 interneurons of both V2a and motor neuron (MN) populations. Altering the connectivity by making V1 connect to V2a and/or MNs located in more distal segments leads to erroneous MN spiking, when MNs spike outside of their cycle within the rostro-caudal spiking wave. Such extraneous MN spikes lead in turn to abnormal swimming patterns. The aim of the simulation was to reproduce the spiking pattern of the network. This project was developed during the 2024 [OIST Computational Neuroscience Course](https://groups.oist.jp/ocnc). 


<p align="center">
<img src="https://github.com/katarzynampiekarz/V1_V2a_MN_circuit_simulation/blob/main/fig7d.png?raw=true" width="800"><br>
<figcap>Fig.1. The network spiking pattern from Sengupta et al. (2021; fig. 7d) with the extraneous spikes marked in red.</figcap>
</p>

The simulation uses Izhikevich neuronal model with parameters specified in Sengupta et al. (2021), and includes both chemical biexponential synapses and gap junctions.

## Network
The network is composed of four neuronal populations distributed among 15 spinal hemisegments:

1)	pacemaker cells (PM; labelled as IC in the original paper) – 5 rostrally located cells that provide input to the network
2)	V1 interneurons (aka CiA) – 10 inhibitory interneurons that project ipsilaterally and provide the local inhibiton to the motor targets
3)	V2a interneurons (aka CiD) – 12 premotor excitatory interneurons, important for speed control
4)	motor neurons (MN) – 15 cells
   
## Connectivity
The connectivity matrices from Sengupta et al. (2021) served as a starting point, and were modified to better render the network firing behavior in Brian2. The five pacemaker neurons are connected all-to-all (except themselves) with each other through gap junctions, as well as to the four most rostrally located V2a neurons (also via gap junctions). V2a interneurons are connected to each other up to three segments away in both directions via excitatory synapses. V2a then provide excitatory inputs to MNs located up to two segments rostrally and three segments caudally, as well as to V1 interneurons located up to six segments caudally. V1 interneurons form inhibitory connections with V2a interneurons and MNs located 1-3 segments rostrally. Lastly, V1 interneurons project back to the pacemaker cells.

<p align="center">
<img src="https://github.com/katarzynampiekarz/V1_V2a_MN_circuit_simulation/blob/main/connectivity_all.png?raw=true" width="700"><br>
<figcap>Fig.2. The network connectivity.</figcap>
</p>


## Results
The re-implemented network firing patterns reproduce the firing shown in the paper, as the locomotor wave propagates smoothly in the rostro-caudal manner (see: ocnc_project_main.ipynb). Some differences in the spiking patterns are to be expected though, due to the fact that the differential equation implementation in Brian2 is not exactly the same as the equations used in the original python-based simulation. Also, the re-implementation does not include the synaptic delays at this point.


In subsequent simulations the connectivity was altered, so that V1 interneurons connect locally to V2a, but distally to MNs (see: ocnc_project_distal_MN_local_V2a.ipynb), and distally both to V2a and MNs (see: ocnc_project_distal_MN_distal_V2a.ipynb). In both cases the extraneous MN spikes appear, which is in agreement with the MN firing pattern shown in the original simulation. The main difference, however, can be noticed in the firing pattern of the pacemaker cells: in the original publication a single burst of PM cells jump-starts the continuous firing of the network, while in the present simulation the PM cells firing is necessary to initiate each cycle. 

<p align="center">
<img src="https://github.com/katarzynampiekarz/V1_V2a_MN_circuit_simulation/blob/main/my_results.png?raw=true" width="900"><br>
<figcap>Fig.3. The raster plot showing the firing patterns in the re-implemented simulation.</figcap>
</p>

## References
Sengupta M, Daliparthi V, Roussel Y, Bui TV, Bagnall MW. Spinal V1 neurons inhibit motor targets locally and sensory targets distally. Curr Biol. 2021 Sep 13;31(17):3820-3833.e4. doi: 10.1016/j.cub.2021.06.053. Epub 2021 Jul 21. PMID: 34289387; PMCID: PMC8440420.

Stimberg M, Brette R, Goodman DFM. Brian 2, an intuitive and efficient neural simulator. eLife 2019 8:e47314. doi: 10.7554/eLife.47314.
