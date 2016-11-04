# CAPDNET Cloud Documentation


## **SSH**

Our machines do not have open port on public network. Instead of that we have an internal network(s), so you need to have a special configuration of SSH or VPN. Only certain machines are available over the Internet as gateways.


##### Linux/OSX (unix like os)

* **Configuration**

Add to `~/.ssh/config` following directives (do not forget to replace `<USERNAME>` with your account name):

```Bash
Host access-1
     user <USERNAME>
     hostname 149.156.65.41

Host *.capdnet
   user <USERNAME>
   ProxyCommand ssh -q -W %h:%p access-1
```

* **Connect**

With the above configuration you are able to use `access-1` host as a gateway to our internal network. As an example, to open a connection type in the terminal:
```Bash
ssh fermi-ii-users.capdnet
```

You can use any host known for you inside the capdnet network.

It is worth to note that other services based on SSH also use this configuration (scp, sftp, git, svn).

* **SSH Keys**

Please use *SSH Keys!!!* Without them you will need to provide password two times. Also use a key protected by pass-phrase.


##### Windows
Read about ProxyCommand equivalent for your SSH client.


## **User directories**

Each user has a home directory exported as a NFS resource. The directories are on many severs (fermi, truten, rysy, nosal). It means that an access is fastest from the same server as the NFS export. On a different server the access is over network, so it is much slower.

However, each user has also *local* directory on each server. You can access them using path `/mnt/users/$USER/local/`. You can also access local files from another server, e.g. to access local files from *nosal* on *truten* use `/mnt/users/$USER/local_nosal`. Be awere that the access is with NFS (over network)!

Virtual machines over *big boxes* (e.g. fermi, rysys) shares *local* directories. They use NFS, however it shouldn't be slow because the network layer is within one machine. Let me know if this is an bottleneck for you.

## **Repositories**

##### CAPD - read-write
For read-write access to CAPD repositories you need to apply our **SSH** configuration. Then you can use following URL syntax
```Bash
svn co svn+ssh://<USERNAME>@repos.capdnet/var/svn-repos/capd
```

Of course please use valid `<USERNAME>`. Also you can use any other repository from CAPD family.

##### CAPD - read-only over http

You can use following URL syntax:
```
https://svn.capdnet.ii.uj.edu.pl/capd
```

Please ignore security warning (we do not have confirmed SSL certificates).


##### User repositories

Please put your private repositories in `~/repos/git` or `~/repos/svn` and access them using:
```Bash
<USERNAME>@repos.capdnet:/var/user_repos/<USERNAME>/git
<USERNAME>@repos.capdnet:/var/user_repos/<USERNAME>/svn
```


## **Http public_html for users**

If you want to access your files via Http you can add them to `~/public_html` and use following URL:
```
http://users.capdnet.ii.uj.edu.pl/~<USERNAME>/
```

Your files need to be at least with read access for `others`.
