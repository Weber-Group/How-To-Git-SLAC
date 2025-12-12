Working with git heavily streamlines code distribution, version tracking, and branching for different beamtimes. However, connecting this (or any) GitHub repository into your S3DF directory is not as straightforward as setting up github on your local computer. Below are the steps to do this.
## Connecting a Git Repository to GitHub via SSH on the S3DF Cluster
To make push/pull requests from Github, there is a little extra legwork, but we only have to do this once and it (should) never have to be done again. Since we are on the S3DF cluster, we cannot use HTTP requests to pull/push changes from the remote repository. We have to use SSH.
### 1. Check for Existing SSH Keys
```bash
ls -al ~/.ssh
```
- Look for files named id_rsa and id_rsa.pub or similar. These are your SSH private and public keys.
- If no keys are present, proceed to the next step to generate one.
### 2. Generate a New SSH Key (if needed)
```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```
- Replace "your_email@example.com" with the email associated with your GitHub account.
- Press Enter to save the key in the default location (```~/.ssh/id_rsa```).
- Optionally, set a passphrase for added security.
### 3. Add the SSH Key to the SSH Agent
Start the SSH agent:
```bash
eval "$(ssh-agent -s)"
```
Add your private key to the agent:
```bash
ssh-add ~/.ssh/id_rsa
```
### 4. Add the SSH Key to Your GitHub Account
Copy your public SSH key to the clipboard:
```bash
cat ~/.ssh/id_rsa.pub
```
Highlight and copy the full output.
Go to your GitHub account:

Navigate to Settings > SSH and GPG keys > New SSH key.
Paste the key into the "Key" field and give it a descriptive title, something like "S3DF".
Click Add SSH key.
### 5. Test the SSH Connection
Run:
```bash
ssh -T git@github.com
```
If successful, you'll see a message like:
```vbnet
Hi username! You've successfully authenticated, but GitHub does not provide shell access.
```
---
Great! Now that you have that set up, you never have to do it again. Now, we can move on to creating or cloning a repository.
## Creating a repository and linking it to GitHub
The simplest way to get started is to use GitHub's guidance. Create a new repository without a .gitignore or README.md file. In the quickstart menu, select SSH as the protocol, and then copy the first set of commands. Navagate to the directory where you want this repository to be saved and paste the commands into the terminal and press enter. Now, you're all set up!
- Use
  ```bash
  git add <filename>
  ```
  to add files to the repository or
  ```bash
  git add .
  ```
  to add all files.
- To commit changes, use
  ```bash
  git commit -m "Put your message here"
  ```
  and add your own message to tell others what you did.
- To push your commits to GitHub, simply use
  ```bash
  git push
  ```
- And to pull changes from GitHub, simply use
  ```bash
  git pull
  ```
## Tips and Tricks
- Add a .gitignore to your directory to specifically mark certain files to not be added to the repository. Specifically in regard to the S3DF cluster, it would probably be a good idea to ignore any hdf5 files, as these are usually pretty large and GitHub has a file size limit of 100 Mb. To do this, simply run the command
  ```bash
  echo '*.h5' >> .gitignore
  ```
  This will create the .gitignore file and add the string "*.h5" into it, which tells git to ignore any file that has the extensions .h5. More documentation on .gitignore syntax can be found easily online.
  
