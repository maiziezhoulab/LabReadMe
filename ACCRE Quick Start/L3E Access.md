# L3E Access — Maizie Zhou Lab (Restricted)

This directory lives in Vanderbilt ACCRE's **L3E secure environment**. Access is only
possible through the secure gateway `l3.accre.vu`, and all L3E data must stay inside
`/data/hs/maiziezhou_lab_restricted/`. Do **not** copy or move L3E data to your home
folder or any other group directory.

---

## 1. Before you connect

- **Off campus?** Connect to the **Vanderbilt VPN first.** You cannot reach
  `l3.accre.vu` without it.
- You must have completed the **L3E Acceptable Use Policy** in KnowBe4 and been added
  to the restricted group. 

---

## 2. Log in to the L3E gateway

From your local terminal (replace `vunetid` with your own VUnetID):

```bash
ssh vunetid@l3.accre.vu
```

**Enter your password:** you will be prompted for your VUnet password. Type it and
press Enter. (Note: the terminal will not show any characters as you type — that is
normal.)

**Two-factor authentication (2FA):** after the password you will be prompted for 2FA
via Okta. You have two options:

- Type `PUSH` and press Enter → approve the push notification in the **Okta mobile
  app**, or
- Open the **Okta mobile app**, read the 6-digit code, type it in, and press Enter.

Once authenticated you land on the gateway (your prompt will show something like
`vunetid@sgw11`). Your L3E directory is:

```
/data/hs/maiziezhou_lab_restricted/
```

---

## 3. Important: what the gateway can and cannot see

- The gateway **only mounts the L3E shares.** You will **not** be able to see your
  normal group directories from `l3.accre.vu`.
- You **can** see both your L3E directory **and** your regular group directories from a
  **compute node**.
- You can still submit Slurm jobs from the gateway, but any work that needs your regular
  group directories must be done from a compute node (see below).

---

## 4. Get onto a compute node (to access regular group directories)

From the gateway, request an interactive session with Slurm. This drops you onto a
compute node where **both** your L3E directory and your regular group directories are
visible.

Using `salloc`:

```bash
salloc --time=4:00:00 --mem=8G --cpus-per-task=2
```

Or, to get an interactive shell directly, using `srun`:

```bash
srun --time=4:00:00 --mem=8G --cpus-per-task=2 --pty bash
```

- The job may **queue** and wait for resources (`queued and waiting for resources`).
  This is normal — requesting fewer resources generally gets you scheduled faster.
- When the job starts, your prompt changes from the gateway name (`sgw11`) to a compute
  node name. You now have access to your regular group directories.
- If Slurm complains about a missing account or partition, you may need to add
  `--account=...` or `--partition=...` — confirm the correct values with ACCRE.
- When you are finished, type `exit` to release the node so you are not holding
  resources.

Adjust the resource flags to your needs:
`--time` (walltime, `HH:MM:SS`), `--mem` (total memory), `--cpus-per-task`, `--nodes`.

---

## 5. Transferring data into your L3E directory

VUIT security requires an **encrypted** transfer method. Use `scp` or `sftp`.

Copy a local file **into** your L3E directory:

```bash
scp /path/to/localfile vunetid@l3.accre.vu:/data/hs/maiziezhou_lab_restricted/
```

Copy a whole folder (recursive):

```bash
scp -r /path/to/localfolder vunetid@l3.accre.vu:/data/hs/maiziezhou_lab_restricted/
```

Or start an interactive `sftp` session:

```bash
sftp vunetid@l3.accre.vu
# then, inside sftp:
#   cd /data/hs/maiziezhou_lab_restricted/
#   put localfile
#   get remotefile
```

You will be prompted for your password and 2FA the same way as an SSH login.

> **Rule:** L3E data must only live in `/data/hs/maiziezhou_lab_restricted/`. Do not move
> or copy it to your home folder or any other group directory.

---

## 6. Using VS Code (optional)

- VS Code Remote-SSH can connect to the **gateway** (`l3.accre.vu`). Because the gateway
  only mounts L3E shares, the editor/file-explorer panels will only show L3E directories.
- To reach your regular group directories, open the **integrated terminal** and start an
  interactive session (`salloc`/`srun`, see Section 4). Work in that terminal.
- Tip for 2FA: enable SSH connection multiplexing in your **local** `~/.ssh/config` so
  you only authenticate once:

  ```
  Host l3.accre.vu
      HostName l3.accre.vu
      User vunetid
      ControlMaster auto
      ControlPath ~/.ssh/cm-%r@%h:%p
      ControlPersist 10m
  ```

  Log in once in a terminal (complete 2FA), keep that terminal open, then let VS Code
  reuse the connection.

---

## 7. Need help?

If you run into problems transferring data or connecting, contact the ACCRE / L3E admin
who handled your onboarding. They can consult VUIT security if additional support is
needed.
