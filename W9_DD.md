 # Week9 - Linux Device Driver
   #### 들어가기 전에 : 이 글은 제가 학습한 내용을 "이렇게 이해했습니다" 로 정리하는 문서이지 강좌글이 아닙니다. 
 > 목차
 >> - Day1 : 
 >> - Day2
 >> - Day3
 >> - Day4
 >> - Day5
 #
 ## Day1    
  - 프로세스의 상태천이 : sleep -> Weakup -> Ready -> Run
  - 펨웨어 레벨은 가상 주소가 없었어요
  - 디바이스 드라이버는 특정 주소로 다이렉트 접근 불가능- 커널 패닉상태로 빠짐
  - 리눅스 커널 메모리 보호기능, 특정주소를 다이렉트로 접근 불가능 에러발생
  - OS야 1:1 맵핑된 주소를 알려주겠니???? 
  - 이번주 디바이스 드라이버는 위 아래를 모두 아우를 수 있기 때문에
  - 디바이스 드라이버는 커널에 속해있기 때문에 커널 컴파일을 해야 함
  - 루트파일시스템은 : /root의 특정 Path로 이동, 압축풀기 옵션
      - xvzf : tar파일
      - xvjf : bin파일
  - 디바이스 드라이버에 대한 간단 특징들 
     - 내용이 매우 방대함 - 뒷부분 못나갈수 있음
     - 구인이 많은부분 - 일이 빡세긴 한데 잘하면 인정 받을 수 있음. 거의 모든 임베디드 분야에 통용되는 곳임
     - 예를들어 사람의 마음을 찍는 프로젝트의 경우, hw->os포팅->dd구현의 순서로 프로젝트가 진행 됨
     - 리눅스 프로젝트의 프레임 혹은 틀임. 많이 만들면 만들수록 실력이 향상 됨
     - 함수포인터 변수와 void 형 포인터 변수 등판!
     - 상위 영역(App영역, User영역)에서는 하드웨어를 파일로 추상화하여 바라 봄
     - File의 동작에 관하여 4가지 동작
       - Open, Read, Write, Close
     - 저수준 파일 입출력에 관하여 : 파일 디스크립터를 둘러 싼 설정들..
     - 개발단계에서는 NAND에 다운로드 안하고, 파일시스템은 NFS로 Host에 위치 시킴
     - 함수에 Static 키워드 설정 : 해당 파일 내에서만 사용하고 접근할 수 있는 함수라는것을 명시적으로 설정 함
     - 커널은 일반 어플리케이션 영역과 다른 함수군을 사용 함 - printk..
  - 리눅스에 관한 몇가지 포인트 복습
     - 리눅스는 무료가 아니며, 제품출시는 유료이다. 단 학습은 무료
     - 커널은 OS 그 자체로 설명할 수 있다.
     - 선점형/비선점형 OS는 커널과 스케쥴링이 다르다
     - 쓰레드는 프로세스에 종속적
     - 기아상태 : 프로그램이 CPU에 컴퓨팅 자원할당을 요구하나, 제공해 주지 못하는 상태를 기아상태라고 명명 함
  - 리눅스 커널의 몇가지 기능에 관하여
     - 프로세스 관리 : SM, MSGQ, Pipi(np, unp)
     - 메모리 관리 : 모든 프로세스에 대한 가상 주소 영역을 구축한다
     - 파일시스템 관리 : 리눅스/유닉스의 철학 : Everything is file
     - 디바이스 제어 : 프로세스 메모리를 제외한 거의모든 대부분의 활동은 DD가 관리함
     - 네트워크 관리
  - 커널의 관점에서 관측 리눅스 구조
     1. fork함수를 유저가 호출
     2. fork의 함수 원형은 어디에 존재하는지 찾음 -> 커널에존재
     3. 디테일하게 공부해야 할 부분이 커널
     4. DD또한 커널영역에 속함
  - 리눅스 커널 2.6버전에서의 변화
    - Overview
        1. 커널코어 및 디바이스 드라이버 구조에 많은 변화
        2. 모듈의 구현 방식의 변화 : .ko
        3. 2.6은 전버전인 .4에 비해서 대대적인 터닝 포인트임
        4. 커널코드 공부 시 C언어로 구성된 객체지향 코드를 확인할 수 있음
        5. 클래스 객체지향은 결국 증복된 코드를 막아 생산성의 향상이 목적임
    - 메모리 부분 변화에 관하여
        1. MMU 없는 시스템의 지원
        2. Sleep하지 않는 메모리의 할당
        3. mempol기능 : 사전할당 후 이용하는
    - 인터럽트 핸들러 : 매우매우 복잡하고 어렵다
        - 그중 하나를 한다면 Request IRQ 만 알면 됨.
    - 디바이스 넘버링
        - Major넘버와 Minor 넘버가 존재
    - 커널모듈 적재/제거
        1. insmod
        2. rmmod
  - 리눅스 디바이스 드라이버의 3가지 : 캐릭터DD, 블럭DD, 네트워크DD
  - 실습 내용  
    ```s
    root@ubuntu-vm /
    # ls
    bin    dev   initrd.img  media  proc  selinux  tmp  vmlinuz
    boot   etc   lib         mnt    root  srv      usr
    cdrom  home  lost+found  opt    sbin  sys      var
    root@ubuntu-vm /
    # cd m
    media/ mnt/   
    root@ubuntu-vm /
    # cd m
    media/ mnt/   
    root@ubuntu-vm /
    # cd mnt/hgfs/Data/
    root@ubuntu-vm /mnt/hgfs/Data
    # ls
    arm-2010q1-202-arm-none-linux-gnueabi-i686-pc-linux-gnu.tar.bz2
    kernel-mds2450-3.0.22-20140306.tar.gz
    rootfs_20140218.tar.gz
    zImage
    root@ubuntu-vm /mnt/hgfs/Data
    # cp arm-2010q1-202-arm-none-linux-gnueabi-i686-pc-linux-gnu.tar.bz2 /root
    root@ubuntu-vm /mnt/hgfs/Data
    # cp kernel-mds2450-3.0.22-20140306.tar.gz /root
    root@ubuntu-vm /mnt/hgfs/Data
    # cp rootfs_20140218.tar.gz /root
    root@ubuntu-vm /mnt/hgfs/Data
    # ^C
    root@ubuntu-vm /mnt/hgfs/Data
    # ^C
    root@ubuntu-vm /mnt/hgfs/Data
    #
    ```
  - tar 명령어로 압축파일 풀기
    ```s
    root@ubuntu-vm ~
    # tar xvjf arm-2010q1-202-arm-none-linux-gnueabi-i686-pc-linux-gnu.tar.bz2 
    ```
  - 커널 압축풀어서 디렉토리 위치 시키기
    ```s
    root@ubuntu-vm ~
    # cd kernel-mds2450-3.0.22 -> 폴더 이동
    root@ubuntu-vm ~/kernel-mds2450-3.0.22
    # ls -> 디렉토리 파일 리스트 출력
    COPYING   (~생략~)   samples  usr
    root@ubuntu-vm ~/kernel-mds2450-3.0.22
    # pwd -> 디렉토리 경로 출력
    /root/kernel-mds2450-3.0.22
    root@ubuntu-vm ~/kernel-mds2450-3.0.22
    # make mds2450_defconfig -> MDS2450보드 설정 파일 셋팅 ★필수★필수★
    HOSTCC  scripts/basic/fixdep
    HOSTCC  scripts/kconfig/conf.o
    HOSTCC  scripts/kconfig/zconf.tab.o
    HOSTLD  scripts/kconfig/conf
    #
    # configuration written to .config
    #
    root@ubuntu-vm ~/kernel-mds2450-3.0.22
    # make clean -> 명령어로 파일 정리
    root@ubuntu-vm ~/kernel-mds2450-3.0.22
    ```
  - 커널 컴파일 시도 -> 에러 발생
    ```s
    root@ubuntu-vm ~/kernel-mds2450-3.0.22
    # make zImage
    make: /project/toolchain/arm-2010q1/bin/arm-none-linux-gnueabi-gcc: 명령을 찾지 못했음
    HOSTCC  scripts/basic/fixdep
    HOSTCC  scripts/kconfig/conf.o
    HOSTCC  scripts/kconfig/zconf.tab.o
    HOSTLD  scripts/kconfig/conf
    scripts/kconfig/conf --silentoldconfig Kconfig
    make: /project/toolchain/arm-2010q1/bin/arm-none-linux-gnueabi-gcc: 명령을 찾지 못했음
    CHK     include/linux/version.h
    CHK     include/generated/utsrelease.h
    make[1]: `include/generated/mach-types.h'는 이미 갱신되었습니다.
    CC      kernel/bounds.s
    /bin/sh: /project/toolchain/arm-2010q1/bin/arm-none-linux-gnueabi-gcc: not found
    make[1]: *** [kernel/bounds.s] 오류 127
    make: *** [prepare0] 오류 2
    root@ubuntu-vm ~/kernel-mds2450-3.0.22
    # 
    ```
  - 에러 보는 눈을 키워여되 크로스컴파일러 아구가 안맞아요 감으로 느끼는 연습을 하시란말이야
  - 고치는법 : gedit Makefile 명령어로 메이크파일 변경 -> 196라인
    ```
    CROSS_COMPILE	?= $(CONFIG_CROSS_COMPILE:"%"=%)
    ```
  - 위의 파일 내용을 아래와 같이 변경
    ```
    #CROSS_COMPILE	?= $(CONFIG_CROSS_COMPILE:"%"=%)
    CROSS_COMPILE	?= arm-none-linux-gnueabi-
    ```
  - ubuntu 컴파일러 설치과정
    1. 컴파일러(arm-linux-noni-eabi)를 압축해제하여 풀어놓기
    2. 컴파일러 경로는 /root/arm2010q1
    3. "source /etc/environment" 명령어 실행 : 환경변수 다시 로드
    4. 확인과정 
       - "arm-"입력후 tab키 눌르면 컴파일러 항목이 자동으로 달라붙어야 함
  - 컴파일러 설정 오류 시 확인&반복해야 함
  - TFTP 설정 진행
    ```s
    root@ubuntu-vm ~
    # tar xvzf kernel-mds2450-3.0.22-20140306.tar.gz 
    ```
    ```s
    root@ubuntu-vm ~/kernel-mds2450-3.0.22
    # make mds2450_defconfig
    ```
    ```s
    root@ubuntu-vm ~/kernel-mds2450-3.0.22
    # make clean
    ```
    - zImage: 임베디드 환경에서의 리눅스 커널 실행파일 이미지
    ```s
    root@ubuntu-vm ~/kernel-mds2450-3.0.22/arch/arm/boot
    # cd /etc/xinetd.d
    root@ubuntu-vm /etc/xinetd.d
    # gedit tftpd
    ```
    - /etc/xinetd.d 파일 내용
    ```s
    service tftp
    {
        protocol = udp
        port = 69
        socket_type = dgram
        wait = yes
        user = nobody
        server = /usr/sbin/in.tftpd
        serve_args = /tftpboot
        disable = no
    }
    ```
    - tftpboot 폴더 생성, 권한 부여, 네트워크 재부팅을 차례로 실행    
    ```s
    root@ubuntu-vm /etc/xinetd.d
    # mkdir /tftpboot
    root@ubuntu-vm /etc/xinetd.d
    # chmod 777 /tftpboot
    root@ubuntu-vm /etc/xinetd.d
    # /etc/init.d/xinetd restart
    * Stopping internet superserver xinetd                                  [ OK ] 
    * Starting internet superserver xinetd                                  [ OK ] 
    root@ubuntu-vm /etc/xinetd.d
    # netstat -au
    Active Internet connections (servers and established)
    Proto Recv-Q Send-Q Local Address           Foreign Address         State      
    udp        0      0 *:57255                 *:*                      
    (중략)
    udp        0      0 *:tftp                  *:*                      
    (중략)
    udp6       0      0 [::]:mdns               [::]:*                            
    root@ubuntu-vm /etc/xinetd.d
    ```
 - 위에서 tftp 가 보이지 않는다면 분명히 오타, 아래 사항으로 확인가능
    ```s
    root@ubuntu-vm ~/kernel-mds2450-3.0.22/arch/arm/boot
    # cp zImage /tftpboot/
    root@ubuntu-vm ~/kernel-mds2450-3.0.22/arch/arm/boot
    # ifconfig eth1 up 192.168.20.90 up
    root@ubuntu-vm ~/kernel-mds2450-3.0.22/arch/arm/boot
    # cp zImage /tftpboot/
    root@ubuntu-vm ~/kernel-mds2450-3.0.22/arch/arm/boot
    ```
 - 이미지 업로드가 안될때는 IP 주소 확인 -> 추후에 static IP 변경 가능
    ```s
    # ifconfig eth1 up 192.168.20.90 up
    ```
 - 리눅스 커널 업로드
    ```s
    tftpboot 30008000 zImage
    ```
 - MDS2450 보드에서 프롬프트 명령
    ```s
    bootm 30008000
    ```
 - MDS2450 보드에서 "printenv"명령어로 보드 셋팅 출력
    ```s
    bootargs=root=/dev/nfs rw nfsroot=192.168.20.90:/nfs/rootfs ip=192.168.20.246:192.168.20.90:192.168.20.1:255.255.255.0::eth0:off console=ttySAC1,115200n81
    stdin=serial
    stdout=serial
    stderr=serial
    ```
 - host 환경설정 NFS 설치
    ```s
    root@ubuntu-vm /etc
    # pwd
    /etc
    root@ubuntu-vm /etc
    # gedit exports 
    ```
 - gedit으로 실행시킨 exports 파일 내용 적는다
    ```s
    /nfs/rootfs *(rw,sync,no_root_squash,no_all_squash)
    ```
 - NFS를 위한 커널 서비스 재 실행
    ```s
    root@ubuntu-vm /etc
    # gedit exports 
    root@ubuntu-vm /etc
    # /etc/init.d/nfs-kernel-server restart
    ```
 - 이후 부팅과정
    ```s
    [teraterm]
    tftp 300800 zImage
    [host ubuntu]
    # ifconfig eth1 192.168.20.90 up
    ->다운이 실행됨
    [teraterm]
    bootm 30008000
    ->다운이 실행됨 혹시 rootfs를 못찾고 있다면
    [host ubuntu]
    # ifconfig eth1 192.168.20.90 up
    ```
 - 보드에 커널(zImage와, rootfs가 모두 업로드 되면 아래 화면을 만난다)
    ```s
    Welcome to MDS2450
    mds2450 login: root
    ```
 - 로그인 비밀번호 : root
 - 위 과정으로 리눅스 부팅 완료!! 커널 확인을 위해서 아래의 명령어 입력
    ```s
    # uname -a
    ```
 - 확인과정 (안되는 경우를 대비하여) - [host ubuntu]
    ```s
    root@ubuntu-vm /etc
    # gedit exports 
    ```
    ```s
    /nfs/rootfs *(rw,sync,no_root_squash,no_all_squash)
    ```
 - 네트워크 시스템 재부팅
    ```s
    root@ubuntu-vm /etc
    # /etc/init.d/nfs-kernel-server restart
    ```
 - 아래와 같은 메시지 확인
    ```s
    root@ubuntu-vm /etc
    # /etc/init.d/nfs-kernel-server restart
    * Stopping NFS kernel daemon                                          [ OK ] 
    * Unexporting directories for NFS kernel daemon...                    [ OK ] 
    * Exporting directories for NFS kernel daemon...                             exportfs: /etc/exports [1]: Neither 'subtree_check' or 'no_subtree_check' specified for export "*:/nfs/rootfs".
    Assuming default behaviour ('no_subtree_check').
    NOTE: this default has changed since nfs-utils version 1.0.x          [ OK ]
    * Starting NFS kernel daemon 
    ```
    ```s
    root@ubuntu-vm /nfs/rootfs
    # ls
    aaa  dev  home  linuxrc  opt   root  sbin  tmp  var
    bin  etc  lib   mnt      proc  run   sys   usr
    root@ubuntu-vm /nfs/rootfs
    # mkdir /root/module
    root@ubuntu-vm /nfs/rootfs
    # ls
    aaa  dev  home  linuxrc  opt   root  sbin  tmp  var
    bin  etc  lib   mnt      proc  run   sys   usr
    root@ubuntu-vm /nfs/rootfs
    # cd /root/module/
    root@ubuntu-vm ~/module
    # ls
    root@ubuntu-vm ~/module
    # pwd
    /root/module
    root@ubuntu-vm ~/module
    # cp /mnt/hgfs/Data/13_module.zip /root/module/
    root@ubuntu-vm ~/module
    # ls
    13_module.zip
    root@ubuntu-vm ~/module
    # ls
    13_module.zip
    root@ubuntu-vm ~/module
    # unzip 13_module.zip 
    Archive:  13_module.zip
    inflating: .hello.ko.cmd           
    inflating: .hello.mod.o.cmd        
    inflating: .hello.o.cmd            
    inflating: .hello_param.ko.cmd     
    inflating: .hello_param.mod.o.cmd  
    inflating: .hello_param.o.cmd      
    inflating: .tmp_versions/hello.mod  
    inflating: .tmp_versions/hello_param.mod  
    inflating: hello.c                 
    inflating: hello.ko                
    inflating: hello.mod.c             
    inflating: hello.mod.o             
    inflating: hello.o                 
    inflating: hello_param.c           
    inflating: hello_param.ko          
    inflating: hello_param.mod.c       
    inflating: hello_param.mod.o       
    inflating: hello_param.o           
    inflating: Makefile                
    extracting: Module.symvers          
    inflating: modules.order           
    root@ubuntu-vm ~/module
    # ls
    13_module.zip   hello.c      hello.mod.o    hello_param.ko     hello_param.o
    Makefile        hello.ko     hello.o        hello_param.mod.c  modules.order
    Module.symvers  hello.mod.c  hello_param.c  hello_param.mod.o
    root@ubuntu-vm ~/module
    # make
    make -C /root/work/embedded/linux-3.12.14 SUBDIRS=/root/module modules
    make: *** /root/work/embedded/linux-3.12.14: 그런 파일이나 디렉터리가 없습니다.  멈춤.
    make: *** [all] 오류 2
    root@ubuntu-vm ~/module
    # make clean
    make -C /root/work/embedded/linux-3.12.14 SUBDIRS=/root/module clean
    make: *** /root/work/embedded/linux-3.12.14: 그런 파일이나 디렉터리가 없습니다.  멈춤.
    make: *** [clean] 오류 2
    root@ubuntu-vm ~/module
    # gedit Makefile 
    ```
 - 커널 디렉토리 확인
    ```
    KDIR	:= /root/kernel-mds2450-3.0.22
    ```
 - target board 에서 작업 
     - 커널모듈 Insert
        ```
        # insmod hello.ko
        Hello, world 4.
        ```
     - 커널 모듈 Remove
        ```
        # rmmod hello.ko
        Goodbye, world 4.
        ```
 - hello.ko 모듈 타겟보드의 nfs로 복사하기
    ```
    root@ubuntu-vm ~/module
    # cp hello.ko /nfs/rootfs/home/default
    ```
 - host 개발PC에서 xx.ko 커널모듈
    ```
    root@ubuntu-vm ~/module
    # pwd
    /root/module
    root@ubuntu-vm ~/module
    # cp myk.ko /nfs/rootfs/home/default/
    ```
 - 기본 커널모듈 소스
    ```c
    #include <linux/module.h>       /* Needed by all modules */
    #include <linux/kernel.h>       /* Needed for KERN_INFO */
    #include <linux/init.h>         /* Needed for the macros */

    #define DRIVER_AUTHOR   "DongKyu, Kim <dongkyu@mdstec.com>"
    #define DRIVER_DESC             "A sample driver"


    static int __init init_hello_4(void)
    {
            printk(KERN_INFO "Hello, world 4.\n");
            return 0;
    }

    static int __init dhkim_km_hello(void)
    {
            printk(KERN_INFO "Hello MDS kernal World!! From:DHKim\n");
            return 0;
    }

    static int __exit dhkim_km_gobye(void)
    {
            printk(KERN_INFO "byebye kernal World~~ From:DHKim\n");
            return 0;
    }

    static void __exit cleanup_hello_4(void)
    {
            printk(KERN_INFO "Goodbye, world 4.\n");
    }

    module_init(dhkim_km_hello);
    module_exit(dhkim_km_gobye);
    /* Get rid of taint message by declaring code as GPL. */
    MODULE_LICENSE("GPL");
    MODULE_AUTHOR(DRIVER_AUTHOR);           /* Who wrote this module? */
    MODULE_DESCRIPTION(DRIVER_DESC);        /* What does this module do */
    MODULE_SUPPORTED_DEVICE("testdevice");
    ```

  ## 1일차 정리
   - KDIR :커널의 위치
   - 커널 모듈이 있어야지만 커널모듈(너컬 오브젝트) 생성이 가능 
   - 커널모듈은 커널이 돌고있는 와중에 인서트/리무브가 가능
   - 실습 진행을 원활하게 하기 위한 몇가지 
      1. 리셋
      2. bootcmd 에다가 환경설정으로 자동으로 nfs까지 전부 딱 정합이 될 수 있는 환경 조성(setenv, bootcmd 프롬프트 명령어 활용)
      3. NFS환경 구축
      4. Hello kernal world 메시지 출력되게
      5. 커널(zImage) 의 위치는 /root/kernel-mds2450-3.0.22/arch/arm/boot
   - NFS(network file system)필요성 정리 
      1. 임베디드 리눅스 시스템  개발시, Filesystem을 계속해서 접근/변경해야 하는데, 매번 Target의 Nand Flash를 계속해서 쓰면 시간이 낭비됨
      2. Host PC의 파일시스템을 공유해서 타겟보드의 루트파일시스템을 제공하는 서비스
      3. Host PC에서 작업한 내용을 PC내부에서 통신없이 Target과 공유하는 nfs폴더로 이동하면, Target의 Filesystem에서 자동으로 반영됨.(매우매우 편리함)
   - TFTP - [위키 설명](https://ko.wikipedia.org/wiki/TFTP) : Trivial 된 FTP프로토콜로써, 간소화된 프로토콜이라 적은 용량과 사이즈로 구현이 간단하여 임베디드 시스템에서 파일 전송용으로 많이 사용됨
   - FTP - [위키설명](https://ko.wikipedia.org/wiki/%ED%8C%8C%EC%9D%BC_%EC%A0%84%EC%86%A1_%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C) : 파일 전송 프로토콜로써 널리 사용됨, 이더넷 통신 기반이다.
   - [NFS 셋팅 가이드 - 1](https://blog.naver.com/meelong0/140025104036)
   - [NFS 셋팅 가이드 - 2](http://forum.falinux.com/zbxe/index.php?mid=lecture_tip&page=118&document_srl=462347)
   - [초보자를 위한 임베디드 리눅스 학습 가이드](http://ebook.pldworld.com/_eBook/Embedded/embedded_guide-v1.1_html.htm)
 #
 ## Day 2 
  - 리눅스를 인공위성쯤에서 큰그림을 보고 내려다봐라
  - 어떤 역할이고 전체 시스템에서 어디쯤 위치하고 있으며, 어떤기능을 하는지 큰그림에서 작은  
  - 대부분의 경우는 DD를 짜고, DD를 활용한 어플리케이션을 짜는 경우가 많음. 현업에 가면 DD와 APP이 별도가 아닌 하나의 개념으로 봐야 함
  - 커널 모듈 제작시, 커널컴파일을 안한 소스를 Makefile의 KDIR에 등록하면 안만들어짐, 커널 컴파일을 하며 생성되는 많은 파일들이 필요함.
  - [리눅스 공부 - 임베디드 홀릭님 카페북](https://cafe.naver.com/lazydigital/book5089864/8630)
  - [라즈베리파이 디바이스트리](https://wikidocs.net/3205)
  ```s
  root@ubuntu-vm /proc/1
  # mknod /dev/led c 240 0
  ```
  - mknod : 특수 장치 파일로 커널에 등록
  - /dev/led : 특수장치파일 생성위치
  - c : 캐릭터 디바이스 
  - 240 : Major number
  - 0 : Minor number
  - 커널은 제작되어있는데 디바이스드라이버는 그때그때
    - 이럴때를 위해서 함수포인터를 등록해서 
    - 함수포인터 변수, void형 포인터 변수는 투성이로 나온다!!
  - DD가 빡센이유
    - DMA살리기 -> 펌웨어 DD는 1비트의 예술, 오동작 혹은 Halt일어날 수 있음
    - Timer Oneshot, AutoReload ... 
    - 삼성 SDS다니시다가 과정에 들어오신분, 연세 60살-> 서버관리 서버확장 외국에서 성대나오고 영어가 기가막혀
    - 삼성 보드 메뉴얼은 2450 보드 후진 영어 DD는-> 삽질이 많이 들어가는 분야, 해야될께많아
    - 리눅스 디바이스 드라이버 유영창 저 절판, 단계별로 올라가기 좋은 책
    - 비특권모드에서 특권모드로는 못넘어와(23p)
  > 아래 소스는 DD의 기본 골격이 되는 SK(스켈레톤) 소스
  - KDD 소스 코드
      ```cpp
      #include <linux/module.h>
      #include <linux/init.h>
      #include <linux/major.h>
      #include <linux/fs.h>
      #include <linux/cdev.h>

      MODULE_LICENSE("GPL");

      static int sk_major = 0, sk_minor = 0;
      static int result;
      static dev_t sk_dev;

      static struct cdev sk_cdev;

      static int sk_register_cdev(void);

      /* TODO: Define Prototype of functions */
      static int sk_open(struct inode *inode, struct file *filp);
      static int sk_release(struct inode *inode, struct file *filp);

      /* TODO: Implementation of functions */
      static int sk_open(struct inode *inode, struct file *filp)
      {
         printk("Device has been opened...\n");

         /* H/W Initalization */

         //MOD_INC_USE_COUNT;  /* for kernel 2.4 */

         return 0;
      }

      static int sk_release(struct inode *inode, struct file *filp)
      {
         printk("Device has been closed...\n");

         return 0;
      }

      struct file_operations sk_fops = {
         .open       = sk_open,
         .release    = sk_release,
      };

      static int __init sk_init(void)
      {
         printk("SK Module is up... \n");

            if((result = sk_register_cdev()) < 0)
            {
                     return result;
            }

         return 0;
      }

      static void __exit sk_exit(void)
      {
         printk("The module is down...\n");
            cdev_del(&sk_cdev);
            unregister_chrdev_region(sk_dev, 1);
      }

      static int sk_register_cdev(void)
      {
            int error;

            /* allocation device number */
            if(sk_major) {
                     sk_dev = MKDEV(sk_major, sk_minor);
                     error = register_chrdev_region(sk_dev, 1, "sk");
                     // sk_major에 0일때 ->> 
            } else {
                     error = alloc_chrdev_region(&sk_dev, sk_minor, 1, "sk");
                     sk_major = MAJOR(sk_dev);
                     // sk_major에 0이 아닌 다른 함수로 설저정일때 ->> 
            }

            if(error < 0) {
                     printk(KERN_WARNING "sk: can't get major %d\n", sk_major);
                     return result;
            }
            printk("major number=%d\n", sk_major);

            /* register chrdev */
            cdev_init(&sk_cdev, &sk_fops);
            sk_cdev.owner = THIS_MODULE;// THis 포인터
            sk_cdev.ops = &sk_fops; // 주소를 넘겨 줘
            error = cdev_add(&sk_cdev, sk_dev, 1);// 최종적으로 이렇게 넘겨 줘, 캐릭터 디바이스 에드

            if(error)
                     printk(KERN_NOTICE "sk Register Error %d\n", error);

            return 0;
      }
      module_init(sk_init);
      module_exit(sk_exit);
      ```
  - APP단 소스
      ```cpp
      #include <stdio.h>
      #include <unistd.h>
      #include <stdlib.h>
      #include <fcntl.h>

      int main(void)
      {
         int fd;

         fd = open("/dev/SK", O_RDWR);//dev sk pointing, mknod 필요!!
         printf("fd = %d\n", fd);

         if (fd<0) {
            perror("/dev/SK error");
            exit(-1);
         }
         else
            printf("SK has been detected...\n");

         getchar();
         close(fd);

         return 0;
      }
      ~        
      ```
> ARM용 크로스 컴파일러로 컴파일됬는지 확인(호스트는 x86)
  - GCC 로 컴파일하지말고 arm용 크로스 컴파일러로 
      ```s
      root@ubuntu-vm ~/open
      # ls
      sk.ko     sk.mod.o  sk_app    sk_app_open
      Makefile  sk.c      sk.mod.c  sk.o      sk_app.c
      =>>파일 확인(sk_app)
      root@ubuntu-vm ~/open
      # file sk_app
      sk_app: ELF 32-bit LSB executable, ARM, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.16, not stripped
      root@ubuntu-vm ~/open
      # cp sk_app /nfs/rootfs/workSpace/
      root@ubuntu-vm ~/open
      # 
      ```
  > 유저 어플리케이션 실행 시도
  -  주목 놓치면 안되!!
      ```s
      # ls
      csk.ko   sk_app
      # ./sk_app
      fd = -1
      /dev/SK error: No such file or directory
      ```
  - 위 실행에서 에러가 난 이유 : 파일이 없어서 , 장치 파일로 등록이 안되있어서 오픈이 안되있다. 장치파일을 다 이런식으로  연결되어 있음
  - 리눅스/유닉스의 철학 : Everything is File
  > 해결 방법 도출
  - mknod /dev/SK c 251 0 라는 디바이스 파일(윈도우의 핸들러 개념)등록함으로 해결
      ```s
      # cd /workSpace/
      # ls
      sk.ko   sk_app
      # insmod sk.ko
      SK Module is up...
      major number=251
      # mknod /dev/SK c 251 0
      # ./sk
      sk.ko   sk_app
      # ./sk_app
      Device has been opened...
      fd = 3
      SK has been detected...
      ```
  - mknod란 : 리눅스 장치파일
  - 리눅스 OS는 하드웨어(디바이스, 장치)를 파일의 형태로 추상화 시켜서 생각하는데, 이때 특정 KDD를 핸들링 하기 위해 커널과 USER사이를 연결시켜주는것이 File임
  - File을 가지고 하드웨어 컨트롤을 할수 있게 도와주는 함수(메서드)를 파일 오퍼레이션라고 
  - write함수 추가하려면
      ```cpp
      struct file_operations sk_fops = {
         .open       = sk_open,
         .release    = sk_release,
         .write      = (여기에 입력)
      };
      ```
  - write함수 프레임만 맞게 짜기 시작해야됭
  - /dev/SK :  디바이스 드라이버 이름 > 만드는법
      ```s
      mknod /dev/SK c 251 0
      ```
  - 디바이스 드라이버는 대문자로 꼭 만들어야됭
  - 디바이스 드라이버를 만드는데 핑요한 구조체 : dev_t
      ```cpp
      static dev_t sk_dev;
      ```
 > 강사님의 핵심
  - 이쪽은 모듈 요기는 스트럭쳐
  - 장치 파일로 바꿔야 특수장치파일로 만들어줘야지 어플리케이션에서 열 수 있다.
  - 오픈을 하면서 파일 디스크립터를 
  - SK : 스켈렉톤, 뼈대라는 의미
  - /dev/SK 특수 장치 파일
      ```cpp
      static int sk_write(struct file *filp, const char *buf, size_t count, loff_t *f_pos)
      {
            char data[11];
            copy_from_user(data, buf, count);
            printk("data >>>>> = %s\n", data);
            return count;
      }

      static int sk_read(struct file *filp, char *buf, size_t count, loff_t *f_pos)
      {
            char data[20] = "this is read func...";
            copy_to_user(buf, data, count);
            return 0;
      }
      ```
  - copy_to_user : 커널에서 유저(APP)단으로
  - copy_from_user : User에서 커널단으로 
  - 디바이스 이름 : sk
  - 0이면 커널이 알아서 dev num 지정해주는것, !0 이면, 
  > user APP 영역의 read() 함수
   ```cpp
   retn = read(fd, buf, 20); //APP에서의 syscall
   ```
  > user APP 영역의 write() 함수
   ```
   retn = write(fd, buf, 10);//APP에서의 syscall
   ```
  > Device Tree에 대한 설명
   ```
   디바이스 트리란? : 커널 4.x 부터 도입된 개념으로 기존의 D.D를 좀더 쉽게 접근하고, 생성해주는 스크립트 언어
   디바이스 트리 공부를 위해서는 : 하드웨어는 라즈베리파이3, 문서는 아래 링크 참고
   ```
  ### Day2 과제
   - 과제1 : 어플리케이션 계층에서 일차원 문자배열을 선언하고 그 주소를 read() 함수를 이용하여 커널영역으로 넘겨주면, 커널영역에서 대문자를 할당하여 copy_to_user/copy_from_user 를 사용하여 다시 응용계층으로 넘겨주면 최종적으로 어플단에서 대문자를 찍는 프로그램을 작성하시고, copy_from_user 도 적용 해보세요(소문자 찍기)
   - 과제 못풀음 삽질하다가 ㅜㅜ 코딩연습 더하자
   - 구조체 과제(조별과제)
   > 이 과제의 목표 : user APP 영역과 Kernel DD영역 사이의 유기적인 연동
   ```s
   copy_to_user()/copy_from_user() 사용
   ```
   - 1차과제 
   > USER APP 의 test_mydrv.c
   ```cpp
   /* test_mydrv.c */
   //유저 영역의 어플리케이션 코드
   #include <stdio.h>
   #include <stdlib.h>
   #include <sys/types.h>
   #include <fcntl.h>

   #define MAX_BUFFER 26

   char buf_in[MAX_BUFFER], buf_out[MAX_BUFFER];
   //입력버퍼 출력버퍼 공간 생성

   int main()
   {
   int fd,i;
   
   fd = open("/dev/mydrv",O_RDWR);
   //DD오픈, mydrv_open() 함수 호출 됨

   read(fd,buf_in,MAX_BUFFER);
   //DD에서의 mydrv_read() 함수 호출됨
   // open()에서 얻은 fd, 입력 버퍼 주소, 버퍼 크기
   printf("buf_in = %s\n",buf_in);
   //버퍼에 어떤 값이 채워져서 buf_in이 됨

   for(i = 0;i < MAX_BUFFER;i++)
      buf_out[i] = 'a' + i;
   write(fd,buf_out,MAX_BUFFER);
   //쓰기버퍼 , 파일디스크립터, 버퍼주소, 버퍼 최대길이
   
   close(fd);
   //디바이스 드라이버 닫기
   
   return (0);
   }

   ```
   > KDD 의 모듈 DD 코드
   ```cpp
   /*
   mydrv.c - kernel 3.0 skeleton device driver
                  copy_to_user()
   */
   // 커널 영역의 디바이스 드라이버 코드
   #include <linux/module.h>
   #include <linux/moduleparam.h>
   #include <linux/init.h>
   #include <linux/kernel.h>   /* printk() */
   #include <linux/slab.h>   /* kmalloc() */
   #include <linux/fs.h>       /* everything... */
   #include <linux/errno.h>    /* error codes */
   #include <linux/types.h>    /* size_t */
   #include <asm/uaccess.h>
   #include <linux/kdev_t.h>
   #include <linux/cdev.h>
   #include <linux/device.h>

   #define DEVICE_NAME "mydrv"
   static int mydrv_major = 240;//디바이스 메이저 넘버 명시적할당
   module_param(mydrv_major, int, 0);//모듈 파라미터.,,,>?


   static int mydrv_open(struct inode *inode, struct file *file)
   {//DD의 오픈함수 
   printk("mydrv opened !!\n");
   return 0;
   }

   static int mydrv_release(struct inode *inode, struct file *file)
   {//DD의 클로즈 함수
   printk("mydrv released !!\n");
   return 0;
   }

   static ssize_t mydrv_read(struct file *filp, char __user *buf, size_t count,loff_t *f_pos)
   {//리드함수, 파일포인터, 유저버퍼 함수포인터, 버퍼사이즈, pos받음, USER영역의 함수와 1:1매칭 되지 않음
   char *k_buf;
   int i;
   // 리드함수니까 유저 영역에서 read함수가 호출 된 상황
   k_buf = kmalloc(count,GFP_KERNEL);//멜록으로 공간 할당
   for(i = 0 ;i < count;i++) {
         k_buf[i] = 'A' + i;//넘겨받은 버퍼 주소 위치에다가 차례로 쓰기
   }
   if(copy_to_user(buf,k_buf,count)) {
      return -EFAULT;//에러 핸들링
   }
   printk("mydrv_read is invoked\n");//커널 메시지 출력
   kfree(k_buf);// 할당받은 공간 해제
   return 0;

   }

   static ssize_t mydrv_write(struct file *filp,const char __user *buf, size_t count,
                              loff_t *f_pos)
   {
   char *k_buf;
   //유저 영역에서 Write함수를 호출한경우 DD에서 Write함수가 호출 되는것을 확인 함.
   k_buf = kmalloc(count,GFP_KERNEL);//커널 맬럭으로 임시 저장공간 할당받음
   if(copy_from_user(k_buf,buf,count)) {
      return -EFAULT;//에러 핸들링,copy_from_user() 함수가 0이 아닌 수를 리턴하면 해당 케이스로 진입하여 에러를 반환
   }
   printk("k_buf = %s\n",k_buf);
   printk("mydrv_write is invoked\n");
   kfree(k_buf);
   return 0;
   }


   /* Set up the cdev structure for a device. */
   static void mydrv_setup_cdev(struct cdev *dev, int minor,
         struct file_operations *fops)
   {
      int err, devno = MKDEV(mydrv_major, minor);//devno 번호에 
      
      cdev_init(dev, fops);
      dev->owner = THIS_MODULE;
      dev->ops = fops;
      err = cdev_add (dev, devno, 1);
      
      if (err)
         printk (KERN_NOTICE "Error %d adding mydrv%d", err, minor);
   }


   static struct file_operations mydrv_fops = {
      .owner    = THIS_MODULE,
         .read	  = mydrv_read,
      .write    = mydrv_write,
      .open     = mydrv_open,
      .release  = mydrv_release,
   };

   #define MAX_MYDRV_DEV 1

   static struct cdev MydrvDevs[MAX_MYDRV_DEV];

   static int mydrv_init(void)
   {// DD 이니셜라이즈 함수
      int result;
      dev_t dev = MKDEV(mydrv_major, 0);//MKDEV()는 시스템 함수
      //dev_t 구조체 변수 이름 dev에다가 MKDEV() 리턴 값 대입
      
      /* Figure out our device number. */
      if (mydrv_major)//mydrv_major!=0 인경우, 디바이스넘버 명시한경우 In case
         result = register_chrdev_region(dev, 1, DEVICE_NAME);//시스템 함수 호출
      else {//디바이스 넘버 명시하지 않은경우 
         result = alloc_chrdev_region(&dev,0, 1, DEVICE_NAME);//시스템 함수 호출
         mydrv_major = MAJOR(dev);//시스템 함수 호출해서 번호 할당
      }
      if (result < 0) {//에러 핸들링
         printk(KERN_WARNING "mydrv: unable to get major %d\n", mydrv_major);
         return result;
      }
      if (mydrv_major == 0)
         mydrv_major = result;

      mydrv_setup_cdev(MydrvDevs,0, &mydrv_fops);//이 함수는 User함수
      printk("mydrv_init done\n");	
      return 0;
   }

   static void mydrv_exit(void)
   {
      cdev_del(MydrvDevs);
      unregister_chrdev_region(MKDEV(mydrv_major, 0), 1);
      printk("mydrv_exit done\n");
   }

   module_init(mydrv_init);
   module_exit(mydrv_exit);

   MODULE_LICENSE("Dual BSD/GPL");
   ```

  ### 과제2
   - 과제2 : 응용프로그램이 보내준 아래 구조체 형식의 데이터를 드라이버가 받아서 출력시키고 드라이버는 같은 구조체 형식으로 또 다른 데이터를 응용프로그램에게 보내주고 응용프로그램이 출력시키는 코드를 구현하세요
   ```cpp
   /*  구조체 포맷  */
   typedef struct
   {
      int age;  //나이 :35
      char name[30];// 이름 : HONG KILDONG
      char address[20]; // 주소 : SUWON CITY
      int  phone_number; // 전화번호 : 1234
      char depart[20]; // 부서 : mds
   } __attribute__ ((packed)) mydrv_data;

   //   sprintf(k_buf->name,"HONG KILDONG");
   ```
 - copy to user 
    1. APP 영역에서의 write함수 호출
    2. DD에서 -> struct file operations 에서 등록 : wrtie = sk_write
    3. 안적었음.. 뭦;
 - attriibute Packed : 구조체 peding bytes 제거하는 예약 키워드  
#
## 3일차
  - 강조 점 fileops. 함수포인터가 늘어나고 있어요
    - 기본기는 항상 중요(C언어, 함수포인터 ..)
  - DD프로그래밍 핵심
    1. 리눅스 커널이 제공해주는 서비스, 내가 다 안짜도 이미 잘 짜여진 틀 프레임워크로써의 리눅스 커널을 느껴보기
    2. 유저 어플리케이션은 잘못해도 커널이 보호해준다(안전장치 존재)
    3. 디바이스드라이버는 커널 프로그래밍 -> DD는 조금만 잘못해도 커널 패닉 발생 
    4. 커널패닉은 매우 위험, 커널 프로그래밍은 책임이 따른다
  - volatile 의미 두가지
    1. 최적화 하지마라
    2. 캐시에서 읽지 말고 메모리에서 읽어와라
  - 현업 가면 만날수 있는 상황
    - LDD(리눅스 디바이스 드라이버)로 어떻게든 빨리 성과를 내라
    - 그래서 지금 해야 할 일은 LDD를 실습하면서 이렇게 짜고 동작한다를 느끼면 됨
      - 유저 어플리케이션에서의 파일 오퍼레이션스 호출이
      - 디바이스 드라이버 단에서 어떤 매서드를 호출하는지
      - 리눅스 응용 어플리케이션과 디바이스 드라이버 사이에 유기적인 연동과정
      - 임베디드 리눅스 환경에서 하드웨어가 어떻게 파일로 추상화되고 , 파일 오퍼레이션스를 통해 어떻게 컨트롤 할수 있는지에 대한 믿음을
      - 프레임워크(작업 환경)으로써의 리눅스 커널과, 작업을 수행하는 DD에 대하여
      > cat /proc/devices : 현제 등록된 디바이스 정보를 출력하는 명령어
      ```s
      # cat /proc/devices
      Character devices:
      1 mem
      2 pty
      3 ttyp
      4 /dev/vc/0
      4 tty
      4 ttyS
      5 /dev/tty
      5 /dev/console
      5 /dev/ptmx
      7 vcs
      10 misc
      13 input
      21 sg
      29 fb
      67 mds2450-kscan
      81 video4linux
      86 ch
      90 mtd
      108 ppp
      116 alsa
      128 ptm
      136 pts
      180 usb
      188 ttyUSB
      189 usb_device
      204 ttySAC
      216 rfcomm
      251 sk
      252 BaseRemoteCtl
      253 usbmon
      254 rtc

      Block devices:
      1 ramdisk
      259 blkext
      7 loop
      8 sd
      31 mtdblock
      65 sd
      66 sd
      67 sd
      68 sd
      69 sd
      70 sd
      71 sd
      128 sd
      129 sd
      130 sd
      131 sd
      132 sd
      133 sd
      134 sd
      135 sd
      179 mmc
      #
      ```
  - 3일차의 핵심 : ictol_mydrv.h
      ```cpp
      #ifndef _IOCTL_MYDRV_H_
      #define _IOCTL_MYDRV_H_
      //헤더파일용 선언
      #define IOCTL_MAGIC    254
      //매직넘버..?
      typedef struct
      {
         unsigned char data[26];	
      } __attribute__ ((packed)) ioctl_buf;
      //구조체가 담고있는것 캐릭터배열 26개짜리 왜 근데 계속 26개냐

      #define IOCTL_MYDRV_TEST           _IO(  IOCTL_MAGIC, 0 )
      #define IOCTL_MYDRV_READ           _IOR( IOCTL_MAGIC, 1 , ioctl_buf )
      #define IOCTL_MYDRV_WRITE          _IOW( IOCTL_MAGIC, 2 , ioctl_buf )
      #define IOCTL_MYDRV_WRITE_READ     _IOWR( IOCTL_MAGIC, 3 , ioctl_buf )

      #define IOCTL_MAXNR                   4

      #endif // _IOCTL_MYDRV_H_
      ```
  - 위 파일은 디바이스드라이버를 짤대 정말 많이 등장 강사님 시간많이투자
  - 어려울껀 없고 금방 하세요
  - 3개를 봐야함  
  1. _IO
  2. _IOR
  3. _IOW
  - 매직넘버는 알아서 줘도됨
   ```cpp
   ioctl(fd, IOCTL_MYDRV_TEST);
   ioctl(fd, IOCTL_MYDRV_READ, buf_in );
   ```
  - 어바웃 포팅 : 리눅스 포팅을 진행하며 
     - 창호형이 리눅스 포팅을 하기로 했어
     - 삼성에서 chip과 보드를 같이 출시를 해 (데모)
     - 왠만큼 소스와 회로 오픈해줘 - 업체에서 가져다가 모디파이해서 만들면 됭
  - 오늘의 버그 증상 : DD가 첫회만 잘 돈다.. 첫번째 부팅 후 정상동작 확인한 다음에 모듈을 내렸다가 다시 올리려고 하면 해당 메시지 발생함.
   ```s
   # insmod hello.ko
   Hello DD world !!
   IRQ_EINT3 failed to request external interrupt.
   insmod: can't insert 'hello.ko': unknown symbol in module, or unknown parameter
   ```
  - APP 코드
   ```cpp
   /* test_mydrv.c */
   #include <stdio.h>
   #include <stdlib.h>
   #include <sys/types.h>
   #include <fcntl.h>

   #define MAX_BUFFER 26
   char buf_in[MAX_BUFFER], buf_out[MAX_BUFFER];

   int main()
   {
   int fd,i,mode;
   
   fd = open("/dev/mydrv",O_RDWR);
   printf("fd = %d\n",fd);

   if(fd == -1) {
      printf("you are fail to open div files, if you success ...\n");
      printf("mknod /dev/mydrv c 240 0\n");
      exit(1);
   }

   printf("APP : Read Func\n");
   read(fd,buf_in,MAX_BUFFER);
   printf("APP : Read() -> buf_in = %s\n",buf_in);
   

   printf("APP : Write Func\n");
   for(i = 0;i < MAX_BUFFER;i++)
      buf_out[i] = 'a' + i;
   write(fd,buf_out,MAX_BUFFER);
   printf("APP : END Defalut DEMO\n");


   printf("APP : start while demo\n");
   printf("APP : \n");
   strcpy(buf_out,"I'm Application\n");
   write(fd,buf_out,MAX_BUFFER);

   close(fd);
   return (0);
   }
   ```
   - DD 코드
   ```cpp
   #include <linux/module.h>	/* Needed by all modules */
   #include <linux/kernel.h>	/* Needed for KERN_INFO	*/
   #include <linux/init.h>		/* Needed for the macros */
   #include <linux/kdev_t.h>
   #include <linux/cdev.h>
   #include <linux/errno.h>
   #include <linux/slab.h>
   #include <linux/delay.h>
   #include <linux/interrupt.h>
   #include <linux/device.h>
   #include <linux/fs.h>       /* everything... */
   #include <linux/types.h>    /* size_t */

   #include <asm/io.h>
   #include <asm/irq.h>
   #include <asm/uaccess.h>

   #include <mach/gpio.h>
   #include <mach/regs-gpio.h>
   #include <plat/gpio-cfg.h>


   #define	DRIVER_AUTHOR	"DHKim of southkorea"
   #define	DRIVER_DESC		"DHKim sample DD"
   #define DRV_NAME        "mydrv"
   #define DEVICE_NAME 	"mydrv"
   #define MAX_MYDRV_DEV 1

   static int mydrv_major = 240;
   module_param(mydrv_major, int, 0);


   //from NCH.. jubjub!
   static char DDin_buf[50];
   static char DDout_buf[50];

   //
   static void mydrv_setup_cdev(struct cdev *dev, int minor,struct file_operations *fops);

   //
   static int mydrv_open(struct inode *inode, struct file *file)
   {
   printk("DHKim DD.drv opened !!\n");
   return 0;
   }

   static int mydrv_release(struct inode *inode, struct file *file)
   {
   printk("DHKim D.drv released !!\n");
   return 0;
   }


   static ssize_t mydrv_read(struct file *filp, char __user *buf, size_t count,
                  loff_t *f_pos)
   {
   char *k_buf;
   int i;
      printk("read in func\n");

   k_buf = kmalloc(count,GFP_KERNEL);
   for(i = 0 ;i < count;i++)
         k_buf[i] = 'A' + i;
   
   if(copy_to_user(buf,k_buf,count)) {
      return -EFAULT;
   }
   printk("read is invoked in kerneland HEAP\n");
   
   if(copy_to_user(buf, DDout_buf, count)) {
      return -EFAULT;
   }
   printk("success cpy2u() ->> DDout_buf");

   kfree(k_buf);
   return 0;

   }

   static ssize_t mydrv_write(struct file *filp,const char __user *buf, size_t count,
                              loff_t *f_pos)
   {
   char *k_buf;
   printk("write in func\n");

   k_buf = kmalloc(count,GFP_KERNEL);
   if(copy_from_user(k_buf,buf,count)) {
      return -EFAULT;
   }
   printk("k_buf = %s\n",k_buf);
   printk("write is invoked\n");
   
   if(copy_from_user(DDin_buf, buf, count)) {
      return -EFAULT;
   }
   printk("copy_from_user() func success ->>DDin_buf");
   kfree(k_buf);
   return 0;
   }


   static irqreturn_t keyinterrupt_func1(int irq, void *dev_id, struct pt_regs *resgs)
   {
         printk("Key 0 pressed!!!(%d)\n",irq);
         printk("KDD : %s",DDin_buf);
         return IRQ_HANDLED;
   }

   static irqreturn_t keyinterrupt_func2(int irq, void *dev_id, struct pt_regs *resgs)
   {
         printk("Key 1 pressed!!!(%d)\n",irq);
         return IRQ_HANDLED;
   }

   static irqreturn_t keyinterrupt_func_mls(int irq, void *dev_id, struct pt_regs *resgs)
   {		//GPG2,3,4,5,6 =>> 5 keys
         //printk("Key (%d) pressed!!!\n",irq);
         switch(irq)
         {
            case 18 ://in case of SW4,9
               printk("in case of SW4,9\n");
               break;
            case 19 ://in case of SW5,10
               printk("in case of SW5,10\n");
               break;
            case 48 ://in case of SW6,11
               printk("in case of SW6,11\n");
               break;
            case 49 ://in case of SW7,12
               printk("in case of SW7,12\n");
               break;
            case 50 ://in case of SW8,13
               printk("in case of SW8,13\n");
               break;
         }


         return IRQ_HANDLED;
   }
   /*
   //how to solv this 10 keys problem
   1.GPF7,GPG0 set 0(Low) -> every 10 key recognize btn
   2.if btn is pushh ->> through in case of inerrput
   3.for ex case 18, ->> 
   4.1. up 5 keys
   rGPFDAT &= ~(0x1<<7);   //Clear GPF7
   rGPGDAT |= (0x1);       //Set GPG0
   4.2 down 5 key
   rGPFDAT |= (0x1<<7);    //Set GPF7
   rGPGDAT &= ~(0x1);      //Clear GPG0
   5. each row(colum?), set port GPIO, NOT EXINT
   6. Read GPIO (decision Making) up or down
   7. save exect key 
   8. recover every key interrupt mode
   9. return exect key (swq about no.7)
   */
   static void mydrv_setup_cdev(struct cdev *dev, int minor,
         struct file_operations *fops)
   {
      int err, devno = MKDEV(mydrv_major, minor);
      
      cdev_init(dev, fops);
      dev->owner = THIS_MODULE;
      dev->ops = fops;
      err = cdev_add (dev, devno, 1);
      
      if (err)
         printk (KERN_NOTICE "Error %d adding mydrv%d", err, minor);
   }


   /***********************************************
   ************************************************
   ************************************************
   ***********************************************/
   static struct file_operations mydrv_fops = {
      .owner   = THIS_MODULE,
         .open    = mydrv_open,
      .read	 = mydrv_read,
      .write   = mydrv_write,
      .release = mydrv_release,
   };


   static struct cdev MydrvDevs[MAX_MYDRV_DEV];

   static int __init init(void)
   {
      int result;
      int ret;
      dev_t dev = MKDEV(mydrv_major, 0);
      //=section of inerterupt=======================================================================
      printk(KERN_INFO "Hello DD world !!\n");
            ///*
         // set Interrupt mode
         s3c_gpio_cfgpin(S3C2410_GPF(0), S3C_GPIO_SFN(2));
         s3c_gpio_cfgpin(S3C2410_GPF(1), S3C_GPIO_SFN(2));
         s3c_gpio_cfgpin(S3C2410_GPF(2), S3C_GPIO_SFN(2));
         s3c_gpio_cfgpin(S3C2410_GPF(3), S3C_GPIO_SFN(2));
         s3c_gpio_cfgpin(S3C2410_GPF(4), S3C_GPIO_SFN(2));
         s3c_gpio_cfgpin(S3C2410_GPF(5), S3C_GPIO_SFN(2));
         s3c_gpio_cfgpin(S3C2410_GPF(6), S3C_GPIO_SFN(2));

         if(request_irq(IRQ_EINT0,(void *)keyinterrupt_func1,IRQF_DISABLED|IRQF_TRIGGER_FALLING, DRV_NAME, NULL))
         {
                  printk("IRQ_EINT0 failed to request external interrupt.\n");
                  ret = -ENOENT;
                  return ret;
         }

         if(request_irq(IRQ_EINT1, (void *)keyinterrupt_func2,IRQF_DISABLED|IRQF_TRIGGER_FALLING, DRV_NAME, NULL)) 
         {
                  printk("IRQ_EINT1 failed to request external interrupt.\n");
                  ret = -ENOENT;
                  return ret;
         }

         if(request_irq(IRQ_EINT2, (void *)keyinterrupt_func_mls,IRQF_DISABLED|IRQF_TRIGGER_FALLING, DRV_NAME, NULL)) 
         {
                  printk("IRQ_EINT2 failed to request external interrupt.\n");
                  ret = -ENOENT;
                  return ret;
         }

         if(request_irq(IRQ_EINT3, (void *)keyinterrupt_func_mls,IRQF_DISABLED|IRQF_TRIGGER_FALLING, DRV_NAME, NULL)) 
         {
                  printk("IRQ_EINT3 failed to request external interrupt.\n");
                  ret = -ENOENT;
                  return ret;
         }

         if(request_irq(IRQ_EINT4, (void *)keyinterrupt_func_mls,IRQF_DISABLED|IRQF_TRIGGER_FALLING, DRV_NAME, NULL)) 
         {
                  printk("IRQ_EINT4 failed to request external interrupt.\n");
                  ret = -ENOENT;
                  return ret;
         }

         if(request_irq(IRQ_EINT5, (void *)keyinterrupt_func_mls,IRQF_DISABLED|IRQF_TRIGGER_FALLING, DRV_NAME, NULL)) 
         {
                  printk("IRQ_EINT5 failed to request external interrupt.\n");
                  ret = -ENOENT;
                  return ret;
         }

         if(request_irq(IRQ_EINT6, (void *)keyinterrupt_func_mls,IRQF_DISABLED|IRQF_TRIGGER_FALLING, DRV_NAME, NULL)) 
         {
                  printk("IRQ_EINT6 failed to request external interrupt.\n");
                  ret = -ENOENT;
                  return ret;
         }

         printk(KERN_INFO "%s successfully intq loaded\n", DRV_NAME);
         //*/
      //=section of inerterupt=======================================================================
      printk("This Module major number : 240\n");
      if (mydrv_major)
         result = register_chrdev_region(dev, 1, DEVICE_NAME);
      else {
         result = alloc_chrdev_region(&dev,0, 1, DEVICE_NAME);
         mydrv_major = MAJOR(dev);
      }
      if (result < 0) {
         printk(KERN_WARNING "mydrv: unable to get major %d\n", mydrv_major);
         return result;
      }
      if (mydrv_major == 0)
         mydrv_major = result;

      mydrv_setup_cdev(MydrvDevs,0, &mydrv_fops);
      printk("Hello.ko module init done\n");	

      return 0;
   }

   static void __exit cleanup(void)
   {
      cdev_del(MydrvDevs);
      unregister_chrdev_region(MKDEV(mydrv_major, 0), 1);
      printk("hello.ko DD module exit done\n");
      //=====================================================
      ///*
      printk(KERN_INFO "Goodbye, world 4.\n");
      free_irq(IRQ_EINT0, NULL);
      free_irq(IRQ_EINT1, NULL);
      free_irq(IRQ_EINT2, NULL);
      //free_irq(IRQ_EINT3, NULL);
      //free_irq(IRQ_EINT4, NULL);
      //free_irq(IRQ_EINT5, NULL);
      printk(KERN_INFO "%s successfully removed\n", DRV_NAME);
      //*/
   }

   module_init(init);
   module_exit(cleanup);
   /* Get rid of taint message by declaring code as GPL. */
   MODULE_LICENSE("GPL");
   MODULE_AUTHOR(DRIVER_AUTHOR);		/* Who wrote this module? */
   MODULE_DESCRIPTION(DRIVER_DESC);	/* What does this module do */
   MODULE_SUPPORTED_DEVICE("testdevice");
   ```
  > 여기서부터 책 정리
  - 캐릭터 디바이스 등록과 해제 관련한 디바이스 드라이버 API 총 정리 : 57p 
      ```cpp
      cdev_init
      cdev_add
      cdev_del
      registor_chrdev_region
      alloc_chrdev_region
      unregistor_chrdev_region
      ```
  - File operations의 4가지 역할/특징
    1. LDD와 APP을 연결
    2. 함수 포인터의 집합
    3. 특정 동작함수를 포인팅
    4. 지정하지 않으면 필수적으로 NULL
  - 시그널의 3가지 동작모드
    1. default : 기본동작
    2. abort : 무시
    3. Handler : 유저 정의 동작
  - KDD와 APP간 데이터의 전달 방법 4가지
    1. set user() : 주어는 커널
    2. get user() : 주어는 커널
    3. copy_to_user() :  주어는 커널
    4. copy_from_user() :  주어는 커널
  - 커널모듈과 DD간의 차이 두가지
    1. 파일 오퍼레이션스 유무(DD는 존재함)
    2. 장치 특수파일(/dev/proc/char?) -> mknod로 장입
  - KDD of 캐릭터 디바이스  
    - 캐릭터 디바이스 API 학습
    - Major 번호 결정 : Mannual allocation과 Dynamic 두가지 방식 존재
    - 디바이스 타입 Dev_t 장치 관리 번호
    - 장치 파일의 생성 : mknod 
    - 목적 : DD로 사용할 장치 파일을 만든다
    - 장치파일의 생성은 어떤 디바이스 이름의 생성과 같음
    - File operations -> 앞에서 선술 함.
 #
 ## Day4
 - 커널로그(타이머 인터럽트 확인)
   ```s
   root@ubuntu-vm
   kernel-mds2450-3.0.22/led
   # tail -f /var/log/messages 
   ```
```cpp
#include <linux/module.h>
#include <linux/kernel.h>
#include <linux/init.h>
#include <linux/delay.h>
#include <linux/timer.h>
struct timer_list timer;

