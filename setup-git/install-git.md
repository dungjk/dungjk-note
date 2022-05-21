## Install git

## Install git on Windows (Win dow 7, Windows 10, Windows 11)

### Download git-scm (use 64-bit version)
* Latest git installation file can be downloaded [here](https://git-scm.com/). Download manually and install.
![git home page](images/download-git.png)
![git home page](images/download-git-2.png)
* Or download using PowerShell.
```sh
>mkdir D:\dit-docs
>cd d:\git-docs
>Invoke-WebRequest https://github.com/git-for-windows/git/releases/download/v2.36.1.windows.1/Git-2.36.1-64-bit.exe -OutFile git-scm.exe
>.\git-scm.exe
```

![git downloading](images/git-downloading.png)
Follow installing step (Basically click Next following by Next).

---
![Install git - step 1](images/git-install-s1.png)
![Install git - step 2](images/git-install-s2.png)
![Install git - step 3](images/git-install-s3.png)
![Install git - step 4](images/git-install-s4.png)
![Install git - step 5](images/git-install-s5.png)
![Install git - step 6](images/git-install-s6.png)
![Install git - finished](images/git-install-s7.png)

### Test git installation
Run following command to check if git installed correctly.
```sh
>git --version
```

You should see result like this.
![Git test](images/git-test.png)

