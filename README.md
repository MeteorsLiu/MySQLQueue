# FastSQLQueue
A Simple MySQL Queue for Golang

# Feature

1. Thread Safe
2. High Performance and fast
3. Lightweight
4. SQL Queue and re-format to Map type
5. Easy to use


# 设计思路

利用Channel特性，完成多Goroutine下排队读写的问题，一个Channel两端会有任意一方被block，直到任务完成为止
在Golang设计中，Channel是一个阻塞的FIFO队列

例如：

A,B,C三人同时请求

那么A或B或C随机一方的SQL被后台的Goroutine接收，开始执行任务，而Channel一旦被接收后，由于case里面代码仍未执行完毕，因此Channel又可处于发送状态，又可以发送，但发送过后由于后台Goroutine仍未完成请求，因此会被暂停接收，直到任务完成为止，才重新开始继续接收，这样达到了前面排队的目的。而出channel是为了确保数据唯一。

# 局限性

没有多worker执行任务的功能，原因是作者明确说明了不允许多goroutine同时操作

# 有待优化

~~1. Query会新产生map，这是完全可以避免的~~

2. 没有Context

3. 每一个查询请求都会产生两个slices，无疑增大GoGC压力，想办法解决中

# 文档

