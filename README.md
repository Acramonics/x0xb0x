x0xb0x - Full GitHub Repo
=========================

**PLEASE NOTE, THIS IS NOT MY CODE OR HARDWARE DESIGN. x0xb0x WAS CREATED BY [LIMOR](https://www.ladyada.net/bio/)**

This repository is cloned from that CVS repo at
https://sourceforge.net/p/x0xb0x/code/

It is licensed under the [MIT Open Source Licence](./LICENCE.md). See https://en.wikipedia.org/wiki/MIT_License

x0xb0x is a full reproduction of the original Roland TB-303
MIDI-controlled synthesizer, with a fully functional sequencer. Full
details are at https://www.ladyada.net/make/x0xb0x/

Unfortunately that page references several items (including the
schematics, source code, panel designs and PCB designs) that were
stored in CVS repositories on SourceForge. SourceForge no longer
supports CVS and the full CVS repo is now just available as a ZIP
file.

For background information, the conversion was done as follows.

Installing the `cvs2svn` tool
-----------------------------
`cvs2cvn` also supports CVS to Git conversion.

Install `cvs2svn` from the release version:

```
sudo bash
   WD=/usr/local/apps/cvs2svn
   mkdir $WD
   cd $WD
   wget https://github.com/mhagger/cvs2svn/releases/download/2.5.0/cvs2svn-2.5.0.tar.gz
   tar xvf cvs2svn-2.5.0.tar.gz
   # edit cvs2git, change the shebang line to use python2.7
   cd /usr/local/bin
   ln -s ../apps/cvs2svn/cvs2svn-2.5.0/cvs2git .
exit
```

Download the CVS repo and perform the conversion
------------------------------------------------

1. Create yourself a working directory:
```
WD=$HOME/electronics/x0xb0x
mkdir -p $WD
```

2. Obtain and unzip the CVS Zip file:
```
cd $WD
wget https://sourceforge.net/code-snapshots/cvs/x/x0/x0xb0x.zip
mv x0xb0x.zip  x0xb0x_cvs.zip
mkdir -p $WD/cvs
cd $WD/cvs
unzip ../x0xb0x_cvs.zip
```

3. Create a variable to store the CVS dir name
```
CVS=$WD/cvs/x0xb0x
```

4. Remove a stray lock file
```
rmdir $CVS/firmware/#cvs.lock/
```

5. Create the x0xb0x repo on GitHub

6. Perform the conversion

```
cd $WD
mkdir -p cvs2git-tmp
cd cvs2git-tmp
cvs2git --blobfile=blob.dat --dumpfile=dump.dat \
    --username=FIXME --default-eol=native \
    --encoding=utf8 --encoding=latin1 --fallback-encoding=ascii \
    $CVS
git clone git@github.com:Acramonics/x0xb0x.git
### git checkout ssh://git.code.sf.net/p/PROJECT/code x0xb0x
cat blob.dat dump.dat | git --git-dir=x0xb0x/.git fast-import
cd x0xb0x
git checkout master
git push origin --mirror
cd ..
rm blob.dat dump.dat
mv x0xb0x ../x0xb0x_git
cd ..
rmdir cvs2git-tmp
```

7. Remove the unpacked CVS version
```
rm -rf $WD/cvs
```

