
- [Prometheus 数据存储]()
  - tsdb 数据
    - 基于相对稳定频率持续产生的一系列指标监测数据，那么存储就是一些标签键加上一个时间序列作为一个大 key，值就是一个数字
  - 对于数据的存储 Prometheus 按冷热数据进行分离，最近的数据肯定是看的最多的，所以缓存在内存里面，为了防止宕机而导致数据丢失因而引入 wal 来做故障恢复
  - 数据超过一定量之后会从内存里面剥离出来以 chunk 的形式存放在磁盘上这就是 head chunk。
  - 对于更早的数据会进行压缩持久化变成 block 存放到磁盘中。
  - 对于 block 中的数据由于是不会变的，数据较为固定，所以每个 block 通过 index 来索引其中的数据，并且为了加快数据的查询引用倒排索引，便于快速定位到对应的 chunk。