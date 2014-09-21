#MAC OS X 10.9.4 mosh locale setting issues. (LC_CTYPE=UTF-8)#

##Issues:##
When i try to using terminal running mosh connect to my raspberry pi. some error message happen:
```
Unfortunately, the local environment (LC_CTYPE=UTF-8) specifies
the character set "US-ASCII",

The client-supplied environment (LC_CTYPE=UTF-8) specifies
the character set "US-ASCII".
```

##Root cause:##
Mosh pulls mac terminal encoding setting and try to using this setting for communicate. 

My mac terminal locale result:
```
| => locale
LANG=
LC_COLLATE="C"
LC_CTYPE="UTF-8"
LC_MESSAGES="C"
LC_MONETARY="C"
LC_NUMERIC="C"
LC_TIME="C"
LC_ALL=
```

Unfortunately, my raspberry pi not support this kind of setting. (I guess name UTF-8 not found on my raspberry pi)
using local -a can list out raspberry pi supported profile.
```
pi@raspberrypi ~ $ locale -a
C
C.UTF-8
en_GB.utf8
POSIX
```

##Solution 1##
I try to export en_GB.utf8 to my my terminal but no working.
```
export LC_ALL=en_GB.utf8
```

using locale -a in mac terminal, maybe profile name difference:
```
| => locale -a | grep en_GB
en_GB
en_GB.ISO8859-1
en_GB.ISO8859-15
en_GB.US-ASCII
en_GB.UTF-8
```

set to correct name, it's work!! you can add this to ~/.bash_profile, that will set automatically every time you login.
```
export LC_ALL=en_GB.UTF-8
```


##Solution 2##
Install required locale on the remote server(raspberry pi)
if you prefer using en_US.UTF-8, you can install in on your remote server:
```
# localedef -i en_US -f UTF-8 en_US.UTF-8
```


The point is, let your remote server support your client encoding.


##### Reference: #####
+ https://groups.google.com/forum/#!topic/issh/3I1FZErIFKU
+ http://www.cyberciti.biz/faq/os-x-terminal-bash-warning-setlocale-lc_ctype-cannot-change-locale/