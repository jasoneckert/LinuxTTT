# Redirection
   * Open a Terminal on your Fedora Workstation virtual machine as root
   - `cd classfiles`
   - `df -hT >diskspace` 
   - `date >>diskspace`
   - `cat diskspace`
   - `ll letter lett >good 2>bad`
   - `cat good`
   - `cat bad`
   - `cp /etc/hosts .`
   - `cat hosts`
   - `sort hosts >hosts`
   - `cat hosts` (the file is empty because the shell clears an existing file before writing stdout to it with the `>` symbol - you should use `>>` to append output or redirect the sorted output to a different file)
   - `tr a A <letter >newletter`
   - `cat newletter`
   - `ls <letter` (`ls` runs normally because it does not accept stdin)

# Piping
   * Open a Terminal on your Fedora Workstation virtual machine as root
   - `cd classfiles`
   - `grep -i mother letter | wc -l`
   - `cat letter | pr -d | less`
   - `cat proposal1 | tr a W | sort | tee newfile`
   - `cat newfile`
   - `cat Poems/Shakespeare/sonnet* | wc -l`
   - `grep -i love Poems/Shakespeare/sonnet* | wc -l`

# Shell Variables
   * Open a Terminal on your Fedora Workstation virtual machine as root
   - `VAR1="Hello World"`
   - `echo $VAR1`
   - `set | grep VAR1`
   - `env | grep VAR1` (VAR1 is not shown, as it was not exported)
   - `export VAR1`
   - `env | grep VAR1`
   - `PS1='C:${PWD////\\\\}>  '` 
   - `cat ~/.bash_profile` (on Fedora, it runs .bashrc first if it exists)
   - `cat ~/.bashrc` (note the aliases for `rm`, `cp` and `mv`)
   - `exit` (open a new shell as the root user)
   - `echo $VAR1`
   - `vi ~/.bashrc` (add the following lines, save and quit)
     - `export VAR1="Hello World"`
     - `alias bye="echo Goodbye ; sleep 5 ; exit"`
     - `alias c=clear`
     - `alias bye="echo Goodbye ; sleep 5 ; exit"`
   - `source ~/.bashrc` (so you don't need to open a new shell)

# Shell Scripts (note that the `tar` command is discussed later)
   * Open a Terminal on your Fedora Workstation virtual machine as root
   - `cd classfiles`
   - `git clone https://github.com/jasoneckert/bashscripts.git`
   - `cd bashscripts`
   - `ls`
   - `chmod 755 script*`
   - `ls`
   - `cat script1.sh`
   - `./script1.sh` 
   - `cat name` (to see the report, where name = file name displayed)
   - `cat script1pretty.sh` (`echo` could instead be replaced by `printf`)
   - `./script1pretty.sh`
   - `cat name` (to see the report, where name = file name displayed)
   - `cat script2.sh`
   - `./script2.sh` (supply `/root/classfiles/Poems` when prompted)
   - `ls ~`
   - `cat script2arguments.sh`
   - `./script2arguments.sh /root/classfiles/Poems`
   - `cat script3.sh` (example of a system-specific maintenance script that would be scheduled)
   
# Advanced Text Tools
   * Open a Terminal on your Fedora Workstation virtual machine as root
   - `cd classfiles`
   - `cat /etc/passwd`        
   - `cut –d: -f3 /etc/passwd`		
   - `cut –d: -f3 /etc/passwd > passfile`
   - `cut –d: -f6 /etc/passwd > passfile2`
   - `paste passfile passfile2`	
   - `sdiff bashscripts/script1.sh bashscripts/script1pretty.sh`
   - `printf "Hello %s! \nYour shell is: %s \n" $LOGNAME $SHELL` 
   - `sed /Mother/s/the/BILLY/ letter` (on lines with Mother, replace the with BILLY)
   - `sed 1s/Mother/Grandma/ letter` (on line 1, replace Mother with Grandma)		
   - `sed '$s/Alan/Mike/’ letter` (on the last line, replace Alan with Mike)		
   - `sed 1,8s/the/WOWEE/g letter` (on lines 1-8, replace the with WOWEE)
   - `sed /the/d letter` (delete all lines with 'the')
   - `cat bashscripts/script2pretty.sh` (note the use of `sed`)
   - `bashscripts/script2pretty.sh` (supply `/root/classfiles/Poems` when prompted)
   - `ls ~`
   - `cat bin/treed` (Optional trace challenge for an advanced `sed` command - use AI if needed!)
   - `bin/treed .`
   - `awk '/Mother/ {print}' letter` (print all lines with 'Mother')
   - `awk '/Mother/ {print $1, $4}' letter`	(on lines with 'Mother', print the first and fourth word separated by a space character)
   - `awk '/Mother/ {print $1 "\t" $4}' letter` (on lines with 'Mother', print the first and fourth word separated by a Tab character)
   - `awk –F: '/root/ {print $3, $5}' /etc/passwd` (on lines with 'root', print the third and fifth field in this colon-delimited file)
   
# Python Scripts 
   * Open a Terminal on your Fedora Workstation virtual machine as root
   - `cd classfiles/bashscripts`
   - `cat script1.sh` (copy the text, paste it into ChatGPT and ask it to convert it to Python, explaining the results to you!)
   - `vi script1.py` (go to INSERT mode and paste the ChatGPT Python version using Ctrl+Shift+V)
   - `chmod 755 script1.py`
   - `./script1.py` 
   - `ls ~`
   - Optionally repeat for other sample scripts, or generate your own using ChatGPT!
   
# Using Git
   * Open a Terminal on your Fedora Workstation virtual machine as root
   - `mkdir /scripts`
   - `cp /root/classfiles/bashscripts/* /scripts`
   - `cd /scripts`
   - `ls`
   - `git config --global user.name name` (where name is your name)
   - `git config --global user.email emailaddress` (where emailaddress is your email address)
   - `git config --global init.defaultBranch main` (sets the default branch to main)
   - `git init`
   - `git status`
   - `git add .`
   - `git status`
   - `git commit -m "My first snapshot"`
   - `git log`
   - `vi script1.sh` (make a change of your choice and save your changes)
   - `git status`
   - `git add .`
   - `git commit -m "Changed script1.sh"`
   - `git log` (copy the first 7 characters of the commit ID)
   - `git reset --hard "commitID"` (commitID is the pasted fist 7 letters of the commit ID from previous step)
   - `git log`
   - `cat script1.sh` (note you've reverted to your first snapshot)
   - `cd` (let's return to our home directory and pretend we're a developer working on the central /scripts repo)
   - `git clone /scripts`
   - `ls` (note the copy in your home directory - it remembers the origin!)
   - `cd scripts`
   - `ls`
   - `git checkout -b newfeature` (creates a new branch for us to add our new feature to)
   - `git branch`
   - `vi script1.sh` (make some changes of your choice)
   - `git add .`
   - `git commit -m "Added new feature"`
   - `git push origin newfeature` (this pushes our newfeature branch to the origin, the /scripts directory)
   - `cd /scripts` (pretend you're the senior developer on the project, reviewing this new feature)
   - `git branch`
   - `cat script1.sh` (you don't see the new feature because you're still in the main branch)
   - `git checkout newfeature`
   - `cat script1.sh` (now you see the new feature because you're in the newfeature branch - you could test it too!)
   - `git checkout main`
   - `git branch` (you're now back at the main branch and want to incorporate the new feature because you have approved it after testing)
   - `git merge newfeature` (merges the contents of the newfeature branch into the main branch)
   - `cat scriptname` (the new feature is now part of the main branch and other developers can pull a new copy from the origin with `git pull`)
   * Optionally create a free account on GitHub and create a public "scripts" repo. You can upload your files to that repo directly on the GitHub website. 
   * Next, use `git clone https://github.com/username/scripts.git` on your local system. 
   * If you make changes locally and want to push them to the origin (GitHub scripts repo), the `git push` command will be prompted to log in with your GitHub username and a personal access token (very long password) that is used in place of your real GitHub password for pushing. 
   * You can generate a personal access token on the GitHub website by clicking your user icon and navigating to Settings > Developer settings > Personal access tokens. When generating a personal access token, you must supply a description, expiration and select the repo scope for it to work. 
