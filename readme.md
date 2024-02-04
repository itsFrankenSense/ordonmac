## Setup Guide for Bitcoin Core and Ord on macOS
This is a how-to guide for setting up Bitcoin-cli and an Ordinals client using Homebrew.

## Quick Reference
- [Node Setup](#node-setup)
  - [1. Install Homebrew](#1-install-homebrew)
  - [2. Install Bitcoin Core](#2-install-bitcoin-core)
  - [3. Set Up External Hard Drive (EXTERNAL HDD ONLY)](#3-set-up-external-hard-drive-external-hdd-only)
  - [4. Configure Bitcoin Core](#3-configure-bitcoin-core)
  - [5. Move Configuration File](#5-move-configuration-file)
  - [6. Start Bitcoin Daemon](#6-start-bitcoin-daemon)
  - [7. Verify Blockchain Information](#7-verify-blockchain-information)
- [Ord Setup](#ord-setup)
  - [1. Download and Install Ord](#1-download-and-install-ord)
  - [2. Create Ord Alias for External HDD Users (EXTERNAL HDD ONLY)](#2-create-ord-alias-for-external-hdd-users-external-hdd-only)
  - [3. Run Ord](#3-run-ord)
  - [4. Sync Ord](#4-sync-ord)
  - [5. Create & Fund Wallet](#5-create--fund-wallet)
  - [6. Create an Inscription](#6-create-an-inscription)
  - [7. Update Ord](#7-update-ord-client)

## Node Setup

### 1. Install Homebrew

First, you need to install Homebrew, a package manager for macOS. Open Terminal and run the following command.

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

For more information, visit [Homebrew's official website](https://brew.sh/).

### 2. Install Bitcoin Core

Once Homebrew is installed, you can install Bitcoin Core using Homebrew by running the following command in Terminal:

```bash
brew install bitcoin
```

### 3 Set Up External Hard Drive **(EXTERNAL HDD ONLY)**

Connect your external hard drive to your Mac and create a new folder named `BitcoinData`.

---

### 3. Configure Bitcoin Core
The Bitcoin Core Configuration file is responsible for setting the configuration for your node. This includes settings like enabling full index (required for Ordinals) and also where your data directory is stored (if you have chosen an external HDD).

To create a configuration file for Bitcoin Core, follow these steps using TextEdit on macOS:

1. **Open TextEdit:** Launch TextEdit from your Applications folder or Spotlight search.

2. **Make the Document Plain Text:** Before typing anything, convert your document to plain text. Choose `Format` > `Make Plain Text` from the menu bar at the top. If prompted, confirm that you want to convert the document to plain text.

3. **Enter Configuration Details:** Type or paste the following configuration into your new plain text document:

   ```
   txindex=1
   # Enables the transaction index which allows the node to retrieve information about any transaction in the blockchain.
   
   datadir=/Volumes/YourExternalDriveName/BitcoinData
   # Specifies a custom data directory for storing blockchain data. PLEASE NOTE this line is ONLY needed if you are not using an external drive.
   ```

   Be sure to replace `/Volumes/YourExternalDriveName/BitcoinData` with the actual path to your external hard drive's Bitcoin data folder.

4. **Save the File:** Choose `File` > `Save`, and in the save dialog, enter `bitcoin.txt` as the file name. Make sure to select a location where you can easily find the file, such as your Desktop.

5. **Rename the File Extension:** Locate the `bitcoin.txt` file you just saved in Finder. Change the file extension from `.txt` to `.conf`, resulting in `bitcoin.conf`. If macOS asks if you are sure you want to change the file extension, confirm by clicking `Use .conf`.

By following these steps, you will have created a `bitcoin.conf` configuration file for Bitcoin Core with the specified settings, ready to be used in your Bitcoin node setup.

---

### 5. Move Configuration File

Navigate to the `~/Library/Application Support/Bitcoin` directory on macOS and move your `bitcoin.conf` file into this folder, follow these steps:

1. **Open Finder:** Click on the Finder icon in your Dock to open a new Finder window.

2. **Go to the Menu Bar:** Look at the top of your screen to find the menu bar while you have Finder active.

3. **Open the "Go" Menu:** Click on `Go` in the menu bar.

4. **Choose "Go to Folder":** From the dropdown menu, select `Go to Folder...` or use the shortcut `Shift + Command (⌘) + G` to open the Go to Folder dialog.

5. **Enter the Path:** Type `~/Library/Application Support` and click `Go`. This will take you directly to the Application Support folder within your Library. Locate the `Bitcoin` Folder.

    - If the `Bitcoin` folder does not exist, you will need to create it. You can do this by right-clicking (or Ctrl-clicking) within the Application Support folder, selecting `New Folder`, and naming it `Bitcoin`.

6. **Move the `bitcoin.conf` File:** Locate the `bitcoin.conf` file you previously created and saved. You can drag this file from its current location (e.g., Desktop) and drop it into the `Bitcoin` folder you've just navigated to in Finder.

By following these steps, you will have successfully moved your `bitcoin.conf` file to the correct directory for Bitcoin Core to use when it runs.

### 6. Start Bitcoin Daemon

To start the Bitcoin daemon, run:

```bash
bitcoind
```

To stop Bitcoin Core, use `Ctrl+C`.

### 7. Verify Blockchain Information

To verify the sync progress, from any terminal window, you can check the blockchain status with:

```bash
bitcoin-cli getblockchaininfo
```

## Ord Setup

### 1. Download and Install Ord

Install Ord using Homebrew:

```bash
brew install ord
```

### 2. Create Ord Alias for External HDD Users **(EXTERNAL HDD ONLY)**
When using an external hard drive with ORD, it's necessary to specify the locations of your bitcoin cookie file and the ORD data directory each time you run ORD. To streamline this process, you can create an 'alias' that automatically appends the required paths to your commands.

If you're using Zsh (the default shell in macOS Catalina and later), follow these steps to make the `ord` alias permanent:

1. **Open Terminal.**

2. **Edit Your `.zshrc` File:** Use a text editor to open your `~/.zshrc` file. You can do this by typing the following command in the Terminal:

   ```bash
   nano ~/.zshrc
   ```

   This opens the `.zshrc` file in Nano, but you can use any text editor you prefer.

3. **Add Your Alias to the `.zshrc` File:** Scroll to the bottom of the file (yours may be empty) and add the alias command. Make sure to replace the placeholder paths with the actual paths on your system:

   ```bash
   alias ord='ord --data-dir /Volumes/YourExternalDriveName/ORD --cookie-file /Volumes/YourExternalDriveName/BitcoinData/.cookie'
   ```

   Ensure there are no typos and that the paths are correct for your setup.

4. **Save Your Changes:** If using Nano, save your changes by pressing `Ctrl + O`, then press Enter, and exit by pressing `Ctrl + X`.

5. **Apply the Changes:** To make sure your current terminal session recognizes the new alias, source your `.zshrc` file by running:

   ```bash
   source ~/.zshrc
   ```

6. **Verify the Alias:** Try using the `ord` alias in your terminal to ensure it works as expected.

### 3. Run Ord

Start the Ord server:

```bash
ord server
```

Once the Ord Server is running, you can access your own local version on http://0.0.0.0:80. 


### 4. Sync Ord

You need to allow some time for Ord to synchronize with Bitcoin Core, specifically for indexing inscriptions. Depending on the specifications of your computer, this process could span several days.

To streamline the setup process (depending on your internet speeds), you can download a pre-built index from the following link: [Shared Delusion Index](https://ordstuff.info/Downloads/indexes/).

- **For Internal HDD Users**: Place the downloaded file in the following directory:
  
  ```
  ~/Library/Application Support/ord
  ```

- **For External HDD Users**: Place the downloaded file in the directory you specified during step 3 of the Ord Setup.

### 5. Create & Fund Wallet

To create a new wallet, run:
```bash
ord wallet create
```
Make sure to securely store the 12-word mnemonic seed phrase provided.

To generate a receiving address for funding:
```bash
ord wallet receive
```
Use this address to send funds for future inscriptions.

---

### 6. Create an Inscription
Create your first inscription! You can use any file you want - I'd suggest a small text file to get started.

To create an inscription, use the following command:

```bash
# To simulate the inscription without actually inscribing, add --dry-run to the command. Remove it when you're ready to inscribe for real.
ord wallet inscribe --fee-rate <FEE_RATE> --file "FILE" --dry-run

# Command for actual inscription (remove --dry-run):
ord wallet inscribe --fee-rate <FEE_RATE> --file "FILE"
```

For instance:
```bash
ord wallet inscribe --fee-rate 45 --file "D:\Files\1.txt"
```
### 7. Update Ord Client

To ensure your Ord client is up to date, follow these steps:

1. **Check Your Current Version:**

   Determine the version of your Ord client by running:

   ```bash
   ord --version
   ```

2. **Update Ord:**

   First, update Homebrew's formulae to ensure you have the latest version of Ord available:

   ```bash
   brew update
   ```

   Then, upgrade the Ord client using:

   ```bash
   brew upgrade ord
   ```

3. **Cleanup:**

   After updating, you can remove outdated versions and clean up your system with:

   ```bash
   brew cleanup
   ```