void my_timer(unsigned long data)//이거는 리눅스 커널개발자들이
{
  int i;
  for(i=1; i<=data; i++) {
    printk("Kernel Timer Time-Out Function Doing_%d...\n", i);
  }
  timer.expires = jiffies + 2*HZ;//여기 두줄이 없으면
  add_timer(&timer);//타이머가 한번밖에 안결려
  // 마치 테엽을 감아주는 듯한 동작
  printk("Kernel Timer Time-Out Function Done!!!\n");
}

int timerTest_init(void)
{
  printk(KERN_INFO "timerTest Module is Loaded!! ....\n");
  init_timer(&timer);
  //timer.expires = get_jiffies_64() + 3*HZ;
  timer.expires = jiffies + 3*HZ;
  timer.function = my_timer;
  timer.data = 5;
  add_timer(&timer);

  return 0;
}

void timerTest_exit(void)
{
  del_timer(&timer);
  printk("timerTest Module is Unloaded ....\n");
}

module_init(timerTest_init);
module_exit(timerTest_exit);
MODULE_LICENSE("Dual BSD/GPL");

/*
 #tail -f /var/log/messages
*/
```

- 강사님의 퀘스트
1. PC-> ARM 포팅
2. ARM기반 보드에서 LED 제어(타이머 인터럽트)
3. DD작성
4. 커널에 포함
```cpp
printk("LED on\n");
GPGDAT ^= (0xf << 4);
```

- 드라이버를 커널에 포함하기 작업
1단계
```s
root@ubuntu-vm ~/kernel-mds2450-3.0.22/drivers/char
# pwd
/root/kernel-mds2450-3.0.22/drivers/char
root@ubuntu-vm ~/kernel-mds2450-3.0.22/drivers/char
# gedit Kconfig
```
- Kconfig 파일속 8라인부터 추가하기
```s
config MDS2450_LED
	tristate "MDS2450 LED driver"
	depends on INPUT
	  help
	mds2450 hello driver
