![Async Logo](https://yunxi-material-library.oss-cn-hangzhou.aliyuncs.com/recordImg/786/M4haWnYNQP.jpg)

JavaScript算法题([ES6](http://es6.ruanyifeng.com/))，不定期更新。。。

收藏和积累，个人向思维逻辑和编码风格（仅供参考）

我很欢迎别人能在自己的代码里获得一些收获，get一些技巧和代码思路等等，
然而我主要意图是给予刚入门或者入门不久的人一些自己个人衷心的见解：<font color='red'>`JavaScript是一门语言！`</font>

我希望你能够把这些层层叠叠的框架里面从眼前挪开，能够去注重和了解它的核心

学习JS，是不是需要研究那些浏览器底层的BOM,DOM实现操作呢？---*前端是扎根于页面，然而我希望你的JS能从客户端的泥潭中挣脱出来*

*但愿你能在这些拙劣的代码里发现JS本身的特点和一些用法([ES6](http://es6.ruanyifeng.com/)也给js注入了一种简洁和优雅)，而真正喜欢并去学习掌握它*


```javascript
// demo...
    const sr = [1, 2, 3, 4, 5, 6, 7, 8, 9];
    const rel = [];

    function getCNum(ss) {
        let _s = 0;

        for (let item of ss) {
            _s += parseInt(item);
        }

        return _s;
    }

    function cycleCumulative(ci = 0, ss = '') {
        for (let i = ci; i < sr.length; i++) {
            const _cs = getCNum(ss);
            const _cd = ss + sr[i];

            if (_cd.length == k) {
                if (_cs + sr[i] == n) {
                    const ra = [];

                    [..._cd].forEach((item) => {
                        ra.push(parseInt(item));
                    });

                    rel.push(ra);
                }
            } else {
                cycleCumulative(sr[i], _cd);
            }
        }
    }
```
