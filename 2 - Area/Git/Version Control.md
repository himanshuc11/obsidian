It is a way to track changes to source code, how it modified evolved over time.

**Why version control?**
- Backup of all files and snapshot of a particular state of project
- Collaboration
- See how file modified, by whom, when and why

Types of VCS (Version Control Systems)
- CVCS (Centralised VCS)
	- A server acts as the main centralised repository which stores every version of code.
	- Every user commits directly to the main branch
	- Works for small teams with effective collaboration and communication
	- If someone breaks the production branch, other developers can’t check in their changes until it’s fixed.
	- ![[CVCS.png]]
- DVCS (Distributed VCS)
	- Every collaborator has a local copy of the centralised server.
	- Works in offline and every collaborator acts as a backup.![[Screenshot 2024-07-13 at 3.48.28 PM.png]]
	

**Git**
- Implementation of DVCS

Installation
`brew install git`
`git config --global user.name "<username>"`
`git config --global user.email "<useremail>"`



Resources
https://www.youtube.com/watch?v=hZS96dwKvt0&t=1936s
https://www.youtube.com/watch?v=zTjRZNkhiEU
https://www.atlassian.com/git/tutorials/why-git 
