# PC Set Up for Bioinformatics Analysis

> ## Enable Bash on macOS
> Mac by default use z-shell. Most of my instructions will be bash-based. So, to enable the bash shell in MacOS run the following code in the terminal.
> ```bash
> chsh -s /bin/bash
> ```
> Then, enter your user password and restart your terminal.  
> **Note: If you want to go back to z-shell, just run the following command and restart your terminal again**:
> ```bash
> chsh -s /bin/zsh
> ```
> 

## 1. Install `micromamba`

> Read the document carefully and follow the steps in order.
>
> Do not change the code if you are not absolutely sure what you are doing.

### 1.1. Update and Upgrade Linux

Open terminal (Shortcut Key: **`Ctrl + Alt + T`**).

> **Tips:** To copy and paste in linux terminal use **`Ctrl + Shift + C`** and **`Ctrl + Shift + v`** respectively.
>
> **Tips 2**: At any point if your codes are messed up or maybe hanging for a long time and you want to cancel the process use **`Ctrl + c`**.

```bash
sudo apt update && sudo apt upgrade -y
```
**Code Explanation:**
- **`sudo`**: Allows to access the system as super user or root user or administrator.
- **`apt`**: "Advanced Package Tool". It is a package management command-line tool used in Debian-based Linux distributions, such as Ubuntu. It simplifies the process of installing, updating, and removing software on Linux systems.
- **`update`**: Refresh the local package index to ensure the latest information about package versions from the configured repositories.
- **`&&`**: The double ampersand (&&) is a logical AND operator in the command line. It ensures that the second command is executed only if the first command is successful.
- **`upgrade`**: command is used to upgrade installed packages to their latest versions.
- **`-y`**: is optional, but it's commonly used in automated scripts or when you want to avoid manual **confirmation** during upgrades.

### 1.2. Install `curl`
This is a command-line tool for downloading content. Currently, ubuntu doesn't contain **`curl`** by default.
```bash
sudo apt install curl -y
```
**Code Explanation:**
- **`install`**: command is used in **`apt`** tool to install a package.


### 1.3. Create a backup copy of the `.bashrc` file

The `.bashrc` is an important file for your system to function properly. `micromamba` will edit this file automatically (which is okay) for its functionality. Some if anything goes wrong your system may not work perfectly. So, to be on the safe side, before installing `micromamba`  just make a backup copy of it.

```bash
cp ~/.bashrc ~/.bashrc.bkup
```

### 1.4. Install `micromamba` directly and easily

**This method is recommended for beginners.**

Copy the following code and run it in your terminal.

> We will install it in the default location. 

```bash
"${SHELL}" <(curl -L micro.mamba.pm/install.sh)
```

After running the code, it will ask:

- **`Micromamba binary folder? [~/.local/bin]`** 
  - As we are installing in the default location, you don't have to type anything here. Just press **Enter**. 
  - If you want to want to change the location for the binary file, write **full/absolute path** of the location.
- **`Init shell (bash)? [Y/n]`**: 
  - Type **Y** and press **Enter**.
  - This setting will initialize micromamba under the current shell (in our case bash shell).
  - It will add some lines in the ~/.bashrc file to make the changes permanent.
- **`Configure conda-forge? [Y/n]`**: 
  - Type **Y** and press **Enter**.
  - By this answer, you instructed micromamba to set **conda-forge** channel in the highest priority while installing tools from conda.
  - You can configure this setting from **~/.condarc** file.
- **`Prefix location? [~/micromamba]`**:
  - This location is perfect. So you don't have to type anything. Just press **Enter**.
  - In this location, micromamba will store the environment files and folders with cache data.
  - When you will install software it will be installed in this location.

#### Now restart the terminal or, type the following code in the terminal

```bash
source ~/.bashrc
```

That's it. Congratulations!! Micromamba installation is done!!!

