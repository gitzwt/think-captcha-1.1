# think-captcha-1.1
thinkphp5.0 验证码类库(修改版)
> 修改部分,新增cache缓存存储验证码信息(便于api接口调用使用)
> 

## 安装
> composer require gitzwt/think-captcha-1.1

~~~
/**
     * 获取图片验证码(接口调用示例,type为cache时)
     * @return \think\response\Json
     */
    public function getCaptcha()
    {
        $captcha_id = Strs::randString(4, 1) . date('s'); //生成随机id
        //接口构建类的返回
        return $this->buildSuccess([
            'CaptchaId' => (String)$captcha_id, //captcha_id
            'CaptchaSrc' => $this->request->domain() . captcha_src($captcha_id) //验证码url
        ]);
    }
 ~~~


##使用

#### extra子目录内配置 captcha.php
|   参数   |   描述  |   默认   |
| ------- | ------- | ------- | 
| codeSet | 验证码字符集合 | 略 |
| expire | 验证码过期时间（s） | 1800 |
| useZh | 使用中文验证码 | false |
| zhSet | 中文验证码字符串 | 略 |
| useImgBg | 使用背景图片 | false |
| fontSize | 验证码字体大小(px) | 25 |
| useCurve | 是否画混淆曲线 | true |
| useNoise | 是否添加杂点 | true |
| imageH | 验证码图片高度，设置为0为自动计算 | 0 |
| imageW | 验证码图片宽度，设置为0为自动计算 | 0 |
| length | 验证码位数 | 5 |
| fontttf | 验证码字体，不设置是随机获取 | 空 |
| bg | 背景颜色 | [243, 251, 254] |
| reset | 验证成功后是否重置 | true |
| type  | 验证码存储驱动类型,支持session和cache | 默认cache |

###模板里输出验证码

~~~
<div>{:captcha_img()}</div>
~~~
或者
~~~
<div><img src="{:captcha_src()}" alt="captcha" /></div>
~~~
> 上面两种的最终效果是一样的

### 控制器里验证
使用TP5的内置验证功能即可
~~~
$this->validate($data,[
    'captcha|验证码'=>'required|captcha'
]);
~~~
或者手动验证
~~~
if(!captcha_check($captcha)){
 //验证失败
};
~~~

具体使用参照tp5.0完全开发手册验证码部分
