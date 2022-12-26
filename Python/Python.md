Well it seems to my like it is indeed a shit show.

### Check All Python Installations

```
ls -ls /usr/bin/python*
```

this shows all the binaries symlinked to whatever it is

there's also this difference in
`usr/local/bin/` folder vs `usr/bin/` thingy

Also pip3 is installed someplace else which is weird to me.

`which pip` and `which pip3` itself gives me various different answers
`noglob pip`?
the output

```
/home/axsae/.local/bin/pip
```

nvm that was because I was on `zsh`

there's also this interesting package
[python-is-python3](https://linuxpip.org/python-is-python3/#:~:text=python%2Dis%2Dpython3%20on%20Ubuntu,-On%20Ubuntu%20version&text=Python%202%20can%20be%20called,OS%20out%20of%20the%20box.)

Also if you're gonna use a different python version with the variables changed

[virtualenv using different python version](https://stackoverflow.com/questions/1534210/use-different-python-version-with-virtualenv#:~:text=The%20module%20used%20to%20create,or%20whichever%20version%20you%20want.)

this might be relevant.

I'm fairly certain I was highly mistake now

`apt show python3 -a` tells us that python3.8.10 is indeed a package released under ubuntu and I don't have any custom packages lying around.
3.8.10 is high verified.

Now I'm trying to remove what was given to me.

```
update-alternatives --config python3
```

```
sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.9 1
```

this is to set overrides

now do I need deadsnakes ppa

how to properly manage packages

Anyways I found out

deadsnakes is the package we use for experimental python builds but yet safe.
the default one managed by Matthias
which shows up on

```
apt show python3 -a
```

shows us that 3.8 is the last build and we're on it.

if we want to use further ones however, we need to have the deadsnakes ppa

which is listed like this on synaptic

you can also use Ubuntu's sources packages and that's a unique way to deal with them.

Oh like Mohan was mentioning yesterday there is something called the IDLE python thing.

### Virtual Environments

the day I'm installing conda I need to take care of these resources

[comparison to other tools // pip vs pipx](https://pypa.github.io/pipx/comparisons/#:~:text=pip%20is%20a%20general%20Python,install%20packages%20with%20cli%20entrypoints.)
[all tools difference // stackoverflow](https://stackoverflow.com/questions/41573587/what-is-the-difference-between-venv-pyvenv-pyenv-virtualenv-virtualenvwrappe#)

- exp user review

[virtualenv vs venv](https://www.infoworld.com/article/3239675/virtualenv-and-venv-python-virtual-environments-explained.html)

- a lott things

[conda for python what & why // youtube](https://www.youtube.com/watch?v=23aQdrS58e0)

- very good points
- anaconda navigator

[pip check date of python packages](./pip-check-date.py)
run it to get a decent list