```
- 파일 내용 전체
```s
#
# Character device configuration
#

menu "Character devices"

source "drivers/tty/Kconfig"

config MDS2450_LED
	tristate "MDS2450 LED driver"
	depends on INPUT
	  help
	mds2450 hello driver

config DEVKMEM
	bool "/dev/kmem virtual device support"
	default y
	help
	  Say Y here if you want to support the /dev/kmem device. The
	  /dev/kmem device is rarely used, but can be used for certain
	  kind of kernel debugging operations.
	  When in doubt, say "N".
....(중략)...
```

- 캐릭터 DD의 Makefile 수정"/root/kernel-mds2450-3.0.22/drivers/char" 경로상의 Makefile 수정
```s
obj-y				+= mem.o random.o
obj-$(CONFIG_TTY_PRINTK)	+= ttyprintk.o
obj-y				+= misc.o
obj-$(CONFIG_MDS2450_LED)	+= timerTest_mod.o
obj-$(CONFIG_ATARI_DSP56K)	+= dsp56k.o
obj-$(CONFIG_VIRTIO_CONSOLE)	+= virtio_console.o
```
리눅스의 모든 커널마다 하나씩 살고 있는 파일
스페이스바 ECS
```s
...(중략)...
  CC      arch/arm/boot/compressed/decompress.o
  SHIPPED arch/arm/boot/compressed/lib1funcs.S
  AS      arch/arm/boot/compressed/lib1funcs.o
  LD      arch/arm/boot/compressed/vmlinux
  OBJCOPY arch/arm/boot/zImage
  Kernel: arch/arm/boot/zImage is ready
