  * **执行avatar抓取快照时提示Permission Denied**
> 部分文件放到了root才可读的目录下，使用sudo avatar进行抓取

  * **Patch时提示 Reversed (or previously applied) patch detected!  Assume -R? [n](n.md) 发现已被patch过的文件**

> 选择回答默认n或直接回车则跳过，则不更改conf文件

> 选择回答y则进行反向patch

  * **Patch结果为空内容**

> 配置文件反复覆盖引起的，比如一个文件隶属于2个及2个以上的包会导致文件发生改变

  * **avatar抓取状态时如果是中文环境则抓取的信息列表不正确**

> 会出现类似 群組 Amusements/Graphics 不包含任何套件 的信息，暂无解决办法，有可能多种语言输出 建议改成英文信息输出