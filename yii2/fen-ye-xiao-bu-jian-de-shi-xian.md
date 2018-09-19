---
title: 分页小部件LinkPager
date: '2017/1/14 20:46:25'
categories: yii2
tags:
  - LinkPager
description: '分页功能是网页中最基本的功能,本文详解通过小部件LinkPager来实现对分页的显示与控制'
---

# 分页小部件的实现

## 小部件 `LinkPager` 显示一个分页按钮的列表

在所需视图中加入下列代码

```php
<?= LinkPager::widget(['pagination' => $pagination]) ?>
```

## 控制器层

```php
<?php
namespace app\controllers;

use yii\web\Controller;
use yii\data\Pagination;
use app\models\Country;

class CountryController extends Controller{

    public function actionIndex(){
        $query = Country::find(); //查询`country`表
        $countQuery = clone $query;
        //获取页码,默认为第一页
        $page=intval(Yii::$app->request->get('page')) > 0 ? intval(Yii::$app->request->get('page')) : 1;
        //获取每页显示条数,默认为20个
        $pageSize=intval(Yii::$app->request->get('pageSize'))>0 ? intval(Yii::$app->request->get('pageSize')) : 20;
        //创建分页实例
        $pagination = new Pagination([
            'defaultPageSize' => 5,//设置分页显示数
            'page'=>$page-1, //注:页码计算从0开始
            'pageSize'=>$pageSize,
            'totalCount' => $countQuery->count(),
        ]);
        //查询页码对应数据
        $countries = $query
            ->offset($pagination->offset)
            ->limit($pagination->limit)
            ->all();
        //渲染视图
        return $this->render('index', [
                'countries' => $countries,
                'pagination' => $pagination,
                'pageCount' => $pagination->getPageCount(), //总页数
        ]);
    }
}
```

## 视图层

```php
//显示分页小部件
echo LinkPager::widget([
    'pagination' => $pagination,
]);
```

## 分享一篇解析配置参数的文章

[http://www.manks.top/yii2\_linkpager\_pagination.html](http://www.manks.top/yii2_linkpager_pagination.html)