root@ubuntu-vm ~/kernel-mds2450-3.0.22
# ^C
root@ubuntu-vm ~/kernel-mds2450-3.0.22
# 

```
- 정리(강사님의)
한일: 커널타이머 만들어썽, 커널내부에서 도는거야 모듈형태로 스터디 디바이스드라이버 형태로 바꾸세요 타이머 핸들러 기능 검증 됬어요 커널 부팅될떄 심어보자, 
- 시퀀스 정리
- 강사님의 추가과제
  - 현상황 커널단에 타이머DD 심어진 상황
  - 
  1. 컴파일러 셋팅
  2. DD 모듈 수정 확인 -> "/root/kernel-mds2450-3.0.22/drivers/char" 경로상에 존재하는지 확인
  3. char폴더 에서의  Kconfig 편집 ->> "Kconfig 파일속 8라인부터 추가하기" 항목 참조
  4. char폴더 에서의 Makefile 수정 ->> 위 항목 참조
5. 
6. 두칸앞나가서 (커널폴더) Make menuconfig 술정
7. DD->char->MDS2450 아스테리스터 커널포함 (스페이스바로 셋팅)->> ESC눌러서 나가는데 꼭 저장
8. 커널폴더에서 make clean , make
9. 아래 확인
```s
root@ubuntu-vm ~/kernel-mds2450-3.0.22/arch/arm/boot
# pwd
/root/kernel-mds2450-3.0.22/arch/arm/boot
root@ubuntu-vm ~/kernel-mds2450-3.0.22/arch/arm/boot
# ls
Image  Makefile  bootp  compressed  install.sh  zImage
root@ubuntu-vm ~/kernel-mds2450-3.0.22/arch/arm/boot
# 
```
10. tftp 폴더에 zImage복사 후 

- Makefile 자동화
```s
cp timerTest_mod.ko /nfs/rootfs/workSpace
cp timerTest_mod.c /root/kernel-mds2450-3.0.22/drivers/char
cp APP_DHKim /nfs/rootfs/workSpace/
```

- 타스크렛과 어포 큐 
```cpp
#include <linux/errno.h>
#include <linux/kernel.h>
#include <linux/module.h>
#include <linux/slab.h>
#include <linux/init.h>
#include <linux/delay.h>
#include <linux/interrupt.h>
#include <linux/device.h>
#include <asm/io.h>
#include <asm/irq.h>

