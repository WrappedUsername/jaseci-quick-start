# [Jaseci](https://docs.jaseci.org/) Quick Start for WSL using Ubuntu - Not Official Docs

- *These are only my notes, not official docs here, official docs are found in the link provided above*

- and [Official Jaseci Installation Guide Found Here](https://docs.jaseci.org/docs/docs/getting_started/installation)

<p align="left">
<img src="https://komarev.com/ghpvc/?username=jaseci-quick-start&label=Profile%20views&color=f79952&style=flat" alt="jaseci-quick-start" />
</p>

## Install Jaseci and dependencies

To run commands for Jaseci we need a terminal that accepts bash arguments.

- The official Jaseci docs recommend using the Ubuntu terminal that comes as the default with WSL.
- To install WSL, first check if WSL is installed by running the following command in Windows powershell terminal,

```bash
wsl -l -v
```

- If nothing is returned like this,

```powershell
  NAME          STATE           VERSION
* kali-linux    Stopped         2
  Ubuntu        Running         2
```

- Run this in Windows powershell terminal to install WSL,

```bash
wsl --install
```

- Next restart your computer and open Ubuntu terminal
- Once opened again, make sure Ubuntu is updated, and feel free to do this regularly,

```bash
sudo apt update && sudo apt upgrade
```

## Add a new user to Ubuntu if you haven't already

```bash
sudo adduser your-username-here
```

- It will prompt you to enter a password, and you will not be able to see the password as you enter it, but it is there!
- Once you have created your new user home, you can login using the `login` command,

```bash
login your-username-here
```

- Grant root permissions for this new user, use `visudo` to edit the sudoers file

```bash
root@YourComputerName:~# visudo
```

- Use down arrow key to scroll to the following section:

```bash
# User privilege specification
root    ALL=(ALL:ALL) ALL
```

- Add your user name to this list below root, like this:

```bash
# User privilege specification
root    ALL=(ALL:ALL) ALL
YourUsernameHere! ALL=(ALL:ALL) ALL
```

- Press `Ctrl x` on your kewboard to exit, then press `y` to save, and click `Enter` to finish.

## Verify the permission change

Use `su` followed by `YourUsernameHere!` to switch to the new user account:

```bash
root@YourComputerName:~# su - YourUsernameHere!
YourUsernameHere!@YourComputerName:~$
```

- Use `sudo -i` to verify that the user account can elevate permissions. At the prompt, enter the new user’s password:

```bash
YourUsernameHere!@YourComputerName:~$ sudo -i
[sudo] password for YourUsernameHere!:
root@YourComputerName:~#
```

- Use `whoami` to verify that you are currently the root user:

```bash
root@YourComputerName:~# whoami
root
```

## Install Jaseci

- Install Jaseci dependencies

```bash
apt-get install python3.10-dev python3-pip git g++ build-essential pkg-config cmake
```

- Install and upgrade pip to the latest

```bash
pip install --upgrade pip
```

- Install Jaseci

```bash
pip install jaseci
```

- Test the Jaseci install, to ensure our installation is working run,

```bash
jsctl -m
```

## Quck start

- Clone this repo into your new project folder!

- Create .venv environment,

```bash
virtualenv venv
```

- Rename venv file to .venv
- Activate `.venv`

```bash
source .venv/bin/activate
```

- Start the Redis server

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

- Training the bi-encoder model with the training data.

- Starting the Redis server

```bash
sudo service redis-server restart
```

```bash
jsctl -m
```

```bash
actions load module jac_nlp.bi_enc
```

- Exit Jaseci, and run the following command,

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

- The training epoch is set in the bi_enc.jac file, I set this to 100.

```bash
Epoch: 100%|████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 100/100 [01:20<00:00,  1.25batch/s]
{
  "success": true,
  "report": [],
  "final_node": "urn:uuid:faed4ef8-45b8-4fd1-b18c-5f2a467f8b66",
  "yielded": false
}
```
