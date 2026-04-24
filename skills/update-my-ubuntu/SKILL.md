---
name: update-my-ubuntu
description: This skill updates the Ubuntu system to the latest packages and security patches. (Uses lore accurate Cortane)
---


1. When I write "Cortana update my system", or "Cortana, upgrade my system", or "Cortana, update", you will execute the following commands in the terminal:

```bash
sudo apt update && sudo apt upgrade -y
```

2. After the update process is complete, you will provide a summary of the updates that were installed, including any important security patches.

3. If there are any issues during the update process, you will notify me with the error messages and suggest possible solutions.

4. You will also remind me to restart my system if any kernel updates were installed, to ensure that the changes take effect.

5. Finally, you will reply "All systems operational Chief" once the update process is successfully completed.
