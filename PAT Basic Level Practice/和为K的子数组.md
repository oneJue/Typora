## 前缀和

```python
class Solution:
	def subarraySum(self, nums: List[int], k: int)-> int:
		cur_sum =0
		adict = {}
		adict[0]= 1
		count =0
		for num in nums:
			cur_sum += num
			if cur_sum- k in adict:
				count += adict[cur_sum- k]
			if cur_sum in adict:
				adict[cur_sum] += 1
			else:
				adict[cur_sum]= 1
		return count
```



