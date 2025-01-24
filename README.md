# ⚠️ Important note
While the Peacock server officially support Linux, the patcher does not officially support Linux/Proton.
This guide is provided as is, it may not work with everyone's Linux setup and may require extra changes to make it work for you.

Because we're not Linux experts, we cannot guarantee to be able to help you fix your issues. You may check [our Discord](https://thepeacockproject.org/discord) in the Linux megathread help channel.
We're also open to pull requests and tips on how to improve this guide!

# Requirements
- `curl`
- `unzip`

# Installation
## Server
- Clone the repository:
```
git clone https://github.com/astra1dev/linux-steam-setup/
```
- Downloading the zip works too if you don't have git installed.
- `cd` to the newly cloned folder.
- Make `start.sh` executable with the following command: `chmod +x ./start.sh` 

Run `./start.sh` to verify the server is working correctly. If it's not, scroll down to the Troubleshooting section. If it's working, proceed with the next steps to get the Patcher working:

## Patcher
- Right-click the game in your Steam library -> Properties and paste the following line in the "Launch Options" box:
```bash
bash -c 'exec "${@/Launcher.exe/WineLaunch.bat}"' -- %command%
```
- Ensure it is copied *exactly*! 
- Press "Play" on Steam, the game should start alongside the Peacock Patcher. If your game starts up full-screen, then you will have to alt-tab out.
Steam Deck users can access the patcher by pressing the Steam button and navigating to the active windows at the bottom of the "HITMAN 3" section.
- Click into the combo box that says "Peacock Local", and enter `127.0.0.1:3000`, then click Re-patch. It's important to change this as the default "Peacock Local" expects it to live on port 80. The Patcher will save this setting and you won't be required to edit it again. 


**Your Game is now ready to play with Peacock!**

# Troubleshooting
If your issue isn't listed here, open an issue on GitHub or post in the [megathread](https://discord.com/channels/826809653181808651/1026456932007034910) on the Peacock Discord.

## Issues on the login screen
If you have issues on the login screen, check if you have the following entry in your `/etc/hosts` file:
```
127.0.0.1    localhost
```
While it's not recommended, you can modify `start.sh` and remove the `PORT=3000` to default back to loading Peacock on port 80 and run Peacock **as root**.

## Error: Cannot find module '/home/deck/build/linux-steam-setup/Peacock/chunk0.js'
These setup instructions wrap around the existing code from Peacock, and to simplify setup we attempt to pull this automatically. If you see this error, this likely means there was an issue pulling this down. This should be reported to us so we can correct the URL if needed; but this could also just be a network issue on your end. Verify other websites work before filing such an issue.

## ./node/bin/node: No such file or directory
This indicates there was some issue downloading NodeJS. This can be fixed yourself by either installing node yourself through your package manager of choice or by downloading the zip and moving it to where `start.sh` lives and rename the folder to `node` (such that `node/bin/node` should return), but this may also indicate something worth filing an issue for.

## [Error] Failed to use the server on 0.0.0.0:3000!
As the subsequent lines suggest, this error means that Peacock is already running somewhere else. `pkill node` should kill everything, but you should first verify with `ps aux | grep node` that the only thing you'll kill is the Peacock process (you should have output similar to the below)

```
deck       71017  1.5  1.1 1000684 138488 pts/5  Sl+  11:20   0:01 node ./Peacock/chunk0.js
deck       71116  0.0  0.0  52616  2416 pts/6    S+   11:21   0:00 grep --color=auto node
```
