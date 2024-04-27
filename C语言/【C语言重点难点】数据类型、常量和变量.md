 

### 文章目录

- [一：C语言包含的数据类型](#C_8)
- [二：各数据类型细节](#_11)
- [三：C语言中常量和变量](#C_14)
- [三：变量命名规则](#_22)

这部分东西在C语言考试中特别爱考，但是十分琐碎，所以总结如下![请添加图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F695ddea8daa744e893c1c3ac38b66a02.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120575979)

# 一：C语言包含的数据类型

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F27eb1e91b1f641409726bb1060df94c2.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120575979)

# 二：各数据类型细节

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F46a50b412c1a4b4cbf7617f983549e0a.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120575979)

# 三：C语言中常量和变量

![请添加图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F0a73fb0dfe7b49fb8c3f88fc517bd2df.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5b-r5LmQ5rGf5rmW%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120575979)

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F0986f85a06ee4564a05c1fd6f5caf7d5.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s%2Cshadow_50%2Ctext_Q1NETiBA5oiR5pOm5LqGREo%3D%2Csize_20%2Ccolor_FFFFFF%2Ct_70%2Cg_se%2Cx_16&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F120575979)

# 三：变量命名规则

这一点还是值得详谈的，因为有的人变量命名是在是不忍直视。

变量命名的基本规则大家应该很熟悉了

1.  变量名只能由**英文字母，数字和下划线**组成
2.  变量名必须以**字母或下划线**开头
3.  不允许使用C关键字作为变量名
4.  变量名可以包含任意多个字符