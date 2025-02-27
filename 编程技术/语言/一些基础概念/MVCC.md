MVCC（Multiversion concurrency control 多版本并发控制）

# 要点

1. 每个 `tx` 事务有唯一事务 ID
2. 一个 `tx` 可以包含多个修改操作（put 和 delete），每一个操作叫做一个 revision（修订），共享同一个 main ID
3. 一个 `tx` 内连续的多个修改操作会被从 0 递增编号，这个编号叫做 sub ID
4. 每个 Revision 由（main ID，sub ID）唯一标识

