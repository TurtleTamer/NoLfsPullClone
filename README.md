# NoLfsPullClone

Scenario:
- Using Git LFS, and using GitHub's data pack or some other provider's LFS server.
- Large repo size X (e.g. 10GB) and large team size Y (e.g. 5).  
- Initial clone of repo will take 10GB * 5 = 50GB, which is the entire bandwidth for a GitHub data pack! 
  - => Need to buy more data pack just for bandwidth due to initial clone.

Solution:
- Assume a repo exists:
  - Someone to clone the repo (uses LFS bandwidth)
  - Others clone the repo without pulling LFS objects
  - Others to copy the \<repo\>/.git/objects/ folder from the initial clone (doesn't use LFS bandwidth)
  - Others to perform a Git LFS pull to populate the master branch with the LFS objects
  - Use Git (and Git LFS) as per normal after that
- Example in Windows command prompt, using this repository:
  - \>SET GIT_LFS_SKIP_SMUDGE=1
    - Tell Git LFS not to pull LFS objects
  - \>git clone https://github.com/TurtleTamer/NoLfsPullClone.git NoLfsPullClone
  - \>cd NoLfsPullClone
  - \>CreateLfsObjects.bat
    - To simulate copying of the \<repo\>/.git/objects/ folder
  - \>git lfs pull
    - Tell Git LFS to pull the LFS objects, and it checks the local cache which is populated, so it gets the LFS objects from there instead from the LFS server (saving bandwidth)
  - Exit this command prompt as we no longer need the environment variable GIT_LFS_SKIP_SMUDGE, and proceed to use Git normally

Other solutions:
- Buy more data packs duing initial cloning
- Host own LFS server

References:
- https://help.github.com/articles/about-storage-and-bandwidth-usage/
- https://github.com/git-lfs/git-lfs/issues/2406
- https://git-scm.com/book/en/v2/Git-Basics-Getting-a-Git-Repository
- https://github.com/git-lfs/git-lfs/wiki/Implementations
