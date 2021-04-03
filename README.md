# PEARL: Efficient Off-policy Meta-learning via Probabilistic Context Variables

on arxiv: http://arxiv.org/abs/1903.08254

by Kate Rakelly*, Aurick Zhou*, Deirdre Quillen, Chelsea Finn, and Sergey Levine (UC Berkeley)

*Note 5/22/20: The ant-goal experiment is currently not reproduced correctly. We are aware of the problem and are looking into it. We do not anticipate pushing a fix before the Neurips 2020 deadline.*

This is the reference implementation of the algorithm; however, some scripts for reproducing a few of the experiments from the paper are missing.
This repository is based on [rlkit](https://github.com/vitchyr/rlkit).

We ran our ProMP, MAML-TRPO, and RL2 baselines in the [reference ProMP repo](https://github.com/jonasrothfuss/ProMP) and our MAESN comparison in the [reference MAESN repo](https://github.com/RussellM2020/maesn_suite).
The results for PEARL as well as all baselines on the six continuous control tasks shown in Figure 3 may be downloaded [here](https://www.dropbox.com/s/3uorwtrqzury6wt/results_cont_control.zip?dl=0).

#### TODO (where is my tiny fork?)
- [ ] fix RNN encoder version that is currently incorrect!
- [ ] add optional convolutional encoder for learning from images
- [x] add Walker2D and ablation experiment scripts
- [x] add jupyter notebook to visualize sparse point robot
- [x] policy simulation script
- [x] add working Dockerfile for running experiments

--------------------------------------

### Instructions for installation via conda

(See original [README.md](https://github.com/katerakelly/oyster) for running it in Docker.)

First clone this repo with `git clone --recurse-submodules`.

#### Install Mujoco

1. For fully usage, please install MuJoCo200, Mjpro150 and Mjpro131, which are available on the [MuJoCo website](https://www.roboti.us/index.html). Also, you can obtain a 30-day free trial [here](https://www.roboti.us/license.html). The license key `mjkey.txt` will arrive in an email with your username and password.
2. Unzip the downloaded `mujoco200 ` directory into `~/.mujoco20/mujoco200`, and place the license key `mjkey.txt` file at `~/.mujoco20/mjkey.txt`.
3. Mjpro150 and Mjpro131 can be installed the same way as MuJoCo200.
4. Test the installation:  
  `cd ~/.mujoco/mujoco200/bin`  
  `./simulate ../model/humanoid.xml`
6. Configure environment variables:  
  `gedit ~/.bashrc`  
  `export MUJOCO_KEY_PATH=$MUJOCO_KEY_PATH!:~/.mujoco`  
  `export LD_LIBRARY_PATH=$LD_LIBRARY_PATH!:~/.mujoco/mujoco200/bin`  
  `export LD_LIBRARY_PATH=$LD_LIBRARY_PATH!:~/.mujoco/mujoco150/bin`  
  `source ~/.bashrc`  

#### Create conda environment

1. First execute `conda config --set restore_free_channel true`, details see [here](https://github.com/katerakelly/oyster/issues/16)
2. Then create the environemnt with `conda env create -f docker/environment.yml`
3. To make use of the module `rand_param_envs`, add `export PYTHONPATH=$PATH_TO_OYSTER$/rand_params_envs:$PYTHONPATH` to the `~/.bashrc` file as previous procedure.

Now the environment should be successfully configured.

#### Reproduce the Experiment
Experiments are configured via `json` configuration files located in `./configs`. To reproduce an experiment, run:
`python launch_experiment.py ./configs/[EXP].json`

By default the code will use the GPU - to use CPU instead, set `use_gpu=False` in the appropriate config file.

Output files will be written to `./output/[ENV]/[EXP NAME]` where the experiment name is uniquely generated based on the date.
The file `progress.csv` contains statistics logged over the course of training.
We recommend `viskit` for visualizing learning curves: https://github.com/vitchyr/viskit

Network weights are also snapshotted during training.
To evaluate a learned policy after training has concluded, run `sim_policy.py`.
This script will run a given policy across a set of evaluation tasks and optionally generate a video of these trajectories.
Rendering is offline and the video is saved to the experiment folder.

--------------------------------------
#### Communication (slurp!)

If you spot a bug or have a problem running the code, please open an issue.

Please direct other correspondence to Kate Rakelly: rakelly@eecs.berkeley.edu
