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