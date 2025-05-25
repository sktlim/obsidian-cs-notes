from reddit

conda and pip are serving completely different use cases despite having similar features.

conda is a **system** package manager. pip is a **Python** package manager.

With conda you can install much more than just Python libraries. You can install entire software stacks such as Python + Django + Celery + PostgreSQL + nginx. You can install R, R libraries, Node.js, Java programs, C and C++ programs and libraries, Perl programs, the list is pretty long and limitless. conda has an `env` system that allows you to have all of these installed across multiple different environments. Also, conda is able to do all these software and package installations in an isolated, userspace manner. This is _critical_ because it means that you can install complex software stacks on a system (such as your employer's heavily regulated production server) **without** needing root privileges. In a lot of ways, conda serves as a lightweight userspace alternative to Docker for isolating software stacks. Also important to note that, as of the most recent versions of conda, conda's env is always active. By default when you install conda for the first time, the default env is used (I think its called "base" or something like that), and will remain in use until you create & switch to another env. You do not need a third-party environment management system with conda.

On the other hand, pip can only install Python packages, and it quite often screws up the installations on multi-user systems, breaking global system dependencies and/or the user's dependency stacks. This is why people who rely only on pip MUST use virtualenv, but even then pip sometimes misbehaves and installs to the wrong places. In general, pip is dangerous and a mess to use. Easy to screw up your user Python library stack or even the entire server's installation. Tread carefully any time you use the globally available system-installed pip.

The thing that a lot of people do not understand is that conda and pip are NOT mutually exclusive. In fact, you are **supposed** to use them together. The intended usage is this;

- first you install and/or set up conda for your project (including conda env as needed) and install all packages you need first from conda channels
    
- second, and **WITH CONDA ACTIVATED**, you use the version of pip _included with conda_ to install and required pip dependencies into your project's conda env
    

This is an important distinction. When you install conda, it brings its own version of pip (and Python) that will automatically become available when you activate conda, and will automatically install packages into your currently activated conda env. If you don't believe me or want to double check, then activate conda and run `which pip` and see for yourself. As long as you use conda's pip to install packages into your active conda env, then you will not have issues.
