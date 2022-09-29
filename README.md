## Install dependencies
Create environment: `conda env create -f environment.yaml`
Activate Environment: `conda activate neural_pathway`


---

# Offline Multitasks 

* `Directory: Offline\`

#### Run trained agents

`python run_trained_agent.py --env_name metaworld --algo_name IQL --eval_episodes 100 --seed 0 --clip_grad --log_dir trained_data --save_video`

#### Train agent
Here we have provided with expert dataset to train MT10.
`python main_parallel.py --algo_name='IQL' --env_name='metaworld' --clip_grad`

#### Trained agents






**Halfcheetah Multitask**
<p float="left">
  <img src="https://imgur.com/HY1ZwBt.gif" width="200" />
  <img src="https://imgur.com/kKrcBme.gif" width="200" /> 
  <img src="https://imgur.com/5BNWfRB.gif" width="200" />
  <img src="https://imgur.com/MKsPDK6.gif" width="200" />
  <img src="https://imgur.com/X6k0xc2.gif" width="200" />
</p>



**Quadrupod Multitasks**
<p float="left">
  <img src="https://imgur.com/u4TOrkV.gif" width="200" />
  <img src="https://imgur.com/EaYfZbE.gif" width="200" /> 
  <img src="https://imgur.com/4FndsXo.gif" width="200" />
  <img src="https://imgur.com/5uCm1Ty.gif" width="200" />
</p>


**Halfcheetah Constrained Multitasks**

* **NOTE**: following trained in restricted veloctiy evaluated regular env
<p float="left">
  <img src="https://imgur.com/DnulPLY.gif" width="200" />
  <img src="https://imgur.com/Fxy2tcK.gif" width="200" /> 
  <img src="https://imgur.com/ZsIVKUO.gif" width="200" />
  <img src="https://imgur.com/arYIcnC.gif" width="200" />
  <img src="https://imgur.com/scLhoCr.gif" width="200" />
</p>


---

# Online Multitasks 
* `Directory: Online\`

#### Train agent

* **Train for single task**
`python train.py seed=0 hidden_dim=1024 batch_size=1024 env="halfcheetah_forward" env_type="multitask" keep_ratio=0.05 iterative_pruning=True continual_pruning=False lr=0.001 mask_update_mavg=5`
    * **Important hyper-parameter**:
        * `env` choose from `halfcheetah_forward, halfcheetah_backward, halfcheetah_jump, halfcheetah_forward_jump, halfcheetah_backward_jump`
        * `mask_update_mavg`: if >1 it uses equation $(2)$.
        * `continual_pruning` : when sets `False`, it check if performance is reached to `threshold` value. For Halfcheetah it's fixed to be episodic return 2000. 
            - if `True` and `mask_update_mavg=1` will generate `figure 3(a) (DAPD)`
            - if `False` and `mask_update_mavg=1` will generate `figure 3(a) (DAPD) with threhold`
    * **Baseline performance**:
        - `python train.py seed=0 hidden_dim=1024 batch_size=1024 env="halfcheetah_forward" env_type="multitask" keep_ratio=0.05 iterative_pruning=False continual_pruning=False lr=0.001 mask_update_mavg=1`

* **Train MetaWorld multitask**
`python run_multitask.py --seed=0 --hidden_dim=400 --batch_size=128 --keep_ratio=0.05 --lr=0.001 --mask_init_method=random --mask_update_mavg=1 --ips_threshold=800 --env_type=metaworld --grad_update_rule=pmean --env MT10 --snip_itr 1 --hidden_depth 3 --experiment B1M_HD400_D3_pretrained_mask_MT10 --num_train_steps 2000000 --optimization_type "adamW" --num_pretrain_steps=10000 --pretrain_masks`
`
    * **Important hyper-parameter**:
        * `mask_update_mavg`:  if >1 it uses equation $(2)$.
        * `ips_threshold`: define the expected threhold, which determins whether to stop searching for pathways.
        * `pretrain_masks`:
            - if `True` pretrains pathway for each task
            - if `False` configures pathway during multitask learning
        * `num_pretrain_steps`: number of steps for which pathways are pretrained before multitask training.
`
