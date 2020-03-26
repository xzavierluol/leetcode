# 最长公共前缀

---

## 题目描述
&nbsp;&nbsp;编写一个函数来查找字符串数组中的最长公共前缀。
## 解题思路
### 个人思路
&nbsp;&nbsp;**思路1**:横向扫描，每两个两个比对，获取对应的前缀，然后如果前缀不为空字符串，则把其获取到的前缀放到对应的数组中，并将原有的两个移除出去，以此类推，最终只会剩下两个字符串。
**思路2**:纵向扫描，首先将每一个的字符串的第一个字符相互做比较，如果全部相等则比较第二个，否则返回已经匹配到的字符串。
**思路3**:这种统计字符串数组的单词前缀,打印指定前缀单词、查找字符串等都可以使用trie树来实现。由于很久没有用到trie树，所以这道题目我用到了trie树。可以构建一个比较简单的trie树，每个节点有以下结构:
```go
type Trie struct {
	num int
	val byte
	son map[byte]*Trie
}
```
num表示有多少字符串有该前缀，val表示当前节点的值(即前缀的最后一个字符串)，son表示该前缀的孩子节点，表示下面有哪些字母。所以我们只要将所有的字符串填充到trie树中，然后遍历trie树，如果当前节点num为字符串数组总长度以及son长度只为1(即只有一个孩子节点)，那么这个是公共前缀。


##代码
### 个人代码
```go
type Trie struct {
	num int
	val byte
	son map[byte]*Trie
}

func NewTrie() *Trie {
	return &Trie{
		num: 0,
		val: 0,
		son: make(map[byte]*Trie),
	}
}

func longestCommonPrefix(strs []string) string {
	head := NewTrie()
	var nt *Trie
	var lstr, lstrs int
	lstrs = len(strs)
	for _, str := range strs {
		nt = head
		lstr = len(str)
		for i := 0; i < lstr; i++ {
			_, ok := nt.son[str[i]]
			if !ok {
                if len(nt.son) != 0 {
                    break
                }
				newT := NewTrie()
				newT.val = str[i]
				nt.son[str[i]] = newT
			}
			nt.num++
			nt = nt.son[str[i]]
		}
	}

	nt = head
	var buf bytes.Buffer
	for {
		if len(nt.son) != 1 || nt.num != lstrs {
			break
		}
		for _, v := range nt.son {
			buf.WriteByte(v.val)
			nt = v
		}
	}
	return buf.String()
}
```
