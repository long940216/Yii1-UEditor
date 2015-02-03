Yii1-UEditor
===================
项目更名为Yii1-UEditor，采用2推荐的方式重新命名，方便管理。

Yii v1.x 的UEditor扩展，支持的UEditor版本为1.4.3。

测试使用的PHP版本为5.5.9，测试使用的Yii版本是1.1.15，采用alias的方式部署在Ubuntu apache 2.4.7。

扩展特性：

支持自动的缩略图管理（默认开启，可以关闭）。

支持水印（默认关闭，可以开启）。

使用TP框架的tpImage来生成和处理图片。

注意：2015.2.2 更新版本与之前并不兼容，本次修改更贴近InputWidget的设计意图，扩展使用时将不再需要原有的输入框。

使用说明
---------------------

1、将ueditor放在项目的/protected/extensions/目录下。

2、配置UEditor的后台控制器

方法有两种

1）、自己写controller

在/protected/controllers目录新建一个controller，并继承UeditorController，如下：

```php
Yii::import('ext.ueditor.UeditorController');
class EditorController extends UeditorController{
//自定义修改
}
```

这时候serverUrl为刚才新建的EditorController，在下面配置widget时候要特别注意。

这样做的好处是，可以自定义多种使用场景，较为灵活。

2）配置controllerMap

在config.php中配置controllerMap，来指定ueditor的后端控制器。

当有多个使用场景时，可以配置多个map，在widget使用时指定serverUrl即可。

```php
    'controllerMap'=>array(
        'ueditor'=>array(
            'class'=>'ext.ueditor.UeditorController',
        ),
    ),
```

可选配置:

```php
    'controllerMap'=>array(
        'ueditor'=>array(
            'class'=>'ext.ueditor.UeditorController',
            'config'=>array(),//参考config.json的配置，此处的配置具备最高优先级
            'thumbnail'=>true,//是否开启缩略图
            'watermark'=>'',//水印图片的地址，使用相对路径
            'locate'=>9,//水印位置，1-9，默认为9在右下角
        ),
    ),
```

这样做的好处是，配置方便快捷，不需要增加额外的controller。

如果thumbnail属性为false，后端将不会生成缩略图。

具体的config配置参考[UEditor后端配置项说明](http://fex.baidu.com/ueditor/#server-config 后端配置项说明.md)。

3、在view中使用widget。

注意：与上一个版本不同，这里需要删除原来的输入框，widget会自动生成。

配合AR使用：

```php
    $this->widget('ext.ueditor.UeditorWidget',
            array(
                'model' => $model,
                'attribute' => 'content',
                'htmlOptions' => array('rows'=>6, 'cols'=>50)
    ));
```

当作普通表单使用:

```php
    $this->widget('ext.ueditor.UeditorWidget',
        array(
            'id'=>'Post_excerpt',
            'name'=>'excerpt_editor',
            'value' => '输入值',
            'config'=>array(
                'serverUrl' => Yii::app()->createUrl('editor/'),//指定serverUrl
                'toolbars'=>array(
                    array('source','link','bold','italic','underline','forecolor','superscript','insertimage','spechars','blockquote')
                ),
                'initialFrameHeight'=>'150',
                'initialFrameWidth'=>'95%'
            ),
            'htmlOptions' => array('rows'=>3,'class'=>'span12 controls')
    ));
```

widget默认的serverUrl为/ueditor，如果自己写了controller或者在controllerMap中配置了多个控制器，那么一定要在widget的配置中增加serverUrl的配置。

如果thumbnail属性为false，前端将不会附加缩略图管理代码。

具体的config配置参考[UEditor前端配置项说明](http://fex.baidu.com/ueditor/#start-config 前端配置项说明.md)。

4、错误排除

- 出现错误首先应该打开调试工具查看请求返回具体信息。

- 因为编辑器通常使用场景为后台，所以图片上传等功能默认需要登录权限，如果不需要可以自行修改。

- 默认上传路径为「应用根目录」，而不是网站根目录，如果上传失败请查看目录权限。

- 不要开启Yii的调试，因为UEditor的返回都是json格式，开启调试会导致返回格式不识别。

- 出现404错误可能是因为widget没有正确配置serverUrl。


其他说明
---------------------
@see https://github.com/fex-team/ueditor

参考：[［更新］UEditor1.4.3-for-Yii1-扩展](http://www.crazydb.com/archive/更新_UEditor1.4.3-for-Yii1-扩展 UEditor1.4.3-for-Yii1-扩展)。