#include <mach/regs-gpio.h>
#include <plat/gpio-cfg.h>
#include <linux/gpio.h>
//#include <linux/workqueue.h>

#define DRV_NAME		"keyint"
static int p;
static int i=0;
//static void  mytasklet_func(unsigned long data);   
//DECLARE_TASKLET( mytasklet, mytasklet_func, (unsigned long)NULL); 
static work_func_t mywork_queue_func(void *data);
DECLARE_DELAYED_WORK(mywork,(work_func_t)mywork_queue_func);//★
//★★★★
/*static void mytasklet_func(unsigned long data)
{
	//printk("[Bottom] mytasklet_func \n");
       printk(" -[Bottom] mytasklet_func : 0x%02x\n",(int)data);
	
    for(p=0;p<0x1000;p++)
      printk("\r test = %d", p); 
}*/
static work_func_t mywork_queue_func (void *data)
{
	for(p=0;p<0x1000;p++)
	printk("\r test = %d",p);
	printk(" -[Bottom] mywork_queue : 0x%02x\n",(int)i);

	return 0;
}

struct rebis_key_detection
{
    int             irq;
    int             pin;
    int             pin_setting;
    char            *name;
    int             last_state;
};

static struct rebis_key_detection rebis_gd = {
   IRQ_EINT7, S3C2410_GPF(7), S3C2410_GPF7_EINT7, "key-detect", 0
};


static irqreturn_t
rebis_keyevent(int irq, void *dev_id, struct pt_regs *regs)
{
    //struct rebis_key_detection *gd = (struct rebis_key_detection *) dev_id;
    //int             state;
	printk("\nkeypad was pressed \n");
	schedule_delayed_work( &mywork,0);
       /*ytasklet.data = i++;
        tasklet_schedule( &mytasklet );*/

//    for(p=0;p<0x1000;p++)
//       printk("\r test = %d", p); 

    return IRQ_HANDLED;

}

static int __init rebis_keyint_init(void)
{
	int ret;

    gpio_request(S3C2410_GPF(2), "led 1");
    gpio_request(S3C2410_GPF(3), "led 2");
    gpio_request(S3C2410_GPF(4), "led 3");
    gpio_request(S3C2410_GPF(5), "led 4");
    gpio_request(S3C2410_GPF(6), "led 5");

    // set output mode
    s3c_gpio_cfgpin(S3C2410_GPF(2), S3C_GPIO_SFN(1));
    s3c_gpio_cfgpin(S3C2410_GPF(3), S3C_GPIO_SFN(1));
    s3c_gpio_cfgpin(S3C2410_GPF(4), S3C_GPIO_SFN(1));
    s3c_gpio_cfgpin(S3C2410_GPF(5), S3C_GPIO_SFN(1));
    s3c_gpio_cfgpin(S3C2410_GPF(6), S3C_GPIO_SFN(1));
    // set data
    gpio_direction_output(S3C2410_GPF(2), 1);
    gpio_direction_output(S3C2410_GPF(3), 1);
    gpio_direction_output(S3C2410_GPF(4), 1);
    gpio_direction_output(S3C2410_GPF(5), 1);
    gpio_direction_output(S3C2410_GPF(6), 1); 

    s3c_gpio_cfgpin(S3C2410_GPF(7), S3C_GPIO_SFN(2));



#if 0
	writel(readl(S3C2410_EXTINT0) & (~(0xf << 12)), S3C2410_EXTINT0);	
	writel(readl(S3C2410_EXTINT0) | (0x2 << 12), S3C2410_EXTINT0); // Falling Edge interrupt
	
#endif 


    if( request_irq(IRQ_EINT7, (void *)rebis_keyevent, IRQF_DISABLED | IRQF_TRIGGER_RISING, DRV_NAME, &rebis_gd) )     
    {   
                printk("failed to request external interrupt.\n");
                ret = -ENOENT;
                return ret;
    }  
	printk(KERN_INFO "%s successfully loaded\n", DRV_NAME);

    return 0;
    
}

static void __exit rebis_keyint_exit(void)
{
   gpio_free(S3C2410_GPF(2));
   gpio_free(S3C2410_GPF(3));
   gpio_free(S3C2410_GPF(4));
   gpio_free(S3C2410_GPF(5));
   gpio_free(S3C2410_GPF(6));

    free_irq(rebis_gd.irq, &rebis_gd);

    printk(KERN_INFO "%s successfully removed\n", DRV_NAME);
}


module_init(rebis_keyint_init);
module_exit(rebis_keyint_exit);

MODULE_LICENSE("GPL");


```
- 핵심이 되는 코드는 아래와 같다
```cpp
DECLARE_DELAYED_WORK(mywork,(work_func_t)mywork_queue_func);//★
```
```
static work_func_t mywork_queue_func (void *data)
{
	for(p=0;p<0x1000;p++)
	printk("\r test = %d",p);
	printk(" -[Bottom] mywork_queue : 0x%02x\n",(int)i);

	return 0;
}
```
- 책 140p 참조



- 강사님의 day4 과제
- APP
```cpp
/* test_mydrv.c */

#include <stdio.h>
#include <stdlib.h>
#include <sys/types.h>
#include <fcntl.h>
#include <signal.h>
//#include <string.h>
#define MAX_BUFFER 26
char buf_in[MAX_BUFFER], buf_out[MAX_BUFFER];
void mySigHdlr(int signo);
char cpid;
int ret=0;
int main()
{
  int fd,i,mode,pid;
  pid = getpid();
  printf("pid is : %d\n",pid );
  fd = open("/dev/mydrv",O_RDWR);
  printf("fd = %d\r\n\n",fd);

  if(fd == -1) {
  	printf("you are fail to open div files, if you success ...\n");
  	printf("mknod /dev/mydrv c 240 0\n");
  	exit(1);
  }

  //sprintf(buf_out, "%d", pid);
  //cpid = itoa(pid);
  //strcpy(buf_out,"ohmygodd!");
  ret = write(fd,&pid,4);
  printf("return of write() : %d",ret);
  
  ret = read(fd,buf_in,20);
  printf("%s\n",buf_in);


  printf("%s\n","APP : I m going to sleep");
  if (signal(SIGINT, mySigHdlr) == SIG_ERR) {
    printf("\ncan't catch SIGINT\n");
  }
  while(1) {
  	sleep(200);
  }
  pause();

  close(fd);
  printf("%s\n","APP : wake up and the end");
  return (0);
}
void mySigHdlr(int signo)
{
  if (signo == SIGINT) {
    printf("received SIGINT\n");
  }
}
```
- KDD
```cpp
/*
  mydrv.c - kernel 3.0 skeleton device driver
               copy_to_user()
 */

