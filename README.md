### Anonymous submissions for RSS 23.

<!--
**RSS23-STIFLO/RSS23-STIFLO** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->

## Accuracy experiments.

We conducted experiments on more sequences and datasets to verify the generalization of our network. 

# KITTI 11-21

We evaluated our models on KITTI Seq. 11-21 (results of all sequences are available on KITTI website now). The mean translation and rotation error is 1.78% and 0.72deg/100m respectively. We will append them in the final version.

For a fair comparison, four more classic odometry methods are evaluated for comparison on KITTI odometry dataset:

(Rrel: deg/100m; trel: %)

SUMA (RSS’18) without mapping: 

07-10: rrel=2.003;  trel=0.970

11-21: rrel=4.91;  trel=1.53

PyLiDAR (IROS’21) without mapping: 

07-10: rrel=1.603;  trel=0.765

11-21: rrel=2.91;  trel=0.80

Lego-LOAM (IROS’18) without mapping: 

07-10: rrel=8.425;  trel=4.730

11-21: rrel=19.57;  trel=4.930

A-LOAM (RSS’14) without mapping: 

00_07: rrel=4.270;  trel=1.873

11-21: rrel=9.37;  trel=1.92

Overall, our STIF-LO is superior to all the above works on both Seq. 07-10 and 11-21 by a large margin.

# Argoverse dataset(CVPR’19) 

In order to test the generalization of our method, we perform the experiments on the Argoverse autonomous driving dataset. As the length of most sequences in the Argoverse dataset is within 100 m, the evaluation metrics used for KITTI odometry dataset are no longer suitable for the Argoverse dataset. We employ Absolute Trajectory Error (ATE) (m) and Relative Pose Error (RPE) (m) to evaluate the performance:

(ATE: m; RTE: m)

STIF-LO(ours): 

ATE=0.111;  RTE=0.027

SUMA (RSS’18) without mapping: 

ATE=3.663;  RTE=0.039

PyLiDAR (IROS’21) without mapping: 

ATE=6.900;  RTE=0.109

Lego-LOAM (IROS’18) without mapping: 

ATE=4.537;  RTE=0.110

A-LOAM without mapping: 

ATE=4.138;  RTE=0.066

Overall, our odometry is superior to all the above works on the Argoverse dataset by a large margin, especially in terms of ATE metric.


## Runtime experiments.

# Runtime of each module:

In order to further validate the real-time performance, we test the runtime of each module on a single NVIDIA RTX 2080Ti GPU as suggested by Reviewer 2. The following results show the average runtime of each module in our STIF-LO and PWCLO-Net on Seq. 00 of KITTI dataset:

STIF-LO(ours): 

Point feature pyramid: 0.0049s

Peephole LSTM: 0.0042s

Cost volume_2: 0.0129s

GRU_2: 0.0024s

Mask update_2: 0.0004s

Pose update_2: 0.0025s

Cost volume_1: 0.0045s

GRU_1: 0.0023s

Mask update_1: 0.0014s

Pose update_1: 0.0023s

Cost volume_0: 0.0043s

GRU_0: 0.0022s

Mask update_0: 0.0014s

Pose update_0: 0.0033s

Total average runtime: 0.049s

PWCLO-Net(CVPR’21): 

Point feature pyramid: 0.0130s

Pose initialization(cost volume): 0.0074s

Cost volume_2: 0.0064s

MLP_2: 0.0031s

Mask update_2: 0.0018s

Pose update_2: 0.0030s

Cost volume_1: 0.0054s

MLP_1: 0.0017s

Mask update_1: 0.0017s

Pose update_1: 0.0028s

Cost volume_0: 0.0052s

MLP_0: 0.0016s

Mask update_0: 0.0017s

Pose update_0: 0.0027s

Total average runtime: 0.0575s

Experimental results show that the pyramid feature sharing strategy results in a half reduction on the runtime of the feature extraction module by eliminating redundant calculations; Our proposed pose initialization strategy takes the last estimated pose as the initial value, while PWCLO-Net needs to calculate the initial value using cost volume, which saves 0.0074s; The average runtime of GRU in the pose warp-refinement module is only slightly longer than MLP used in PWCLO-Net, but it is effective on improving accuracy as we have demonstrated in ablation study; The Peephole LSTM, as a newly proposed structure for fusing temporal information, only increases the inference time by 0.0042s, which is acceptable.

# Runtime of other methods:

In addition to the methods listed in Table 2, we have also tested the runtime of several classic geometric-based methods and listed the latest learning-based methods. Their runtime is as follows: 

Lego-LOAM(IROS’18) without mapping: 

0.025s on i5 9300 CPU; 

LOAM(RSS’14) without mapping:

0.023s on AMD R7-5800H CPU; 

SelfVoxeLO(CoRL’21):

0.088s on NVIDIA Tesla V100 GPU; 

RSLO(ICRA’22) without mapping:

0.098s on NVIDIA Tesla V100 GPU.

Our STIF-LO outperforms all learning-based methods with a runtime of 0.049s on NVIDIA RTX 2080Ti GPU. As for the geometric-based methods, although they have good real-time performance, their accuracy is far inferior to our network.



## Trajectories comparisons.
# Seq. 07 of KITTI dataset
![3D trajectory on Seq.07](docs/07_path_3D.png "3D trajectory on Seq.07")

![2D trajectory on Seq.07](docs/07_path.png "2D trajectory on Seq.07")

# Seq. 08 of KITTI dataset
![3D trajectory on Seq.08](docs/08_path_3D.png "3D trajectory on Seq.08")

![2D trajectory on Seq.08](docs/08_path.png "2D trajectory on Seq.08")

# Seq. 09 of KITTI dataset
![3D trajectory on Seq.09](docs/09_path_3D.png "3D trajectory on Seq.09")

![2D trajectory on Seq.09](docs/09_path.png "2D trajectory on Seq.09")

# Seq. 10 of KITTI dataset
![3D trajectory on Seq.10](docs/10_path_3D.png "3D trajectory on Seq.10")

![2D trajectory on Seq.10](docs/10_path.png "2D trajectory on Seq.10")
