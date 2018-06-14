![Async Logo](https://yunxi-material-library.oss-cn-hangzhou.aliyuncs.com/recordImg/786/EsSfKfAaMz.png)

JavaScript算法题(ES6)，不定期更新。。。

收藏和积累，个人向思维逻辑和编码风格（仅供参考）

我很欢迎别人能在自己的代码里获得一些收获，get一些技巧和代码思路等等
然而我真正希望看到的，是希望给予刚入门或者入门不久的人一些自己个人衷心的见解：JavaScript是一门语言！
在平时无论小到一个简单的页面jQuery交互，或者承担一个大型前端项目的业务逻辑我们都会使用到它(大都会依赖一些框架)，look！这些都是JS,没有错！
可不知道你是否有一种似是而非的感觉，是它但又感觉不是它，没错，我希望你能够从这层层叠叠的框架里面从眼前挪开，能够去了解它更加本质的核心；
那是否就是要去研究那些浏览器底层的BOM,DOM操作呢？前端是扎根于页面，然而我希望你的JS能从客户端的泥潭中挣脱出来；

这些JavaScript算法题，没有任何框架依赖或者封装，非常纯粹；我使用JS来实现自己的想法和思维逻辑，只需专注于思路，而不用在意JS语言本身实现，就像说话一样自然
但愿你能在这些拙劣的代码里发现JS本身的特点和一些用法(es6也给js注入了一种简洁和优雅)，而真正喜欢并去学习掌握它；

*JavaScript是一门语言*


```javascript
// demo...
const sr = s.split('');
    let rel = '';
    let l = 0;

    function calLongStr(sr) {
        let _s = '';

        for (let [index, item] of sr.entries()) {
            if (_s.indexOf(item) == -1) {
                _s += item;
            } else {
                if (_s.length > l) {
                    rel = _s;
                    l = _s.length;
                }

                if (sr.slice(index).length > l) {
                    calLongStr(sr.slice(index));
                }

                break;
            }

            rel = _s;
            l = _s.length;
        }
    }
```