#include <linux/module.h>
#include <linux/moduleparam.h>
#include <linux/init.h>

#include <linux/kernel.h>   /* printk() */
#include <linux/slab.h>   /* kmalloc() */
#include <linux/fs.h>       /* everything... */
#include <linux/errno.h>    /* error codes */
#include <linux/types.h>    /* size_t */
#include <asm/uaccess.h>
#include <linux/kdev_t.h>
#include <linux/cdev.h>
#include <linux/device.h>
#include <linux/errno.h>
#include <linux/kernel.h>
#include <linux/module.h>
#include <linux/slab.h>
#include <linux/init.h>
#include <linux/delay.h>
#include <linux/interrupt.h>
#include <linux/device.h>
#include <asm/io.h>
#include <asm/irq.h>

#include <mach/regs-gpio.h>
#include <plat/gpio-cfg.h>
#include <linux/gpio.h>
#include <linux/sched.h>

#define DEVICE_NAME "mydrv"
static int mydrv_major = 240;
module_param(mydrv_major, int, 0);
static int my_kill_proc(pid_t pid, int sig);

static int mydrv_open(struct inode *inode, struct file *file)
{
  printk("mydrv opened !!\n");
  return 0;
}

static int mydrv_release(struct inode *inode, struct file *file)
{
  printk("mydrv released !!\n");
  return 0;
}

static ssize_t mydrv_read(struct file *filp, char __user *buf, size_t count,
                loff_t *f_pos)
{
  char *k_buf;
  int i;
  
  k_buf = kmalloc(count,GFP_KERNEL);
  for(i = 0 ;i < count;i++)
      k_buf[i] = 'A' + i;
  
  if(copy_to_user(buf,k_buf,count))
  	return -EFAULT;
  printk("mydrv_read is invoked\n");
  kfree(k_buf);

  return 0;

}

static ssize_t mydrv_write(struct file *filp,const char __user *buf, size_t count,
                            loff_t *f_pos)
{	int id;
	char data[11];
	get_user(id,(int *)buf);
	printk("\n [ KDD ] id = %d",id);\
	mdelay(1000);
	mdelay(1000);
	my_kill_proc(id,SIGUSR1);
	return count;
/*
  char *k_buf;
    
  k_buf = kmalloc(count,GFP_KERNEL);
  if(copy_from_user(k_buf,buf,count))
  	return -EFAULT;
  printk("k_buf = %s\n",k_buf);
  printk("mydrv_write is invoked\n");
  kfree(k_buf);
  return 0;*/
}


/* Set up the cdev structure for a device. */
static void mydrv_setup_cdev(struct cdev *dev, int minor,
		struct file_operations *fops)
{
	int err, devno = MKDEV(mydrv_major, minor);
    
	cdev_init(dev, fops);
	dev->owner = THIS_MODULE;
	dev->ops = fops;
	err = cdev_add (dev, devno, 1);
	
	if (err)
		printk (KERN_NOTICE "Error %d adding mydrv%d", err, minor);
}


static struct file_operations mydrv_fops = {
	.owner   = THIS_MODULE,
   	.read	   = mydrv_read,
    	.write   = mydrv_write,
	.open    = mydrv_open,
	.release = mydrv_release,
};

#define MAX_MYDRV_DEV 1

static struct cdev MydrvDevs[MAX_MYDRV_DEV];

static int mydrv_init(void)
{
	int result;
	dev_t dev = MKDEV(mydrv_major, 0);

	/* Figure out our device number. */
	if (mydrv_major)
		result = register_chrdev_region(dev, 1, DEVICE_NAME);
	else {
		result = alloc_chrdev_region(&dev,0, 1, DEVICE_NAME);
		mydrv_major = MAJOR(dev);
	}
	if (result < 0) {
		printk(KERN_WARNING "mydrv: unable to get major %d\n", mydrv_major);
		return result;
	}
	if (mydrv_major == 0)
		mydrv_major = result;

	mydrv_setup_cdev(MydrvDevs,0, &mydrv_fops);
	printk("mydrv_init done\n");	
	return 0;
}

static void mydrv_exit(void)
{
	cdev_del(MydrvDevs);
	unregister_chrdev_region(MKDEV(mydrv_major, 0), 1);
	printk("mydrv_exit done\n");
}



static int my_kill_proc(pid_t pid, int sig) {
    int error = -ESRCH;           /* default return value */
    struct task_struct* p;
    struct task_struct* t = NULL; 
    struct pid* pspid;
    rcu_read_lock();
    p = &init_task;               /* start at init */
    do {
        if (p->pid == pid) {      /* does the pid (not tgid) match? */
            t = p;    
            break;
        }
        p = next_task(p);         /* "this isn't the task you're looking for" */
    } while (p != &init_task);    /* stop when we get back to init */
    if (t != NULL) {
        pspid = t->pids[PIDTYPE_PID].pid;
        if (pspid != NULL) error = kill_pid(pspid,sig,1);
    }
    rcu_read_unlock();
    return error;
}
module_init(mydrv_init);
module_exit(mydrv_exit);

MODULE_LICENSE("Dual BSD/GPL");
```
> 4일차 과제 강사님의 코드리뷰

- APP 소스
```cpp
```
- KDD 소스
```cpp
```
#
## Day5
  - 공부는 
  - EXEC의 아예 다른 
  - 고유메모리 킬, 다른프로세스 나갈수 있고
  - 바램이 있다면, 리눅스가 만만하고 할만하구나
      ```
      [5일차 프로젝트 - 요구조건]
      프로젝트
      필수사항
      1) 디바이스드라이버 작성
      2) IOCTRL (IOCTL) 사용
      3) 커널타이머 사용
      4) 인터럽트 사용
      5) 응용계층과 커널계층 데이터 전송 및 복사
      6) 테스트완료된 디바이스드라이버를 커널에 포함해서 부팅시 확인
      7) 어플리케이션과 연동 필수!!!(시그널, fork, 쓰레드, IPC, 공유메모리.. 다양하게 사용할것)
      8) 타스크렛 이나 워크큐 사용할것
      9) 주제는 자유
      4:30까지 서버에 제출..
      ```
  - 나의 구현목표
      ```
      [5일차 프로젝트 - 나의 구현목표]
      프로젝트
      필수사항
      1) 디바이스드라이버 작성 -> ok
      2) IOCTRL (IOCTL) 사용 -> ok
      3) 커널타이머 사용 -> LED 1초마다 점멸하는 인 커널 모듈 
      4) 인터럽트 사용 -> 버튼을 누르면 LED가 바뀜  버튼에 따라 다르게
      5) 응용계층과 커널계층 데이터 전송 및 복사 -> 시그널 발송해서 APP에서 시그널핸들러가 출력
      6) 테스트완료된 디바이스드라이버를 커널에 포함해서 부팅시 확인 -> LED 1초마다 점멸하는 인 커널 모듈 
      7) 어플리케이션과 연동 필수!!!(시그널, fork, 쓰레드, IPC, 공유메모리.. 다양하게 사용할것)
      8) 타스크렛 이나 워크큐 사용할것
      9) 주제는 자유
      4:30까지 서버에 제출..
      ```
  > 나의 코드
```cpp
#include <linux/module.h>
#include <linux/moduleparam.h>
#include <linux/init.h>
#include <linux/kernel.h>   /* printk() */
#include <linux/slab.h>   /* kmalloc() */
#include <linux/fs.h>       /* everything... */
#include <linux/errno.h>    /* error codes */
#include <linux/types.h>    /* size_t */
#include <asm/uaccess.h>
#include <linux/kdev_t.h>
#include <linux/cdev.h>
#include <linux/device.h>
#include <linux/errno.h>
#include <linux/kernel.h>
#include <linux/module.h>
#include <linux/slab.h>
#include <linux/init.h>
#include <linux/delay.h>
#include <linux/interrupt.h>
#include <linux/device.h>
#include <asm/io.h>
#include <asm/irq.h>
#include <mach/regs-gpio.h>
#include <plat/gpio-cfg.h>
#include <linux/gpio.h>
#include <linux/sched.h>
#include "ioctl_mydrv.h"

MODULE_LICENSE("Dual BSD/GPL");

#define GPGCON *(volatile unsigned long *)(kva)
#define GPGDAT *(volatile unsigned long *)(kva + 4)
#define DEVICE_NAME "mydrv"

static int mydrv_major = 241, mydrv_minor = 0;;
module_param(mydrv_major, int, 0);
//module_param(sk_major, int, 0);
static int my_kill_proc(pid_t pid, int sig);
static int result;
static void *kva;
static dev_t mydrv_dev;
static struct cdev mydrv_cdev;

//mydrv_setup_cdev
static void mydrv_setup_cdev(struct cdev *dev, int minor,
    struct file_operations *fops);
//
static int mydrv_register_cdev(void);
static int mydrv_open(struct inode *inode, struct file *filp);
static int mydrv_release(struct inode *inode, struct file *filp);
static int mydrv_write(struct file *filp, const char *buf, size_t count, loff_t *f_pos);
static int mydrv_read(struct file *filp, char *buf, size_t count, loff_t *f_pos);
static int mydrv_ioctl ( struct file *filp, unsigned int cmd, unsigned long arg);
//


static int mydrv_open(struct inode *inode, struct file *file)
{
  printk("mydrv opened !!\n");
  kva = ioremap(0x56000060, 16);
  printk("kva = 0x%x\n", (int)kva);
  GPGDAT |= 0xf << 4;
  GPGCON &= ~(0xff << 8);
  GPGCON |= 0x55 << 8;
  return 0;
}

static int mydrv_release(struct inode *inode, struct file *file)
{
  printk("mydrv released !!\n");
  return 0;
}

static ssize_t mydrv_read(struct file *filp, char __user *buf, size_t count,
                loff_t *f_pos)
{
  //volatile int i;
  
  printk("KDD : Hello i'm kernel mydrv_read()\n");
  
  //LED Toggle func in READ method
  GPGDAT ^= (0xf << 4);

  return 0;

}

static ssize_t mydrv_write(struct file *filp,const char __user *buf, size_t count,
                            loff_t *f_pos)
{	
  int id;
	//char data[11];
	get_user(id,(int *)buf);
	printk("\n [ KDD ] id = %d\n",id);\
	//mdelay(1000);
	//mdelay(1000);
	my_kill_proc(id,SIGUSR1);
	return count;
/*
  char *k_buf;
    
  k_buf = kmalloc(count,GFP_KERNEL);
  if(copy_from_user(k_buf,buf,count))
  	return -EFAULT;
  printk("k_buf = %s\n",k_buf);
  printk("mydrv_write is invoked\n");
  kfree(k_buf);
  return 0;*/
}


static int sk_ioctl (struct file *filp, unsigned int cmd, unsigned long arg)
{  
  
  ioctl_buf *k_buf;
  int   i,err, size;  
  int leddata=0;    


  switch( cmd ) {  
      
    case IOCTL_LED_1_ON :
      printk("LED_1_ON\n");
      leddata = 0x01;
      GPGDAT=GPGDAT & ~(leddata<<4);
      mdelay(10);
      break;

    case IOCTL_LED_2_ON :
      for(i=0; i<30; i++)
      {
      GPGDAT ^= (0xf << 4);
      mdelay(1000);
      }
      /*
      printk("LED_2_ON\n");
      leddata = 0x10;
      GPGDAT=GPGDAT & ~(leddata<<4);
      mdelay(10);
      */
      break;
    /*
    case IOCTL_LED_1_OFF :
      printk("LED_1_OFF\n");
      break;
               
    case IOCTL_LED_2_OFF :
      printk("LED_2_OFF\n");
      break;
    
    case test :
      printk("IOCTL_TESTED Processed!!\n");
      break;
       */
    default :
      printk("Invalid IOCTL  Processed!!\n");
      break; 
  }  
  
    return 0;  
}  



static struct file_operations mydrv_fops = {
	.owner           = THIS_MODULE,
  .read	           = mydrv_read,
  .write           = mydrv_write,
	.open            = mydrv_open,
  .unlocked_ioctl  = sk_ioctl,
	.release         = mydrv_release,
};

#define MAX_MYDRV_DEV 1

static struct cdev MydrvDevs[MAX_MYDRV_DEV];

static int mydrv_init(void)
{
	int result;
	dev_t dev = MKDEV(mydrv_major, 0);

	/* Figure out our device number. */
	if (mydrv_major)
		result = register_chrdev_region(dev, 1, DEVICE_NAME);
	else {
		result = alloc_chrdev_region(&dev,0, 1, DEVICE_NAME);
		mydrv_major = MAJOR(dev);
	}
	if (result < 0) {
		printk(KERN_WARNING "mydrv: unable to get major %d\n", mydrv_major);
		return result;
	}
	if (mydrv_major == 0)
		mydrv_major = result;

	mydrv_setup_cdev(MydrvDevs,0, &mydrv_fops);


  if((result = mydrv_register_cdev()) < 0)
  {
    return result;
  }

	printk("mydrv_init done\n");	
	return 0;
}

static void mydrv_exit(void)
{
	cdev_del(MydrvDevs);
	unregister_chrdev_region(MKDEV(mydrv_major, 0), 1);
	printk("mydrv_exit done\n");
}



static int my_kill_proc(pid_t pid, int sig) {
    int error = -ESRCH;           /* default return value */
    struct task_struct* p;
    struct task_struct* t = NULL; 
    struct pid* pspid;
    rcu_read_lock();
    p = &init_task;               /* start at init */
    do {
        if (p->pid == pid) {      /* does the pid (not tgid) match? */
            t = p;    
            break;
        }
        p = next_task(p);         /* "this isn't the task you're looking for" */
    } while (p != &init_task);    /* stop when we get back to init */
    if (t != NULL) {
        pspid = t->pids[PIDTYPE_PID].pid;
        if (pspid != NULL) error = kill_pid(pspid,sig,1);
    }
    rcu_read_unlock();
    return error;
}

/* Set up the cdev structure for a device. */
static void mydrv_setup_cdev(struct cdev *dev, int minor,
    struct file_operations *fops)
{
  int err, devno = MKDEV(mydrv_major, minor);
    
  cdev_init(dev, fops);
  dev->owner = THIS_MODULE;
  dev->ops = fops;
  err = cdev_add (dev, devno, 1);
  
