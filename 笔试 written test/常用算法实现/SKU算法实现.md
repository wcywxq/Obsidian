---
title: SKU算法实现
url: https://www.yuque.com/wcywxq/sd8kbc/rs9nxb
---

```javascript
// 生成SKU
// 已知规格数据
const spu = 'AB1234567';
const specList = [ 
 ["red", "yellow"],
 ["XL", "S"], 
 ['a1', 'a2'],
 ['b1', 'b2'],
];

/** 
  输出结果：
 AB1234567-red-XL-a1-b1;
 AB1234567-red-XL-a1-b2;
 AB1234567-red-XL-a2-b1;
 AB1234567-red-XL-a2-b2;
 AB1234567-red-S-a1-b1;
 AB1234567-red-S-a1-b2;
 AB1234567-red-S-a2-b1;
 AB1234567-red-S-a2-b2;
 AB1234567-yellow-XL-a1-b1;
 AB1234567-yellow-XL-a1-b2;
 AB1234567-yellow-XL-a2-b1;
 AB1234567-yellow-XL-a2-b2;
 AB1234567-yellow-S-a1-b1;
 AB1234567-yellow-S-a1-b2;
 AB1234567-yellow-S-a2-b1;
 AB1234567-yellow-S-a2-b2;
*/
function createSKU(spu, specList) {
    let res = specList.reduce(
        (prev, cur) => {
            let ans = []
            prev.forEach(val => {
                cur.forEach(child => {
                    ans.push(val.concat(`-${child}`))
                })
            })
            return ans
        },
        [spu]
        // [[spu]]
    )
    return res
}

console.log(createSKU(spu, specList))
```
