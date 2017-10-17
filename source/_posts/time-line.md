title: CSS制作时间轴
date: 2015-08-13
categories: tech
---

使用CSS实现时间轴（类似：快递进度）。

<!--more-->

## 效果

[http://runjs.cn/code/en7a5nio](http://runjs.cn/code/en7a5nio)

## HTML结构

```
<div class="m-order-refund">
    <ul class="yo-list">
        <li class="item item-on"><span class="text">提交申请</span></li>
        <li class="item item-on"><span class="text">接受处理</span></li>
        <li class="item"><span class="text">退款成功</span></li>
    </ul>
</div>
```
<!--more-->

## CSS

```
    <style>
        /**
         * RESET
         */
        *,
        ::before,
        ::after {
          -webkit-box-sizing: border-box;
          box-sizing: border-box;
        }
        html {
          background-color: #fff;
          color: #333;
          font-size: 100px;
          -webkit-user-select: none;
          user-select: none;
        }

        body {
          margin: 0;
          font-size: 16px;
          line-height: 1.5;
          font-family: Helvetica Neue, Helvetica, STHeiTi, sans-serif;
        }

        /**
         * 退款流程
         */
        .m-order-refund {
          background-color: #fff;
          padding: .1rem;
        }
        .m-order-refund .yo-list {
            padding: 0;
            margin: 0;
            list-style: none;
        }
        .m-order-refund .yo-list .item {
          position: relative;
          background-image: none;
          padding: .07rem 0 .07rem 0;
          font-size: 0.16rem;
        }
        .m-order-refund .yo-list .item:before {
          display: inline-block;
          content: "";
          width: .08rem;
          height: .08rem;
          background-color: #ccc;
          border-radius: 8px;
          margin: 0 .1rem 0 .01rem;
          z-index: 3;
        }
        .m-order-refund .yo-list .item:after {
          position: absolute;
          left: .04rem;
          top: -.13rem;
          height: .37rem;
          width: .02rem;
          display: inline-block;
          background-color: #ccc;
          content: "|";
          color: transparent;
          z-index: 2;
        }
        .m-order-refund .yo-list .item:first-child:after {
          display: none;
        }
        .m-order-refund .yo-list .item .text {
          color: #000;
          width: .9rem;
          overflow: hidden;
          white-space: nowrap;
          text-overflow: ellipsis;
        }
        .m-order-refund .yo-list .item-on:before, .m-order-refund .yo-list .item-on:after {
          background-color: #1ba9ba;
        }
    </style>
```
