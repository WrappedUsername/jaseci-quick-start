# [Jaseci](https://docs.jaseci.org/) Quick Start for WSL using Ubuntu - Not Official Docs

- *These are only my notes, not official docs here, official docs are found in the link provided above*

- and [Official Jaseci Installation Guide Found Here](https://docs.jaseci.org/docs/docs/getting_started/installation)

<p align="left">
<img src="https://komarev.com/ghpvc/?username=jaseci-quick-start&label=Profile%20views&color=f79952&style=flat" alt="jaseci-quick-start" />
</p>

- Create .venv environment,

```bash
virtualenv venv
```

- Rename venv file to .venv
- Activating `.venv`

```bash
source .venv/bin/activate
```

- Start Redis server

```bash
sudo service redis-server restart
```

- Start Jaseci runtime

```bash
jsctl -m
```

- Create graph

```bash
jaseci > graph create -set_active true
```

- Compile the project

```bash
jaseci > sentinel register main.jac
```

- Create the alias list

```bash
jaseci > alias list
```

- Create the main.dot file

```bash
jaseci > graph get -mode dot -o main.dot
```

- Exit jsctl

```bash
jaseci > exit
```

- Create the main.pdf file

```bash
dot -Tpdf main.dot -o main.pdf
```

## Training the bi-encoder model with the training data

- If you are getting this error,

```bash
jaseci > jac run bi_enc.jac -walk train -ctx '{"train_file": "training_data.json"}'
Saving shared model to : modeloutput
shared model created
Traceback (most recent call last):
  File "/usr/local/lib/python3.10/dist-packages/jac_nlp/bi_enc/bi_enc.py", line 186, in train
    with open(t_config_fname, "w+") as jsonfile:
PermissionError: [Errno 13] Permission denied: '/usr/local/lib/python3.10/dist-packages/jac_nlp/bi_enc/utils/train_config.json'
2023-05-10 13:56:38,591 - ERROR - rt_error: bi_enc.jac:blank - line 10, col 43 - rule atom_trailer - Execption within action call bi_enc.train! 
2023-05-10 13:56:38,591 - ERROR - rt_error: bi_enc.jac:train - line 10, col 43 - rule atom_trailer - Internal Exception: 
{
  "success": false,
  "report": [],
  "final_node": "urn:uuid:e8bfb046-3719-4037-9874-456ff6790fc1",
  "yielded": false,
  "errors": [
    "bi_enc.jac:train - line 10, col 43 - rule atom_trailer - Internal Exception: ",
    "bi_enc.jac:blank - line 10, col 43 - rule atom_trailer - Execption within action call bi_enc.train! "
  ]
}
```

- Try this,

```bash
(venv) wrappedusername@Arrakis:~/jaseci-quick-start$ sudo jsctl -m jac run bi_enc.jac -walk train -ctx '{"train_file": "training_data.json"}'
[sudo] password for wrappedusername:
2023-05-10 14:05:27,231 - WARNING - rt_warn: bi_enc.jac:bi_enc.jac - line 2, col 4 - rule can_stmt - Attempting auto-load for bi_enc.train
2023-05-10 14:05:27.381448: I tensorflow/tsl/cuda/cudart_stub.cc:28] Could not find cuda drivers on your machine, GPU will not be used.
2023-05-10 14:05:27.432466: I tensorflow/tsl/cuda/cudart_stub.cc:28] Could not find cuda drivers on your machine, GPU will not be used.
2023-05-10 14:05:27.432895: I tensorflow/core/platform/cpu_feature_guard.cc:182] This TensorFlow binary is optimized to use available CPU instructions in performance-critical operations.
To enable the following instructions: AVX2 FMA, in other operations, rebuild TensorFlow with the appropriate compiler flags.
2023-05-10 14:05:28.223594: W tensorflow/compiler/tf2tensorrt/utils/py_utils.cc:38] TF-TRT Warning: Could not find TensorRT
shared model created
Using device for training ->  cpu
Saving shared model to : modeloutput
shared model created
100%|██████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 3/3 [00:00<00:00,  6.62it/s]
100%|██████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 3/3 [00:00<00:00,  7.14it/s]

            Epoch : 1
            loss : 0.14623326311508814
            LR : 0.0002307692307692308
```
