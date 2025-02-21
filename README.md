# Nexus
The Nexus Layer 1 is a planetary-scale supercomputer, designed to host the world’s commerce.

This is complete guide how to install and run your Nexus Node by 

**== Jinxpro Ngaprek ==**


## Requirements

1. Make screen to make sure the node running in the background
   ```
   screen -S nexus
   ```
    
3. Install pkg-config libssl-dev
   ```
   apt install -y pkg-config libssl-dev
   ```

4. Install protobuf-compiler
   ```
   apt install -y protobuf-compiler
   ```

5. Install Cargo
   ```
   curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
   ```
  Select 1 for default installation
   ```
   source $HOME/.cargo/env
   ```
6. Install build-essential
   ```
   apt install -y build-essential
   ```
    
8. Allocate Memory
   ```
   sudo fallocate -l 10G /swapfile && sudo chmod 600 /swapfile && sudo mkswap /swapfile && sudo swapon /swapfile && echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab && sudo sysctl vm.swappiness=100 && echo 'vm.swappiness=100' | sudo tee -a /etc/sysctl.conf
   ```


## Install and Run Node

1. Install Node
  ```
  curl https://cli.nexus.xyz/ | sh
  ```
2. Agree user aggrements by typing "Y" if prompted
3. Enter your Node ID

## Setup Auto Reconnect Script
1. Make auto.sh file and paste the script
  ```
#!/bin/sh

# -----------------------------------------------------------------------------
# 1) Ensure Rust is installed.
# -----------------------------------------------------------------------------
rustc --version || curl -sSf https://sh.rustup.rs | sh -s -- -y

# -----------------------------------------------------------------------------
# 2) Define environment variables and colors for terminal output.
# -----------------------------------------------------------------------------
NEXUS_HOME="$HOME/.nexus"
GREEN='\033[1;32m'
ORANGE='\033[1;33m'
NC='\033[0m'  # No Color

# Ensure the $NEXUS_HOME directory exists.
[ -d "$NEXUS_HOME" ] || mkdir -p "$NEXUS_HOME"

# -----------------------------------------------------------------------------
# 3) Check for 'git' availability. If not found, exit.
# -----------------------------------------------------------------------------
git --version 2>&1 >/dev/null
if [ $? -ne 0 ]; then
  echo "Unable to find git. Please install it and try again."
  exit 1
fi

# -----------------------------------------------------------------------------
# 4) Clone or update the network-api repository in $NEXUS_HOME.
# -----------------------------------------------------------------------------
REPO_PATH="$NEXUS_HOME/network-api"
if [ -d "$REPO_PATH" ]; then
  echo "$REPO_PATH exists. Updating."
  (
    cd "$REPO_PATH" || exit
    git stash
    git fetch --tags
  )
else
  (
    cd "$NEXUS_HOME" || exit
    git clone https://github.com/nexus-xyz/network-api
  )
fi

# -----------------------------------------------------------------------------
# 5) Check out the latest tagged commit in the repository.
# -----------------------------------------------------------------------------
(
  cd "$REPO_PATH" || exit
  git -c advice.detachedHead=false checkout "$(git rev-list --tags --max-count=1)"
)

# -----------------------------------------------------------------------------
# 6) Run the Rust CLI in an infinite loop (auto-restart if killed), auto-confirming prompts.
# -----------------------------------------------------------------------------
while true; do
  echo "Starting Nexus CLI..."
  (
    cd "$REPO_PATH/clients/cli" || exit
    yes "y" | cargo run -r -- start --env beta  # Auto-confirm prompts
  ) < /dev/tty

  echo "Nexus CLI stopped. Restarting in 5 seconds..."
  sleep 5
done
  ```

2. Make the script executable
   ```
   chmod +x auto.sh
   ```
   
3. Run the script
   ```
   ./auto.sh
   ``` 
   
6. Check your point
   ```
   https://beta.orchestrator.nexus.xyz/users/yourwallet
   ```
   
7. How to read
   ```
   {"id":4295638,"nodeType":2,"testnet_two_points":3200,"lastUpdated":"2025-02-21T03:01:36.398Z"}
   ```
ID= Your node ID

testnet_two_points= Your node point
   
7. Done enjoy your cigarete and a cup of coffee
