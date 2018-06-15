# scan key_start key_end limit

列出处于区间 `(key_start, key_end]` 的 key-value 列表.

`("", ""]` 表示整个区间.

这个命令可实现类似通配符 `*` 号的查找, 但是, 仅支持前缀查找, 而且, `*` 必须被省略 - 不要在查询参数里输入 `*` 号!

__注意, scan 并不是前缀搜索, 即使不带有指定参数前缀的 key 也会返回, 因为这是区间搜索!__ 

在保证你的 key 只包含 ASCII 字符(不含'}')的前提下, 你可以把 key_end 设置为 key_start + '}'.

## 参数

* `key_start` - 返回的起始 key(不包含), 空字符串表示 -inf(无限小).
* `key_end` - 返回的结束 key(包含), 空字符串表示 +inf(无限大).
* `limit` - 最多返回这么多个元素. 

## 返回值

Key-value List reply.

返回 key value 依次出现的列表.

## 示例

	ssdb 127.0.0.1:8888> scan "" "" 10
	key             value
	-------------------------
	  a               : 1
	  aa              : 1
	  b               : 2
	  c               : 3
	2 result(s) (0.000 sec)
	ssdb 127.0.0.1:8888> scan "a" "" 10
	key             value
	-------------------------
	  aa              : 1
	  b               : 2
	  c               : 3
	2 result(s) (0.000 sec)
	# 'a' 不返回! 另外, 因为没有指定 key_end, 所以 'b', 'c' 也会被返回!
	ssdb 127.0.0.1:8888> scan a a} 10
	key             value
	-------------------------
	  aa             : 1
	1 result(s) (0.000 sec)
	(0.000 sec)
	# 实现真正的前缀搜索
	ssdb 127.0.0.1:8888> scan "a" "b" 10
	key             value
	-------------------------
	  aa              : 1
	  b               : 2
	2 result(s) (0.000 sec)
	# 指定 key_end 为 'b'(包含), 所以 'b' 也会被返回! 'c' 不会.
