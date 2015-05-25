Comparando repositorios git
=========

Qué repositorios de git hay y qué te ofrecen. 

Incluir una tabla de diferentes repositorios git y qué tipo de facilidades ofrecen al usuario. 


Hola soy la cuenta alias samiracho2, voy a hacer un pull request con estos cambios.

The Differences Between Mercurial and Git
I realized recently that I've been using distributed revision control for several years now. It's always been an exciting landscape for me, although it's been a bit lonely. I used gnu arch for most of my code for a long time, and dabbled some in darcs at the same time. It wasn't until I saw Brian O'Sullivan's tech talk on mercurial that I started wondering if my needs weren't being fully met. He showed what mercurial provided that my systems lacked and provided a generally useful tool, so I dove in and enjoyed it greatly.

Neither darcs nor gnu arch were particularly fast systems, but they did a reasonably good job. The most obvious difference between them was that darcs had a really nice UI, and the philosophy of gnu arch was all but against having a useful interface. The idea was that it was something on which revision control systems could be built, and wasn't so much one on its own.

I heard about git on the darcs list a while back, but didn't see what it provided that was so great. In its early days it was... well, considered difficult enough to use that only the sickest bothered. Once git started rising in popularity, I tried to understand what it had to offer that I was missing. Linus Torvalds' tech talk on git seemed to just be selling distributed systems (and pretty much said mercurial was decent). Randal Schwartz's git talk promised to tell me why I should use it instead of CVS, Subversion, SVK, Arch, Darcs, Mercurial, Monotone, Bazaar, and just about every other repository manager, but I didn't really get the impression he'd actually used any of these to compare with. Certainly didn't convince me.

But I eventually did start looking at git, only because there were projects to which I contribute that also use git. I've been taking the time to fully understand it and its ways and explored much of its dirty parts (but I'm not quite done yet). So now I feel I can start writing a bit about the differences I've encountered.

Technical Differences

The primary difference between distributed systems and centralized systems is in granularity. i.e. in a centralized system, there's no distinction between saving a change and making it available. In this regard, git is more granular than mercurial. There are lots of small parts that work together. It's possible for this to be mostly transparent to the user, but as we've seen in comparing the difference between centralized and distributed, it can be very beneficial in creating new types of workflows.

History is a DAG

In both git and mercurial, the history is just a directed acyclic graph, but mercurial attempts to provide a linear history and this has a negative effect in a few places. For one, the rev number is displayed a lot and people try to use it, but it varies from repo to repo and probably does little more than cause confusion.

git will show you from a particular point all of the changesets that led up to it by following the history backwards, but otherwise it represents what happens in the real world. From a given point, there's a change. That change may be desirable or may not be. It may have been at one point, but isn't any longer. The only thing that matters is a head.

And I believe this is where mercurial has gone wrong. A head in mercurial is inferred... it's just a point on the DAG where there are no children. A branch is inferred to be inactive when it's not a head.

Contrast to git where all heads are explicit. A tag or a branch points to a particular node in the graph, and there are tools to compare the changes between two nodes. This is obviously right for tags, but it's also really nice for heads. You can simply dump mass changesets into another repo (for example in a mob branch), but only include heads you feel are worth noting. It's this distinction that allows for private branches.

Mutability Tools

The culture of mercurial is one of immutability. This is quite a good thing, and it's one of my favorite aspects of gnu arch. If I commit something, I like to know that it's going to be there. Because of this, there are no tools to manipulate history by default.

git is all about manipulating history. There's rebase, commit amend, reset, filter-branch, and probably other commands I'm not thinking of, many of which make it into day-to-day workflows. Then again, there's reflog, which adds a big safety net around this mutability.

Branch Management

I heard many people say they felt git handled branches better, but nobody could explain why. I've described the foundation above, but I'll go into more detail here.

In mercurial there are two types of branches you might see as a user: named branches and repository clones. Cloning a repository is how you branch in darcs and is a really simple concept. When you want to do something different, you just clone your repo somewhere else and start working. Named branches allow you to have a workflow where you can switch back and forth between branches in the same working directory. However, named branches are flawed.

In mercurial, every changeset belongs to a named branch. That is, the branch name is stored in the changeset. The flaw is that it's quite easy to have more than one branch with the same name, and it's difficult to tell when this has happened. This can cause confusion in a team where one is left wondering what changes, exactly, have made it into the stable branch when multiple people have reopened and merged the branch on different timelines.

Also, because the branch name is in the changeset, the branch lives forever. The only short-term branches are clone branches. That just doesn't encourage quick experiments.

In git, a branch is just a head (see above). Making changes to a branch actually moves the pointer to the new changeset. This head must be explicitly shared across repositories.

In practice, this drops the cost down to approximately zero. You won't accidentally push code you don't mean to. You won't have to be reminded of a failed experiment for the rest of your life, and you won't have to fear naming them in such a way that they don't collide with something someone else has done somewhere else.

Interoperability Tools

Mercurial has the convert extension, which does a great job of converting repositories from some other system to mercurial. It's clean, consistent, and fast.

git has a variety of tools for common systems, some of which are bidirectional. Not many of them seem similar to each other (mob design), but the most common ones get the best treatment.

git-svn, in particular, is so good it's evil. Some of my earliest experiences with git involved cloning an svn repo, creating a git branch of an svn branch, doing various changes there, git merging svn trunk on top of it, doing more changes, and pushing all that work as individual changesets back onto svn. That's good stuff and just shouldn't work.

There's a similar tool for CVS, but neither really makes up for a proper distributed system. You can do all your work in your git tree, but the git tree itself can't be meaningfully cloned (e.g. you can't train svn or cvs to merge).

Non-Technical Differences

The biggest non-technical difference between git and mercurial is the rabid culture surrounding git. mercurial users fairly happily and quietly use their tool, while I've had to send two separate door-to-door git missionaries away today alone.

While popularity is a terrible reason to use something, it's contributed a lot to making git a usable system, and you can see this happening on the mailing list. Take a quick look at some recent posts. There's generally more development going on on that list than discussion.

And along came github. There was repo.or.cz before that, and gitorious is also quite nice, but github has made people really want it. Tons of people signing up constantly, tons of projects entered, and a very easy way to contribute to these projects.

Although mercurial may still feel nicer today, the change feels inevitable. This flood of people leaving centralized systems means that it's way easier to contribute to their projects than ever before. This is the important part.

In the end, we all win either way.
