
# Installation:

Hooks have to be:  
- either placed into templates (C:\Program Files\Git\share\git-core\templates\hooks\) for automatic placement in a newly created repository.  
- or placed under .git/hooks/ for each repository it is to be applied on. 
 
Mtimestore:  
- mtimestore has to be placed somewhere in $PATH, (i.e. /usr/bin/ under Linux or Program Files/Git/bin/ under windows)  

# Info:

### Note this file is just a copy paste of http://www.ktweb.cz/blog/index.php?page=page&id=116   

We had a problem with git not storing modify/acces times of files (which is from design not to confuse compilers with older times), which broke some diffing software (which relied on these). Unlike other available solutions we needed to store the actual modify times (not just commit times), so a new solution had to be written. This stores mtimes of edited files to a .mtimes file (which is passed to users over git server) and then applies them on every checkout. Hooks have to be applied on client-side. (You do not need to do anything on server-side.) This solution works well with cygwin/mingw versions of git, you may just need to copy some files from cygwin environment.  

The solution consists of 3 files:  
-mtimestore - core script providing 3 option -a (save all - for initialization in already existing repo (works with git-versed files)), -s (to save staged changes), and -r to restore them. This actually comes in 2 versions - a bash one (portable, nice, easy to read/modify), and c version (messy one but fast, because mingw bash is horribly slow which makes impossible to use the bash solution on big projects).   
-pre-commit hook  
-post-checkout hook   


an older, a bit deprecated package which uses these binaries may be found at http://ktweb.cz/blog/download/git-mtimestore-1.4.rar  

### HOW DOES IT INTERNALLY WORK/HOW TO USE IT   

- before every commit "mtimestore -s" is called which stores mtimes of staged files into .mtimes file (that is done by pre-commit hook)  
- after every checkout all mtimes in repo are corrected using "mtimestore -r" (that is done by the post-checkout hook)  
- on first use .mtimes has to be manually created using "mtimestore -a" and commited to the repository   
