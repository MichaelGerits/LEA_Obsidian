---

Asset type:
  - CAD
URL 1: https://3dswym.3dexperience.3ds.com/post/enovia-user-community/revision-and-branching_GWFPdrZfTdqpIRKC7oF33A
---
### About asset

URL 1 is a community post on ENOVIA that describes in a pretty straightforward way how the 3Dx branch and revision system works. 

To create branches and revisions like that post describes, open your part/assembly/whatever in CATIA in the Collaborative Lifecycle App. In the Lifecycle tabs there are buttons for creating revisions, branches, and “merging” using “New revision from”. 

TLDR:

Revisions can be created by anyone afaik and it is in general a good practice to create revisions of a part every time some decision is made to modify it (e.g: the structures branch finds an issue with the current design at their work session and they decide to change it → revision!). Text can be added to revisions similarly to a GitHub commit so that we can keep track of what changed and why. 

New branches and merges can only be done by people with administrator access, usually 1-2 people within the team. It is a good practice to make a new branch within the current revision every time two or more alternative design directions are explored. When one option is chosen eventually as the one to move forward in the design cycle, we create a “New Revision From” using that branch. This is essentially equivalent to merging to main in GitHub.

![[Untitled.png]]

### Supporting files

![]()

[]()