  if (err)
    printk (KERN_NOTICE "Error %d adding mydrv%d", err, minor);
}

static int mydrv_register_cdev(void)
{
  int error;

  /* allocation device number */
  if(mydrv_major) {
    mydrv_dev = MKDEV(mydrv_major, mydrv_minor);
    printk(KERN_INFO "timerTest Module is Loaded!! ....\n");
    error = register_chrdev_region(mydrv_dev, 1, "mydrv");
  } else {
    error = alloc_chrdev_region(&mydrv_dev, mydrv_minor, 1, "mydrv");
    mydrv_major = MAJOR(mydrv_dev);
  }

  if(error < 0) {
    printk(KERN_WARNING "mydrv: can't get major %d\n", mydrv_major);
    return result;
  }
  printk("mydrv major number=%d\n", mydrv_major);

  /* register chrdev */
  cdev_init(&mydrv_cdev, &mydrv_fops);
  mydrv_cdev.owner = THIS_MODULE;
  mydrv_cdev.ops = &mydrv_fops;
  error = cdev_add(&mydrv_cdev, mydrv_dev, 1);

  if(error)
    printk(KERN_NOTICE "mydrv Register Error %d\n", error);

  return 0;
}

module_init(mydrv_init);
module_exit(mydrv_exit);
```
```cpp
#include <linux/module.h>
#include <linux/moduleparam.h>
#include <linux/init.h>
#include <linux/kernel.h>   /* printk() */
#include <linux/slab.h>   /* kmalloc() */
#include <linux/fs.h>       /* everything... */
#include <linux/errno.h>    /* error codes */
#include <linux/types.h>    /* size_t */
#include <asm/uaccess.h>
#include <linux/kdev_t.h>
#include <linux/cdev.h>
#include <linux/device.h>
#include <linux/errno.h>
#include <linux/kernel.h>
#include <linux/module.h>
#include <linux/slab.h>
#include <linux/init.h>
#include <linux/delay.h>
#include <linux/interrupt.h>
#include <linux/device.h>
#include <asm/io.h>
#include <asm/irq.h>
#include <mach/regs-gpio.h>
#include <plat/gpio-cfg.h>
#include <linux/gpio.h>
#include <linux/sched.h>
#include "ioctl_mydrv.h"

MODULE_LICENSE("Dual BSD/GPL");

#define GPGCON *(volatile unsigned long *)(kva)
#define GPGDAT *(volatile unsigned long *)(kva + 4)
#define DEVICE_NAME "mydrv"

static int mydrv_major = 241, mydrv_minor = 0;;
module_param(mydrv_major, int, 0);
//module_param(sk_major, int, 0);
static int my_kill_proc(pid_t pid, int sig);
static int result;
static void *kva;
static dev_t mydrv_dev;
static struct cdev mydrv_cdev;

//mydrv_setup_cdev
static void mydrv_setup_cdev(struct cdev *dev, int minor,
    struct file_operations *fops);
//
static int mydrv_register_cdev(void);
static int mydrv_open(struct inode *inode, struct file *filp);
static int mydrv_release(struct inode *inode, struct file *filp);
static int mydrv_write(struct file *filp, const char *buf, size_t count, loff_t *f_pos);
static int mydrv_read(struct file *filp, char *buf, size_t count, loff_t *f_pos);
static int mydrv_ioctl ( struct file *filp, unsigned int cmd, unsigned long arg);
//


static int mydrv_open(struct inode *inode, struct file *file)
{
  printk("mydrv opened !!\n");
  kva = ioremap(0x56000060, 16);
  printk("kva = 0x%x\n", (int)kva);
  GPGDAT |= 0xf << 4;
  GPGCON &= ~(0xff << 8);
  GPGCON |= 0x55 << 8;
  return 0;
}

static int mydrv_release(struct inode *inode, struct file *file)
{
  printk("mydrv released !!\n");
  return 0;
}

static ssize_t mydrv_read(struct file *filp, char __user *buf, size_t count,
                loff_t *f_pos)
{
  //volatile int i;
  
  printk("KDD : Hello i'm kernel mydrv_read()\n");
  
  //LED Toggle func in READ method
  GPGDAT ^= (0xf << 4);

  return 0;

}

static ssize_t mydrv_write(struct file *filp,const char __user *buf, size_t count,
                            loff_t *f_pos)
{	
  int id;
	//char data[11];
	get_user(id,(int *)buf);
	printk("\n [ KDD ] id = %d\n",id);\
	//mdelay(1000);
	//mdelay(1000);
	my_kill_proc(id,SIGUSR1);
	return count;
/*
  char *k_buf;
    
  k_buf = kmalloc(count,GFP_KERNEL);
  if(copy_from_user(k_buf,buf,count))
  	return -EFAULT;
  printk("k_buf = %s\n",k_buf);
  printk("mydrv_write is invoked\n");
  kfree(k_buf);
  return 0;*/
}


static int sk_ioctl (struct file *filp, unsigned int cmd, unsigned long arg)
{  
  
  ioctl_buf *k_buf;
  int   i,err, size;  
  int leddata=0;    


  switch( cmd ) {  
      
    case IOCTL_LED_1_ON :
      printk("LED_1_ON\n");
      leddata = 0x01;
      GPGDAT=GPGDAT & ~(leddata<<4);
      mdelay(10);
      break;

    case IOCTL_LED_2_ON :
      for(i=0; i<30; i++)
      {
      GPGDAT ^= (0xf << 4);
      mdelay(1000);
      }
      /*
      printk("LED_2_ON\n");
      leddata = 0x10;
      GPGDAT=GPGDAT & ~(leddata<<4);
      mdelay(10);
      */
      break;
    /*
    case IOCTL_LED_1_OFF :
      printk("LED_1_OFF\n");
      break;
               
    case IOCTL_LED_2_OFF :
      printk("LED_2_OFF\n");
      break;
    
    case test :
      printk("IOCTL_TESTED Processed!!\n");
      break;
       */
    default :
      printk("Invalid IOCTL  Processed!!\n");
      break; 
  }  
  
    return 0;  
}  



static struct file_operations mydrv_fops = {
	.owner           = THIS_MODULE,
  .read	           = mydrv_read,
  .write           = mydrv_write,
	.open            = mydrv_open,
  .unlocked_ioctl  = sk_ioctl,
	.release         = mydrv_release,
};

#define MAX_MYDRV_DEV 1

static struct cdev MydrvDevs[MAX_MYDRV_DEV];

static int mydrv_init(void)
{
	int result;
	dev_t dev = MKDEV(mydrv_major, 0);

	/* Figure out our device number. */
	if (mydrv_major)
		result = register_chrdev_region(dev, 1, DEVICE_NAME);
	else {
		result = alloc_chrdev_region(&dev,0, 1, DEVICE_NAME);
		mydrv_major = MAJOR(dev);
	}
	if (result < 0) {
		printk(KERN_WARNING "mydrv: unable to get major %d\n", mydrv_major);
		return result;
	}
	if (mydrv_major == 0)
		mydrv_major = result;

	mydrv_setup_cdev(MydrvDevs,0, &mydrv_fops);


  if((result = mydrv_register_cdev()) < 0)
  {
    return result;
  }

	printk("mydrv_init done\n");	
	return 0;
}

static void mydrv_exit(void)
{
	cdev_del(MydrvDevs);
	unregister_chrdev_region(MKDEV(mydrv_major, 0), 1);
	printk("mydrv_exit done\n");
}



static int my_kill_proc(pid_t pid, int sig) {
    int error = -ESRCH;           /* default return value */
    struct task_struct* p;
    struct task_struct* t = NULL; 
    struct pid* pspid;
    rcu_read_lock();
    p = &init_task;               /* start at init */
    do {
        if (p->pid == pid) {      /* does the pid (not tgid) match? */
            t = p;    
            break;
        }
        p = next_task(p);         /* "this isn't the task you're looking for" */
    } while (p != &init_task);    /* stop when we get back to init */
    if (t != NULL) {
        pspid = t->pids[PIDTYPE_PID].pid;
        if (pspid != NULL) error = kill_pid(pspid,sig,1);
    }
    rcu_read_unlock();
    return error;
}

/* Set up the cdev structure for a device. */
static void mydrv_setup_cdev(struct cdev *dev, int minor,
    struct file_operations *fops)
{
  int err, devno = MKDEV(mydrv_major, minor);
    
  cdev_init(dev, fops);
  dev->owner = THIS_MODULE;
  dev->ops = fops;
  err = cdev_add (dev, devno, 1);
  
  if (err)
    printk (KERN_NOTICE "Error %d adding mydrv%d", err, minor);
}

static int mydrv_register_cdev(void)
{
  int error;

  /* allocation device number */
  if(mydrv_major) {
    mydrv_dev = MKDEV(mydrv_major, mydrv_minor);
    printk(KERN_INFO "timerTest Module is Loaded!! ....\n");
    error = register_chrdev_region(mydrv_dev, 1, "mydrv");
  } else {
    error = alloc_chrdev_region(&mydrv_dev, mydrv_minor, 1, "mydrv");
    mydrv_major = MAJOR(mydrv_dev);
  }

  if(error < 0) {
    printk(KERN_WARNING "mydrv: can't get major %d\n", mydrv_major);
    return result;
  }
  printk("mydrv major number=%d\n", mydrv_major);

  /* register chrdev */
  cdev_init(&mydrv_cdev, &mydrv_fops);
  mydrv_cdev.owner = THIS_MODULE;
  mydrv_cdev.ops = &mydrv_fops;
  error = cdev_add(&mydrv_cdev, mydrv_dev, 1);

  if(error)
    printk(KERN_NOTICE "mydrv Register Error %d\n", error);

  return 0;
}

module_init(mydrv_init);
module_exit(mydrv_exit);
```



> 강사님 샘플 코드
```cpp
#include <linux/module.h>
#include <linux/init.h>
#include <linux/major.h>
#include <linux/fs.h>
#include <linux/cdev.h>
#include <linux/kernel.h>
#include <asm/io.h>
#include <asm/uaccess.h>
#include <linux/delay.h>
#include <linux/timer.h>
#include <linux/kdev_t.h>
#include <linux/device.h>
#include <linux/slab.h>   /* kmalloc() */
#include <linux/errno.h>    /* error codes */
#include <linux/types.h>    /* size_t */
#include <asm/irq.h>
#include <linux/sched.h>
#include <linux/interrupt.h>

#include <mach/gpio.h>
#include <mach/regs-gpio.h>
#include <plat/gpio-cfg.h>
#include <linux/gpio.h>
#include "project_ioctl.h"

#define DEVICE_NAME "project"
#define GPGCON *(volatile unsigned long *)(Gkva)
#define GPGDAT *(volatile unsigned long *)(Gkva + 4)
#define GPFCON *(volatile unsigned long *)(Fkva)
#define GPFDAT *(volatile unsigned long *)(Fkva + 4)

struct timer_list timer;
static int project_major = 241, project_minor = 0;
static int result;
static dev_t project_dev;
static void *Gkva;
static void *Fkva;
static pid_t userpid;
static int status;
int led = 0x1;

static struct cdev project_cdev;
static int project_register_cdev(void);

static work_func_t mywork_queue_func(void *data);
DECLARE_DELAYED_WORK(mywork,(work_func_t)mywork_queue_func);

/* TODO: Define Prototype of functions */
static int project_open(struct inode *inode, struct file *filp);
static int project_release(struct inode *inode, struct file *filp);
static int project_write(struct file *filp, const char *buf, size_t count, loff_t *f_pos);
static int project_read(struct file *filp, char *buf, size_t count, loff_t *f_pos);

int my_kill_proc(pid_t pid, int sig) {
    int error = -ESRCH;           /* default return value */
    struct task_struct* p;
    struct task_struct* t = NULL; 
    struct pid* pspid;
    rcu_read_lock();
    p = &init_task;               /* start at init */
    do {
        if (p->pid == pid) {      /* does the pid (not tgid) match? */
            t = p;    
            break;
        }
        p = next_task(p);         /* "this isn't the task you're looking for" */
    } while (p != &init_task);    /* stop when we get back to init */
    if (t != NULL) {
        pspid = t->pids[PIDTYPE_PID].pid;
        if (pspid != NULL) error = kill_pid(pspid,sig,1);
    }
    rcu_read_unlock();
    return error;
}

static work_func_t mywork_queue_func (void *data)
{
	unsigned int key;
	int i;
    printk("Work queue func's got started!\n");

    while(1){
		//status = 0;

        GPFDAT &= ~(0x80); //GPF7 Low
        GPGDAT |= (0x01); //GPG0 High

        for(i = 0; i < 5; i++){
            key = GPFDAT;
            key &= (0x01 << (i + 2));
            key = (key >> (i + 2));
            if(key == 0){
                status = i + 1;
            } 	
        }

        GPFDAT |= 0x80; //GPF7 High
        GPGDAT &= ~(0x01); //GPG0 Low

        for(i = 0; i < 5; i++){
            key = GPFDAT;
            key &= (0x01 << (i + 2));
            key = (key >> (i + 2));
            if(key == 0) {
                status = i + 6;	
            }
        }

		switch(status){
			case 1: 
				del_timer(&timer);
				printk("\nkeypad 2 was pressed \n");
				GPGDAT |= (0x0f << 4);
				GPGDAT &= ~(0x01 << 4);
				status = 0; break;

			case 2: 
				del_timer(&timer);
				printk("\nkeypad 3 was pressed \n");
				GPGDAT |= (0x0f << 4);
				GPGDAT &= ~(0x02 << 4);
				status = 0; break;

			case 3: 
				del_timer(&timer);
				printk("\nkeypad 4 was pressed \n");
				GPGDAT |= (0x0f << 4);
				GPGDAT &= ~(0x04 << 4);
				status = 0; break;

			case 4: 
				del_timer(&timer);
				printk("\nkeypad 5 was pressed \n");
				GPGDAT |= (0x0f << 4);
				GPGDAT &= ~(0x08 << 4);
				status = 0; break;

			case 5: 
				del_timer(&timer);
				printk("\nkeypad 6 was pressed \n");
				GPGDAT |= (0x0f << 4);
				GPGDAT &= ~(0x0f << 4);
				status = 0; break;

			case 6: 
				del_timer(&timer);
				printk("\nkeypad 7 was pressed \n");
				GPGDAT |= (0x0f << 4);
				GPGDAT &= ~(0x01 << 4);
				status = 0; break;

			case 7: 
				del_timer(&timer);
				printk("\nkeypad 8 was pressed \n");
				GPGDAT |= (0x0f << 4);
				GPGDAT &= ~(0x02 << 4);
				status = 0; break;

			case 8: 
				del_timer(&timer);
				printk("\nkeypad 9 was pressed \n");
				GPGDAT |= (0x0f << 4);
				GPGDAT &= ~(0x04 << 4);
				status = 0; break;

			case 9: 
				del_timer(&timer);
				printk("\nkeypad 10 was pressed \n");
				GPGDAT |= (0x0f << 4);
				GPGDAT &= ~(0x08 << 4);
				status = 0; break;

			case 10: 
				del_timer(&timer);
				printk("\nkeypad 11 was pressed \n");
				GPGDAT |= (0x0f << 4);
				GPGDAT &= ~(0x0f << 4);
				status = 0; break;

			default : break;
		}
	}

	return 0;
}

