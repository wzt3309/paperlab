# 问题背景
## part.1

云计算发展的关键是重新定义了资源使用的方式，可以通过按需付费来购买不同等级的服务，而实现这一突破的核心就是虚拟化技术。

虚拟机与容器技术简介，虚拟化技术的含义在于：运行环境隔离和资源控制，虚拟机和容器有各自不同实现方式：
1. 虚拟机是通过在硬件和操作系统之间引入虚拟化层（Hypervisor）来实现同一台物理服务运行多个操作系统实例
    - 每个虚拟机得到独立的模拟出的硬件设备
    - 每个虚拟机必须按照自己的操作系统（Guest OS）
2. 容器并不能与传统"虚拟化"技术等同，其本质上只是操作系统中一个隔离的用户空间。它的实现主要依靠以下两个技术：
    - 命名空间（Namespace）。Linux内核一大特性，每个容器都有各自独立的命名空间，它能限制容器中的应用查看和访问系统软硬件资源，
    使容器间互不干扰。因此，容器内的应用都像在独立操作系统中运行一样，无法感知其他容器外进程的存在
    - 控制组（CGroups）。Linux内核提供的对进程进行分组管理的机制。它能对分配给容器的资源加以控制，并设置相应的上限，确保不会因为
    单个容器占用太多资源而导致系统整体陷入瘫痪

综上，容器虚拟化优势在于：
1. 简单部署。开发者使用一个标准镜像来构建开发容器，开发完成后，运维人员可直接将这个容器部署到运行环境而无需安装。不论服务部署到哪里，
容器都可以通过一行命令完成部署，从而从根本上简化了部署应用的工作。
2. 快速可用。容器可以很轻快，因为它是对操作系统进行再次抽象，而非对整个物理机资源进行虚拟化。通过这种方式，打包在容器内的服务可以在
毫秒级的时间快速启动。而如果是启动一台虚拟机一般可能需要一分钟时间。
3. 更高效的虚拟化。容器是内核级虚拟化，运行时不需要Hypervisor支持。因此容器可以实现接近系统原生的性能。
4. 更易扩展和迁移。容器运行对资源进行比虚拟机更细粒度的划分，相对于单个服务运行资源来说，一个小型虚拟机提供的资源过于庞大。因此无论是
进行服务扩展或者迁移，使用容器的成本和工作量总是远低于虚拟机。

## part.2

虚拟化技术的进步推动了云计算运行模式的转变，随着今年来容器技术发展势头逐渐超过虚拟机，云计算的运行模式从原先的以虚拟机为核心的基础设施即服务、
平台即服务和软件即服务转变到以容器为核心的容器即服务。

CaaS是指将容器置于虚拟化层之上，通过虚拟机来运行容器引擎。这样做的原因在于：
1. 云服务直接与容器进行交互，而对底层技术透明，从而充分利用容器简单部署和快速可用特性。
2. 安全问题。容器使用Namespace无法进行彻底的环境隔离，某些特权容器可以对物理机上的文件进行访问和修改。而虚拟机可以作为物理机和容器间
的隔离缓冲区域，帮助容器克服这一问题。
3. 容器在资源控制上不够彻底。因为cgroups只能限制容器消耗最大资源，但不能隔绝其他程序占用自己的资源，无法保证分配给某一服务的系统资源
固定可用。而使用虚拟机先行对这一部分资源进行大粒度划分更有利于服务运行。
4. 性能问题。根据VMware和IBM实验（文献93-94），与直接在物理机上运行容器相比，采用CaaS模式产生的性能损耗微乎其微

而且由于升级到CaaS模式不需要对现行云数据中心软硬件设施进行太多修改，因此成为了目前像google、亚马逊、阿里等云供应商提供容器服务的首选
架构。

## part.3
由于容器的性能代价更低、扩展性和可迁移性更强，因此容器的发展进一步扩大了云计算的规模，但这也导致了云数据中心的能耗问题日益严重。

据调查（文献15）预计到2025年数据中心的消耗的电力将占世界电力消耗的20%，因此通过研究基于容器的负载预测、资源动态配置和调度方法来
控制CaaS模式下云数据中心能耗实现绿色计算，将是未来云计算领域重要研究方向。
