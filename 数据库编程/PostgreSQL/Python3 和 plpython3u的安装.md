### 一、python3的安装
* #### MAC 系统
```shell
xcode-select --install
brew install python3
sudo ln -s /Users/gadflybsd/.pyenv/versions/3.7.4/bin/python3 /usr/bin/python3
sudo ln -s /Users/gadflybsd/.pyenv/versions/3.7.4/bin/pip3 /usr/bin/pip3
sudo ln -s /usr/local/Cellar/python/3.7.4_1/Frameworks/Python.framework/Versions/3.7/lib/libpython3.7m.dylib /usr/lib/libpython3.7m.dylib
wget https://anaconda.org/conda-forge/postgresql-plpython/11.2/download/osx-64/postgresql-plpython-11.2-py37hc1b21e5_0.tar.bz2
cp -R share/extension/*.* /usr/local/Cellar/postgresql/11.2/share/postgresql/extension/
cp -R lib/*.* /usr/local/Cellar/postgresql/11.2/lib/postgresql/
```
* ### [the compiled plpython3 from conda-forge](https://anaconda.org/conda-forge/postgresql-plpython/files)
  * decompress it and copy
    ```shell
    share/extension/plpython3u--unpackaged--1.0.sql
    share/extension/plpython3u.control
    share/extension/hstore_plpython3u.control
    share/extension/hstore_plpython3u--1.0.sql
    share/extension/plpython3u--1.0.sql
    ```
  * to
    ```shell
    /usr/local/Cellar/postgresql/11.2/share/postgresql/extension
    ```
  * and copy
    ```shell
    lib/plpython3.so
    lib/pgxs
    lib/hstore_plpython3.so
    ```
  * to
    ```shell
    /usr/local/Cellar/postgresql/11.2/lib/postgresql 
    ```