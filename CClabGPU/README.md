# CClab GPU server introdution


## Physical location: Stevenson complex unit 5, 8th floor
Not so important tho

## Server IP: 10.8.129.58, use SSH to connect to the server (or anything you like)
exp: [ssh username@10.8.129.58], type in [PASSWD]

extra notice: you need to use the Vandy VPN if you are not remote from campus, refer to this: https://it.vanderbilt.edu/services/catalog/end-point_computing/network_access/remote-access/index.php

## GPU node info

-------------------------------------------------------------------------------
| NVIDIA-SMI 470.57.02    Driver Version: 470.57.02    CUDA Version: 11.4     |
|------------------------------------------------------------------------------
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|=============================================================================|
|   0  Quadro RTX 8000     On   | 00000000:19:00.0 Off |                  Off |
| 33%   37C    P8    20W / 260W |      5MiB / 48601MiB |      0%      Default |
|                               |                      |                  N/A |
-------------------------------------------------------------------------------
|   1  Quadro RTX 8000     On   | 00000000:1A:00.0 Off |                  Off |
| 34%   42C    P8    21W / 260W |      5MiB / 48601MiB |      0%      Default |
|                               |                      |                  N/A |
-------------------------------------------------------------------------------
|   2  Quadro RTX 8000     On   | 00000000:67:00.0 Off |                  Off |
| 34%   42C    P8    31W / 260W |      5MiB / 48601MiB |      0%      Default |
|                               |                      |                  N/A |
-------------------------------------------------------------------------------
|   3  Quadro RTX 8000     On   | 00000000:68:00.0 Off |                  Off |
| 34%   43C    P8    22W / 260W |     98MiB / 48593MiB |      0%      Default |
|                               |                      |                  N/A |
-------------------------------------------------------------------------------

so basically, we have 4 gpu nodes and each of those has 48g working memory. Super good for large batch training.

## Other questions?

Just contact me: yunfei.hu@vanderbilt.edu, or GOTO: SC5 5917 my office. 
