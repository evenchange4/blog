title: Using SSH Connect to Windows Vmware Ubuntu
date: 2014-03-15 21:24:55
tags: [linux, vmware]
permalink: vmware-port-forwarding
---

當你的電腦是 Windows 系統，卻需要 Linux (Ubuntu) 環境的話，選擇架設 Virtual maching 是一個不錯的選擇。或許有許多環境有 For windows 的版本，但是很難避免一些小問題，畢竟 Linux 的資源比較多。

最近的一個需求情境是這樣的，LAB 的電腦是 Windows 系統，但是我想要 Ruby 去做資料的 Preprocess，但是本身使用 Mac 的我，實在不習慣 Windows 的 CMD ，所以選擇使用 Vmware 架設一個 `Ubuntu 12.04.4 LTS` 環境，但是離開實驗室後，要怎麼使用 LAB 電腦撰寫程式呢？

<!-- 1. Port forwarding
2. Folder Sharing -->

<!-- more -->

> 使用 SSH 登入你遠端電腦的 Vmware Ubuntu

但是要怎麼透過 Windows 的固定 IP 連進你的 Virtual machine 呢？透過 Port Forwarding 的設定將 Vmware Ubuntu 的 Port 對應到 Windows 的 Port。

## Configuration Environment
1. Windows 7 Enterprise, 64-bit 6.1.7601, Service Pack 1, With Memory 16GB
2. Licensed VMware® Workstation 10 
3. Setup Virtual Machine with Ubuntu 12.04.4 LTS
4. Use a NTU fixed IP 
5. Want to make the SSH/FTP (22/TCP) available to the network

## Port forwarding

1. Vmware NAT Setting: 首先將 Vmware 新增設定，開啓 `Edit` > `Virtual Network Editor` ，選擇使用 `NAT`（基本上就是 Vmnet8） 的網路連線形態，點選 `NAT Setting...`，然後 `Add` 新增一筆。

	![VMware Port forwardind - NAT setting](http://media-cache-ec0.pinimg.com/736x/58/09/a8/5809a8710536ecb65a7894e7288dd994.jpg)
		
	*上束這個例子就是將 Windows 的實體 IP port 5555 對應到 VM 中的 Ubuntu port 22。*

2. Setup custom Firewall for Port: `Windows 防火牆` > `進階` > `輸入規則` > `新增規則` ，接者選擇 TCP 以及欲開啟的 Port number （上述例子 55555），就完成開啟 Windows Port 的設定了。
	
	![Open port for Windows](http://media-cache-ec0.pinimg.com/736x/21/d5/75/21d575df4c43fbee3c4e928d5c8ef8a6.jpg)

## How to Start Connecting

當然你需要將你的 Ubuntu 安裝好 SSH (參考 Reference)， 然後直接在 terminal 輸入以下指令，或是使用 Putty 等軟體登入。

```
$ ssh <userid>@<IP> -p <port_number>
# example: 
# ssh michaelhsu@140.112.117.123 -p 55555
```

> 恭喜你可以在任意地方使用 SSH 登入你的 VM 工作了！，當然 FTP 也是可以。

## Folder Sharing
當然你也可以直接操作 LAB 電腦來使用 Windows Putty SSH 登入你的 VM Ubuntu，但是問題來了，原本存在 Windows 實體硬碟中欲處理的 Dataset 該怎麼搬移進去 Virtual Machine 呢？你可以想說用拖曳的（VMware 小檔案的確可以！），或是使用 FTP 連線上傳，但是當資料量大到無法這樣做呢？例如我最近在處理 Twitter 7 month raw Dataset 將近 25GM 的檔案，該怎麼辦呢！？頭疼了。

> 直接與你的 Virtual Machine Share Folder 是一個很不錯的選擇。

a. Install Vmware Tools: 安裝 Vmware 共享資料夾所需要的工具：透過設定 `VM` > `Install Vmware Toola` 接者會跳出下載下來的目錄資料夾，將 `VMwareTools-8.4.5-324285.tar.gz`（版本號可能不同） 複製到 `/tmp` 底下接者解壓縮


```
$ cd /tmp
$ tar zxvf VMwareTools-8.4.5-324285.tar.gz 
```

並且進行安裝：

```
$ cd vmware-tools-distrib
$ sudo ./vmware-install.pl

# 隨者指示安裝完畢後出現
#
# Enjoy,
# 　　--the VMware team
```


b. Setup Shared Folders: 透過 Vmware 設定 `Virtual Machine Setting` > `Options` ，在 VM Power Off 的情況下可以新增一筆欲共用的資料夾。

![Folder Sharing](http://media-cache-ec0.pinimg.com/736x/e0/0a/79/e00a7913afa6c5d590984e1304c898b4.jpg)


於是乎你的資料就來去自如，你可能會覺得這樣很像 Dropbox 的功能，他是真正的不透過網路共用資料，也是無時差的同步，不過必須額外小心處理！

![Working with Vmware Ubuntu in Windows 7](http://media-cache-ak0.pinimg.com/736x/48/37/71/483771d3e05ed978d2bf6bf2ef044ab2.jpg)
	
*上述應用為使用 Windows 的 Sublime3 寫完程式後並儲存在分享的資料夾中，直接使用 Putty 登入的 VM Ubuntu 的環境執行該程式*

## 延伸應用與後記
突然想起來之前在 TMI (台灣創意工場)  intern 的時候，曾經使用 Vmware Centos 架設 PHP Server ，原來我很早就接觸過這種連線的設定，只是一直沒有沒有機會使用到。這邊我另外做了一個嘗試多開一個 Port 給 VM Ubuntu run `Rails` project ，並透過上面教學的步驟設定，成功可以將外面的連線導到 Virtual Machine 架設的 Server 中，如此以來就可以打在一個最佳的架設環境了！感到熱血沸騰啊。

最後感謝坐我隔壁的 LAB 同學，一起討論學到蠻多東西的，我覺得未來一年半應該也會常常提到他，應此以後就稱他為 ~~陽光宅男~~ `Mr. Sunshine` 好了。

## Reference
- [在 Windows 防火牆中開啟連接埠](http://windows.microsoft.com/zh-tw/windows/open-port-windows-firewall#1TC=windows-7)
- [How to Setup Port Forwarding in VMware Workstation 9](https://www.virten.net/2013/03/how-to-setup-port-forwarding-in-vmware-workstation-9/)
- [Using SSH Locally to Work With Ubuntu VM + VMware Tools Installation via Shell](http://compositecode.com/2013/11/10/using-ssh-locally-to-work-with-ubuntu-vm-vmware-tools-installation-via-shell/)
- [[转载]VMware 虚拟机安装Ubuntu 11.10使用share folders共享目录](http://www.cnblogs.com/RealOnlyme/archive/2012/04/08/2437811.html)