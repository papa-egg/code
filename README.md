![Async Logo](https://yunxi-material-library.oss-cn-hangzhou.aliyuncs.com/recordImg/786/EsSfKfAaMz.png)

JavaScript算法题([ES6](http://es6.ruanyifeng.com/))，不定期更新。。。

收藏和积累，个人向思维逻辑和编码风格（仅供参考）

我很欢迎别人能在自己的代码里获得一些收获，get一些技巧和代码思路等等，
然而我主要意图是给予刚入门或者入门不久的人一些自己个人衷心的见解：<font color='red'>***JavaScript是一门语言！***</font>

我希望你能够把这些层层叠叠的框架里面从眼前挪开，能够去注重和了解它的核心

学习JS，是不是需要研究那些浏览器底层的BOM,DOM实现操作呢？---前端是扎根于页面，然而我希望你的JS能从客户端的泥潭中挣脱出来

*但愿你能在这些拙劣的代码里发现JS本身的特点和一些用法([ES6](http://es6.ruanyifeng.com/)也给js注入了一种简洁和优雅)，而真正喜欢并去学习掌握它*


```javascript
// demo...
let _spr = [];

    function splitItem(sr = '', k, n) {
        const _cArr = sr ? sr.split('|') : [];

        if (_cArr.length == n) {
            _cArr.sort((a, b) => a - b);

            if (_spr.indexOf(_cArr.join('|')) < 0) {
                _spr.push(_cArr.join('|'));
            }

            return;
        }

        for (let i = 1; i <= k; i++) {
            if (_cArr.indexOf(String(i)) < 0) {
                let _cr = sr;

                if (_cArr.length > 0) {
                    _cr = _cr + '|' + i;
                } else {
                    _cr += i;
                }

                splitItem(_cr, k, n);
            }
        }
    }
```
