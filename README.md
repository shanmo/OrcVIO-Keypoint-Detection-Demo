## readme 

This repo is used in [OrcVIO: Object residual constrained Visual-Inertial Odometry](http://me-llamo-sean.cf/orcvio_githubpage/), the related papers are: 

- [IROS 2020](https://arxiv.org/abs/2007.15107)
- [Journal version TBD]()

If you find this repo useful, kindly cite our publications 

```
@inproceedings{orcvio,
	  title = {OrcVIO: Object residual constrained Visual-Inertial Odometry},
          author={M. {Shan} and Q. {Feng} and N. {Atanasov}},
          year = {2020},
          booktitle={IEEE Intl. Conf. on Intelligent Robots and Systems (IROS).},
          url = {http://erl.ucsd.edu/pages/orcvio.html},
          pdf = {https://arxiv.org/abs/2007.15107}
}
```

## about

This repo demonstrates how to compute the covariances using MC dropout for the semantic keypoints in [OrcVIO](http://me-llamo-sean.cf/orcvio_githubpage/). 

### how to train 

- use [this repo](https://github.com/shanmo/OrcVIO-Keypoint-Detection-Training)
- pretrained weights are [here](https://www.dropbox.com/sh/ftr48u964auwje4/AAAX7rAeMLtZjeydqPpkyo8Za?dl=0)

### how to run 

* modify the path 

```
    root = '/home/erl/moshan/other_stuff/star_map_semantic_keypoints/'
    model_path = '/home/erl/moshan/orcvio_gamma/orcvio_gamma/pytorch_models/starmap/trained_models/with_dropout/model_cpu.pth'
    img_path = root + 'images/car2.png'
    det_name = root + 'det/car2.png'
```

### demo 

* run the main 

```
python src/main.py
```

* sample output 

![img](/assets/kp_cov.png)

### modifications to make it work with python 3

* modify pytorch

run

```
atom /lib/python3.6/site-packages/torch/serialization.py
```

and modify the code in `_load`

```
unpickler = pickle_module.Unpickler(f)
unpickler.persistent_load = persistent_load
result = unpickler.load()
```

change to

```
try:
    unpickler = pickle_module.Unpickler(f)
    unpickler.persistent_load = persistent_load
    result = unpickler.load()
except:
    unpickler = pickle_module.Unpickler(f, encoding='latin1')
    unpickler.persistent_load = persistent_load
    result = unpickler.load()
```

this is because the model is saved in python 2 but we use python 3

* modify mhParser

In src/mhParser.py, change the original part to this

```
for i in range(size // 2, det.shape[0] - size // 2):
  for j in range(size // 2, det.shape[1] - size // 2):
    pool[i, j] = (max(det[i - 1, j - 1], det[i - 1, j], det[i - 1, j + 1],
                     det[i, j - 1], det[i, j], det[i, j + 1],
                     det[i + 1, j - 1], det[i + 1, j], det[i + 1, j + 1]))
```

### reference 

- https://github.com/xingyizhou/StarMap
