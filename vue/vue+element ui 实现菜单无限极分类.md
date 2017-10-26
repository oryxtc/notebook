---
title: vue+element ui 实现菜单无限极分类
date: 2017/10/26
categories:
- vue
description: 通过实现一个element ui的菜单组件,再递归该组件,实现菜单的无限极分类
---

### 需后端返回数据结构如下:
> 后端实现方法可参考:

```php
[
    {
        "id": 1,
        "parent_id": 0,
        "menu_name": "第一级菜单 1",
        "sorting": 0,
        "node": [
            {
                "id": 2,
                "parent_id": 1,
                "menu_name": "第二级菜单 1-1",
                "sorting": 0,
                "node": [
                    {
                        "id": 3,
                        "parent_id": 2,
                        "menu_name": "第三级菜单 1-1-1",
                        "sorting": 1
                    }
                ]
            }
        ]
    },
    {
        "id": 4,
        "parent_id": 0,
        "menu_name": "第一级菜单 2",
        "sorting": 0,
        "node": [
            {
                "id": 5,
                "parent_id": 4,
                "menu_name": "第二级菜单 2-1",
                "sorting": 0
            }
        ]
    }
]
```

