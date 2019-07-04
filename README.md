﻿# 分页式存储管理

## [Paged-storage-management]

> 推荐使用火狐、谷歌、safri、opera 等浏览器查看，IE9 以下没有效果，完整版请使用 IE9+浏览器打开。

在存储器管理中，连续分配方式会形成许多“碎片”，虽然可通过“紧凑”方法将许多碎片拼接成可用的大块空间，但须为之付出很大开销。

如果允许将一个进程直接分散地装入到许多不相邻的分区中，则无须再进行“紧凑”。基于这一思想而产生了离散分配方式。如果离散分配的基本单位是页，则称为分页存储管理方式。在分页存储管理方式中，如果不具备页面对换功能，则称为基本分页存储管理方式，或称为纯分页存储管理方式，它不具有支持实现虚拟存储器的功能，它要求把每个作业全部装入内存后方能运行。

## 基本概念

**页面**  

分页存储管理方式将进程的连续的逻辑地址空间分割为若干个大小相等 的地址片，称之为页或者页面。并将其进行编号，如第 0 页、第 1 页。在逻辑上，这些页中的各个地址拼接起来是连续的，但是他们对应的物理块地址是可以不连续的。对应的物理块也相应的被命名为第 0 块、第 1 块等。在进程分配内存时，进程的若干个页分别被装入对应的物理块，由于最后一页经常装不满，所以形成了无法被利用的页内碎片

采用分页存储管理方式的目的就是更多的利用内存碎片，因此，页面的大小相对来说是小于正常进程所需的地址空间大小。那么页面多大是比较合适的呢？页面如果过于小，虽然可以使得内存中不可利用碎片的最大大小降低，提升内存空间的利用率，但是也会导致进程占用过多的页面，使得页表过长，降低页面的换进换出的效率。同样，如果页面过大，虽然提升了页面换进换出的效率，但是也会使得内存碎片增大。所以，根据经验，页面的大小最好选择 512B~8KB 左右。

**页表**  

页表就是进程中维护的一段地址空间，其中存储了页面和物理块的映射关系，在讨论页表之前，先理清分页中的地址结构。  
对于物理地址空间，总的内存地址空间是固定的。而对于一个进程，分配的逻辑内存空间也是固定且连续的，该逻辑地址空间从 0 开始，假设某个进程中的一个字节的逻辑地址为 1088B, 而页面大小为 512B。在介绍页面 时说过，逻辑地址空间虽然分给为若干个页，但是这些页的逻辑地址仍然是连续的。那么我们可以知道该字节所属的页号 `P = 2 (从 0 开始)`，页内地址 `d = (1088 - 512 * 2) = 64`。

也就是说在页面长度已知的情况下，根据逻辑地址可以简单的知道该地址空间所属的页面号和在该页面中的相对地址。根据这两个参数和页表，就能够知道该地址对应的物理地址了。

通过页表和页号，我们知道了物理块号，也就知道了对应物理块空间的起始地址。再加上页内地址，就知道了对应的物理地址。通过这种方式，我们能够实现从逻辑地址到物理地址的转换。

此版本目前只有功能版。