void my_timer(unsigned long data)
{
	led = led << 1;
	if(led > 0x8) led = 0x1;

	GPGDAT |= (0xf << 4);
	GPGDAT &= ~(led << 4);

	timer.expires = jiffies + 1*HZ;
	add_timer(&timer);

	//printk("Kernel Timer Time-Out Function Done!!!\n");
}

static irqreturn_t key14int_func(int irq, void *dev_id, struct pt_regs *resgs)
{
    printk("SIGUSR1 sent....\n");
	my_kill_proc(userpid, SIGUSR1);
	schedule_delayed_work( &mywork,0);
    return IRQ_HANDLED;
}

static irqreturn_t key15int_func(int irq, void *dev_id, struct pt_regs *resgs)
{
    printk("SIGUSR2 sent....\n");
	my_kill_proc(userpid, SIGUSR2);

	// set timer
    init_timer(&timer);
	//timer.expires = get_jiffies_64() + 3*HZ;
	timer.expires = jiffies + 3*HZ;
	timer.function = my_timer;
	timer.data = 5;
	add_timer(&timer);

    return IRQ_HANDLED;
}

int project_init(void)
{
	int ret;
	printk(KERN_INFO "project Module is Loaded!! ....\n");
	// register module
	if((result = project_register_cdev()) < 0)
	{
		return result;
	}

	// set Interrupt mode
	s3c_gpio_cfgpin(S3C2410_GPF(0), S3C_GPIO_SFN(2));
	s3c_gpio_cfgpin(S3C2410_GPF(1), S3C_GPIO_SFN(2));

	if( request_irq(IRQ_EINT0, (void *)key14int_func,
			IRQF_DISABLED|IRQF_TRIGGER_FALLING, DEVICE_NAME, NULL) )
	{
		printk("failed to request external interrupt.\n");
		ret = -ENOENT;
		return ret;
	}
	if(request_irq(IRQ_EINT1, (void *)key15int_func, 
			IRQF_DISABLED|IRQF_TRIGGER_FALLING, DEVICE_NAME, NULL))
	{
		printk("failed to request external interrupt.\n");
		ret = -ENOENT;
		return ret;
	}

    /* H/W Initalization */
    Gkva = ioremap(0x56000060, 16);
    printk("Gkva = 0x%x\n", (int)Gkva);
	Fkva = ioremap(0x56000050, 16);
    printk("Gkva = 0x%x\n", (int)Fkva);
    
    GPGDAT |= 0xf << 4;

    GPGCON &= ~(0xff << 8);
    GPGCON |= 0x55 << 8;	

	// key input mode
	GPFCON &= ~(0x3 << 14);
	GPFCON |= (0x1 << 14);	// GPF7 output
	GPGCON &= ~(0x3); //GPG0 output
	GPFCON &= ~(0x3ff << 4); // GPF2, 3, 4, 5, 6 input

	printk(KERN_INFO "%s successfully loaded\n", DEVICE_NAME);

	return 0;
}

static int project_open(struct inode *inode, struct file *filp)
{
    printk("Device has been opened...\n");
    

    return 0;
}

static int project_release(struct inode *inode, struct file *filp)
{
    printk("Device has been closed...\n");
    
    return 0;
}

static int project_write(struct file *filp, const char *buf, size_t count, loff_t *f_pos)
{
	printk("project_write is invoked\n");
	
	get_user(userpid, (int *)buf);
	printk("userpid : %d\n", userpid);
	//my_kill_proc(userpid, SIGUSR1);
    
	return count;
}

static int project_read(struct file *filp, char *buf, size_t count, loff_t *f_pos)
{
	  printk("project_read is invoked\n");
	  return 0;
}

static int project_ioctl ( struct file *filp, unsigned int cmd, unsigned long arg)  
{  
  
   ioctl_buf *k_buf;
   int   i,err, size;  
      
   if( _IOC_TYPE( cmd ) != IOCTL_MAGIC ) return -EINVAL;  
   if( _IOC_NR( cmd )   >= IOCTL_MAXNR ) return -EINVAL;  

   size = _IOC_SIZE( cmd );   
 
   if( size )  
   {  
       err = -EFAULT;  
       if( _IOC_DIR( cmd ) & _IOC_READ  ) 
		err = !access_ok( VERIFY_WRITE, (void __user *) arg, size );  
       else if( _IOC_DIR( cmd ) & _IOC_WRITE ) 
		err = !access_ok( VERIFY_READ , (void __user *) arg, size );  
       if( err ) return err;          
   }  
            
   switch( cmd )  
   {  
      
    //    case IOCTL_PROJECT_TEST :
    //         printk("IOCTL_PROJECT_TEST Processed!!\n");
	//     break;

       case IOCTL_PROJECT_READ :
            k_buf = kmalloc(size,GFP_KERNEL);
            
            //sprintf(k_buf->data, %s, "Hi, this was sent from Kernel to App");
			strcpy(k_buf->data, "Hi, this was sent from kernel to App");
			//printk("k_buf->data : %s\n", k_buf->data);
            if(copy_to_user ( (void __user *) arg, k_buf, (unsigned long ) size ))
	          return -EFAULT;   
            kfree(k_buf);
            printk("IOCTL_PROJECT_READ Processed!!\n");
	    break;
	                              
		default :
				printk("Invalid IOCTL  Processed!!\n");
				break; 
    }  
  
    return 0;  
}  
 
struct file_operations project_fops = { 
    .open       = project_open,
    .release    = project_release,
	.write		= project_write,
	.read		= project_read,
	.unlocked_ioctl   = project_ioctl,
};

void project_exit(void)
{
	del_timer(&timer);

	// gpio 해제
	gpio_free(S3C2410_GPF(0));
	gpio_free(S3C2410_GPF(1));

	// 인터럽트 해제
	free_irq(IRQ_EINT0, NULL);
	free_irq(IRQ_EINT1, NULL);

	// device 등록 해제
    cdev_del(&project_cdev);
	unregister_chrdev_region(MKDEV(project_major, 0), 1);

	printk("project Module is Unloaded ....\n");
}

static int project_register_cdev(void)
{
	int error;

	/* allocation device number */
	if(project_major) {
		project_dev = MKDEV(project_major, project_minor);
		error = register_chrdev_region(project_dev, 1, "project");
	} else {
		error = alloc_chrdev_region(&project_dev, project_minor, 1, "project");
		project_major = MAJOR(project_dev);
	}

	if(error < 0) {
		printk(KERN_WARNING "project: can't get major %d\n", project_major);
		return result;
	}
	printk("major number=%d\n", project_major);

	/* register chrdev */
	cdev_init(&project_cdev, &project_fops);
	project_cdev.owner = THIS_MODULE;
	project_cdev.ops = &project_fops;
	error = cdev_add(&project_cdev, project_dev, 1);

	if(error)
		printk(KERN_NOTICE "project Register Error %d\n", error);

	return 0;
}

module_init(project_init);
module_exit(project_exit);

MODULE_LICENSE("Dual BSD/GPL");


/*
 #tail -f /var/log/messages
*/

```
> sk_app.c
```cpp
/***************************************
 * Filename: sk_app.c
 * Title: Skeleton Device Application
 * Desc: Implementation of system call
 ***************************************/
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>
#include <time.h>
#include <mqueue.h>
#include <sys/types.h>
#include <sys/ioctl.h>
#include <fcntl.h>
#include <signal.h>
#include <unistd.h>
#include <sys/ioctl.h>

#include "project_ioctl.h"

#define MAX_BUFFER 10

int fd,size;

char buf_out[MAX_BUFFER];
ioctl_buf *buf_in;

pid_t pid;

pthread_t threads[2];
pthread_attr_t      attr0, attr1;

struct mq_attr attr;
mqd_t mq;

void recv_thread( void ) {
	char msg[20];
	int len;
	unsigned int prio=0;

    len = mq_receive(mq, msg, sizeof(msg), &prio);
    sleep(1);

    if ( len > 0 ) {
        msg[len] = '\0';
        printf( "\t\tI got [%s] message\n", msg);
    }
}

void send_thread( void ) {
	char msg[20] = "Program start!";
	unsigned int prio = 0;

   // printf( "S(%c, prio = %d)\n", c, prio );
/* TODO: send c via message-q */ 
    mq_send(mq, msg, sizeof(msg), prio);

    sleep(1);
}

void mySigHdlr1(int signo)
{
  if (signo == SIGUSR1) {
    printf("received SIGUSR1\n");
    pthread_create(&threads[1], &attr1, (void*)send_thread, NULL);
    pthread_detach(threads[1]);
  }
}

void mySigHdlr2(int signo)
{
    size = sizeof(ioctl_buf);
    buf_in = (ioctl_buf *)malloc(size);
    if (signo == SIGUSR2) {
        printf("received SIGUSR2\n");
        
        ioctl(fd, IOCTL_PROJECT_READ, buf_in);
        printf("mySigHdlr2 : %s\n",buf_in->data);
  }
  free(buf_in);
}

int main(void)
{

    // 메시지 큐 생성
	attr.mq_maxmsg  =  3;
	attr.mq_msgsize = 20;
	attr.mq_flags   =  0;

	mq = mq_open("/mqueue", O_RDWR|O_CREAT,0,&attr);
	if(mq == -1)
	{
		perror("mq_open error");
		exit(0);
	}	

   // 시그널 생성
    if (signal(SIGUSR1, mySigHdlr1) == SIG_ERR) {
        printf("\ncan't catch SIGUSR1\n");
    }
    if (signal(SIGUSR2, mySigHdlr2) == SIG_ERR) {
        printf("\ncan't catch SIGUSR2\n");
    }

	//system("insmod project_mod.ko");
	//system("mknod /dev/project c 241 0");

    // device file open
    fd = open("/dev/project",O_RDWR);
    
    // pid 전달
    pid = getpid();
    printf("app process id : %d\n", pid);
    write(fd, &pid, 4);

    pthread_attr_init(&attr0); pthread_attr_init(&attr1);
	//pthread_create(&threads[0], &attr0, (void*)recv_thread, NULL);

    // start message가 큐에 들어가면 프로그램 계속 실행
    printf("waiting for start message... press SW14 to start!\n");
    recv_thread();

	while(1){
        printf("put 'close' :" );
        gets(buf_out);
        if(strcmp(buf_out, "close") == 0){
            //system("rmmod project_mod.ko");
            //system("rm /dev/project");
            break;
        }
    }

	close(fd);
	return (0);
}
```
 # # 내용정리
  - 리눅스 커널 컴파일 개발환경 구축 
    - 하드웨어
    - 컴파일러
    - 커널 소스코드
    - 루트파일시스템
    - 통신장비 : 디버거/UART/LAN...
    - 각종 유틸리티 도구들
  - mknod의 사용 : 리눅스에서는 File is everything
  - VFS : 를 통해 모든것을 F.S로 관리 커널강좌, 협회 커널모듈 적재
  - udev : 커널라인 3.x부터 등장 핫플러그 기능 추가를 위함
  - Open/Release 호출의 기본 처리 : 초기화 동작
  - 임베디드 리눅스 보드상의 인터럽트 처리 : 처리를 두 단계로 분리
    1. 탑 하프
    2. 바텀 하프
  - 페리의 종류나 혹은 두 가지 사이의 구분이 전혀 없음, 소프트웨어 구성자의 마음임 따라서 두가지의 구분은 오로지 경험에 의함
  - 인터럽트는 do_IRQ() 함수의 호출에 의함
  - 인터럽트 서비스 처리 루틴
  - 인터럽트 핸들러 : 리눅스의 3대 핸들러
    1. 시그널 핸들러
    2. 인터럽트 핸들러
    3. 타이머 핸들러
  - 워크 큐 함수포인터로 등록
  - 커널에서의 타이머 서비스 등록
    1. 하드웨어 타이머가 타이밍 관련 테스크를 처리하기 위해서 커널이 타이머 관련 기능을 제공 함
    2. 주요 개념용어들
       1. HZ : 1초당 발생되는 타이머 인터럽트 횟수
       2. USER_HZ
       3. jiffies: 초당 HZ값 만큼 증가하는 전역변수
  - 마지막 최고 참고자료 : [리눅스 커널의 이해 요약](../리눅스커널의이해요약.md)
> copy_to_user(), copy_from_user() 함수
- 헤더파일 
```
#include <asm/uaccess.h>
```
> copy_to_user() 함수
 - 기능 : 커널 메모리 -> 사용자 메모리   
 - 형태 : int copy_to_user(void* send_buf, const void* recv_buf, unsigned long len)  
 - 설명 : 커널에서 -> 사용자에게 메모리 주소를 통해 데이터를 전달한다
 - 매개변수
   1. send_buf: 사용자 메모리 블록 선두 주소
   2. recv_buf: 커널 메모리 블록 선두 주소
   3. len: 써넣을 바이트 단위의 크기
 - 반환값  복사되지 않은 바이트수를 리턴. 정상적으로 수행이 되었다면 0. 
> copy_from_user() 함수
 - 기능 : 사용자 메모리 -> 커널 메모리 
 - 형태 : int copy_from_user(void* recv_buf, const void * send_buf, unsigned long len)
 - 설명 : 사용자에서 -> 커널에게 메모리 주소를 통해 데이터를 전달한다.
 - 매개변수
   1. recv_buf: 사용자 메모리 블록 선두 주소
   2. send_buf: 커널 메모리 블록 선두 주소
   3. len: 써넣을 바이트 단위의 크기
 - 반환값  복사되지 않은 바이트수를 리턴. 정상적으로 수행이 되었다면 0. 
### 파라미터의 recv_buf와 send_buf사이가 바뀌는것에 주의하자!!! 



















