pyconcrete
==============
Protect your python script, encrypt .pyc to .pye and decrypt when import it

--------------

Version
--------------
0.9.x Beta


Protect python script work flow
--------------
* your_script.py `import pyconcrete`
* pyconcrete will hook import module
* when your script do `import MODULE`, pyconcrete import hook will try to find `MODULE.pye` first
  and then decrypt `MODULE.pye` via `_pyconcrete.pyd` and execute decrypted data (as .pyc content)
* encrypt & decrypt secret key record in `_pyconcrete.pyd` (like DLL or SO)
  the secret key would be hide in binary code, can't see it directly in HEX view


Encryption
--------------
* only support AES 128 bit now
* encrypt & decrypt by library OpenAES


Usage
--------------
* get the pyconcrete source code
```sh
$ git clone <pyconcrete repo> <pyconcre dir>
```

* install pyconcrete
```sh
$ python setup.py install
```
  * need to input your passphrase create secret key for encrypt python script.
  * same passphrase will generate the same secret key
  * installation will add `pyconcrete.pth` into your `site-packages` for execute `sitecustomize.py` under pyconcrete which will automatic import pyconcrete

* convert your script to `*.pye`
```sh
$ pyconcrete-admin.py compile --source=<your py script>  --pye
$ pyconcrete-admin.py compile --source=<your py module dir> --pye
```

* remove `*.py` `*.pyc` or copy `*.pye` to other folder

* main script
  * recommendation project layout
  * python execute main script as *.pye will cause exception, so main script can't be encrypted
```sh
main.py  # your main scirpt, must can't be encrypted
src/*.pye  # your libs
```


Usage (pyconcrete as lib)
--------------
* install pyconcrete as lib
```sh
$ python setup.py install \
  --install-lib=<your project path> \
  --install-scripts=<where you want to execute pyconcrete-admin.py>
```

* import pyconcrete in your main script
  * recommendation project layout
```sh
main.py       # import pyconcrete and your lib
pyconcrete/*  # put pyconcrete lib in root, keep it as original files
src/*.pye     # your libs
```


Test
--------------
* test all case
```sh
$ ./pyconcrete-admin.py test
```


* test all case, setup `TEST_PYE_PERFORMANCE_COUNT` env to reduce testing time
```sh
$ TEST_PYE_PERFORMANCE_COUNT=1 ./pyconcrete-admin.py test
```

