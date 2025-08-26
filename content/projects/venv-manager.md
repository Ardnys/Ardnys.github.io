+++

title = "Python Virtual Environment Manager"

date = "2025-08-11"

description = "It was too much of a hassle to manage multiple virtual environments so I wrote a tool to simplify and automate the process."

weight = 1

[taxonomies]

tags=["python", "rust"]

[extra]

remote_image = "https://raw.githubusercontent.com/Ardnys/venv-rs/refs/heads/main/images/venv_rs_logo.png"

+++

# Why Virtual Environment Manager

## My Problems with Python Virtual Environments

```bash
alias venv='virtual environment'
```

Using venvs to install isolated packages for development is the easiest package management solution in Python. It's not featureful, but it'll get you your beloved packages!

`python -m venv <PATH>` command creates a venv at `PATH`. Depending on personal preference, some (including me) put the venvs in a hidden folder like `.virtualenvs` in user directory, some put it in a hidden file in the root of the project that uses it. [Read more about venvs here](https://docs.python.org/3/library/venv.html)

### venv sizes become an issue when I don't pay attention

I like a known, easily accessible folder for the reusability of venvs for AI / data science projects which use roughly the same set of packages. That's especially convenient for deep learning packages that takes around ~10GB disk space.

But it is a management nightmare; I forget what I named them or what was in that venv. I end up with near duplicates because I use one odd package for a project.

{{ note(header="Painful Fact!", body="Last year I deleted ~50GBs of venvs. That's a lot of Gee Bees.") }}

50GBs wasted? In this economy? Unacceptable.

It's Rust levels of disk space waste to create standalone venvs. I only have a 512GB SSD to play with until I prioritize digital storage over coffee equipment.

Let me show you what I'm talking about.

### It's annoying to manage them

To inspect a venv I have to do the following:

```bash
# remember the venv name type the long path of activation script
# also it's on ...\Scripts\activate on Windows. that's annoying too
# even if I alias it, I still have to remember the names
$ source ~/.virtualenvs/da/bin/activate

# list the packages
$ pip list

# check a package
$ pip show <package>
```

which is alright when I am already in an activated environment. But that venv was `da` (for **d**ata **a**nalysis) and it doesn't have deep learning packages. Now repeat the same steps for... whatever I named that deep learning venv. Hmm I seem to have a venv for **image analysis** but also for **computer vision** too... which one has the most correct libraries...

It's shame really, it's a problem I brought upon myself. But it's engineering if I solve it.

## Solution

My venvs reside in a known place and I want to display some basic information about them. All the information is available in the venv, hidden in some directories and files. A simple terminal interface, file manipulation and parsing should do the trick. Rust is perfect for this.

_Actually Python is perfect-er but let's ignore it for now_

I'll use [ratatui](https://ratatui.rs/) for the interface. I just love TUI apps, looks amazing and so fun to use. This project fits perfectly for this kind of thing.

### Features I want

- Finding venvs in a directory.
- Searching for venvs recursively.
- Showing the path and size of venv, installed packages, number of packages
- Showing the size of package, its dependencies, number of dependencies, versions and summary
- Activation commands depending on OS and shell, printing requirements.txt
- Must work on Linux & Windows.
- Automated releases on Github via Github Actions.