To learn more about micromamba go to the following link: [Mamba’s documentation!](https://mamba.readthedocs.io/en/latest/index.html)

### 1.5. Uninstall `micromamba` from your PC

If for some reason you want to uninstall ` micromamba` from your PC, that's quite straight forward process. You have to just remove the relevant files and folders.

```bash
# Remove the micromamba binary file
rm -rf ~/.local/bin/micromamba

# Remove the micromamba folder
rm -rf ~/micromamba
```

**If you installed micromamba in different locations then remove those locations.**

#### Cleaning the `.bashrc` file (Optional)

As `micromamba` installation added some extra lines in the `.bashrc` file. you may want to remove these lines. Run the following code in the terminal:

```bash
sed -i '/# >>> mamba initialize >>>/,/# <<< mamba initialize <<<$/d' ~/.bashrc
```

During installation `micromamba` adds some code chunk in the `.bashrc` file that starts and ends with the line `# >>> mamba initialize >>>` .

By using the above code we will remove all the text between these two lines.

**Code explanation:**

- **`sed`**: This command stands for stream editor, and it's used for text manipulation.
- **`-i`**: This option tells `sed` to edit files in place. In other words, the changes will be made directly to the file specified without creating a backup.
- **`'/# >>> mamba initialize >>>/,/# <<< mamba initialize <<<$/d'`**: This is the sed script. It uses the `d` command to delete lines that match a specified range. In this case, it deletes lines between `# >>> mamba initialize >>>` and `# <<< mamba initialize <<<`.
- **`~/.bashrc`**: This is the file on which the `sed` command will operate. In this case, it's the `~/.bashrc` file.

**After running the code, restart your terminal to refresh the changes.**

### 1.6. Create a BASH profile (Optional but Recommended)

To make the terminal more user-friendly and function consistently we will create a custom profile file.

The original file was created by [The Biostar Handbook](https://www.biostarhandbook.com/). So, we save the file the same as their filename (**`biostar.sh`**)

Copy the whole code and paste it on the terminal. It will create a file with the name **`.biostar.sh`** in the home directory.

```bash
cat <<EOF > ~/.biostar.sh
# Credit: Biostar Handbook.
# A minimal BASH profile.
# This file is sourced by the .bashrc file.

# ls uses different flags for colors on Mac and Linux
if [ "$(uname)" == "Darwin" ]; then
  alias ls='ls -hG'
else
  alias ls='ls -h --color=auto'
fi

# Safe versions of the default commands.
# Ask permission before overwriting files.
alias rm='rm -i'
alias mv='mv -i'
alias cp='cp -i'

# Makes the prompt more user-friendly: colors, hostname, path, etc.
export PS1='\[\e]0;\w\a\]\n\[\e[32m\]\u@\h \[\e[33m\]\w\[\e[0m\]\n\$ '

# Necessary for the command line sort to work correctly.
export LC_ALL=C

# This is used on macOS to turn off zsh warning.
export BASH_SILENCE_DEPRECATION_WARNING=1
EOF
```

**Code Explanation:** All the content between **`<<EOF`** and **`EOF`** will be saved in the **`.biostar.sh`** file.

**Now link the `.biostar.sh` file with `.bashrc` file**

```bash
cat <<EOF >> ~/.bashrc

# BIOSTAR BASH PROFILE
source ~/.biostar.sh
# BIOSTAR BASH PROFILE

EOF
```

**After running the code, restart your terminal to refresh the changes.**

## 2. Install Bioinformatics Software

### 2.1. Rearrange conda channel priority

Run these two lines of code one by one.

Thus you are adding `conda-forge` later and it will be on the top of the priority list.

```bash
micromamba config prepend channels bioconda
micromamba config prepend channels conda-forge
```

### 2.2. Create a New Environment

We are going the create a new environment with the name **"bioinfo"**. 

We will also install the current **python 3.10 version** version at the same time.

```bash
micromamba create -n bioinfo python=3.10* -y
```

**Code Explanation:**

- **`create`**: This is a sub-command of **`micromamba`**  command to create a new conda environment.
- **`-n bioinfo`**: Specifies the name of the new environment, in this case, **"bioinfo"**.
- **`python`**: Indicates that Python is among the packages to be installed in the new environment.
- **`-y`**: Stands for "yes" and is used to automatically answer "yes" to any prompts or confirmations during the environment creation process.

### 2.3. Activate the bioinfo Environment

```bash
micromamba activate bioinfo
```

### 2.4. Install Bioinformatics Software

Now, we will install some bioinformatics software/tools/packages from the anaconda repository.

We can install the tools one by one, or, more than one, or, from a list.

```bash
# Install a single tool
micromamba install trimmomatic 

# Install more than one tool
micromamba install fastqc fastp
```

The code will ask for your confirmation **`[Y/n]`**. Type **`y`** and press **`Enter`**. Or, you can put a **`-y`** at the end of the code.

**Now lets create a list of some bioinformatics tools and save it in a file. Then we will use that file to install all the tools together.**

 Create the file and save it with the name **tools.txt**

> **NB**: Keep one tool name per line. And don't forget to activate bioinfo environment.

```bash
cat <<EOF > ~/tools.txt
bcftools
bedtools
blast
bowtie2
bwa
emboss
hisat2
mafft
minimap2
ncbi-datasets-cli
parallel
samtools>=1.14
seqkit
seqtk
ucsc-bedgraphtobigwig
EOF
```

**Now install all the tools together.**

```
micromamba install -f ~/tools.txt -y
```

Based on the internet speed of your PC it may take some time.

Each time you use one or more tools from this environment, you have to first activate the target environment.

That's it !! :boom:

Happy Learning !! :blush:
